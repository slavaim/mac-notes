
```
  * frame #0: 0xffffff800d4d115a kernel.development`machine_switch_context(old=0xffffff8020645d48, continuation=0x0000000000000000, new=0xffffff801ca17e30) at pcb.c:456 [opt]
    frame #1: 0xffffff800d39aa98 kernel.development`thread_invoke(self=0xffffff8020645d48, thread=0xffffff801ca17e30, reason=0) at sched_prim.c:2505 [opt]
    frame #2: 0xffffff800d39965c kernel.development`thread_block_reason(continuation=<unavailable>, parameter=0x0000000000000001, reason=<unavailable>) at sched_prim.c:3042 [opt]
    frame #3: 0xffffff800d359ed7 kernel.development`ipc_mqueue_receive [inlined] thread_block(continuation=<unavailable>) at sched_prim.c:3058 [opt]
    frame #4: 0xffffff800d359ecc kernel.development`ipc_mqueue_receive(mqueue=0xffffff801efe6b50, option=<unavailable>, max_size=<unavailable>, rcv_timeout=<unavailable>, interruptible=<unavailable>) at ipc_mqueue.c:935 [opt]
    frame #5: 0xffffff800d382912 kernel.development`mach_msg_rpc_from_kernel_body(msg=0xffffff801efec800, send_size=<unavailable>, rcv_size=64, legacy=<unavailable>) at ipc_mig.c:427 [opt]
    frame #6: 0xffffff7f8fad777f autofs`autofs_lookup + 478
    frame #7: 0xffffff7f8fadb5da autofs`auto_lookup_request + 152
    frame #8: 0xffffff7f8fadb701 autofs`auto_lookup_aux + 198
    frame #9: 0xffffff7f8fad8065 autofs`auto_lookup + 625
```
