```
    frame #1: 0xffffff80292c02c1 kernel`VNOP_SETXATTR(vp=0xffffff80399f4930, name=<unavailable>, uio=<unavailable>, options=<unavailable>, ctx=<unavailable>) at kpi_vfs.c:5510 [opt]
    frame #2: 0xffffff80292b40a1 kernel`vn_setxattr(vp=0xffffff80399f4930, name=<unavailable>, uio=0xffffff90c73e3620, options=8, context=0xffffff80376dceb0) at vfs_xattr.c:216 [opt]
    frame #3: 0xffffff802970709d kernel`mac_vnop_setxattr(vp=0xffffff80399f4930, name=<unavailable>, buf="0086;5a806eff;TextEdit;", len=23) at mac_vfs_subr.c:173 [opt]
    frame #4: 0xffffff7fac157eeb Quarantine`quarantine_set_ea + 119
    frame #5: 0xffffff7fac156921 Quarantine`hook_vnode_notify_create + 637
    frame #6: 0xffffff80296fda84 kernel`mac_vnode_notify_create(ctx=<unavailable>, mp=0xffffff803578d6e0, dvp=0xffffff803e0883e0, vp=0xffffff80399f4930, cnp=0xffffff90c73e3d28) at mac_vfs.c:395 [opt]
    frame #7: 0xffffff8029706e6d kernel`vnode_label(mp=0xffffff803578d6e0, dvp=0xffffff803e0883e0, vp=0xffffff80399f4930, cnp=<unavailable>, flags=1, ctx=<unavailable>) at mac_vfs_subr.c:56 [opt]
    frame #8: 0xffffff802928e37f kernel`vn_create(dvp=0xffffff803e0883e0, vpp=0xffffff90c73e3c00, ndp=<unavailable>, vap=0xffffff90c73e3d70, flags=16, fmode=1538, statusp=<unavailable>, ctx=<unavailable>) at vfs_subr.c:5808 [opt]
    frame #9: 0xffffff80292b02f5 kernel`vn_open_auth [inlined] vn_open_auth_do_create(ndp=0xffffff90c73e3bd8, vap=0xffffff90c73e3d70, ctx=0xffffff80376dceb0) at vfs_vnops.c:242 [opt]
    frame #10: 0xffffff80292b028e kernel`vn_open_auth(ndp=0xffffff90c73e3bd8, fmodep=0xffffff90c73e39d4, vap=0xffffff90c73e3d70) at vfs_vnops.c:440 [opt]
    frame #11: 0xffffff802929a737 kernel`open1(ctx=0xffffff80376dceb0, ndp=0xffffff90c73e3bd8, uflags=<unavailable>, vap=0xffffff90c73e3d70, fp_zalloc=<unavailable>, cra=<unavailable>, retval=0xffffff80376dcd98) at vfs_syscalls.c:3487 [opt]
    frame #12: 0xffffff802929b270 kernel`open [inlined] open1at(fp_zalloc=<unavailable>, cra=<unavailable>) at vfs_syscalls.c:3731 [opt]
    frame #13: 0xffffff802929b250 kernel`open [inlined] openat_internal(ctx=<unavailable>, path=<unavailable>, flags=<unavailable>, mode=<unavailable>, fd=-2, segflg=UIO_USERSPACE, retval=0xffffff80376dcd98) at vfs_syscalls.c:3876 [opt]
    frame #14: 0xffffff802929b137 kernel`open [inlined] open_nocancel(uap=<unavailable>) at vfs_syscalls.c:3891 [opt]
    frame #15: 0xffffff802929b125 kernel`open(p=<unavailable>, uap=<unavailable>, retval=0xffffff80376dcd98) at vfs_syscalls.c:3884 [opt]
    frame #16: 0xffffff80295fb3e8 kernel`unix_syscall64(state=<unavailable>) at systemcalls.c:382 [opt]
    frame #17: 0xffffff8029002906 kernel`hndl_unix_scall64 + 22
```
