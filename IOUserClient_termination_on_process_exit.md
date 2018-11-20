
```
  * frame #0: 0xffffff8026466014 kernel.development`IOUserClient::clientClose(this=0xffffff803bb23200) at IOUserClient.cpp:1552 [opt]
    frame #1: 0xffffff8026469164 kernel.development`::iokit_client_died(obj=0xffffff803bb23200, (null)=<unavailable>, type=<unavailable>, mscount=<unavailable>) at IOUserClient.cpp:586 [opt]
    frame #2: 0xffffff8025e8427c kernel.development`iokit_notify at iokit_rpc.c:359 [opt]
    frame #3: 0xffffff8025e8422e kernel.development`iokit_notify(msg=0xffffff80329f2c90) at iokit_rpc.c:385 [opt]
    frame #4: 0xffffff8025d81d6f kernel.development`ipc_kobject_server(request=0xffffff80329f2c00, option=327681) at ipc_kobject.c:364 [opt]
    frame #5: 0xffffff8025d54d0d kernel.development`ipc_kmsg_send(kmsg=0xffffff80329f2c00, option=327681, send_timeout=0) at ipc_kmsg.c:1867 [opt]
    frame #6: 0xffffff8025d82500 kernel.development`mach_msg_send_from_kernel_proper(msg=<unavailable>, send_size=44) at ipc_mig.c:186 [opt]
    frame #7: 0xffffff8025d62842 kernel.development`ipc_right_terminate [inlined] mach_notify_no_senders at mach_notify_user.c:370 [opt]
    frame #8: 0xffffff8025d62801 kernel.development`ipc_right_terminate [inlined] ipc_notify_no_senders at ipc_notify.c:144 [opt]
    frame #9: 0xffffff8025d62801 kernel.development`ipc_right_terminate(space=0xffffff8042fb56e0, name=84227, entry=<unavailable>) at ipc_right.c:677 [opt]
    frame #10: 0xffffff8025d688dc kernel.development`ipc_space_terminate(space=0xffffff80262759c9) at ipc_space.c:426 [opt]
    frame #11: 0xffffff8025dab097 kernel.development`task_terminate_internal(task=0xffffff804195bd88) at task.c:2294 [opt]
    frame #12: 0xffffff8026275c6f kernel.development`exit_with_reason(p=0xffffff803acfe490, rv=0, retval=<unavailable>, thread_can_terminate=1, perf_notify=1, jetsam_flags=<unavailable>, exit_reason=<unavailable>) at kern_exit.c:833 [opt]
    frame #13: 0xffffff80262759f3 kernel.development`exit1 [inlined] exit1_internal(p=<unavailable>, rv=<unavailable>, retval=<unavailable>, thread_can_terminate=1, perf_notify=1, jetsam_flags=0) at kern_exit.c:707 [opt]
    frame #14: 0xffffff80262759d4 kernel.development`exit1(p=<unavailable>, rv=<unavailable>, retval=<unavailable>) at kern_exit.c:700 [opt]
    frame #15: 0xffffff80262759c9 kernel.development`exit(p=<unavailable>, uap=<unavailable>, retval=<unavailable>) at kern_exit.c:683 [opt]
    frame #16: 0xffffff80263a60ca kernel.development`unix_syscall64(state=<unavailable>) at systemcalls.c:382 [opt]
    frame #17: 0xffffff8025d20a36 kernel.development`hndl_unix_scall64 + 22
```
