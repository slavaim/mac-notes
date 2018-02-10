
```
    frame #1: 0xffffff800ae882c1 kernel`vclean [inlined] VNOP_RECLAIM(vp=0xffffff801a777268, ctx=<unavailable>) at kpi_vfs.c:5087 [opt]
    frame #2: 0xffffff800ae88299 kernel`vclean(vp=0xffffff801a777268, flags=<unavailable>) at vfs_subr.c:2287 [opt]
    frame #3: 0xffffff800ae87b26 kernel`vnode_reclaim_internal [inlined] vgone(vp=0xffffff801a777268, flags=8) at vfs_subr.c:2442 [opt]
    frame #4: 0xffffff800ae87b17 kernel`vnode_reclaim_internal(vp=0xffffff801a777268, locked=1, reuse=<unavailable>, flags=<unavailable>) at vfs_subr.c:4820 [opt]
    frame #5: 0xffffff800ae87989 kernel`vnode_put_locked(vp=0xffffff801a777268) at vfs_subr.c:4536 [opt]
    frame #6: 0xffffff800aeaf56e kernel`vn_closefile [inlined] vnode_put(vp=<unavailable>) at vfs_subr.c:4480 [opt]
    frame #7: 0xffffff800aeaf55e kernel`vn_closefile(fg=0xffffff8021b86b60, ctx=0xffffff8012873e60) at vfs_vnops.c:1602 [opt]
    frame #8: 0xffffff800b0ee5b3 kernel`closef_locked [inlined] fo_close(fg=0xffffff8021b86b60, ctx=0xffffff801ae973c0) at kern_descrip.c:5994 [opt]
    frame #9: 0xffffff800b0ee5a4 kernel`closef_locked(fp=0xffffff801e50cdb0, fg=0xffffff8021b86b60, p=0xffffff8026039000) at kern_descrip.c:5225 [opt]
    frame #10: 0xffffff800b0ee244 kernel`close_internal_locked(p=0xffffff8026039000, fd=<unavailable>, fp=0xffffff801e50cdb0, flags=0) at kern_descrip.c:2887 [opt]
    frame #11: 0xffffff800b0f2b44 kernel`close_nocancel(p=0xffffff8026039000, uap=<unavailable>, retval=<unavailable>) at kern_descrip.c:2784 [opt]
    frame #12: 0xffffff800b1fb3e8 kernel`unix_syscall64(state=<unavailable>) at systemcalls.c:382 [opt]
    frame #13: 0xffffff800ac02906 kernel`hndl_unix_scall64 + 22
```
