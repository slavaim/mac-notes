
A mapped vnode is held by bumping its ```v_usecount``` in ```ubc_map``` and releasing it in ```ubc_unmap```.

```
ubc_map(vnode_t vp, int flags)
{
  struct ubc_info *uip;
...
  int need_ref = 0;
...
      uip = vp->v_ubcinfo;

      if ( !ISSET(uip->ui_flags, UI_ISMAPPED))
              need_ref = 1;          
 ...
      if (need_ref) {
        /*
         * Make sure we get a ref as we can't unwind from here
         */
        if (vnode_ref_ext(vp, 0, VNODE_REF_FORCE))
          panic("%s : VNODE_REF_FORCE failed\n", __FUNCTION__);
      }
...
}
```

```
  * frame #0: 0xffffff802b8248f4 kernel`vnode_pager_map(mem_obj=0xffffff80392dfa50, prot=5) at bsd_vm.c:737 [opt]
    frame #1: 0xffffff802b848e86 kernel`vm_map_enter_mem_object_control [inlined] memory_object_map(memory_object=<unavailable>, prot=<unavailable>) at memory_object.c:2241 [opt]
    frame #2: 0xffffff802b848e7d kernel`vm_map_enter_mem_object_control(target_map=0xffffff8046fb1e08, address=0xffffff80c06ab410, initial_size=20480, mask=0, flags=0, vmk_flags=vm_map_kernel_flags_t @ 0xffffff80c06ab374, tag=0, control=0xffffff80390ae2b0, offset=0, copy=1, cur_protection=5, max_protection=7, inheritance=1) at vm_map.c:4792 [opt]
    frame #3: 0xffffff802bd43333 kernel`map_segment(map=<unavailable>, vm_start=4525547520, vm_end=<unavailable>, control=0xffffff80390ae2b0, file_start=0, file_end=<unavailable>, initprot=5, maxprot=7, result=0xffffff80c06aba40) at mach_loader.c:1474 [opt]
    frame #4: 0xffffff802bd42dcb kernel`load_segment(lcp=<unavailable>, filetype=2, control=0x0000000000005000, pager_offset=<unavailable>, macho_size=-548793093992, vp=0xffffff80392e1838, map=0xffffff8046fb1e08, slide=230580224, result=0xffffff80c06aba40) at mach_loader.c:1838 [opt]
    frame #5: 0xffffff802bd415c0 kernel`parse_machfile(vp=0xffffff80392e1838, map=0xffffff8046fb1e08, thread=0xffffff803f9a7bf0, header=0xffffff80c0bc7000, file_offset=<unavailable>, macho_size=44048, depth=<unavailable>, aslr_offset=230580224, dyld_aslr_offset=89870336, result=0xffffff80c06aba40, binresult=0x0000000000000000, imgp=0xffffff80422d6680) at mach_loader.c:975 [opt]
    frame #6: 0xffffff802bd4091d kernel`load_machfile(imgp=<unavailable>, header=0xffffff80c0bc7000, thread=0xffffff803f9a7bf0, mapp=0xffffff80c06ab988, result=0xffffff80c06aba40) at mach_loader.c:428 [opt]
    frame #7: 0xffffff802bcaf1c0 kernel`exec_mach_imgact(imgp=0xffffff80422d6680) at kern_exec.c:996 [opt]
    frame #8: 0xffffff802bcb57e1 kernel`exec_activate_image(imgp=0xffffff80422d6680) at kern_exec.c:1531 [opt]
    frame #9: 0xffffff802bcb4bea kernel`posix_spawn(ap=0xffffff80450089b0, uap=<unavailable>, retval=0xffffff803c4fa738) at kern_exec.c:2864 [opt]
    frame #10: 0xffffff802bdb619b kernel`unix_syscall64(state=<unavailable>) at systemcalls.c:381 [opt]
    frame #11: 0xffffff802b75c466 kernel`hndl_unix_scall64 + 22
```

```
ubc_unmap(struct vnode *vp)
{
	....
	int	need_rele = 0;
  ....

		if (ISSET(uip->ui_flags, UI_ISMAPPED)) {
			if (ISSET(uip->ui_flags, UI_MAPPEDWRITE))
				want_fsevent = true;

			need_rele = 1;
		}

		if (need_rele) {

		        (void)VNOP_MNOMAP(vp, ctx);

            ...

		        vnode_rele(vp);
     }
 }
```

```
  * frame #0: 0xffffff802bd03980 kernel`ubc_unmap [inlined] lck_mtx_lock_spin(lock=0xffffff803c108000) at locks_i386_opt.c:302 [opt]
    frame #1: 0xffffff802bd03980 kernel`ubc_unmap [inlined] vnode_lock_spin(vp=0xffffff803c108000) at vfs_subr.c:4407 [opt]
    frame #2: 0xffffff802bd03980 kernel`ubc_unmap [inlined] vget_internal(vp=0xffffff803c108000, vid=0, vflags=0) at vfs_subr.c:1551 [opt]
    frame #3: 0xffffff802bd03980 kernel`ubc_unmap [inlined] vnode_getwithref(vp=0xffffff803c108000) at vfs_subr.c:4472 [opt]
    frame #4: 0xffffff802bd03980 kernel`ubc_unmap(vp=0xffffff803c108000) at ubc_subr.c:2042 [opt]
    frame #5: 0xffffff802b82491d kernel`vnode_pager_last_unmap(mem_obj=<unavailable>) at bsd_vm.c:758 [opt]
    frame #6: 0xffffff802b8587cf kernel`vm_object_deallocate [inlined] memory_object_last_unmap(memory_object=<unavailable>) at memory_object.c:2252 [opt]
    frame #7: 0xffffff802b8587c5 kernel`vm_object_deallocate(object=0xffffff803c117e00) at vm_object.c:845 [opt]
    frame #8: 0xffffff802b843f89 kernel`vm_map_delete [inlined] vm_map_entry_delete(map=<unavailable>, entry=<unavailable>) at vm_map.c:7290 [opt]
    frame #9: 0xffffff802b843e64 kernel`vm_map_delete(map=<unavailable>, start=<unavailable>, end=<unavailable>, flags=-759644660, zap_map=0x0000000000000000) at vm_map.c:8148 [opt]
    frame #10: 0xffffff802b84b332 kernel`vm_map_remove(map=0xffffff80395e6b20, start=4563341312, end=4563369984, flags=0) at vm_map.c:8249 [opt]
    frame #11: 0xffffff802b7a1833 kernel`_kernelrpc_mach_vm_deallocate_trap [inlined] mach_vm_deallocate(map=<unavailable>, start=<unavailable>, size=<unavailable>) at vm_user.c:324 [opt]
    frame #12: 0xffffff802b7a17f0 kernel`_kernelrpc_mach_vm_deallocate_trap(args=<unavailable>) at mach_kernelrpc.c:73 [opt]
    frame #13: 0xffffff802b8c166b kernel`mach_call_munger64(state=0xffffff803940f720) at bsd_i386.c:573 [opt]
    frame #14: 0xffffff802b75c486 kernel`hndl_mach_scall64 + 22
```
