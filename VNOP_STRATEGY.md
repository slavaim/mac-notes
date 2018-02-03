
```
(lldb) bt
* thread #5, name = '0xffffff803a2608b0', queue = '0x0', stop reason = breakpoint 1.5
  * frame #0: 0xffffff8027a774ab kernel`cluster_io [inlined] buf_vnode(bp=0xffffff80bd02d140) at vfs_bio.c:679 [opt]
    frame #1: 0xffffff8027a774ab kernel`cluster_io [inlined] VNOP_STRATEGY(bp=0xffffff80bd02d140) at kpi_vfs.c:5693 [opt]
    frame #2: 0xffffff8027a774ab kernel`cluster_io(vp=0xffffff80357b1ba0, upl=0xffffff803bbc8c60, upl_offset=0x0000000000008000, f_offset=32768, non_rounded_size=0, flags=141, real_bp=<unavailable>, iostate=<unavailable>, callback=<unavailable>, callback_arg=<unavailable>) at vfs_cluster.c:1801 [opt]
    frame #3: 0xffffff8027a780c4 kernel`cluster_pagein_ext(vp=0x000000000800055c, upl=<unavailable>, upl_offset=<unavailable>, f_offset=<unavailable>, size=<unavailable>, filesize=-549755813815, flags=<unavailable>, callback=<unavailable>, callback_arg=<unavailable>) at vfs_cluster.c:2171 [opt]
    frame #4: 0xffffff8027a77f85 kernel`cluster_pagein(vp=<unavailable>, upl=<unavailable>, upl_offset=<unavailable>, f_offset=<unavailable>, size=<unavailable>, filesize=<unavailable>, flags=0) at vfs_cluster.c:2116 [opt]
    frame #5: 0xffffff7fa8e41a5d
    frame #6: 0xffffff8027da5107 kernel`vnode_pagein [inlined] VNOP_PAGEIN(size=32768, flags=0, ctx=<unavailable>) at kpi_vfs.c:5273 [opt]
    frame #7: 0xffffff8027da50c0 kernel`vnode_pagein(vp=0xffffff80357b1ba0, upl=<unavailable>, upl_offset=<unavailable>, f_offset=0, size=<unavailable>, flags=0, errorp=<unavailable>) at vnode_pager.c:593 [opt]
    frame #8: 0xffffff80278bf189 kernel`vnode_pager_cluster_read(vnode_object=0xffffff8035729e88, base_offset=0, offset=<unavailable>, io_streaming=<unavailable>, cnt=0x0000000000008000) at bsd_vm.c:850 [opt]
    frame #9: 0xffffff80278bef03 kernel`vnode_pager_data_request(mem_obj=0xffffff8035729e88, offset=0, length=<unavailable>, desired_access=<unavailable>, fault_info=<unavailable>) at bsd_vm.c:638 [opt]
    frame #10: 0xffffff80278cdebe kernel`vm_fault_page [inlined] memory_object_data_request(memory_object=<unavailable>, offset=<unavailable>, length=4096, desired_access=1, fault_info=<unavailable>) at memory_object.c:2134 [opt]
    frame #11: 0xffffff80278cdea8 kernel`vm_fault_page(first_object=0xffffff80357ad100, first_offset=0, fault_type=1, must_be_resident=0, caller_lookup=0, protection=0xffffff90c5afbe80, result_page=<unavailable>, top_page=<unavailable>, type_of_fault=<unavailable>, error_code=<unavailable>, no_zero_fill=<unavailable>, data_supply=994924032, fault_info=<unavailable>) at vm_fault.c:1770 [opt]
    frame #12: 0xffffff80278d27ea kernel`vm_fault_internal(map=<unavailable>, vaddr=4319510528, caller_prot=1, change_wiring=<unavailable>, wire_tag=0, interruptible=<unavailable>, caller_pmap=<unavailable>, caller_pmap_addr=<unavailable>, physpage_p=<unavailable>) at vm_fault.c:4610 [opt]
    frame #13: 0xffffff802796f662 kernel`user_trap [inlined] vm_fault(map=<unavailable>, fault_type=<unavailable>, change_wiring=0, wire_tag=0, interruptible=2, caller_pmap_addr=0) at vm_fault.c:3416 [opt]
    frame #14: 0xffffff802796f63f kernel`user_trap(saved_state=0xffffff803a53f3e0) at trap.c:1112 [opt]
    frame #15: 0xffffff8027802032 kernel`hndl_alltraps + 226
(lldb) f 1
frame #1: 0xffffff8027a774ab kernel`cluster_io [inlined] VNOP_STRATEGY(bp=0xffffff80bd02d140) at kpi_vfs.c:5693 [opt]
(lldb) p/x *bp
(buf) $6 = {
  b_hash = {
    le_next = 0x0000000000000000
    le_prev = 0x0000000000000000
  }
  b_vnbufs = {
    le_next = 0x0000000087654321
    le_prev = 0x0000000000000000
  }
  b_freelist = {
    tqe_next = 0x0000000000000000
    tqe_prev = 0xffffff80bd02d140
  }
  b_timestamp = 0x00000000
  b_timestamp_tv = (tv_sec = 0x00000000000000ff, tv_usec = 0x000be950)
  b_whichq = 0xffffffff
  b_flags = 0x418000c3
  b_lflags = 0x00000005
  b_error = 0x00000000
  b_bufsize = 0x00000000
  b_bcount = 0x00008000
  b_resid = 0x00000000
  b_dev = 0xffffffff
  b_datap = 0x0000000000000000
  b_lblkno = 0x0000000000000000
  b_blkno = 0x0000000000160eb3
  b_iodone = 0xffffff8027a77a60 (kernel`cluster_iodone at vfs_cluster.c:725)
  b_vp = 0xffffff80357b1ba0
  b_rcred = 0x0000000000000000
  b_wcred = 0x0000000000000000
  b_upl = 0xffffff803bbc8c60
  b_real_bp = 0x0000000000000000
  b_act = {
    tqe_next = 0x0000000000000000
    tqe_prev = 0x0000000000000000
  }
  b_drvdata = 0xffffff803a7499c0
  b_fsprivate = 0x0000000000000000
  b_transaction = 0x0000000000000000
  b_dirtyoff = 0x00000000
  b_dirtyend = 0x00000000
  b_validoff = 0x00000000
  b_validend = 0x00000000
  b_redundancy_flags = 0x00000000
  b_proc = 0x0000000000000000
  b_attr = {
    ba_cpx = 0x0000000000000000
    ba_cp_file_off = 0x0000000000000000
    ba_flags = 0x0000000000000000
  }
}
(lldb) 
```

