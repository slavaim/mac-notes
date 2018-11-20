
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

Scheduling a client ```stop``` and ```detach```.

```
* thread #11, name = '0xffffff80339d0a90', queue = '0x0', stop reason = breakpoint 10.1
  * frame #0: 0xffffff80264115b6 kernel.development`IOService::scheduleTerminatePhase2(this=0xffffff8033fbd000, options=4) at IOService.cpp:2257 [opt]
    frame #1: 0xffffff80264152b7 kernel.development`IOService::terminatePhase1(this=<unavailable>, options=4) at IOService.cpp:2212 [opt]
    frame #2: 0xffffff7fa7fdd83a IOAcceleratorFamily2`IOAccelCommandQueue::clientClose() + 24
    frame #3: 0xffffff8026469164 kernel.development`::iokit_client_died(obj=0xffffff8033fbd000, (null)=<unavailable>, type=<unavailable>, mscount=<unavailable>) at IOUserClient.cpp:586 [opt]
    frame #4: 0xffffff8025e8427c kernel.development`iokit_notify at iokit_rpc.c:359 [opt]
    frame #5: 0xffffff8025e8422e kernel.development`iokit_notify(msg=0xffffff80421d6790) at iokit_rpc.c:385 [opt]
    frame #6: 0xffffff8025d81d6f kernel.development`ipc_kobject_server(request=0xffffff80421d6700, option=327681) at ipc_kobject.c:364 [opt]
    frame #7: 0xffffff8025d54d0d kernel.development`ipc_kmsg_send(kmsg=0xffffff80421d6700, option=327681, send_timeout=0) at ipc_kmsg.c:1867 [opt]
    frame #8: 0xffffff8025d82500 kernel.development`mach_msg_send_from_kernel_proper(msg=<unavailable>, send_size=44) at ipc_mig.c:186 [opt]
    frame #9: 0xffffff8025d62842 kernel.development`ipc_right_terminate [inlined] mach_notify_no_senders at mach_notify_user.c:370 [opt]
    frame #10: 0xffffff8025d62801 kernel.development`ipc_right_terminate [inlined] ipc_notify_no_senders at ipc_notify.c:144 [opt]
    frame #11: 0xffffff8025d62801 kernel.development`ipc_right_terminate(space=0xffffff8042fb4720, name=36099, entry=<unavailable>) at ipc_right.c:677 [opt]
    frame #12: 0xffffff8025d688dc kernel.development`ipc_space_terminate(space=0xffffff80262759c9) at ipc_space.c:426 [opt]
    frame #13: 0xffffff8025dab097 kernel.development`task_terminate_internal(task=0xffffff8041d10370) at task.c:2294 [opt]
    frame #14: 0xffffff8026275c6f kernel.development`exit_with_reason(p=0xffffff803acffb60, rv=0, retval=<unavailable>, thread_can_terminate=1, perf_notify=1, jetsam_flags=<unavailable>, exit_reason=<unavailable>) at kern_exit.c:833 [opt]
    frame #15: 0xffffff80262759f3 kernel.development`exit1 [inlined] exit1_internal(p=<unavailable>, rv=<unavailable>, retval=<unavailable>, thread_can_terminate=1, perf_notify=1, jetsam_flags=0) at kern_exit.c:707 [opt]
    frame #16: 0xffffff80262759d4 kernel.development`exit1(p=<unavailable>, rv=<unavailable>, retval=<unavailable>) at kern_exit.c:700 [opt]
    frame #17: 0xffffff80262759c9 kernel.development`exit(p=<unavailable>, uap=<unavailable>, retval=<unavailable>) at kern_exit.c:683 [opt]
    frame #18: 0xffffff80263a60ca kernel.development`unix_syscall64(state=<unavailable>) at systemcalls.c:382 [opt]
    frame #19: 0xffffff8025d20a36 kernel.development`hndl_unix_scall64 + 22
```

```
* thread #12, name = '0xffffff803251b168', queue = '0x0', stop reason = breakpoint 10.1
  * frame #0: 0xffffff80264115b6 kernel.development`IOService::scheduleTerminatePhase2(this=0xffffff803ccfc900, options=4) at IOService.cpp:2257 [opt]
    frame #1: 0xffffff80264152b7 kernel.development`IOService::terminatePhase1(this=<unavailable>, options=4) at IOService.cpp:2212 [opt]
    frame #2: 0xffffff7fa813a846 corecapture`CCPipe::removeCapture() + 44
    frame #3: 0xffffff7fa813d2c7 corecapture`CCPipeUserClient::freeResources() + 41
    frame #4: 0xffffff7fa814066f corecapture`CCDataPipeUserClient::clientDied() + 13
    frame #5: 0xffffff8026469164 kernel.development`::iokit_client_died(obj=0xffffff80383fdd80, (null)=<unavailable>, type=<unavailable>, mscount=<unavailable>) at IOUserClient.cpp:586 [opt]
    frame #6: 0xffffff8025e8427c kernel.development`iokit_notify at iokit_rpc.c:359 [opt]
    frame #7: 0xffffff8025e8422e kernel.development`iokit_notify(msg=0xffffff803b974990) at iokit_rpc.c:385 [opt]
    frame #8: 0xffffff8025d81d6f kernel.development`ipc_kobject_server(request=0xffffff803b974900, option=327681) at ipc_kobject.c:364 [opt]
    frame #9: 0xffffff8025d54d0d kernel.development`ipc_kmsg_send(kmsg=0xffffff803b974900, option=327681, send_timeout=0) at ipc_kmsg.c:1867 [opt]
    frame #10: 0xffffff8025d82500 kernel.development`mach_msg_send_from_kernel_proper(msg=<unavailable>, send_size=44) at ipc_mig.c:186 [opt]
    frame #11: 0xffffff8025d63a13 kernel.development`ipc_right_dealloc [inlined] mach_notify_no_senders at mach_notify_user.c:370 [opt]
    frame #12: 0xffffff8025d639d3 kernel.development`ipc_right_dealloc [inlined] ipc_notify_no_senders at ipc_notify.c:144 [opt]
    frame #13: 0xffffff8025d639d3 kernel.development`ipc_right_dealloc(space=<unavailable>, name=8451, entry=<unavailable>) at ipc_right.c:994 [opt]
    frame #14: 0xffffff8025d70723 kernel.development`mach_port_deallocate(space=0xffffff8042fb4ea0, name=8451) at mach_port.c:811 [opt]
    frame #15: 0xffffff8025d6e434 kernel.development`_kernelrpc_mach_port_deallocate_trap(args=0xffffff90c8ddbf08) at mach_kernelrpc.c:198 [opt]
    frame #16: 0xffffff8025ebe0ea kernel.development`mach_call_munger64(state=0xffffff8035aed220) at bsd_i386.c:573 [opt]
    frame #17: 0xffffff8025d20a56 kernel.development`hndl_mach_scall64 + 22
```

Object's ```finalize``` is called from a termination thread.

```
  * frame #0: 0xffffff8026414807 kernel.development`IOService::scheduleStop(IOService*) [inlined] IORegistryEntry::getRegistryEntryID() at IORegistryEntry.cpp:1755 [opt]
    frame #1: 0xffffff8026414807 kernel.development`IOService::scheduleStop(this=0xffffff803c4832c0, provider=0xffffff8036422800) at IOService.cpp:2342 [opt]
    frame #2: 0xffffff802640e8cc kernel.development`IOService::finalize(this=0xffffff803c4832c0, options=<unavailable>) at IOService.cpp:2853 [opt]
    frame #3: 0xffffff802643bdce kernel.development`IOWorkLoop::runAction(this=0xffffff80314b8870, inAction=(kernel.development`IOService::actionFinalize(IOService*, unsigned int, void*, void*, void*) at IOService.cpp:2554), target=<unavailable>, arg0=<unavailable>, arg1=<unavailable>, arg2=<unavailable>, arg3=0x0000000000000000)(OSObject*, void*, void*, void*, void*), OSObject*, void*, void*, void*, void*) at IOWorkLoop.cpp:505 [opt]
    frame #4: 0xffffff80264122d0 kernel.development`IOService::terminateWorker(unsigned int) [inlined] _workLoopAction(p3=<unavailable>)(OSObject*, void*, void*, void*, void*), IOService*, void*, void*, void*, void*) at IOService.cpp:2017 [opt]
    frame #5: 0xffffff802641228b kernel.development`IOService::terminateWorker(options=1) at IOService.cpp:2730 [opt]
    frame #6: 0xffffff802641c2f7 kernel.development`IOService::terminateThread(arg=0x0000000000000000, waitResult=<unavailable>) at IOService.cpp:2332 [opt]
    frame #7: 0xffffff8025d1f5c7 kernel.development`call_continuation + 23
```

Object's ```stop``` is also scheduled to a termination thread.

```
  * frame #0: 0xffffff802640f584 kernel.development`IOService::detach(this=0xffffff803ad4fe30, provider=0xffffff80368c0f40) at IOService.cpp:695 [opt]
    frame #1: 0xffffff802643bdce kernel.development`IOWorkLoop::runAction(this=0xffffff80307679b0, inAction=(kernel.development`IOService::actionStop(IOService*, IOService*, void*, void*, void*) at IOService.cpp:2569), target=<unavailable>, arg0=<unavailable>, arg1=<unavailable>, arg2=<unavailable>, arg3=0x0000000000000000)(OSObject*, void*, void*, void*, void*), OSObject*, void*, void*, void*, void*) at IOWorkLoop.cpp:505 [opt]
    frame #2: 0xffffff8026412635 kernel.development`IOService::terminateWorker(unsigned int) [inlined] _workLoopAction(p3=<unavailable>)(OSObject*, void*, void*, void*, void*), IOService*, void*, void*, void*, void*) at IOService.cpp:2017 [opt]
    frame #3: 0xffffff80264125e7 kernel.development`IOService::terminateWorker(options=1) at IOService.cpp:2781 [opt]
    frame #4: 0xffffff802641c2f7 kernel.development`IOService::terminateThread(arg=0x0000000000000000, waitResult=<unavailable>) at IOService.cpp:2332 [opt]
    frame #5: 0xffffff8025d1f5c7 kernel.development`call_continuation + 23
```

An object is detached from ```stop``` in a termination thread. Some classes decide to detach in ```clientDied```.

```
  * frame #0: 0xffffff802640f584 kernel.development`IOService::detach(this=0xffffff80382a5000, provider=0xffffff8036422800) at IOService.cpp:695 [opt]
    frame #1: 0xffffff7fa7fdd746 IOAcceleratorFamily2`IOAccelCommandQueue::stopLocked() + 124
    frame #2: 0xffffff7fa7fdd7a2 IOAcceleratorFamily2`IOAccelCommandQueue::stop(IOService*) + 76
    frame #3: 0xffffff8026413442 kernel.development`IOService::actionStop(provider=0xffffff8036422800, client=0xffffff80382a5000, unused1=<unavailable>, unused2=<unavailable>, unused3=<unavailable>) at IOService.cpp:2581 [opt]
    frame #4: 0xffffff802643bdce kernel.development`IOWorkLoop::runAction(this=0xffffff80314b8870, inAction=(kernel.development`IOService::actionStop(IOService*, IOService*, void*, void*, void*) at IOService.cpp:2569), target=<unavailable>, arg0=<unavailable>, arg1=<unavailable>, arg2=<unavailable>, arg3=0x0000000000000000)(OSObject*, void*, void*, void*, void*), OSObject*, void*, void*, void*, void*) at IOWorkLoop.cpp:505 [opt]
    frame #5: 0xffffff8026412635 kernel.development`IOService::terminateWorker(unsigned int) [inlined] _workLoopAction(p3=<unavailable>)(OSObject*, void*, void*, void*, void*), IOService*, void*, void*, void*, void*) at IOService.cpp:2017 [opt]
    frame #6: 0xffffff80264125e7 kernel.development`IOService::terminateWorker(options=1) at IOService.cpp:2781 [opt]
    frame #7: 0xffffff802641c2f7 kernel.development`IOService::terminateThread(arg=0x0000000000000000, waitResult=<unavailable>) at IOService.cpp:2332 [opt]
    frame #8: 0xffffff8025d1f5c7 kernel.development`call_continuation + 23
```
