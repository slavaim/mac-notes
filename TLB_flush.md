
```
/*
 * Called with pmap locked, we:
 *  - scan through per-cpu data to see which other cpus need to flush
 *  - send an IPI to each non-idle cpu to be flushed
 *  - wait for all to signal back that they are inactive or we see that
 *    they are at a safe point (idle).
 *  - flush the local tlb if active for this pmap
 *  - return ... the caller will unlock the pmap
 */

void
pmap_flush_tlbs(pmap_t	pmap, vm_map_offset_t startv, vm_map_offset_t endv, int options, pmap_flush_context *pfc)
{
      ...
      for (cpu = 0, cpu_bit = 1; cpu < real_ncpus; cpu++, cpu_bit <<= 1) {
        if (!cpu_datap(cpu)->cpu_running)
          continue;
        uint64_t	cpu_active_cr3 = CPU_GET_ACTIVE_CR3(cpu);
        uint64_t	cpu_task_cr3 = CPU_GET_TASK_CR3(cpu);

        if ((pmap_cr3 == cpu_task_cr3) ||
            (pmap_cr3 == cpu_active_cr3) ||
            (pmap_is_shared)) {
            ...

                  /*
                   * We don't need to signal processors which will flush
                   * lazily at the idle state or kernel boundary.
                   * For example, if we're invalidating the kernel pmap,
                   * processors currently in userspace don't need to flush
                   * their TLBs until the next time they enter the kernel.
                   * Alterations to the address space of a task active
                   * on a remote processor result in a signal, to
                   * account for copy operations. (There may be room
                   * for optimization in such cases).
                   * The order of the loads below with respect
                   * to the store to the "cpu_tlb_invalid" field above
                   * is important--hence the barrier.
                   */
                  if (CPU_CR3_IS_ACTIVE(cpu) &&
                      (pmap_cr3 == CPU_GET_ACTIVE_CR3(cpu) ||
                       pmap->pm_shared ||
                       (pmap_cr3 == CPU_GET_TASK_CR3(cpu)))) {
                      cpus_to_signal |= cpu_bit;
                      i386_signal_cpu(cpu, MP_TLB_FLUSH, ASYNC);
                  }
            ...
          }
          ...
      }
      
      if (cpus_to_signal) {
        ...
        for (cpu = 0, cpu_bit = 1; cpu < real_ncpus; cpu++, cpu_bit <<= 1) {
            /* Consider checking local/global invalidity
             * as appropriate in the PCID case.
             */
            if ((cpus_to_respond & cpu_bit) != 0) {
              if (!cpu_datap(cpu)->cpu_running ||
                  cpu_datap(cpu)->cpu_tlb_invalid == FALSE ||
                  !CPU_CR3_IS_ACTIVE(cpu)) {
                cpus_to_respond &= ~cpu_bit;
              }
              cpu_pause();
            }
            if (cpus_to_respond == 0)
              break;
        }
        ...
    }
}

void
i386_signal_cpu(int cpu, mp_event_t event, mp_sync_t mode)
{
	volatile int	*signals = &cpu_datap(cpu)->cpu_signals;
	uint64_t	tsc_timeout;

	
	if (!cpu_datap(cpu)->cpu_running)
		return;

	if (event == MP_TLB_FLUSH)
	        KERNEL_DEBUG(TRACE_MP_TLB_FLUSH | DBG_FUNC_START, cpu, 0, 0, 0, 0);

	DBGLOG(cpu_signal, cpu, event);
	
	i_bit_set(event, signals);
	i386_cpu_IPI(cpu);
	if (mode == SYNC) {
	   again:
		tsc_timeout = !machine_timeout_suspended() ?
					rdtsc64() + (1000*1000*1000) :
					~0ULL;
		while (i_bit(event, signals) && rdtsc64() < tsc_timeout) {
			cpu_pause();
		}
		if (i_bit(event, signals)) {
			DBG("i386_signal_cpu(%d, 0x%x, SYNC) timed out\n",
				cpu, event);
			goto again;
		}
	}
	if (event == MP_TLB_FLUSH)
	        KERNEL_DEBUG(TRACE_MP_TLB_FLUSH | DBG_FUNC_END, cpu, 0, 0, 0, 0);
}

void
i386_cpu_IPI(int cpu)
{
#ifdef	MP_DEBUG
	if(cpu_datap(cpu)->cpu_signals & 6) {	/* (BRINGUP) */
		kprintf("i386_cpu_IPI: sending enter debugger signal (%08X) to cpu %d\n", cpu_datap(cpu)->cpu_signals, cpu);
	}
#endif	/* MP_DEBUG */

	lapic_send_ipi(cpu, LAPIC_VECTOR(INTERPROCESSOR));
}

void
lapic_send_ipi(int cpu, int vector)
{
	boolean_t	state;

	if (vector < lapic_interrupt_base)
		vector += lapic_interrupt_base;

	state = ml_set_interrupts_enabled(FALSE);

	/* Wait for pending outgoing send to complete */
	while (LAPIC_READ_ICR() & LAPIC_ICR_DS_PENDING) {
		cpu_pause();
	}

	LAPIC_WRITE_ICR(cpu_to_lapic[cpu], vector | LAPIC_ICR_DM_FIXED);

	(void) ml_set_interrupts_enabled(state);
}
```
