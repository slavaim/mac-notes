```
/*
 * Switch to a new thread.
 * Save the old thread`s kernel state or continuation,
 * and return it.
 */
thread_t
machine_switch_context(
	thread_t			old,
	thread_continue_t	continuation,
	thread_t			new)
{
	assert(current_cpu_datap()->cpu_active_stack == old->kernel_stack);

#if KPC
	kpc_off_cpu(old);
#endif /* KPC */

	/*
	 *	Save FP registers if in use.
	 */
	fpu_switch_context(old, new);

	old->machine.specFlags &= ~OnProc;
	new->machine.specFlags |= OnProc;

	/*
	 * Monitor the stack depth and report new max,
	 * not worrying about races.
	 */
	vm_offset_t	depth = current_stack_depth();
	if (depth > kernel_stack_depth_max) {
		kernel_stack_depth_max = depth;
		KERNEL_DEBUG_CONSTANT(
			MACHDBG_CODE(DBG_MACH_SCHED, MACH_STACK_DEPTH),
			(long) depth, 0, 0, 0, 0);
	}

	/*
	 *	Switch address maps if need be, even if not switching tasks.
	 *	(A server activation may be "borrowing" a client map.)
	 */
	pmap_switch_context(old, new, cpu_number());

	/*
	 *	Load the rest of the user state for the new thread
	 */
	act_machine_switch_pcb(old, new);

#if HYPERVISOR
	ml_hv_cswitch(old, new);
#endif

	return(Switch_context(old, continuation, new));
}

/*
 * thread_t Switch_context(
 *		thread_t old,				// %rsi
 *		thread_continue_t continuation,		// %rdi
 *		thread_t new)				// %rdx
 */
Entry(Switch_context)
	popq	%rax				/* pop return PC */

	/* Test for a continuation and skip all state saving if so... */
	cmpq	$0, %rsi
	jne 	5f
	movq	%gs:CPU_KERNEL_STACK,%rcx	/* get old kernel stack top */
	movq	%rbx,KSS_RBX(%rcx)		/* save registers */
	movq	%rbp,KSS_RBP(%rcx)
	movq	%r12,KSS_R12(%rcx)
	movq	%r13,KSS_R13(%rcx)
	movq	%r14,KSS_R14(%rcx)
	movq	%r15,KSS_R15(%rcx)
	movq	%rax,KSS_RIP(%rcx)		/* save return PC */
	movq	%rsp,KSS_RSP(%rcx)		/* save SP */
5:
	movq	%rdi,%rax			/* return old thread */
	/* new thread in %rdx */
	movq    %rdx,%gs:CPU_ACTIVE_THREAD      /* new thread is active */
	movq	TH_KERNEL_STACK(%rdx),%rdx	/* get its kernel stack */
	lea	-IKS_SIZE(%rdx),%rcx
	add	EXT(kernel_stack_size)(%rip),%rcx /* point to stack top */

	movq	%rdx,%gs:CPU_ACTIVE_STACK	/* set current stack */
	movq	%rcx,%gs:CPU_KERNEL_STACK	/* set stack top */

	movq	KSS_RSP(%rcx),%rsp		/* switch stacks */
	movq	KSS_RBX(%rcx),%rbx		/* restore registers */
	movq	KSS_RBP(%rcx),%rbp
	movq	KSS_R12(%rcx),%r12
	movq	KSS_R13(%rcx),%r13
	movq	KSS_R14(%rcx),%r14
	movq	KSS_R15(%rcx),%r15
	jmp	*KSS_RIP(%rcx)			/* return old thread */
  
/*
 * KSS_* are offsets from the top of the kernel stack (cpu_kernel_stack)
 */
DECLARE("KSS_RBX",	offsetof(struct thread_kernel_state, machine.k_rbx));
DECLARE("KSS_RSP",	offsetof(struct thread_kernel_state, machine.k_rsp));
DECLARE("KSS_RBP",	offsetof(struct thread_kernel_state, machine.k_rbp));
DECLARE("KSS_R12",	offsetof(struct thread_kernel_state, machine.k_r12));
DECLARE("KSS_R13",	offsetof(struct thread_kernel_state, machine.k_r13));
DECLARE("KSS_R14",	offsetof(struct thread_kernel_state, machine.k_r14));
DECLARE("KSS_R15",	offsetof(struct thread_kernel_state, machine.k_r15));
DECLARE("KSS_RIP",	offsetof(struct thread_kernel_state, machine.k_rip));
```
