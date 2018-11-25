
A call stack for read() syscall.

```
  * frame #0: 0xffffff802fa20751 kernel.development`VNOP_READ(vp=0xffffff8041f4ec18, uio=0xffffff90d2823e60, ioflag=1048576, ctx=0xffffff90d2823f00) at kpi_vfs.c:3463 [opt]
    frame #1: 0xffffff802fa12b8c kernel.development`vn_read(fp=0xffffff8041a05048, uio=0xffffff90d2823e60, flags=0, ctx=0xffffff90d2823f00) at vfs_vnops.c:1052 [opt]
    frame #2: 0xffffff802fcab455 kernel.development`dofileread [inlined] fo_read(fp=0xffffff8041a05048, uio=0xffffff90d2823e98, flags=0, ctx=0xffffff90d2823f00) at kern_descrip.c:5917 [opt]
    frame #3: 0xffffff802fcab43a kernel.development`dofileread(ctx=0xffffff90d2823f00, fp=0xffffff8041a05048, bufp=140375309166432, nbyte=119, offset=<unavailable>, flags=0, retval=<unavailable>) at sys_generic.c:373 [opt]
    frame #4: 0xffffff802fcab1f3 kernel.development`read_nocancel(p=0xffffff803c0f4240, uap=0xffffff8041438000, retval=<unavailable>) at sys_generic.c:223 [opt]
    frame #5: 0xffffff802fda60ca kernel.development`unix_syscall64(state=<unavailable>) at systemcalls.c:382 [opt]
    frame #6: 0xffffff802f720a36 kernel.development`hndl_unix_scall64 + 22
```

As you can see the kernel directly calls ```VNOP_READ```. That means that a decision about file caching is left to file system driver. The same as in Windows.
