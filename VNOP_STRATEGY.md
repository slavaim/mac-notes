
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
   
A connection between ```VNOP_STRATEGY``` and ```IOKit```.

```
    frame #8: 0xffffff80099d3df1 kernel.development`zalloc_internal(zone=0xffffff800975e6e0, canblock=1, nopagewait=0, reqsize=0x0000000000000030, tag=<unavailable>) at zalloc.c:3084 [opt]
    frame #9: 0xffffff800998878c kernel.development`kalloc_canblock [inlined] zalloc_canblock_tag(zone=<unavailable>, canblock=1, reqsize=<unavailable>, tag=<unavailable>) at zalloc.c:3370 [opt]
    frame #10: 0xffffff8009988778 kernel.development`kalloc_canblock(psize=<unavailable>, canblock=1, site=0xffffff800a2b1fe0) at kalloc.c:693 [opt]
    frame #11: 0xffffff8009fb67f0 kernel.development`OSObject::operator new(size=48) at OSObject.cpp:259 [opt]
    frame #12: 0xffffff8009fbac04 kernel.development`OSData::withCapacity(inCapacity=144) at OSData.cpp:140 [opt]
    frame #13: 0xffffff800a05106a kernel.development`IOGeneralMemoryDescriptor::initMemoryEntries(this=0xffffff8022823100, size=<unavailable>, mapper=0xffffff80154fd400) at IOMemoryDescriptor.cpp:3286 [opt]
    frame #14: 0xffffff800a04d638 kernel.development`IOGeneralMemoryDescriptor::initWithOptions(this=<unavailable>, buffers=<unavailable>, count=<unavailable>, offset=578957688, task=<unavailable>, options=<unavailable>, mapper=0xffffff80154fd400) at IOMemoryDescriptor.cpp:1699 [opt]
    frame #15: 0xffffff800a053273 kernel.development`IOMemoryDescriptor::withAddressRange(unsigned long long, unsigned long long, unsigned int, task*) [inlined] IOMemoryDescriptor::withAddressRanges(rangeCount=1, options=<unavailable>) at IOMemoryDescriptor.cpp:1178 [opt]
    frame #16: 0xffffff800a0531fb kernel.development`IOMemoryDescriptor::withAddressRange(address=<unavailable>, length=<unavailable>, options=1, task=0xffffff8014f7d1b8) at IOMemoryDescriptor.cpp:1161 [opt]
    frame #17: 0xffffff7f8a25e4cc IOStorageFamily`dkreadwrite(void*, dkrtype_t) at IOMediaBSDClient.cpp:2723 [opt]
    frame #18: 0xffffff7f8a25e45a IOStorageFamily`dkreadwrite(dkr=0xffffff809ec30230, dkrtype=DKRTYPE_BUF) at IOMediaBSDClient.cpp:2871 [opt]
    frame #19: 0xffffff7f8be3110e BootCache`BC_strategy + 1198
    frame #20: 0xffffff8009c2b6f6 kernel.development`spec_strategy(ap=<unavailable>) at spec_vnops.c:2409 [opt]
    frame #21: 0xffffff8009c237d2 kernel.development`VNOP_STRATEGY(bp=<unavailable>) at kpi_vfs.c:5696 [opt]
    frame #22: 0xffffff7f8c484786 apfs`nx_buf_bread + 387
    frame #23: 0xffffff7f8c484514 apfs`_vnode_dev_read + 504
    frame #24: 0xffffff7f8c483557 apfs`vnode_dev_read + 34
    frame #25: 0xffffff7f8c48e800 apfs`obj_read + 212
    frame #26: 0xffffff7f8c48db54 apfs`obj_get + 3033
    frame #27: 0xffffff7f8c47191b apfs`btree_node_get_internal + 340
    frame #28: 0xffffff7f8c4717b8 apfs`btree_node_get + 106
    frame #29: 0xffffff7f8c479edc apfs`_bt_lookup_variant + 814
    frame #30: 0xffffff7f8c47a36c apfs`bt_iterator_init_with_hint + 292
    frame #31: 0xffffff7f8c454106 apfs`iterate_jobjs_with_hint_and_snap + 302
    frame #32: 0xffffff7f8c45a9f8 apfs`fs_lookup_name_with_parent_id + 509
    frame #33: 0xffffff7f8c45df85 apfs`fs_lookup_name_and_hash + 65
    frame #34: 0xffffff7f8c42f697 apfs`apfs_lookup_name_with_alt_name + 63
    frame #35: 0xffffff7f8c43046e apfs`apfs_internal_lookup + 234
    frame #36: 0xffffff7f8c42497c apfs`apfs_vnop_lookup + 1095
```
