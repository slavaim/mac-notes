```
  * frame #0: 0xffffff802c4fbefe kernel.development`parse_machfile [inlined] load_dylinker(map=<unavailable>, thread=<unavailable>, depth=<unavailable>, slide=<unavailable>, result=<unavailable>, imgp=<unavailable>) at mach_loader.c:2273 [opt]
    frame #1: 0xffffff802c4fbefe kernel.development`parse_machfile(vp=0xffffff8039b17078, map=0xffffff8048abc500, thread=0xffffff804199b250, header=0xffffff80c1399000, file_offset=<unavailable>, macho_size=43696, depth=<unavailable>, aslr_offset=<unavailable>, dyld_aslr_offset=<unavailable>, result=<unavailable>, binresult=<unavailable>, imgp=<unavailable>) at mach_loader.c:1163 [opt]
    frame #2: 0xffffff802c4fa624 kernel.development`load_machfile(imgp=0xffffff8039576a80, header=0xffffff80c1399000, thread=0xffffff804199b250, mapp=0xffffff803415b980, result=0xffffff803415ba40) at mach_loader.c:423 [opt]
    frame #3: 0xffffff802c46d1bc kernel.development`exec_mach_imgact(imgp=<unavailable>) at kern_exec.c:963 [opt]
    frame #4: 0xffffff802c473041 kernel.development`exec_activate_image(imgp=0xffffff8039576a80) at kern_exec.c:1477 [opt]
    frame #5: 0xffffff802c471dcb kernel.development`posix_spawn(ap=0xffffff8043a07db0, uap=<unavailable>, retval=0xffffff8044214040) at kern_exec.c:2797 [opt]
    frame #6: 0xffffff802c5a60ca kernel.development`unix_syscall64(state=<unavailable>) at systemcalls.c:382 [opt]
    frame #7: 0xffffff802bf20a36 kernel.development`hndl_unix_scall64 + 22
```
