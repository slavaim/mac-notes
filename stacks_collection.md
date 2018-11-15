A collection of unclassified stacks. Some functions were replaced with ```private + XXXX``` as they belong to a private product.

```
	0xffffff90d26983b0 0xffffff802de68da1 vm_object_iopl_request((vm_object_t) object = 0x0000000000000000, (vm_object_offset_t) offset = 0, (upl_size_t) size = <>, , (upl_t *) upl_ptr = 0xffffff90d26985c8, (upl_page_info_array_t) user_page_list = <>, , (unsigned int *) page_list_count = 0xffffff90d2698618, (upl_control_flags_t) cntrl_flags = <>, , (vm_tag_t) tag = 57) 
	0xffffff90d2698470 0xffffff802de68b8a vm_map_create_upl((vm_map_t) map = <>, , (vm_map_address_t) offset = <>, , (upl_size_t *) upl_size = 0xffffff90d2698614, (upl_t *) upl = 0xffffff90d26985c8, (upl_page_info_array_t) page_list = 0xffffff803a78cdb0, (unsigned int *) count = 0xffffff90d2698618, (upl_control_flags_t *) flags = 0xffffff90d2698578, (vm_tag_t) tag = 57) 
	0xffffff90d2698530 0xffffff802de68a6a vm_map_create_upl((vm_map_t) map = <>, , (vm_map_address_t) offset = <>, , (upl_size_t *) upl_size = 0xffffff90d2698614, (upl_t *) upl = 0xffffff90d26985c8, (upl_page_info_array_t) page_list = 0xffffff803a78cdb0, (unsigned int *) count = 0xffffff90d2698618, (upl_control_flags_t *) flags = 0xffffff90d2698578, (vm_tag_t) tag = 57) 
	0xffffff90d2698650 0xffffff802e46d646 IOGeneralMemoryDescriptor::wireVirtual(unsigned int)((IOGeneralMemoryDescriptor *) this = <>, , (IODirection) forDirection = <>, ) 
	0xffffff90d2698680 0xffffff802e46b2a9 IOGeneralMemoryDescriptor::prepare(unsigned int)((IOGeneralMemoryDescriptor *) this = 0xffffff80434ae540, (IODirection) forDirection = 0) 
	0xffffff90d2698740 0xffffff7fae98f337 dkreadwrite(void*, dkrtype_t)((dkr_t) dkr = 0xffffff80c3d62350, (dkrtype_t) dkrtype = DKRTYPE_BUF) 
	0xffffff90d26987b0 0xffffff802e03785f spec_strategy((vnop_strategy_args *) ap = <>, ) 
	0xffffff90d26987f0 0xffffff802e02edc2 VNOP_STRATEGY((buf *) bp = <>, ) 
	0xffffff90d2698840 0xffffff7faefd7697 com.apple.filesystems.apfs + 0x2697 
	0xffffff90d26988c0 0xffffff7faefd7323 com.apple.filesystems.apfs + 0x2323 
	0xffffff90d26988e0 0xffffff7faefd634c com.apple.filesystems.apfs + 0x134c 
	0xffffff90d2698940 0xffffff7faf083851 com.apple.filesystems.apfs + 0xae851 
	0xffffff90d2698a40 0xffffff7faf083209 com.apple.filesystems.apfs + 0xae209 
	0xffffff90d2698b10 0xffffff7faf06260d com.apple.filesystems.apfs + 0x8d60d 
	0xffffff90d2698b80 0xffffff7faf0624a7 com.apple.filesystems.apfs + 0x8d4a7 
	0xffffff90d2698c50 0xffffff7faf06aa59 com.apple.filesystems.apfs + 0x95a59 
	0xffffff90d2698c80 0xffffff7faf0591cd com.apple.filesystems.apfs + 0x841cd 
	0xffffff90d2698d20 0xffffff7faf040ec2 com.apple.filesystems.apfs + 0x6bec2 
	0xffffff90d2698ff0 0xffffff7faf04edbe com.apple.filesystems.apfs + 0x79dbe 
	0xffffff90d2699170 0xffffff7faeffb0f0 com.apple.filesystems.apfs + 0x260f0 
	0xffffff90d2699360 0xffffff7fb131d882 VifFsdBlockmapHook(vnop_blockmap_args*)((vnop_blockmap_args *) ap = 0xffffff90d2699400) 
	0xffffff90d26993f0 0xffffff7fb12e5371 private + XXXX
	0xffffff90d2699480 0xffffff802e02ec8f VNOP_BLOCKMAP((vnode *) vp = 0xffffff803c606aa8, (off_t) foffset = <>, , (size_t) size = 0, (daddr64_t *) bpn = <>, , (size_t *) run = 0x0000000000000010, (void *) poff = <>, , (int) flags = 2, (vfs_context_t) ctx = <>, ) 
	0xffffff90d26995d0 0xffffff802dfe2de0 cluster_io((vnode_t) vp = 0xffffff803c606aa8, (upl_t) upl = <>, , (vm_offset_t) upl_offset = <>, , (off_t) f_offset = 16384, (int) non_rounded_size = <>, , (int) flags = <>, , (buf_t) real_bp = 0x0000000000000000, (clios *) iostate = 0x0000000000000000, (int (*)(buf_t, void *)) callback = 0x0000000000000000, (void *) callback_arg = 0x0000000000000000) 
	0xffffff90d2699640 0xffffff802dfe27dc cluster_pageout_ext((vnode_t) vp = 0xffffff803c606aa8, (upl_t) upl = <>, , (upl_offset_t) upl_offset = <>, , (off_t) f_offset = 16384, (int) size = <>, , (off_t) filesize = <>, , (int) flags = 89, (int (*)(buf_t, void *)) callback = 0x0000000000000000, (void *) callback_arg = 0x0000000000000000) 
	0xffffff90d2699670 0xffffff802dfe26b5 cluster_pageout((vnode_t) vp = <>, , (upl_t) upl = <>, , (upl_offset_t) upl_offset = <>, , (off_t) f_offset = <>, , (int) size = <>, , (off_t) filesize = <>, , (int) flags = 89) 
	0xffffff90d2699760 0xffffff7faeffa0cb com.apple.filesystems.apfs + 0x250cb 
	0xffffff90d26998e0 0xffffff7fb131ad3e private + XXXX
	0xffffff90d26999e0 0xffffff802e354095 VNOP_PAGEOUT [inlined]((vnode *) vp = <>, , (upl_offset_t) pl_offset = 0, (off_t) f_offset = <>, , (size_t) size = <>, , (int) flags = 89, (vfs_context_t) ctx = <>, ) 
	0xffffff90d26999e0 0xffffff802e35403b vnode_pageout((vnode *) vp = <>, , (upl_t) upl = <>, , (upl_offset_t) upl_offset = 0, (vm_object_offset_t) f_offset = <>, , (upl_size_t) size = <>, , (int) flags = 89, (int *) errorp = 0xffffff90d2699a14) 
	0xffffff90d2699a40 0xffffff802de22dbf vnode_pager_cluster_write((vnode_pager_t) vnode_object = 0xffffff803b701960, (vm_object_offset_t) offset = 16384, (vm_size_t) cnt = 0x0000000000004000, (vm_object_offset_t *) resid_offset = 0x0000000000000000, (int *) io_error = <>, , (int) upl_flags = 89) 
	0xffffff90d2699a50 0xffffff802de22c0f vnode_pager_data_return((memory_object_t) mem_obj = <>, , (memory_object_offset_t) offset = <>, , (memory_object_cluster_size_t) data_cnt = <>, , (memory_object_offset_t *) resid_offset = <>, , (int *) io_error = <>, , (boolean_t) dirty = <>, , (boolean_t) kernel_copy = 0, (int) upl_flags = 17) 
	0xffffff90d2699e10 0xffffff802de30a03 memory_object_data_return [inlined]((memory_object_t) memory_object = <>, , (memory_object_offset_t *) resid_offset = 0x0000000000000000, (int *) io_error = 0xffffff90d2699eb4, (boolean_t) dirty = 0, (boolean_t) kernel_copy = 0, (int) upl_flags = 17) 
	0xffffff90d2699e10 0xffffff802de309d6 vm_object_update_extent [inlined]((vm_object_t) object = 0xffffff803e8d2200, (vm_object_offset_t) offset = <>, , (vm_object_offset_t) offset_end = 32767, (vm_object_offset_t *) offset_resid = 0x0000000000000000, (int *) io_errno = 0xffffff90d2699eb4, (boolean_t) should_flush = 1, (memory_object_return_t) should_return = 2, (boolean_t) should_iosync = <>, , (vm_prot_t) prot = <no location, value may have been optimized out>, ) 
	0xffffff90d2699e10 0xffffff802de3086d vm_object_update((vm_object_t) object = 0xffffff803e8d2200, (vm_object_offset_t) offset = 1, (vm_object_size_t) size = <>, , (vm_object_offset_t *) resid_offset = 0x0000000000000000, (int *) io_errno = 0xffffff90d2699eb4, (memory_object_return_t) should_return = 2, (int) flags = 35, (vm_prot_t) protection = 8) 
	0xffffff90d2699e80 0xffffff802de2f974 memory_object_lock_request((memory_object_control_t) control = <>, , (memory_object_offset_t) offset = 0, (memory_object_size_t) size = <>, , (memory_object_offset_t *) resid_offset = 0x0000000000000000, (int *) io_errno = 0xffffff90d2699eb4, (memory_object_return_t) should_return = 2, (int) flags = 35, (vm_prot_t) prot = 8) 
	0xffffff90d2699ee0 0xffffff802e303158 ubc_msync_internal [inlined]((vnode_t) vp = <>, , (off_t) beg_off = 0, (off_t) end_off = <>, , (off_t *) resid_off = <>, , (int) flags = <>, , (int *) io_errno = 0x3c606aa800000000) 
	0xffffff90d2699ee0 0xffffff802e30309b ubc_msync((vnode_t) vp = 0xffffff803c606aa8, (off_t) beg_off = <>, , (off_t) end_off = <>, , (off_t *) resid_off = 0x0000000000000000, (int) flags = <>, ) 
	0xffffff90d2699f60 0xffffff802dff5b4a vclean((vnode_t) vp = 0xffffff803c606aa8, (int) flags = <>, ) 
	0xffffff90d2699fa0 0xffffff802dff53a6 vgone [inlined]((vnode_t) vp = <>, , (int) flags = <>, ) 
	0xffffff90d2699fa0 0xffffff802dff5397 vnode_reclaim_internal((vnode *) vp = 0xffffff803c606aa8, (int) locked = 1, (int) reuse = <>, , (int) flags = <>, ) 
	0xffffff90d2699fd0 0xffffff802dff6864 process_vp((vnode_t) vp = 0xffffff803c606aa8, (int) want_vp = 1, (int *) deferred = 0xffffff90d2699ff4) 
	0xffffff90d269a060 0xffffff802dff8a0e new_vnode [inlined](void) 
	0xffffff90d269a060 0xffffff802dff87f5 vnode_create_internal((uint32_t) flavor = <>, , (uint32_t) size = <>, , (void *) data = <>, , (vnode_t *) vpp = 0xffffff90d269bd70, (int) init_vnode = 1) 
	0xffffff90d269a1b0 0xffffff7faeffda23 com.apple.filesystems.apfs + 0x28a23 
	0xffffff90d269a280 0xffffff7faf0058ad com.apple.filesystems.apfs + 0x308ad 
	0xffffff90d269a3f0 0xffffff7fb1315d3a private + XXXX
	0xffffff90d269a4b0 0xffffff7fb12ca33d private + XXXX
	0xffffff90d269b820 0xffffff7fb12c2627 private + XXXX
	0xffffff90d269b8e0 0xffffff7fb12c23dd private + XXXX
	0xffffff90d269b920 0xffffff7fb12c22ac private + XXXX
	0xffffff90d269ba90 0xffffff802dfefbce VNOP_LOOKUP [inlined]((vnode_t) dvp = <>, , (vnode_t *) vpp = 0xffffff90d269bd70, (componentname *) cnp = <>, , (vfs_context_t) ctx = <>, ) 
	0xffffff90d269ba90 0xffffff802dfefb77 lookup((nameidata *) ndp = <>, ) 
	0xffffff90d269bba0 0xffffff802dfeee55 namei((nameidata *) ndp = 0xffffff90d269bd48) 
	0xffffff90d269bbf0 0xffffff802e007e12 nameiat((nameidata *) ndp = 0xffffff90d269bd48, (int) dirfd = -2) 
	0xffffff90d269bf10 0xffffff802e00d6f9 fstatat_internal((vfs_context_t) ctx = 0xffffff80404916b8, (user_addr_t) path = 140563185718144, (user_addr_t) ub = 140563185717880, (user_addr_t) xsecurity = 140563185719104, (user_addr_t) xsecurity_size = 123145478804168, (int) isstat64 = 1, (uio_seg) segflg = UIO_USERSPACE, (int) fd = -2, (int) flag = 32) 
	0xffffff90d269bf40 0xffffff802e00e06f lstat64_extended((proc_t) p = <>, , (lstat64_extended_args *) uap = 0xffffff8040491550, (int32_t *) retval = <>, ) 
	0xffffff90d269bfa0 0xffffff802e3b5b7b unix_syscall64((x86_saved_state_t *) state = <>, ) 
	0x0000000000000000 0xffffff802dd5a466 kernel`hndl_unix_scall64 + 0x16 
```
