This is a copy of a thread from the darwin mailing list regarding preemption level retrieval in a kernel debug session. 
The delta values for cpu_preemption_level on 10.14 Mojave are as follows. To get an actual value for a CPU subtract this deltas from cpu_data_ptr[CPU_NUM]->cpu_preemption_level

1) A CPU interrupted by a NMI to enter debugger - 4 .   
   Others CPUS - 1 .   

2) A CPU which called panic() - 3 .   
   Other CPUs - 1 .   
     
The value is increased by an interrupt handler that processes IPI or NMI to freeze CPUs and by panic code, e.g.

```
panic_trap_to_debugger(...)
{
...
	disable_preemption();
...
}

hndl_allintrs(...)
{
	...
	incl	%gs:CPU_PREEMPTION_LEVEL
	...
}

kdp_i386_trap(...)
{
    ...
    disable_preemption();
    ...
}
```

<------------------------>  
On Sun, Jun 17, 2018 at XXXXX, Slava Imameev XXXXXXXX@gmail.com wrote: 

Hi,

The cpu_preemption_level values were altered by the panic code. The actual values might be calculated by subtracting 3 from the cpu_preemption_level value for a CPU that panic'ed (panic_trap_to_debugger, kdp_i386_trap increase it) and 1 for other CPUs(a value is increased when processing IPI / NMI sent to freeze CPUs). That said the CPU's preemption level for a cpu that called msleep() was -1 before calling panic() and 0 for another cpu. The showcurrentstacks command has a bug by reporting values altered either by panic() or a kernel debugger code.

<------------------------>

On Sun, Jun 17, 2018 at 9:25 PM, Irad K <XXXXXXXX@gmail.com> wrote:
Hi Slava and thanks for your support,

I've followed your instructions and found out that both processors I've got (my VM runs 2 processors)  have positive value in the preemption level unlike the panic log that print negative preemption value (= -1) 
Notice that the thread the produces the panic, runs my driver code and it's calling `msleep` command with NULL mutex so I'm not sure how did it got to "unlocking an unlocked mutex or spinlock".

1. Perhaps do you know what does cpu_preemption_level means ? (my guess is that it represent the number of threads in scheduler that wait on mutex - this can explain that negative value means that we unlocked more than we've locked). 
2. How can we explain the contradiction between cpu_data_ptr[X]->cpu_preemption_level and the paniclog preemption level... perhaps it's a bug in 10.14 ? 


I attached here below the panic log, the data from cpu_data_ptr[X] for both cpus, and output from showcurrentstacks.

```
panic(cpu 1 caller 0xffffff800affef06): "thread_invoke: preemption_level -1, possible cause: unlocking an unlocked mutex or spinlock"@/BuildRoot/Library/Caches/com.apple.xbs/Sources/xnu/xnu-4903.200.199.11.1/osfmk/kern/sched_prim.c:2263
Backtrace (CPU 1), Frame : Return Address

0xffffff801201b850 : 0xffffff800afe2c3d mach_kernel : _handle_debugger_trap + 0x48d
0xffffff801201b8a0 : 0xffffff800b11b373 mach_kernel : _kdp_i386_trap + 0x153
0xffffff801201b8e0 : 0xffffff800b10ceb4 mach_kernel : _kernel_trap + 0x594
0xffffff801201b960 : 0xffffff800af90c80 mach_kernel : _return_from_trap + 0xe0
0xffffff801201b980 : 0xffffff800afe2657 mach_kernel : _panic_trap_to_debugger + 0x197
0xffffff801201baa0 : 0xffffff800afe24a3 mach_kernel : _panic + 0x63
0xffffff801201bb10 : 0xffffff800affef06 mach_kernel : _thread_unstop + 0x1366
0xffffff801201bb90 : 0xffffff800affdaff mach_kernel : _thread_block_reason + 0xaf
0xffffff801201bbe0 : 0xffffff800b4fcfe1 mach_kernel : _sleep + 0x331
0xffffff801201bc60 : 0xffffff800b4fd352 mach_kernel : _msleep + 0x62

...


(lldb) p cpu_data_ptr[0]->cpu_preemption_level
  cpu_this = 0xffffff800b80c900
  cpu_active_thread = 0xffffff8016ed5440
  cpu_nthread = 0x0000000000000000
  cpu_preemption_level = 1
  cpu_number = 0
  cpu_int_state = 0xffffff809da73cf0
  cpu_active_stack = 0xffffff809da70000
  cpu_kernel_stack = 0xffffff809da73fb0
  cpu_int_stack_top = 0xffffff807d45a000
  cpu_interrupt_level = 1
  cpu_signals = 0
  cpu_prior_signals = 2
  cpu_pending_ast = 0
  cpu_running = 1
  ...
  cpu_task_map = TASK_MAP_64BIT
  cpu_task_cr3 = 1851977728
  cpu_kernel_cr3 = 235036672
  cpu_ucr3 = 654248203
  cpu_pagezero_mapped = 0
  cpu_uber = (cu_isf = 18446743524420743424, cu_tmp = 33554549, cu_user_gs_base = 123145317695712)
  cd_estack = 0xfffffd000007b200
  cpu_xstate = 2
  cd_shadow = 0xffffff800ad08000
  cpu_processor = 0xffffff800b9e3db8

(lldb) p *cpu_data_ptr[1]
(cpu_data_t) $5 = {
  cpu_this = 0xffffff800b80da00
  cpu_active_thread = 0xffffff801b7a4030
  cpu_nthread = 0x0000000000000000
  cpu_preemption_level = 2
  cpu_number = 1
  cpu_int_state = 0x0000000000000000
  cpu_active_stack = 0xffffff8012018000
  cpu_kernel_stack = 0xffffff801201bfb0
  cpu_int_stack_top = 0xffffff809d4ce000
  cpu_interrupt_level = 0
  cpu_signals = 0
  cpu_prior_signals = 8
  cpu_pending_ast = 0
  cpu_running = 1
  ...
  cpu_task_map = TASK_MAP_64BIT
  cpu_task_cr3 = 2564120576
  cpu_kernel_cr3 = 235036672
  cpu_ucr3 = 2564126986
  cpu_pagezero_mapped = 0
  cpu_uber = (cu_isf = 18446743524356580864, cu_tmp = 33554438, cu_user_gs_base = 140735814677600)
  cd_estack = 0xfffffd000006c630
  cpu_xstate = 2
  cd_shadow = 0xffffff800ad09100
  cpu_processor = 0xffffff807d485000
```

Looking at showcurrentstacks command, it seems that both processors have preemption disabled bit on 

```
Processor 0xffffff800b9e3db8 cpu_id  0x0 AST:        State RUNNING      Preemption Disabled

task                 vm_map               ipc_space            #acts flags    pid       process             io_policy  wq_state  command             
0xffffff801dbb1808   0xffffff801dbb9648   0xffffff801d88dde0       9 D        727   0xffffff801d896290             TQ  2  2  0    myproc1    
	thread                   thread_id  processor            base   pri    sched_mode      io_policy       state    ast          waitq                            wait_event           wmesg                thread_name         
	0xffffff8016ed5440       0x30f4     0xffffff800b9e3db8   20     20     timeshare       T               R                                                                                                                    
	Backtrace:
	kernel_stack = 0xffffff809da70000
	stacktop = 0xffffff809da73f40
	0xffffff809da73f40 0xffffff800b4f4630 calcru((proc *) p = 0x0000000000000000, (timeval *) up = 0x0000000000000000, (timeval *) sp = 0x0000000000000000, (timeval *) ip = 0x0000000000000000) 
	0xffffff809da73f40 0xffffff800b4f482b getrusage((proc *) p = 0xffffff801d896290, (getrusage_args *) uap = 0xffffff801a7fa208, (int32_t *) retval = <>, ) 
	0xffffff809da73fa0 0xffffff800b5dc213 unix_syscall64((x86_saved_state_t *) state = <>, ) 
	0x0000000000000000 0xffffff800af91446 kernel`hndl_unix_scall64 + 0x16 

	stackbottom = 0xffffff809da73fa0



Processor 0xffffff807d485000 cpu_id  0x1 AST:        State RUNNING      Preemption Disabled

task                 vm_map               ipc_space            #acts flags    pid       process             io_policy  wq_state  command             
0xffffff801dc4ec98   0xffffff801dbb8c98   0xffffff801d88dc60       4 D        736   0xffffff801d892470             TQ  2  2  0    myproc2      
	thread                   thread_id  processor            base   pri    sched_mode      io_policy       state    ast          waitq                            wait_event           wmesg                thread_name         
	0xffffff801b7a4030       0x30b6     0xffffff807d485000   20     20     timeshare       T               WRU                   0xffffff807cb44c40               0x1000004001ddd89                                             
	Backtrace:
	kernel_stack = 0xffffff8012018000
	stacktop = 0xffffff801201baa0
	0xffffff801201baa0 0xffffff800afe2657 current_cpu_datap [inlined](void) 
	0xffffff801201baa0 0xffffff800afe2657 current_processor [inlined](void) 
	0xffffff801201baa0 0xffffff800afe2657 DebuggerTrapWithState [inlined]((debugger_op) db_op = DBOP_PANIC, (const char *) db_message = <>, , (const char *) db_panic_str = <>, , (va_list *) db_panic_args = <>, , (uint64_t) db_panic_options = <>, , (void *) db_panic_data_ptr = <>, , (boolean_t) db_proceed_on_sync_failure = 1, (unsigned long) db_panic_caller = <>, ) 
	0xffffff801201baa0 0xffffff800afe2635 panic_trap_to_debugger((const char *) panic_format_str = <>, , (va_list *) panic_args = <>, , (unsigned int) reason = 0, (void *) ctx = <>, , (uint64_t) panic_options_mask = <>, , (void *) panic_data_ptr = <>, , (unsigned long) panic_caller = 18446743524138282758) 
	0xffffff801201bb10 0xffffff800afe24a3 panic((const char *) str = <>, ) 
	0xffffff801201bb90 0xffffff800affef06 thread_invoke((thread_t) self = 0xffffff801b7a4030, (thread_t) thread = 0xffffff801afe4540, (ast_t) reason = 0) 
	0xffffff801201bbe0 0xffffff800affdaff thread_block_reason((thread_continue_t) continuation = <>, , (void *) parameter = <>, , (ast_t) reason = <>, ) 
	0xffffff801201bc60 0xffffff800b4fcfe1 thread_block [inlined]((thread_continue_t) continuation = <>, ) 
	0xffffff801201bc60 0xffffff800b4fcfd6 _sleep((caddr_t) chan = <>, , (int) pri = 0, (const char *) wmsg = <>, , (u_int64_t) abstime = 1299691844730, (int (*)(int)) continuation = 0x0000000000000000, (lck_mtx_t *) mtx = 0x0000000000000000) 
	0xffffff801201bca0 0xffffff800b4fd352 msleep((void *) chan = 0x01000004001ddd89, (lck_mtx_t *) mtx = 0x0000000000000000, (int) pri = 0, (const char *) wmsg = 0xffffff7f8d6f2e07 "", (timespec *) ts = <>, ) 
	0xffffff801201bd00 0xffffff7f8d6e8d1e com.my.driver + 0x5d1e 
	0xffffff801201be30 0xffffff7f8d6eb144 com.my.driver + 0x8144 
```

<------------------------>

On Sat, Jun 16, 2018 at 7:12 PM, Slava Imameev <XXXXXXXX@gmail.com> wrote:
Hi,

The GS register doesn't contain a virtual address for a segment. The GS register contains an offset into a GDT that contains segment descriptors. 

Actually, you don't need to access per-CPU data this way. Per-CPU data can be found by dereferencing elements from an array of pointers

```
cpu_data_t *cpu_data_ptr[MAX_CPUS]

For example, per-cpu data for a core which has been assigned an index 0 


(lldb) p *cpu_data_ptr[0]
(cpu_data_t) $2 = {
  cpu_this = 0xffffff800ec0c900
  cpu_active_thread = 0xffffff8019e017b0
  cpu_nthread = 0xffffff8019e017b0
  cpu_preemption_level = 4
  cpu_number = 0
  cpu_int_state = 0xffffff801604be40
  cpu_active_stack = 0xffffff8016048000
  cpu_kernel_stack = 0xffffff801604bfb0
  cpu_int_stack_top = 0xffffff8081a2a000
  cpu_interrupt_level = 1
  cpu_signals = 0
  cpu_prior_signals = 0

....
```

The above assumes that you have loaded a kernel dump or attached to a live system with valid kernel symbols.
If you are not sure on which core a kext is running use showcurrentstacks command.

Regards,
Slava Imameev

<------------------------>

On Thu, Jun 14, 2018 at 6:14 PM, Zohar Cabeli <XXXXXXX@gmail.com> wrote:
Hi, 

I was wondering if you could give me some direction in lldb issue I'm currently struggling with ... 

I've got a kext that produce panic for "preemption level -1" in macOS Mojave (10.14), and I'd like to further investigate the issue. 
It looks like preemption_level is read from the per-cpu data which that be parsed using cpu_data_t struct (defined in xnu cpu_data.h). 

However, it seems like the memory is inaccessible using memory read of %gs+0x18: 

register read gs 
gs = 0x000000009da40000



memory read 0x000000009da40018

error: kdp read memory failed (error 4)


perhaps there is another way of reading the cpu data ? 


I also tried another approach by looking how could the preemption level be negative. I guess that when using spin lock, it blocks the preemption of the cpu... however, I've tried to look for such locks in all threads (using `showallstacks` lldb command) but couldn't find any. 

Perhaps do you know better way to checking in lldb if spin lock is currently active on any cpu ? 

Thanks,
Zohar

 _______________________________________________
Do not post admin requests to the list. They will be ignored.
Darwin-kernel mailing list      (Darwin-kernel@lists.apple.com)
Help/Unsubscribe/Update your Subscription:
https://lists.apple.com/mailman/options/darwin-kernel/XXXXXX%40gmail.com

This email sent to XXXXXXX@gmail.com

<------------------------>
