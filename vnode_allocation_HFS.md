
```
	0xffffff90d303a260 0xffffff802ffd5e7d try_alloc_from_zone((zone_t) zone = 0xffffff80308c1670, (vm_tag_t) tag = <>, , (boolean_t *) check_poison = <>, ) 
	0xffffff90d303a3b0 0xffffff802ffd3df1 zalloc_internal((zone_t) zone = 0xffffff80308c1670, (boolean_t) canblock = 1, (boolean_t) nopagewait = 0, (vm_size_t) reqsize = 0x0000000000000206, (vm_tag_t) tag = <>, ) 
	0xffffff90d303a420 0xffffff802ff8878c zalloc_canblock_tag [inlined]((zone_t) zone = <>, , (boolean_t) canblock = 1, (vm_size_t) reqsize = <>, , (vm_tag_t) tag = <>, ) 
	0xffffff90d303a420 0xffffff802ff88778 kalloc_canblock((vm_size_t *) psize = <>, , (boolean_t) canblock = 1, (vm_allocation_site_t *) site = 0xffffff80308074d0) 
	0xffffff90d303a440 0xffffff802ff889e1 kalloc_external((vm_size_t) size = <>, ) 
	0xffffff90d303a4c0 0xffffff7fb257f1ea HFS`cat_lookup + 0x37 
	0xffffff90d303a610 0xffffff7fb25999e2 HFS`hfs_valid_cnode + 0xda 
	0xffffff90d303a720 0xffffff7fb2598f8b HFS`hfs_getnewvnode + 0x478 
	0xffffff90d303a8d0 0xffffff7fb2586b80 HFS`hfs_vnop_lookup + 0x37c 
....
	0xffffff90d303ba90 0xffffff80301e8d4c VNOP_LOOKUP [inlined]((componentname *) cnp = <>, , (vfs_context_t) ctx = <>, ) 
	0xffffff90d303ba90 0xffffff80301e8cfe lookup((nameidata *) ndp = <>, ) 
	0xffffff90d303bba0 0xffffff80301e7f12 namei((nameidata *) ndp = 0xffffff90d303bd48) 
	0xffffff90d303bbf0 0xffffff80301ff324 nameiat((nameidata *) ndp = 0xffffff90d303bd48, (int) dirfd = <>, ) 
	0xffffff90d303bf10 0xffffff8030204293 fstatat_internal((vfs_context_t) ctx = <no location, value may have been optimized out>, , (user_addr_t) path = <no location, value may have been optimized out>, , (user_addr_t) ub = <no location, value may have been optimized out>, , (user_addr_t) xsecurity = <no location, value may have been optimized out>, , (user_addr_t) xsecurity_size = <no location, value may have been optimized out>, , (int) isstat64 = <no location, value may have been optimized out>, , (uio_seg) segflg = UIO_USERSPACE, (int) fd = <no location, value may have been optimized out>, , (int) flag = <no location, value may have been optimized out>, ) 
	0xffffff90d303bf40 0xffffff8030204cbc fstatat64((proc_t) p = <>, , (fstatat64_args *) uap = <>, , (int32_t *) retval = <>, ) 
	0xffffff90d303bfa0 0xffffff80305a60ca unix_syscall64((x86_saved_state_t *) state = <>, ) 
	0x0000000000000000 0xffffff802ff20a36 kernel.development`hndl_unix_scall64 + 0x16 
```
