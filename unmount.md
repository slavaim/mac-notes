
A stack with a forced unmount on reboot

```
    frame #1: 0xffffff8008bf73a0 kernel`vclean [inlined] VNOP_FSYNC(vp=<unavailable>, waitfor=1, ctx=<unavailable>) at kpi_vfs.c:3798 [opt]
    frame #2: 0xffffff8008bf737c kernel`vclean(vp=0xffffff801c2e87c0, flags=<unavailable>) at vfs_subr.c:2234 [opt]
    frame #3: 0xffffff8008bf6c46 kernel`vnode_reclaim_internal [inlined] vgone(vp=<unavailable>, flags=<unavailable>) at vfs_subr.c:2452 [opt]
    frame #4: 0xffffff8008bf6c37 kernel`vnode_reclaim_internal(vp=0xffffff801c2e87c0, locked=1, reuse=<unavailable>, flags=<unavailable>) at vfs_subr.c:4828 [opt]
    frame #5: 0xffffff8008bfb470 kernel`vflush(mp=0xffffff8014fb22a0, skipvp=0x0000000000000000, flags=27) at vfs_subr.c:2098 [opt]
    frame #6: 0xffffff8008c06bc5 kernel`dounmount(mp=0xffffff8014fb22a0, flags=524288, withref=<unavailable>, ctx=0xffffff8016ffb370) at vfs_syscalls.c:2073 [opt]
    frame #7: 0xffffff8008bfc943 kernel`unmount_callback(mp=0xffffff8014fb22a0, arg=0xffffff90c8993d00) at vfs_subr.c:3051 [opt]
    frame #8: 0xffffff8008bf61f9 kernel`vfs_iterate(flags=<unavailable>, callout=(kernel`unmount_callback at vfs_subr.c:3039), arg=0xffffff90c8993d00) at vfs_subr.c:5324 [opt]
    frame #9: 0xffffff8008bfc75b kernel`vfs_unmountall at vfs_subr.c:3077 [opt]
    frame #10: 0xffffff8008ecdfb6 kernel`reboot_kernel(howto=<unavailable>, message="") at kern_shutdown.c:227 [opt]
    frame #11: 0xffffff8008ee6b5c kernel`reboot(p=0xffffff80154f3630, uap=0xffffff8016ffb208, retval=<unavailable>) at kern_xxx.c:142 [opt]
    frame #12: 0xffffff8008fb619b kernel`unix_syscall64(state=<unavailable>) at systemcalls.c:381 [opt]
    frame #13: 0xffffff800895c466 kernel`hndl_unix_scall64 + 22
```
