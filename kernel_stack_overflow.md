An example of a kernel stack overflow caused by nested calls. This type of stack overflow usually manifests itself as a kernel panic with a double fault error.

```
    frame #0: 0xffffff802485002c kernel`panic_trap_to_debugger [inlined] current_cpu_datap at cpu_data.h:401 [opt]
    frame #1: 0xffffff802485002c kernel`panic_trap_to_debugger [inlined] current_processor at cpu.c:220 [opt]
    frame #2: 0xffffff802485002c kernel`panic_trap_to_debugger [inlined] DebuggerTrapWithState(db_op=DBOP_PANIC, db_message=<unavailable>, db_panic_str="\"%s at 0x%016llx, registers:\\n\" \"CR0: 0x%016lx, CR2: 0x%016lx, CR3: 0x%016lx, CR4: 0x%016lx\\n\" \"RAX: 0x%016llx, RBX: 0x%016llx, RCX: 0x%016llx, RDX: 0x%016llx\\n\" \"RSP: 0x%016llx, RBP: 0x%016llx, RSI: 0x%016llx, RDI: 0x%016llx\\n\" \"R8:  0x%016llx, R9:  0x%016llx, R10: 0x%016llx, R11: 0x%016llx\\n\" \"R12: 0x%016llx, R13: 0x%016llx, R14: 0x%016llx, R15: 0x%016llx\\n\" \"RFL: 0x%016llx, RIP: 0x%016llx, CS:  0x%016llx, SS:  0x%016llx\\n\" \"Error code: 0x%016llx%s\\n\"@/BuildRoot/Library/Caches/com.apple.xbs/Sources/xnu/xnu-4570.31.3/osfmk/i386/trap_native.c:168", db_panic_args=<unavailable>, db_panic_options=<unavailable>, db_proceed_on_sync_failure=1, db_panic_caller=<unavailable>) at debug.c:463 [opt]
    frame #3: 0xffffff802485000d kernel`panic_trap_to_debugger(panic_format_str="\"%s at 0x%016llx, registers:\\n\" \"CR0: 0x%016lx, CR2: 0x%016lx, CR3: 0x%016lx, CR4: 0x%016lx\\n\" \"RAX: 0x%016llx, RBX: 0x%016llx, RCX: 0x%016llx, RDX: 0x%016llx\\n\" \"RSP: 0x%016llx, RBP: 0x%016llx, RSI: 0x%016llx, RDI: 0x%016llx\\n\" \"R8:  0x%016llx, R9:  0x%016llx, R10: 0x%016llx, R11: 0x%016llx\\n\" \"R12: 0x%016llx, R13: 0x%016llx, R14: 0x%016llx, R15: 0x%016llx\\n\" \"RFL: 0x%016llx, RIP: 0x%016llx, CS:  0x%016llx, SS:  0x%016llx\\n\" \"Error code: 0x%016llx%s\\n\"@/BuildRoot/Library/Caches/com.apple.xbs/Sources/xnu/xnu-4570.31.3/osfmk/i386/trap_native.c:168", panic_args=<unavailable>, reason=<unavailable>, ctx=0x0000000000000000, panic_options_mask=<unavailable>, panic_caller=0) at debug.c:724 [opt]
    frame #4: 0xffffff802484fdac kernel`panic(str=<unavailable>) at debug.c:611 [opt]
    frame #5: 0xffffff802496fd1a kernel`panic_64(sp=0xffffff802454c6e0, pc=<unavailable>, msg="Double fault", do_mca_dump=<unavailable>) at trap_native.c:152 [opt]
    frame #6: 0xffffff80248029dd kernel`hndl_double_fault + 29
    frame #7: 0x000000000000000f
    frame #8: 0xffffff802485c204 kernel`kalloc_canblock [inlined] zalloc_canblock_tag(zone=0x0000000000000008, canblock=<unavailable>) at zalloc.c:3366 [opt]
    frame #9: 0xffffff802485c1fa kernel`kalloc_canblock(psize=0xffffff802c328140, canblock=<unavailable>, site=<unavailable>) at kalloc.c:720 [opt]
    frame #10: 0xffffff80249016e5 kernel`upl_create(type=2, flags=512, size=<unavailable>) at vm_pageout.c:4794 [opt]
    frame #11: 0xffffff8024902449 kernel`vm_object_iopl_request(object=0xffffff802523f540, offset=<unavailable>, size=<unavailable>, upl_ptr=0xffffff802c328748, user_page_list=<unavailable>, page_list_count=0xffffff802c3287bc, cntrl_flags=5120, tag=<unavailable>) at vm_pageout.c:8242 [opt]
    frame #12: 0xffffff8024902297 kernel`vm_map_create_upl(map=<unavailable>, offset=<unavailable>, upl_size=0xffffff802c3287b8, upl=0xffffff802c328748, page_list=0xffffff8039ab6bb0, count=0xffffff802c3287bc, flags=<unavailable>, tag=<unavailable>) at vm_pageout.c:6286 [opt]
    frame #13: 0xffffff802490216e kernel`vm_map_create_upl(map=<unavailable>, offset=<unavailable>, upl_size=0xffffff802c3287b8, upl=0xffffff802c328748, page_list=0xffffff8039ab6bb0, count=0xffffff802c3287bc, flags=<unavailable>, tag=<unavailable>) at vm_pageout.c:6152 [opt]
    frame #14: 0xffffff8024ea430b kernel`IOGeneralMemoryDescriptor::wireVirtual(this=<unavailable>, forDirection=<unavailable>) at IOMemoryDescriptor.cpp:3173 [opt]
    frame #15: 0xffffff8024ea20ba kernel`IOGeneralMemoryDescriptor::prepare(this=0xffffff80347b72c0, forDirection=0) at IOMemoryDescriptor.cpp:3488 [opt]
    frame #16: 0xffffff7fa5275479 IOStorageFamily`dkreadwrite(dkr=0xffffff80b9c9f110, dkrtype=DKRTYPE_BUF) at IOMediaBSDClient.cpp:2915 [opt]
    frame #17: 0xffffff8024ac8b04 kernel`spec_strategy(ap=<unavailable>) at spec_vnops.c:2404 [opt]
  * frame #18: 0xffffff8024ac0712 kernel`VNOP_STRATEGY(bp=<unavailable>) at kpi_vfs.c:5696 [opt]
    frame #19: 0xffffff7fa5eaeb9e
    frame #20: 0xffffff7fa5eae92c
    frame #21: 0xffffff7fa5ead970
    frame #22: 0xffffff7fa5eb8a57
    frame #23: 0xffffff7fa5eb7f74
    frame #24: 0xffffff7fa5e9c108
    frame #25: 0xffffff7fa5e9bf7b
    frame #26: 0xffffff7fa5ea468f
    frame #27: 0xffffff7fa5ea4d86
    frame #28: 0xffffff7fa5e7d7ca
    frame #29: 0xffffff7fa5e83f63
    frame #30: 0xffffff7fa5e882ca
    frame #31: 0xffffff7fa5e5a827
    frame #32: 0xffffff7fa5e5b5f6
    frame #33: 0xffffff7fa5e4e8a3
    frame #34: 0xffffff7fa7a4f0fa VFSFilter`CmdVnopLookupHook(ap=0xffffff802c329d78) at VFSHooks.cpp:497
    frame #35: 0xffffff8024a82bf9 kernel`lookup [inlined] VNOP_LOOKUP(cnp=<unavailable>, ctx=<unavailable>) at kpi_vfs.c:3010 [opt]
    frame #36: 0xffffff8024a82bab kernel`lookup(ndp=<unavailable>) at vfs_lookup.c:1150 [opt]
    frame #37: 0xffffff8024a81dc2 kernel`namei(ndp=0xffffff802c329f50) at vfs_lookup.c:390 [opt]
    frame #38: 0xffffff8024a8dd35 kernel`vnode_lookup(path=<unavailable>, flags=<unavailable>, vpp=0xffffff802c32a680, ctx=<unavailable>) at vfs_subr.c:5544 [opt]
    frame #39: 0xffffff7fa7a502c7 VFSFilter`CmdVnopLookupHook(ap=0xffffff802c32acb8) at VFSHooks.cpp:702
    frame #40: 0xffffff8024a82bf9 kernel`lookup [inlined] VNOP_LOOKUP(cnp=<unavailable>, ctx=<unavailable>) at kpi_vfs.c:3010 [opt]
    frame #41: 0xffffff8024a82bab kernel`lookup(ndp=<unavailable>) at vfs_lookup.c:1150 [opt]
    frame #42: 0xffffff8024a81dc2 kernel`namei(ndp=0xffffff802c32af48) at vfs_lookup.c:390 [opt]
    frame #43: 0xffffff8024ab22ea kernel`open_xattrfile(vp=0xffffff8030fbe8b8, fileflags=<unavailable>, xvpp=0xffffff802c32b340, context=<unavailable>) at vfs_xattr.c:2539 [opt]
    frame #44: 0xffffff8024ab1b02 kernel`vn_getxattr [inlined] default_getxattr(options=0) at vfs_xattr.c:1621 [opt]
    frame #45: 0xffffff8024ab1aaa kernel`vn_getxattr(vp=0xffffff8030fbe8b8, name="com.apple.FinderInfo", uio=0xffffff802c32b500, size=0xffffff802c32b450, options=912436912, context=0xffffff803662aeb0) at vfs_xattr.c:169 [opt]
    frame #46: 0xffffff8024a655b5 kernel`vfs_attr_pack_internal at vfs_attrlist.c:1706 [opt]
    frame #47: 0xffffff8024a654f1 kernel`vfs_attr_pack_internal(vp=<unavailable>, auio=<unavailable>, alp=<unavailable>, options=<unavailable>, vap=<unavailable>, fndesc=<unavailable>, ctx=<unavailable>, is_bulk=<unavailable>, vtype=<unavailable>, fixedsize=<unavailable>) at vfs_attrlist.c:2625 [opt]
    frame #48: 0xffffff8024a67fc2 kernel`getattrlist_internal(ctx=0xffffff803662aeb0, vp=0xffffff8030fbe8b8, alp=<unavailable>, attributeBuffer=4408281593728, bufferSize=<unavailable>, options=37, segflg=<unavailable>, authoritative_name=<unavailable>, file_cred=<unavailable>) at vfs_attrlist.c:2956 [opt]
    frame #49: 0xffffff8024a6a6bf kernel`getattrlistat_internal(ctx=0xffffff803662aeb0, path=<unavailable>, alp=0xffffff802c32bf18, attributeBuffer=140549983849064, bufferSize=1016, options=37, segflg=<unavailable>, pathsegflg=<unavailable>, fd=<unavailable>) at vfs_attrlist.c:3042 [opt]
    frame #50: 0xffffff8024a6a590 kernel`getattrlist(p=<unavailable>, uap=0xffffff803662ad58, retval=<unavailable>) at vfs_attrlist.c:3067 [opt]
    frame #51: 0xffffff8024dfb3e8 kernel`unix_syscall64(state=<unavailable>) at systemcalls.c:382 [opt]
    frame #52: 0xffffff8024802906 kernel`hndl_unix_scall64 + 22
```

Let's get the system kernel stack size.

```
(lldb) p/x kernel_stack_size
(vm_offset_t) $14 = 0x0000000000004000
```

Get the current thread stack base. First get the list of the threads executed on each CPU and locate a thread of interest.

```
(lldb) showcurrentthreads
Processor 0xffffff80251e03b8 cpu_id  0x0 AST:        State RUNNING      Preemption Disabled
task                 vm_map               ipc_space            #acts flags    pid       process             io_policy  wq_state  command             
0xffffff8037164648   0xffffff8035b65a00   0xffffff8035eaf9c0      18 DB       458   0xffffff8038976910              Q -1 -1 -1    Safari              
	thread                   thread_id  processor            base   pri    sched_mode      io_policy       state    ast          waitq                            wait_event           wmesg                thread_name         
	0xffffff8035ab0000  [WQ] 0x2321     0xffffff80251e03b8   37     37     timeshare                       R                                                                                                                    
Processor 0xffffff80b7d96000 cpu_id  0x1 AST:        State IDLE         Preemption Disabled
task                 vm_map               ipc_space            #acts flags    pid       process             io_policy  wq_state  command             
0xffffff803062ec28   0xffffff8028888520   0xffffff802fc8ccc0     152            0   0xffffff80252117f8                -1 -1 -1    kernel_task         
	thread                   thread_id  processor            base   pri    sched_mode      io_policy       state    ast          waitq                            wait_event           wmesg                thread_name         
	0xffffff8030bdb650       0x93       0xffffff80b7d96000   0      0      fixed bound                     RI                                                                                               idle #1             
Processor 0xffffff80b7d95000 cpu_id  0x2 AST:        State IDLE         Preemption Disabled
task                 vm_map               ipc_space            #acts flags    pid       process             io_policy  wq_state  command             
0xffffff803062ec28   0xffffff8028888520   0xffffff802fc8ccc0     152            0   0xffffff80252117f8                -1 -1 -1    kernel_task         
	thread                   thread_id  processor            base   pri    sched_mode      io_policy       state    ast          waitq                            wait_event           wmesg                thread_name         
	0xffffff8030bdb160       0x90       0xffffff80b7d95000   0      0      fixed bound                     RI                                                                                               idle #2             
Processor 0xffffff80b7d9d000 cpu_id  0x3 AST:        State IDLE         Preemption Disabled
task                 vm_map               ipc_space            #acts flags    pid       process             io_policy  wq_state  command             
0xffffff803062ec28   0xffffff8028888520   0xffffff802fc8ccc0     152            0   0xffffff80252117f8                -1 -1 -1    kernel_task         
	thread                   thread_id  processor            base   pri    sched_mode      io_policy       state    ast          waitq                            wait_event           wmesg                thread_name         
	0xffffff8030bdca10       0x96       0xffffff80b7d9d000   0      0      fixed bound                     RI                                                                                               idle #3     
```

Get the thread's stack base

```
(lldb) p/x ((struct thread*)0xffffff8035ab0000)->kernel_stack
(vm_offset_t) $15 = 0xffffff802c328000
```

Check that at the top of a call stack we have a full stack available

```
(lldb) f 50
frame #50: 0xffffff8024a6a590 kernel`getattrlist(p=<unavailable>, uap=0xffffff803662ad58, retval=<unavailable>) at vfs_attrlist.c:3067 [opt]
(lldb) p/x $rsp-0xffffff802c328000
(unsigned long) $16 = 0x0000000000003ef0
```

Check that at the bottom of the call stack the stack has been depleted

```
(lldb) f 8
frame #8: 0xffffff802485c204 kernel`kalloc_canblock [inlined] zalloc_canblock_tag(zone=0x0000000000000008, canblock=<unavailable>) at zalloc.c:3366 [opt]
(lldb) p/x $rsp-0xffffff802c328000
(unsigned long) $17 = 0x00000000000000f0
```
