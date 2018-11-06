```
  * frame #0: 0xffffff800d3acdd8 kernel.development`task_wait [inlined] task_wait_locked(task=0xffffff801bce87a0, until_not_runnable=<unavailable>) at task.c:2650 [opt]
    frame #1: 0xffffff800d3acdd8 kernel.development`task_wait(task=0xffffff801bce87a0, until_not_runnable=0) at task.c:2628 [opt]
    frame #2: 0xffffff800d8902a7 kernel.development`postsig_locked [inlined] sig_lock_to_exit at kern_sig.c:3583 [opt]
    frame #3: 0xffffff800d890278 kernel.development`postsig_locked(signum=2) at kern_sig.c:3095 [opt]
    frame #4: 0xffffff800d8909d7 kernel.development`bsd_ast(thread=<unavailable>) at kern_sig.c:3420 [opt]
    frame #5: 0xffffff800d372ea4 kernel.development`ast_taken_user at ast.c:207 [opt]
    frame #6: 0xffffff800d32021c kernel.development`return_from_trap + 172
```

Delivering a caught signal to a debugger from an AST. The ```issignal_locked``` routine calls ```do_bsdexception``` that in turn calls ```exception_deliver```.

```
  * frame #0: 0xffffff80280d115a kernel.development`machine_switch_context(old=0xffffff803f833168, continuation=0x0000000000000000, new=0xffffff80352c2250) at pcb.c:456 [opt]
    frame #1: 0xffffff8027f9aa98 kernel.development`thread_invoke(self=0xffffff803f833168, thread=0xffffff80352c2250, reason=0) at sched_prim.c:2505 [opt]
    frame #2: 0xffffff8027f9965c kernel.development`thread_block_reason(continuation=<unavailable>, parameter=0x0000000000000001, reason=<unavailable>) at sched_prim.c:3042 [opt]
    frame #3: 0xffffff8027f59ed7 kernel.development`ipc_mqueue_receive [inlined] thread_block(continuation=<unavailable>) at sched_prim.c:3058 [opt]
    frame #4: 0xffffff8027f59ecc kernel.development`ipc_mqueue_receive(mqueue=0xffffff803f4c92c0, option=<unavailable>, max_size=<unavailable>, rcv_timeout=<unavailable>, interruptible=<unavailable>) at ipc_mqueue.c:935 [opt]
    frame #5: 0xffffff8027f82912 kernel.development`mach_msg_rpc_from_kernel_body(msg=0xffffff802f873350, send_size=<unavailable>, rcv_size=52, legacy=<unavailable>) at ipc_mig.c:427 [opt]
    frame #6: 0xffffff8027fdf403 kernel.development`mach_exception_raise [inlined] mach_msg_rpc_from_kernel_proper(msg=0xffffff8080001513, send_size=<unavailable>, rcv_size=52) at ipc_mig.c:338 [opt]
    frame #7: 0xffffff8027fdf3f9 kernel.development`mach_exception_raise(exception_port=0xffffff803e3fb428, thread=<unavailable>, task=<unavailable>, exception=<unavailable>, code=0xffffff802f873e60, codeCnt=2) at mach_exc_user.c:280 [opt]
    frame #8: 0xffffff8027f7de4e kernel.development`exception_deliver(thread=0xffffff803f833168, exception=<unavailable>, code=0xffffff802f873e60, codeCnt=2, excp=<unavailable>, mutex=<unavailable>) at exception.c:280 [opt]
    frame #9: 0xffffff802848fa6d kernel.development`issignal_locked [inlined] bsd_exception(exception=5, codeCnt=2) at exception.c:524 [opt]
    frame #10: 0xffffff802848fa33 kernel.development`issignal_locked [inlined] do_bsdexception(exc=5, code=65539) at kern_sig.c:3467 [opt]
    frame #11: 0xffffff802848fa27 kernel.development`issignal_locked(p=0xffffff803ab1f920) at kern_sig.c:2734 [opt]
    frame #12: 0xffffff80284909df kernel.development`bsd_ast(thread=<unavailable>) at kern_sig.c:3419 [opt]
    frame #13: 0xffffff8027f72ea4 kernel.development`ast_taken_user at ast.c:207 [opt]
    frame #14: 0xffffff8027f2021c kernel.development`return_from_trap + 172
```

The related code 
```
int
issignal_locked(proc_t p)
{
...
		if (p->p_lflag & P_LTRACED && (p->p_lflag & P_LPPWAIT) == 0)  {
			/*
			 * If traced, deliver the signal to the debugger, and wait to be
			 * released.
			 */
			task_t	task;
			p->p_xstat = signum;

			if (p->p_lflag & P_LSIGEXC) {
				p->sigwait = TRUE;
				p->sigwait_thread = cur_act;
				p->p_stat = SSTOP;
				OSBitAndAtomic(~((uint32_t)P_CONTINUED), &p->p_flag);
				p->p_lflag &= ~P_LWAITED;
				ut->uu_siglist &= ~mask; /* clear the current signal from the pending list */
				proc_signalend(p, 1);
				proc_unlock(p);
				do_bsdexception(EXC_SOFTWARE, EXC_SOFT_SIGNAL, signum);
				proc_lock(p);
				proc_signalstart(p, 1);
			} 
...
}
```

A call to ast on return from an interrupt
```
    frame #8: 0xffffff8027ed556d kernel.development`setPop(time=<unavailable>) at rtclock.c:473 [opt]
    frame #9: 0xffffff8027ec4deb kernel.development`timer_resync_deadlines at i386_timer.c:229 [opt]
    frame #10: 0xffffff8027dc5b10 kernel.development`timer_call_quantum_timer_enter(call=0xffffff80bb205058, param1=0xffffff8045855928, deadline=1438564288467, ctime=1438561879599) at timer_call.c:735 [opt]
    frame #11: 0xffffff8027d9bd78 kernel.development`thread_dispatch(thread=<unavailable>, self=0xffffff8045855928) at sched_prim.c:2950 [opt]
    frame #12: 0xffffff8027d9aac8 kernel.development`thread_invoke(self=0xffffff8045855928, thread=0xffffff8036871b78, reason=5) at sched_prim.c:2518 [opt]
    frame #13: 0xffffff8027d9965c kernel.development`thread_block_reason(continuation=<unavailable>, parameter=0x0000000000000000, reason=<unavailable>) at sched_prim.c:3042 [opt]
    frame #14: 0xffffff8027d72d0b kernel.development`ast_taken_kernel at ast.c:130 [opt]
    frame #15: 0xffffff8027d20523 kernel.development`return_to_iret + 282
```
