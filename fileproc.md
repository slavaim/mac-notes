```
(lldb) f 6
frame #6: 0xffffff800d34306f kernel`dofileread(ctx=0xffffff8014a43f00, fp=0xffffff80204e55e8, bufp=140380549015696, nbyte=418, offset=<unavailable>, flags=0, retval=<unavailable>) at sys_generic.c:373 [opt]
(lldb) p *fp
(fileproc) $36 = {
  f_flags = 0
  f_iocount = 1
  f_fglob = 0xffffff802a24bea0
  f_wset = 0x0000000000000000
}
(lldb) p *fp->f_fglob
(fileglob) $37 = {
  f_msglist = {
    le_next = 0x0000000000000000
    le_prev = 0x0000000000000000
  }
  fg_flag = 1
  fg_count = 1
  fg_msgcount = 0
  fg_lflags = 128
  fg_cred = 0xffffff801e138830
  fg_ops = 0xffffff800d864ff8
  fg_offset = 0
  fg_data = 0xffffff801a4022e8
  fg_vn_data = 0x0000000000000000
  fg_lock = {
    opaque = ([0] = 0, [1] = 18446744069414584320)
  }
  fg_label = 0xffffff802334f000
}
(lldb) p *fp->f_fglob->fg_ops
(const fileops) $38 = {
  fo_type = DTYPE_VNODE
  fo_read = 0xffffff800d0ae6e0 (kernel`vn_read at vfs_vnops.c:999)
  fo_write = 0xffffff800d0aea60 (kernel`vn_write at vfs_vnops.c:1073)
  fo_ioctl = 0xffffff800d0af050 (kernel`vn_ioctl at vfs_vnops.c:1459)
  fo_select = 0xffffff800d0af350 (kernel`vn_select at vfs_vnops.c:1547)
  fo_close = 0xffffff800d0af400 (kernel`vn_closefile at vfs_vnops.c:1578)
  fo_kqfilter = 0xffffff800d0af5a0 (kernel`vn_kqfilt_add at vfs_vnops.c:1692)
  fo_drain = 0x0000000000000000
}
(lldb) p fp->f_fglob->fg_data
(void *) $39 = 0xffffff801a4022e8
(lldb) showvnodepath 0xffffff801a4022e8
/System/Library/PrivateFrameworks/SPSupport.framework/Versions/A/Resources/SPProperties.plist
```
