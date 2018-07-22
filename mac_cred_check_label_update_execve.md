
A process structure provided as a seventh parameter is a copy of a parent process one(i.e. a forked one). A vnode provided as a second parameter should be used to query for an executable path. The context is the parent one as is shown by the ```currentstack``` command and a code commentary for ```exec_mach_imgact```

```
	/*
	 * We are being called to activate an image subsequent to a vfork()
	 * operation; in this case, we know that our task, thread, and
	 * uthread are actually those of our parent, and our proc, which we
	 * obtained indirectly from the image_params vfs_context_t, is the
	 * new child process.
	 */
     if (vfexec) {
		imgp->ip_new_thread = fork_create_child(task, NULL, p, FALSE, (imgp->ip_flags & IMGPF_IS_64BIT), FALSE);
		/* task and thread ref returned, will be released in __mac_execve */
		if (imgp->ip_new_thread == NULL) {
			error = ENOMEM;
			goto bad;
		}
	}
```

```
(lldb) bt
* thread #3, name = '0xffffff804129e758', queue = '0x0', stop reason = breakpoint 1.1
    frame #0: 0xffffff802d0b3814 kernel.development`mac_cred_check_label_update_execve [inlined] vfs_context_ucred(ctx=0xffffff90df09bd60) at kpi_vfs.c:1369 [opt]
    frame #1: 0xffffff802d0b3814 kernel.development`mac_cred_check_label_update_execve(ctx=0xffffff90df09bd60, vp=0xffffff803a2386c8, offset=0, scriptvp=0x0000000000000000, scriptvnodelabel=0x0000000000000000, execlabel=0x0000000000000000, p=<unavailable>, macextensions=<unavailable>) at mac_vfs.c:733 [opt]
    frame #2: 0xffffff802ce6dfb3 kernel.development`exec_mach_imgact [inlined] exec_handle_sugid(imgp=0xffffff803a665a80) at kern_exec.c:4563 [opt]
    frame #3: 0xffffff802ce6df4e kernel.development`exec_mach_imgact(imgp=<unavailable>) at kern_exec.c:1040 [opt]
    frame #4: 0xffffff802ce73b01 kernel.development`exec_activate_image(imgp=0xffffff803a665a80) at kern_exec.c:1477 [opt]
    frame #5: 0xffffff802ce7288b kernel.development`posix_spawn(ap=0xffffff8041d4b920, uap=<unavailable>, retval=0xffffff804198d040) at kern_exec.c:2797 [opt]
    frame #6: 0xffffff802cfa69fa kernel.development`unix_syscall64(state=<unavailable>) at systemcalls.c:382 [opt]
    frame #7: 0xffffff802c9229d6 kernel.development`hndl_unix_scall64 + 22
    
(lldb) f 4
frame #4: 0xffffff802ce73b01 kernel.development`exec_activate_image(imgp=0xffffff803a665a80) at kern_exec.c:1477 [opt]
(lldb) p imgp->ip_vfs_context->vc_thread->task->bsd_info
(void *) $22 = 0xffffff8041d4b920
(lldb) p *(struct proc*)0xffffff8041d4b920
(struct proc) $23 = {
  p_list = {
    le_next = 0xffffff8043c19b60
    le_prev = 0xffffff802d45df60
  }
  p_pid = 610
  task = 0xffffff803e88d1b8
  p_pptr = 0xffffff8039acd920
  p_ppid = 1
  p_pgrpid = 1
  p_uid = 0
  p_gid = 0
  p_ruid = 0
  p_rgid = 0
  p_svuid = 0
  p_svgid = 0
  p_uniqueid = 610
  p_puniqueid = 1
  p_mlock = {
    opaque = ([0] = 0, [1] = 18446744069414584320)
  }
  p_stat = '\x02'
  p_shutdownstate = '\0'
  p_kdebug = '\0'
  p_btrace = '\0'
  p_pglist = {
    le_next = 0x0000000000000000
    le_prev = 0xffffff8039acd990
  }
  p_sibling = {
    le_next = 0xffffff8043c19b60
    le_prev = 0xffffff8039acd9b0
  }
  p_children = (lh_first = 0x0000000000000000)
  p_uthlist = {
    tqh_first = 0xffffff803d177000
    tqh_last = 0xffffff803d177168
  }
  p_hash = {
    le_next = 0xffffff803a4bc6d0
    le_prev = 0xffffff8039658b10
  }
  p_evlist = {
    tqh_first = 0x0000000000000000
    tqh_last = 0xffffff8041d4b9d8
  }
  p_fdmlock = {
    opaque = ([0] = 0, [1] = 18446744069414584320)
  }
  p_ucred_mlock = {
    opaque = ([0] = 0, [1] = 18446744069414584320)
  }
  p_ucred = 0xffffff803967b950
  p_fd = 0xffffff8043d9c908
  p_stats = 0xffffff8042f4a548
  p_limit = 0xffffff802d45d0c8
  p_sigacts = 0xffffff80449845a0
  p_siglist = 0
  p_slock = {
    opaque = ([0] = 0, [1] = 0, [2] = 0, [3] = 0, [4] = 0, [5] = 0, [6] = 0, [7] = 0, [8] = 0, [9] = 0)
  }
  p_olimit = 0x0000000000000000
  p_flag = 4
  p_lflag = 9437248
  p_listflag = 196608
  p_ladvflag = 0
  p_refcount = 0
  p_childrencnt = 0
  p_parentref = 0
  p_oppid = 0
  p_xstat = 0
  p_xhighbits = '\0'
  p_realtimer = {
    it_interval = (tv_sec = 0, tv_usec = 0)
    it_value = (tv_sec = 0, tv_usec = 0)
  }
  p_rtime = (tv_sec = 0, tv_usec = 0)
  p_vtimer_user = {
    it_interval = (tv_sec = 0, tv_usec = 0)
    it_value = (tv_sec = 0, tv_usec = 0)
  }
  p_vtimer_prof = {
    it_interval = (tv_sec = 0, tv_usec = 0)
    it_value = (tv_sec = 0, tv_usec = 0)
  }
  p_rlim_cpu = (tv_sec = 0, tv_usec = 0)
  p_debugger = 0
  sigwait = 0
  sigwait_thread = 0x0000000000000000
  exit_thread = 0x0000000000000000
  p_vforkcnt = 0
  p_vforkact = 0x0000000000000000
  p_fpdrainwait = 0
  p_contproc = 0
  si_pid = 0
  si_status = 0
  si_code = 0
  si_uid = 0
  vm_shm = 0x0000000000000000
  p_dtrace_argv = 0
  p_dtrace_envp = 0
  p_dtrace_sprlock = {
    opaque = ([0] = 0, [1] = 18446744069414584320)
  }
  p_dtrace_probes = 0
  p_dtrace_count = 0
  p_dtrace_stop = '\0'
  p_dtrace_ptss_pages = 0x0000000000000000
  p_dtrace_ptss_free_list = 0x0000000000000000
  p_dtrace_helpers = 0x0000000000000000
  p_dtrace_lazy_dofs = 0x0000000000000000
  p_argslen = 336
  p_argc = 1
  user_stack = 140732910862336
  p_textvp = 0xffffff8039a11360
  p_textoff = 0
  p_sigmask = 0
  p_sigignore = 2147151879
  p_sigcatch = 0
  p_priority = '\x11'
  p_resv0 = '\0'
  p_nice = '\0'
  p_resv1 = '\0'
  p_comm = {
    [0] = 'l'
    [1] = 'a'
    [2] = 'u'
    [3] = 'n'
    [4] = 'c'
    [5] = 'h'
    [6] = 'd'
    [7] = '\0'
    [8] = 'a'
    [9] = 's'
    [10] = 'k'
    [11] = '\0'
    [12] = '\0'
    [13] = '\0'
    [14] = '\0'
    [15] = '\0'
    [16] = '\0'
  }
  p_name = {
    [0] = 'l'
    [1] = 'a'
    [2] = 'u'
    [3] = 'n'
    [4] = 'c'
    [5] = 'h'
    [6] = 'd'
    [7] = '\0'
    [8] = 'a'
    [9] = 's'
    [10] = 'k'
    [11] = '\0'
    [12] = '\0'
    [13] = '\0'
    [14] = '\0'
    [15] = '\0'
    [16] = '\0'
    [17] = '\0'
    [18] = '\0'
    [19] = '\0'
    [20] = '\0'
    [21] = '\0'
    [22] = '\0'
    [23] = '\0'
    [24] = '\0'
    [25] = '\0'
    [26] = '\0'
    [27] = '\0'
    [28] = '\0'
    [29] = '\0'
    [30] = '\0'
    [31] = '\0'
    [32] = '\0'
  }
  p_pgrp = 0xffffff8039c5e200
  p_csflags = 570444289
  p_pcaction = 0
  p_uuid = {
    [0] = '\x1c'
    [1] = '?'
    [2] = '@'
    [3] = '1'
    [4] = '|'
    [5] = '@'
    [6] = '9'
    [7] = '['
    [8] = '?'
    [9] = '?'
    [10] = '\x9d'
    [11] = '\x04'
    [12] = '\x88'
    [13] = 'U'
    [14] = '\x1f'
    [15] = '?'
  }
  p_cputype = 16777223
  p_cpusubtype = -2147483645
  p_aio_total_count = 0
  p_aio_active_count = 0
  p_aio_activeq = {
    tqh_first = 0x0000000000000000
    tqh_last = 0xffffff8041d4bc68
  }
  p_aio_doneq = {
    tqh_first = 0x0000000000000000
    tqh_last = 0xffffff8041d4bc78
  }
  p_klist = {
    slh_first = 0x0000000000000000
  }
  p_ru = 0x0000000000000000
  p_sigwaitcnt = 0
  p_signalholder = 0xffffff804129e758
  p_transholder = 0xffffff804129e758
  p_acflag = 1
  p_vfs_iopolicy = 0
  p_threadstart = 140735004109804
  p_wqthread = 140735004109788
  p_pthsize = 8192
  p_pth_tsd_offset = 224
  p_stack_addr_hint = 0
  p_wqptr = 0x0000000000000000
  p_start = (tv_sec = 1532260772, tv_usec = 673905)
  p_rcall = 0xffffff8043692340
  p_ractive = 0
  p_idversion = 609
  p_pthhash = 0xffffff8042f37000
  was_throttled = 0
  did_throttle = 0
  p_dispatchqueue_offset = 160
  p_dispatchqueue_serialno_offset = 64
  p_return_to_kernel_offset = 40
  p_mach_thread_self_offset = 24
  vm_pressure_last_notify_tstamp = (tv_sec = 0, tv_usec = 0)
  p_memstat_list = {
    tqe_next = 0x0000000000000000
    tqe_prev = 0xffffff8042f46668
  }
  p_memstat_state = 0
  p_memstat_effectivepriority = 18
  p_memstat_requestedpriority = 18
  p_memstat_dirty = 0
  p_memstat_userdata = 0
  p_memstat_idledeadline = 0
  p_memstat_idle_start = 0
  p_memstat_idle_delta = 0
  p_memstat_memlimit = 0
  p_memstat_memlimit_active = 0
  p_memstat_memlimit_inactive = 0
  p_responsible_pid = 610
  p_user_faults = 0
  p_exit_reason = 0x0000000000000000
  p_user_data = 0
}

(lldb) p *imgp
(image_params) $25 = {
  ip_user_fname = 4305099856
  ip_user_argv = 123145307842128
  ip_user_envv = 140209404522800
  ip_seg = 8
  ip_vp = 0xffffff803a2386c8
  ip_vattr = 0xffffff803a665d98
  ip_origvattr = 0xffffff803a665f38
  ip_origcputype = 16777223
  ip_origcpusubtype = -2147483645
  ip_vdata = 0xffffff80c1d47000 "\xffffffcf\xfffffffa\xffffffed\xfffffffe\a"
  ip_flags = 28
  ip_argc = 2
  ip_envc = 1
  ip_applec = 0
  ip_startargv = 0xffffff80c1d06028 "xpcproxy"
  ip_endargv = 0xffffff80c1d06040 "XPC_FLAGS=0x0"
  ip_endenvv = 0xffffff80c1d06050 <no value available>
  ip_strings = 0xffffff80c1d06000 "executable_path=/usr/libexec/xpcproxy"
  ip_strendp = 0xffffff80c1d06050 <no value available>
  ip_argspace = 262064
  ip_strspace = 266160
  ip_arch_offset = 0
  ip_arch_size = 43488
  ip_interp_buffer = {
    [0] = '\0'
    [1] = '\0'
    [2] = '\0'
    [3] = '\0'
    [4] = '\0'
    [5] = '\0'
    ...
  }
  ip_interp_sugid_fd = 0
  ip_vfs_context = 0xffffff90df09bd60
  ip_ndp = 0xffffff8042e2f600
  ip_new_thread = 0xffffff803b45e420
  ip_execlabelp = 0x0000000000000000
  ip_scriptlabelp = 0x0000000000000000
  ip_scriptvp = 0x0000000000000000
  ip_csflags = 570441729
  ip_mac_return = 0
  ip_px_sa = 0xffffff90df09bd88
  ip_px_sfa = 0xffffff804391d000
  ip_px_spa = 0x0000000000000000
  ip_px_smpx = 0x0000000000000000
  ip_px_persona = 0x0000000000000000
  ip_cs_error = 0x0000000000000000
  ip_dyld_fsid = 16777222
  ip_dyld_fsobjid = 5354643
}
(lldb) showvnodepath 0xffffff803a2386c8
/usr/libexec/xpcproxy

(lldb) showalltasks
task                 vm_map               ipc_space            #acts flags    pid       process             io_policy  wq_state  command             
0xffffff803901d5e8   0xffffff80310bc820   0xffffff8038476de0     152            0   0xffffff802d45c998                -1 -1 -1    kernel_task         
0xffffff803901d000   0xffffff8039ae5300   0xffffff8038476ea0       4            1   0xffffff8039acd920                -1 -1 -1    launchd             
0xffffff803a501000   0xffffff8039ae5b00   0xffffff8038476420       4 D         45   0xffffff8039acd490                -1 -1 -1    UserEventAgent      
0xffffff803a502d88   0xffffff803a6f8b00   0xffffff8038477bc0       3 D         48   0xffffff8039acd000             TQ -1 -1 -1    uninstalld          
0xffffff803a503370   0xffffff803a2fbb00   0xffffff80384763c0       2 D         47   0xffffff8039ace240                -1 -1 -1    wifiFirmwareLoad    
0xffffff803a503958   0xffffff803a55b000   0xffffff8038476360       2 D         49   0xffffff8039ace6d0                -1 -1 -1    kextd               
0xffffff803a557d88   0xffffff803a55be00   0xffffff8038477c20       9 D         50   0xffffff8039aceb60                -1 -1 -1    fseventsd           
0xffffff803a5577a0   0xffffff8039ae5f00   0xffffff8038476300       9 D         57   0xffffff803a2a6b60                -1 -1 -1    configd             
0xffffff803a5571b8   0xffffff803a7d5800   0xffffff80384762a0       3 D         58   0xffffff803a3046d0                -1 -1 -1    powerd              
0xffffff803a558958   0xffffff803a55b100   0xffffff8038476240       2 D         60   0xffffff803a303db0                -1 -1 -1    keybagd             
0xffffff803a5565e8   0xffffff803a55b600   0xffffff8038477d40       5 D         61   0xffffff803a304b60                -1 -1 -1    logd                
0xffffff803a5ab1b8   0xffffff803a2fb600   0xffffff8038477da0       4 D         64   0xffffff803a303000                -1 -1 -1    warmd               
0xffffff803a5abd88   0xffffff803a7d5000   0xffffff8038476120      12 D         65   0xffffff803a352db0         RAGE P -1 -1 -1    mds                 
0xffffff803a5ac370   0xffffff803a610e00   0xffffff8038477ec0       2 D         72   0xffffff803a3afdb0                -1 -1 -1    diskarbitrationd    
0xffffff803a623370   0xffffff803a55b900   0xffffff8038476c00       9 D         78   0xffffff803a3af000                -1 -1 -1    opendirectoryd      
0xffffff803a6221b8   0xffffff803a7d5500   0xffffff8038476000       4 D         94   0xffffff803a4bc240              Q -1 -1 -1    loginwindow         
0xffffff803a621bd0   0xffffff803a2fb700   0xffffff803a65d020       8 D         84   0xffffff803a411000                -1 -1 -1    securityd           
0xffffff803a623958   0xffffff8039ae5400   0xffffff803a65cf60       2 D         88   0xffffff803a466db0             TQ -1 -1 -1    autofsd             
0xffffff803a621000   0xffffff803a55b700   0xffffff803a65cf00       3 D         87   0xffffff803a466920             TQ -1 -1 -1    displaypolicyd      
0xffffff803a695d88   0xffffff803a7d5400   0xffffff803a65d080       3 D         95   0xffffff803a4bb920                -1 -1 -1    logind              
0xffffff803a6951b8   0xffffff803a2fb100   0xffffff803a65cde0       3 D         96   0xffffff803a4bb490                -1 -1 -1    KernelEventAgent    
0xffffff803a694bd0   0xffffff803a2fb400   0xffffff803a65d0e0       3 D         98   0xffffff803a4bc6d0                -1 -1 -1    bluetoothd          
0xffffff803a696958   0xffffff8039ae5000   0xffffff803a65cd20       2 D        100   0xffffff803a51c240                -1 -1 -1    corebrightnessd     
0xffffff803a6945e8   0xffffff8039ae5800   0xffffff803a65ccc0       2 D        101   0xffffff803a51bdb0                -1 -1 -1    AirPlayXPCHelper    
0xffffff803a694000   0xffffff803a3c9d00   0xffffff803a65cc60       2 R         52   0xffffff803a2a5000            BLT -1 -1 -1    mediaremoted        
0xffffff803a363370   0xffffff803a2fb300   0xffffff803a65d140       4 R         44   0xffffff8039acddb0           BLTP -1 -1 -1    syslogd             
0xffffff803a6f0d88   0xffffff803a2fb800   0xffffff803a65cc00       3 R         54   0xffffff803a2a5db0            BLT -1 -1 -1    systemstats         
0xffffff803a6f07a0   0xffffff803a7d5b00   0xffffff803a65cba0       2           53   0xffffff803a2a5920       RAGE BLT -1 -1 -1    MRT                 
0xffffff803a6f01b8   0xffffff803a7d5200   0xffffff803a65d1a0       2 R         70   0xffffff803a3536d0            BLT -1 -1 -1    iconservicesagen    
0xffffff803a6efbd0   0xffffff803a55bf00   0xffffff803a65d200       2           68   0xffffff803a353240            BLT -1 -1 -1    firmwaresyncd       
0xffffff803a6f1370   0xffffff8039ae5a00   0xffffff803a65d260       2 R         74   0xffffff803a3af920            BLT -1 -1 -1    coreduetd           
0xffffff803a6ef000   0xffffff803a7d5a00   0xffffff803a65cae0       3           75   0xffffff803a3b06d0            BLT -1 -1 -1    backupd-helper      
0xffffff803a6f1958   0xffffff803a7d5300   0xffffff803a65ca80       4 R         82   0xffffff803a411490            BLT -1 -1 -1    launchservicesd     
0xffffff803a7401b8   0xffffff8039ae5c00   0xffffff803a65ca20       2 R         91   0xffffff803a4676d0            BLT -1 -1 -1    dasd                
0xffffff803a7407a0   0xffffff803a6f8100   0xffffff803a65d320       2           81   0xffffff803a412240            BLT -1 -1 -1    nbstated            
0xffffff803a740d88   0xffffff803a3c9400   0xffffff803a65d380       3 R         93   0xffffff803a4bbdb0            BLT -1 -1 -1    revisiond           
0xffffff803a73f000   0xffffff803a2fb000   0xffffff803a65c960       3 D        102   0xffffff803a51c6d0                -1 -1 -1    notifyd             
0xffffff803a741958   0xffffff803a55bd00   0xffffff803a65c900       2 D         80   0xffffff803a411db0                -1 -1 -1    timed               
0xffffff803a49b7a0   0xffffff803a55b300   0xffffff803a65c8a0       5 D         99   0xffffff803a4bcb60              Q -1 -1 -1    hidd                
0xffffff803a7cf7a0   0xffffff803a7d5700   0xffffff803a65c840       3 D         83   0xffffff803a4126d0                -1 -1 -1    usbmuxd             
0xffffff803a7cfd88   0xffffff803a7d5e00   0xffffff803a65d440       3 R         56   0xffffff803a2a66d0            BLT -1 -1 -1    appleeventsd        
0xffffff803a7cf1b8   0xffffff803a55bc00   0xffffff803a65c7e0       7 R         85   0xffffff803a412b60            BLT -1 -1 -1    locationd           
0xffffff803a7cebd0   0xffffff803a7d5100   0xffffff803a65c780       2 R         71   0xffffff803a353b60            BLT -1 -1 -1    iconservicesd       
0xffffff803a7ce5e8   0xffffff803a6f8800   0xffffff803a65c6c0       3 R        103   0xffffff803a51cb60            BLT -1 -1 -1    authd               
0xffffff803a7d0958   0xffffff803a55b400   0xffffff803a65c660       4 D        104   0xffffff803a51b920                -1 -1 -1    coreservicesd       
0xffffff803a43ebd0   0xffffff803a2fbf00   0xffffff803a65c720       6 R        107   0xffffff803a3b0240            BLT -1 -1 -1    cfprefsd            
0xffffff803a43fd88   0xffffff803a2fb200   0xffffff803a65d4a0       2 D        108   0xffffff803a466490              T -1 -1 -1    aslmanager          
0xffffff803a29c5e8   0xffffff803a610a00   0xffffff803a65d3e0       2 D        106   0xffffff803a51b000                -1 -1 -1    distnoted           
0xffffff803a29cbd0   0xffffff803a610400   0xffffff803a65cea0       6 RB       133   0xffffff803b628490                -1 -1 -1    airportd            
0xffffff803a29d1b8   0xffffff803a610d00   0xffffff803a65d560       6 D        135   0xffffff803b629b60                -1 -1 -1    coreaudiod          
0xffffff803a29d7a0   0xffffff803a610500   0xffffff803a65cb40       3 R        136   0xffffff803b628000            BLT -1 -1 -1    nehelper            
0xffffff803a29c000   0xffffff803a610900   0xffffff803a65c600       8 D        138   0xffffff803a467240              Q -1 -1 -1    WindowServer        
0xffffff803a361bd0   0xffffff803a610600   0xffffff803a65d680       2 R        140   0xffffff803a3b0b60            BLT -1 -1 -1    com.apple.audio.    
0xffffff803a361000   0xffffff803a610300   0xffffff803a65d6e0       2 R        142   0xffffff803a304240            BLT -1 -1 -1    com.apple.ctkpcs    
0xffffff803a362d88   0xffffff803a610800   0xffffff803a65c5a0       7 RB       143   0xffffff803a352920                -1 -1 -1    trustd              
0xffffff803a3615e8   0xffffff803a610f00   0xffffff803a65d2c0       2 D        145   0xffffff803a51b490             TQ -1 -1 -1    kdc                 
0xffffff803a49a000   0xffffff803a3c9e00   0xffffff803a65d740       2 R        146   0xffffff803a466000            BLT -1 -1 -1    ctkd                
0xffffff803901e1b8   0xffffff803a3c9800   0xffffff803a65d8c0      16 D        154   0xffffff803bee9240             TQ -1 -1 -1    rpcsvchost          
0xffffff803901f370   0xffffff803a3c9600   0xffffff803a65d5c0       2 D        156   0xffffff803bee96d0                -1 -1 -1    ocspd               
0xffffff803a7d0370   0xffffff803a6f8000   0xffffff803a65d7a0       3 D        171   0xffffff803bee8490                -1 -1 -1    mDNSResponder       
0xffffff803a43f1b8   0xffffff803a6f8f00   0xffffff803a65cd80       3 D        175   0xffffff803bee8920                -1 -1 -1    mDNSResponderHel    
0xffffff803a440370   0xffffff803a6f8300   0xffffff803a65d920       2 R        177   0xffffff803bee9b60            BLT -1 -1 -1    mobileassetd        
0xffffff803a29e958   0xffffff803a6f8600   0xffffff803a65d800       3 RB       178   0xffffff803bee8000                -1 -1 -1    nsurlsessiond       
0xffffff803a29dd88   0xffffff803a6f8900   0xffffff803a65d860       2 R        179   0xffffff803b628db0            BLT -1 -1 -1    syspolicyd          
0xffffff803a49c958   0xffffff803a6f8700   0xffffff803a65d980       2 D        182   0xffffff803a4bb000             TQ -1 -1 -1    apfsd               
0xffffff803901e7a0   0xffffff8039ae5700   0xffffff803a65da40       2 R        185   0xffffff803a303920            BLT -1 -1 -1    com.apple.ifdrea    
0xffffff803901ed88   0xffffff8039ae5100   0xffffff803a65c540       2          186   0xffffff803a2a6240            BLT -1 -1 -1    ethcheck            
0xffffff803901f958   0xffffff8039ae5200   0xffffff803a65c420       2          183   0xffffff803a352490            BLT -1 -1 -1    usbd                
0xffffff803a5015e8   0xffffff803a55ba00   0xffffff803a65d9e0       3 D        189   0xffffff803cceadb0                -1 -1 -1    watchdogd           
0xffffff803a501bd0   0xffffff803a7d5900   0xffffff803a65db00       4 R        191   0xffffff803ccea490            BLT -1 -1 -1    symptomsd           
0xffffff803a73f5e8   0xffffff803a2fb500   0xffffff803a65c480       2 R        193   0xffffff803ccea920            BLT -1 -1 -1    awdd                
0xffffff803a73fbd0   0xffffff803a2fb900   0xffffff803a65d500       2 R        194   0xffffff803cceb240            BLT -1 -1 -1    analyticsd          
0xffffff803a558370   0xffffff803a610c00   0xffffff803a65dbc0       6 D        196   0xffffff803cceb6d0                -1 -1 -1    sandboxd            
0xffffff803a6957a0   0xffffff803a610b00   0xffffff803a65dc20       2 R        198   0xffffff803ccebb60            BLT -1 -1 -1    coresymbolicatio    
0xffffff803a49b1b8   0xffffff803a3c9700   0xffffff803a65c360       2 D        204   0xffffff803a467b60      RAGE BLTQ -1 -1 -1    backupd             
0xffffff803a49c370   0xffffff803a3c9900   0xffffff803a65c4e0       2 RB       205   0xffffff803a2a5490                -1 -1 -1    lsd                 
0xffffff803a556000   0xffffff803a3c9500   0xffffff803a65c3c0       3 R        213   0xffffff803d3a26d0            BLT -1 -1 -1    signpost_notific    
0xffffff803a696370   0xffffff803a6f8400   0xffffff803a65dc80       2 R        214   0xffffff803d3a2b60            BLT -1 -1 -1    secinitd            
0xffffff803a3621b8   0xffffff803a6f8200   0xffffff803a65c1e0       2 R        215   0xffffff803d3a1490            BLT -1 -1 -1    sysmond             
0xffffff803a3627a0   0xffffff803a6f8a00   0xffffff803a65c240       2          216   0xffffff803d3a1000            BLT -1 -1 -1    systemstats         
0xffffff803a440958   0xffffff803a3c9000   0xffffff803a65de60       2 D        221   0xffffff803d5e8920             TQ -1 -1 -1    periodic-wrapper    
0xffffff803a556bd0   0xffffff803a3c9f00   0xffffff803a65dec0       3 D        222   0xffffff803d5e9240             TQ -1 -1 -1    periodic-wrapper    
0xffffff803a7ce000   0xffffff803a6f8e00   0xffffff803a65df80       8 R        223   0xffffff803d5e8490            BLT -1 -1 -1    softwareupdated     
0xffffff803a49abd0   0xffffff803a2fbd00   0xffffff803a65d620       1 D        227   0xffffff803d3a1db0             TQ -1 -1 -1    sh                  
0xffffff803a43f7a0   0xffffff803a55b800   0xffffff803a65c120       2 D        230   0xffffff803b628920                -1 -1 -1    CVMServer           
0xffffff803a622d88   0xffffff803a7d5600   0xffffff803a65dda0       2 R        231   0xffffff803b629240            BLT -1 -1 -1    colorsync.displa    
0xffffff803a307958   0xffffff803a6f8500   0xffffff803a65c000       3 R        237   0xffffff803d946db0            BLT -1 -1 -1    colorsyncd          
0xffffff803a307370   0xffffff803a6f8c00   0xffffff8038477e00       2 R        239   0xffffff803d9476d0            BLT -1 -1 -1    com.apple.CodeSi    
0xffffff803a5aabd0   0xffffff803a7d5f00   0xffffff8038477b60       2 R        242   0xffffff803d946000            BLT -1 -1 -1    suhelperd           
0xffffff803a3cc000   0xffffff803a498400   0xffffff8038476540       2 R        256   0xffffff803de3bb60            BLT -1 -1 -1    AudioComponentRe    
0xffffff803a3cd7a0   0xffffff803a498200   0xffffff80384761e0       2 R        260   0xffffff803de72490            BLT -1 -1 -1    com.apple.audio.    
0xffffff803df13d88   0xffffff803a498b00   0xffffff80384766c0       4 R        270   0xffffff803de3a490            BLT -1 -1 -1    captiveagent        
0xffffff803df12000   0xffffff803a498900   0xffffff80384779e0       4 D        271   0xffffff803de3a000           BLTQ -1 -1 -1    netbiosd            
0xffffff803a6ef5e8   0xffffff803e030400   0xffffff8038477140       2 D        275   0xffffff803e015920              Q -1 -1 -1    spindump            
0xffffff803a6215e8   0xffffff803a7d5c00   0xffffff8038476fc0       1 D        280   0xffffff803e015db0             TQ -1 -1 -1    sh                  
0xffffff803df12bd0   0xffffff803a498f00   0xffffff8038476960       1 D        281   0xffffff803e015490             TQ -1 -1 -1    sh                  
0xffffff803e02c1b8   0xffffff803e030800   0xffffff8038476480       1 D        283   0xffffff803e016240             TQ -1 -1 -1    cat                 
0xffffff803e02bbd0   0xffffff803e030600   0xffffff80384778c0       1 D        284   0xffffff803e015000             TQ -1 -1 -1    sh                  
0xffffff803e02c7a0   0xffffff803e030200   0xffffff80384764e0       1 D        285   0xffffff803e0166d0             TQ -1 -1 -1    man                 
0xffffff803df14370   0xffffff803e030c00   0xffffff8038476f00       2 R        297   0xffffff803de73b60            BLT -1 -1 -1    nsurlstoraged       
0xffffff803df137a0   0xffffff803e030a00   0xffffff8038477200       2          298   0xffffff803de736d0            BLT -1 -1 -1    SubmitDiagInfo      
0xffffff803df131b8   0xffffff803e030e00   0xffffff8038477920       4 R        299   0xffffff803de3adb0            BLT -1 -1 -1    com.apple.geod      
0xffffff803e02b5e8   0xffffff803e030f00   0xffffff8038476b40       2 R        300   0xffffff803e016b60            BLT -1 -1 -1    secinitd            
0xffffff803e02cd88   0xffffff803a3c9c00   0xffffff8038477560       2 R        301   0xffffff803d5e9b60            BLT -1 -1 -1    cfprefsd            
0xffffff803e02d370   0xffffff803a3c9a00   0xffffff8038477320       3 R        302   0xffffff803d3a2240            BLT -1 -1 -1    trustd              
0xffffff803a5ac958   0xffffff803a498100   0xffffff8038477f20      11 D        308   0xffffff803de73240                -1 -1 -1    mds_stores          
0xffffff803a29e370   0xffffff803a498e00   0xffffff8038477f80       2 D        309   0xffffff803e4acdb0             TQ -1 -1 -1    bootinstalld        
0xffffff803a5027a0   0xffffff803a498300   0xffffff80384765a0       2 R        310   0xffffff803e4ad240            BLT -1 -1 -1    sharedfilelistd     
0xffffff803a3ce370   0xffffff803a498a00   0xffffff8038477620       2 R        311   0xffffff803e4ac920            BLT -1 -1 -1    storeaccountd       
0xffffff803e88dd88   0xffffff803a3c9300   0xffffff8038476cc0       3 R        326   0xffffff803e881490           BLTQ -1 -1 -1    commerced           
0xffffff803a3ccbd0   0xffffff803a3c9200   0xffffff8038476660       2 R        338   0xffffff803e8826d0           BLTQ -1 -1 -1    com.apple.Perfor    
0xffffff803a5aa5e8   0xffffff803a498700   0xffffff8038476840       2 R        339   0xffffff803e882b60            BLT -1 -1 -1    ctkahp              
0xffffff803a6227a0   0xffffff803a498d00   0xffffff8038477500       2 D        343   0xffffff803bee8db0                -1 -1 -1    systemsoundserve    
0xffffff803e88d7a0   0xffffff803e90c900   0xffffff8038477aa0       2 R        344   0xffffff803ebfcdb0            BLT -1 -1 -1    tccd                
0xffffff803ec3ebd0   0xffffff803e90cb00   0xffffff8038476a20       2 R        345   0xffffff803ebfd240            BLT -1 -1 -1    com.apple.Accoun    
0xffffff803ec3f1b8   0xffffff803e90c700   0xffffff8038476600       2 D        346   0xffffff803ebfc920                -1 -1 -1    distnoted           
0xffffff803ec3e5e8   0xffffff803e90c000   0xffffff8038477260       1 D        347   0xffffff803ebfc490             TQ -1 -1 -1    xcodebuild          
0xffffff803ec40370   0xffffff803e90c400   0xffffff80384768a0       2 R        349   0xffffff803ebfd6d0            BLT -1 -1 -1    coreauthd           
0xffffff803ee337a0   0xffffff803e90c800   0xffffff80384775c0       3 R        359   0xffffff803edbcb60            BLT -1 -1 -1    cupsd               
0xffffff803ee325e8   0xffffff803e90ce00   0xffffff8038476f60       3 D        379   0xffffff803edbbdb0             TQ -1 -1 -1    GSSCred             
0xffffff803ee34370   0xffffff803e90c500   0xffffff8038476180       2 D        381   0xffffff803edbb920             TQ -1 -1 -1    securityd_servic    
0xffffff803ee34958   0xffffff803e90cd00   0xffffff8038477080       3 RB       382   0xffffff803edbb490                -1 -1 -1    com.apple.Ambien    
0xffffff803ee32000   0xffffff803e90cf00   0xffffff80384771a0       7 R        383   0xffffff803edbb000            BLT -1 -1 -1    cfprefsd            
0xffffff803e88c000   0xffffff803e030d00   0xffffff8038477440       3 D        386   0xffffff803e882240                -1 -1 -1    distnoted           
0xffffff803a5ab7a0   0xffffff803e030500   0xffffff8038477c80       3 D        387   0xffffff803a352000              Q -1 -1 -1    universalaccessd    
0xffffff803a5aa000   0xffffff803a7d5d00   0xffffff8038477800       4 D        384   0xffffff803e881000                -1 -1 -1    UserEventAgent      
0xffffff803ec40958   0xffffff803a498500   0xffffff80384760c0       9 D        388   0xffffff803ebfdb60                -1 -1 -1    CommCenter          
0xffffff803ec3e000   0xffffff803a2fbc00   0xffffff8038477ce0       3 R        389   0xffffff803ebfc000            BLT -1 -1 -1    lsd                 
0xffffff803a741370   0xffffff803a610100   0xffffff8038477740       7 R        390   0xffffff803de3b6d0            BLT -1 -1 -1    trustd              
0xffffff803a3cdd88   0xffffff803a610000   0xffffff8038477680       7 D        391   0xffffff803de3a920                -1 -1 -1    secd                
0xffffff803a3cd1b8   0xffffff803e90c100   0xffffff80384776e0      14 RB       392   0xffffff803de3b240                -1 -1 -1    cloudd              
0xffffff803a3ce958   0xffffff803e90cc00   0xffffff80384773e0       2 R        393   0xffffff803ccea000            BLT -1 -1 -1    CloudKeychainPro    
0xffffff803a305000   0xffffff803a55b500   0xffffff80384767e0       2 D        395   0xffffff803d3a1920                -1 -1 -1    WirelessRadioMan    
0xffffff803a3067a0   0xffffff803e90c600   0xffffff8038477e60       2 R        394   0xffffff803a411920            BLT -1 -1 -1    callservicesd       
0xffffff803a363958   0xffffff8039ae5900   0xffffff80384774a0       2 R        396   0xffffff803de72db0            BLT -1 -1 -1    syncdefaultsd       
0xffffff803e02d958   0xffffff803e030b00   0xffffff8038476720       2 R        397   0xffffff803de72920            BLT -1 -1 -1    accountsd           
0xffffff803ee33d88   0xffffff803e030900   0xffffff8038476060       6 D        398   0xffffff803de72000                -1 -1 -1    identityservices    
0xffffff803a43e5e8   0xffffff803a55b200   0xffffff8038476a80       2 R        399   0xffffff803a303490            BLT -1 -1 -1    tccd                
0xffffff803e88e370   0xffffff803a2fbe00   0xffffff8038476780       6 R        400   0xffffff803d946490            BLT -1 -1 -1    nsurlsessiond       
0xffffff803e88c5e8   0xffffff8039ae5e00   0xffffff8038476900       2 D        401   0xffffff803d946920                -1 -1 -1    imagent             
0xffffff803ec3f7a0   0xffffff803a6f8d00   0xffffff8038477380       5 D        402   0xffffff803d947240                -1 -1 -1    apsd                
0xffffff803a3cc5e8   0xffffff803a498000   0xffffff80384770e0       3 R        403   0xffffff803d947b60            BLT -1 -1 -1    IMDPersistenceAg    
0xffffff803a3055e8   0xffffff803a498600   0xffffff80384777a0       6 D        404   0xffffff803d5e8db0              Q -1 -1 -1    Terminal            
0xffffff803a305bd0   0xffffff803a498c00   0xffffff8038477a40       3 R        405   0xffffff803d5e8000            BLT -1 -1 -1    sharedfilelistd     
0xffffff803a306d88   0xffffff803a55bb00   0xffffff80384769c0       2 D        406   0xffffff803d5e96d0                -1 -1 -1    rapportd            
0xffffff803e88cbd0   0xffffff803a610200   0xffffff803a65cfc0       3 R        407   0xffffff803e4ac000            BLT -1 -1 -1    ContactsAccounts    
0xffffff803ec3fd88   0xffffff8039ae5d00   0xffffff803a65daa0       2 R        408   0xffffff804002ab60            BLT -1 -1 -1    secinitd            
0xffffff803a5021b8   0xffffff803a2fba00   0xffffff803a65df20       2 R        409   0xffffff804002a6d0            BLT -1 -1 -1    mapspushd           
0xffffff803e88e958   0xffffff803e90c200   0xffffff803a65c300       9 DB       410   0xffffff804002a240              Q -1 -1 -1    Console             
0xffffff804013b7a0   0xffffff8040134800   0xffffff803a65de00       5 D        411   0xffffff8040029db0              Q -1 -1 -1    Dock                
0xffffff804013bd88   0xffffff8040134700   0xffffff803a65dd40       2 D        412   0xffffff8040029920              Q -1 -1 -1    talagent            
0xffffff804013c370   0xffffff8040134a00   0xffffff803a65c060       3 D        413   0xffffff8040029490              Q -1 -1 -1    SystemUIServer      
0xffffff804013abd0   0xffffff8040134600   0xffffff803a65c180       6 ND       414   0xffffff8040029000           LTQA -1 -1 -1    Finder              
0xffffff804013c958   0xffffff8040134c00   0xffffff803a65c2a0       2 D        417   0xffffff8040263db0                -1 -1 -1    pboard              
0xffffff80403f3000   0xffffff8040475b00   0xffffff80403fb380       3 R        418   0xffffff8040263920            BLT -1 -1 -1    assistantd          
0xffffff80403f5370   0xffffff8040475800   0xffffff80403fb3e0       3 R        420   0xffffff8040263490            BLT -1 -1 -1    akd                 
0xffffff80403f5958   0xffffff8040475700   0xffffff80403fb440       2 R        421   0xffffff80402646d0            BLT -1 -1 -1    suggestd            
0xffffff80404717a0   0xffffff8040475500   0xffffff80403fb140       3 RB       419   0xffffff8040264240              Q -1 -1 -1    sharingd            
0xffffff80404711b8   0xffffff8040475900   0xffffff80403fb0e0       2 R        424   0xffffff8040415240            BLT -1 -1 -1    siriknowledged      
0xffffff8040470bd0   0xffffff8040475a00   0xffffff80403fb4a0       5 R        423   0xffffff8040264b60            BLT -1 -1 -1    gamed               
0xffffff8040471d88   0xffffff8040475400   0xffffff80403fb080       2 RB       422   0xffffff8040263000                -1 -1 -1    keyboardservices    
0xffffff80404705e8   0xffffff8040475e00   0xffffff80403fb020       4 D        425   0xffffff8040414db0             TQ -1 -1 -1    SafariBookmarksS    
0xffffff8040472370   0xffffff8040475600   0xffffff80403fb200       2 R        426   0xffffff8040414920            BLT -1 -1 -1    fmfd                
0xffffff8040470000   0xffffff8040475300   0xffffff80403fb320       4 R        427   0xffffff80404156d0            BLT -1 -1 -1    nsurlstoraged       
0xffffff804013a000   0xffffff8040475200   0xffffff80403fb1a0       2 R        428   0xffffff8040415b60            BLT -1 -1 -1    cdpd                
0xffffff803a3061b8   0xffffff8040475000   0xffffff80403fb500       2 R        429   0xffffff8040414490            BLT -1 -1 -1    routined            
0xffffff80403f41b8   0xffffff8040134500   0xffffff80403fb560       2 D        430   0xffffff8040414000                -1 -1 -1    usernoted           
0xffffff80403f4d88   0xffffff8040134000   0xffffff80403fb260       3 R        431   0xffffff803e881920            BLT -1 -1 -1    fontd               
0xffffff80403f35e8   0xffffff8040134100   0xffffff80403fb5c0       3 D        432   0xffffff80408b76d0              Q -1 -1 -1    NotificationCent    
0xffffff8040472958   0xffffff8040134d00   0xffffff80403faf60       2 R        433   0xffffff80408b7240       RAGE BLT -1 -1 -1    bird                
0xffffff804013b1b8   0xffffff8040134300   0xffffff80403fb620       2 D        435   0xffffff80408b6db0                -1 -1 -1    networkservicepr    
0xffffff80403f3bd0   0xffffff8040134900   0xffffff80403faf00       7 D        436   0xffffff80408b6920                -1 -1 -1    automountd          
0xffffff8040a9b1b8   0xffffff803a610700   0xffffff80403faea0       2 R        438   0xffffff80408b6490            BLT -1 -1 -1    pkd                 
0xffffff8040a9a5e8   0xffffff8040475c00   0xffffff80403fb680       2 R        440   0xffffff8040acab60            BLT -1 -1 -1    CrashReporterSup    
0xffffff8040a9c958   0xffffff8040475100   0xffffff80403fb7a0       3 R        444   0xffffff8040aca6d0            BLT -1 -1 -1    useractivityd       
0xffffff804013a5e8   0xffffff8040134f00   0xffffff80403fad80       3 DB       445   0xffffff8040ac9db0              Q -1 -1 -1    com.apple.dock.e    
0xffffff8040a9abd0   0xffffff8040134200   0xffffff80403fb2c0       2 R        446   0xffffff8040ac9920            BLT -1 -1 -1    iconservicesagen    
0xffffff8040a9a000   0xffffff8040475d00   0xffffff80403fade0       3 R        448   0xffffff8040ac9000            BLT -1 -1 -1    com.apple.iCloud    
0xffffff803df125e8   0xffffff8040c73600   0xffffff80403fad20       2 D        449   0xffffff80408b6000                -1 -1 -1    filecoordination    
0xffffff8040da7d88   0xffffff8040c73300   0xffffff80403fb800       2 RB       450   0xffffff8040d66240                -1 -1 -1    CalendarAgent       
0xffffff8040da77a0   0xffffff8040c73900   0xffffff80403fac60       2 R        451   0xffffff8040d65db0            BLT -1 -1 -1    wirelessproxd       
0xffffff8040da71b8   0xffffff8040c73400   0xffffff80403facc0       8 DB       452   0xffffff8040d65920              Q -1 -1 -1    Spotlight           
0xffffff8040da6bd0   0xffffff8040c73800   0xffffff80403fac00       2 R        453   0xffffff8040d666d0            BLT -1 -1 -1    soagent             
0xffffff8040da65e8   0xffffff8040c73a00   0xffffff80403faba0       6 R        454   0xffffff8040d65490            BLT -1 -1 -1    com.apple.geod      
0xffffff804133b7a0   0xffffff8040c73700   0xffffff80403faa80       5 R        455   0xffffff8040d66b60            BLT -1 -1 -1    parsecd             
0xffffff804133b1b8   0xffffff8040c73200   0xffffff80403fae40       3 R        456   0xffffff8040d65000            BLT -1 -1 -1    IMRemoteURLConne    
0xffffff804133c370   0xffffff8040c73500   0xffffff80403fab40       2 R        457   0xffffff8041295240            BLT -1 -1 -1    knowledge-agent     
0xffffff804133c958   0xffffff8040c73d00   0xffffff80403fa9c0       2 R        458   0xffffff80412956d0            BLT -1 -1 -1    spindump_agent      
0xffffff804133a000   0xffffff803e030100   0xffffff80403fa960       3 D        459   0xffffff8041294db0              Q -1 -1 -1    CoreLocationAgen    
0xffffff8040da8958   0xffffff8040134e00   0xffffff80403fa900       3 R        460   0xffffff8041295b60           BLTQ -1 -1 -1    storeaccountd       
0xffffff8040da6000   0xffffff8040c73c00   0xffffff80403fb860       2 R        461   0xffffff8041294920            BLT -1 -1 -1    WiFiProxy           
0xffffff804133a5e8   0xffffff8040c73f00   0xffffff80403fb8c0       2 R        462   0xffffff8041294490            BLT -1 -1 -1    adid                
0xffffff804133abd0   0xffffff8040c73e00   0xffffff80403faa20       2 R        463   0xffffff8041294000            BLT -1 -1 -1    coreauthd           
0xffffff80417e91b8   0xffffff80417df500   0xffffff80403fa8a0       2 R        465   0xffffff80417a5db0            BLT -1 -1 -1    pbs                 
0xffffff80417e97a0   0xffffff80417df400   0xffffff80403faae0       5 R        466   0xffffff80417a5920           BLTQ -1 -1 -1    commerce            
0xffffff80417ea370   0xffffff80417df800   0xffffff80403fb9e0       2 D        467   0xffffff80417a5490                -1 -1 -1    login               
0xffffff80417e8bd0   0xffffff80417df300   0xffffff80403fa840       2 R        468   0xffffff80417a66d0            BLT -1 -1 -1    CalNCService        
0xffffff80417e85e8   0xffffff80417df200   0xffffff80403fa7e0       1 D        469   0xffffff80417a6b60                -1 -1 -1    bash                
0xffffff80417ea958   0xffffff8040c73b00   0xffffff80403fbb00       2 R        480   0xffffff8041a55000            BLT -1 -1 -1    CallHistoryPlugi    
0xffffff8041bee000   0xffffff80417df900   0xffffff80403fa660       3 R        482   0xffffff8041a55db0            BLT -1 -1 -1    ContactsAgent       
0xffffff8041bef7a0   0xffffff8040c73100   0xffffff80403fbb60       2 R        483   0xffffff8041a56240            BLT -1 -1 -1    APFSUserAgent       
0xffffff8041e0a958   0xffffff8042669900   0xffffff80403fbd40       2 R        481   0xffffff8041a55920            BLT -1 -1 -1    IMRemoteURLConne    
0xffffff8041f735e8   0xffffff8041d8bf00   0xffffff80403fa360       3 D        497   0xffffff8041e19000              Q -1 -1 -1    com.apple.CoreSi    
0xffffff8041f73000   0xffffff8041d8b400   0xffffff80403fbda0       2 R        498   0xffffff8041e1a6d0            BLT -1 -1 -1    com.apple.notifi    
0xffffff8041f741b8   0xffffff8041d8b300   0xffffff80403fbe00       2 D        499   0xffffff8041e1ab60                -1 -1 -1    login               
0xffffff8041f747a0   0xffffff8041d8b200   0xffffff80403fa300       1 D        500   0xffffff80421296d0                -1 -1 -1    bash                
0xffffff8041f75370   0xffffff8041d8b100   0xffffff80403fa240       4 D        510   0xffffff8042129240                -1 -1 -1    diskmanagementd     
0xffffff8041f75958   0xffffff80417dfc00   0xffffff80403fbe60       2 D        511   0xffffff8042128db0              Q -1 -1 -1    imklaunchagent      
0xffffff8041f74d88   0xffffff8042669a00   0xffffff80403fa1e0       2 D        512   0xffffff8042128920                -1 -1 -1    SafeEjectGPUAgen    
0xffffff80426bd7a0   0xffffff8042669800   0xffffff80403fbec0       2 R        513   0xffffff8042128490            BLT -1 -1 -1    SafeEjectGPUServ    
0xffffff80426be958   0xffffff8042669400   0xffffff80403fa180       3 ND       515   0xffffff80417a5000           LTQA -1 -1 -1    PAH_Extension       
0xffffff80426bd1b8   0xffffff8042669600   0xffffff80403fa720       2 R        479   0xffffff8041a55490            BLT -1 -1 -1    findmydeviced       
0xffffff80426bcbd0   0xffffff8042669300   0xffffff80403fa120       6 D        516   0xffffff80427f2490                -1 -1 -1    diagnosticd         
0xffffff80426bc5e8   0xffffff8042669d00   0xffffff80403fa2a0       2 R        485   0xffffff8041a56b60            BLT -1 -1 -1    photoanalysisd      
0xffffff8041e08000   0xffffff8042669200   0xffffff80403fbaa0       2 R        517   0xffffff80427f2000            BLT -1 -1 -1    videosubscriptio    
0xffffff80417e9d88   0xffffff8042669700   0xffffff80403fb980       6 R        434   0xffffff80408b7b60           BLTQ -1 -1 -1    WiFiAgent           
0xffffff8041bef1b8   0xffffff80417df600   0xffffff80403fafc0       2 R        484   0xffffff8041a566d0            BLT -1 -1 -1    deleted             
0xffffff8040a9b7a0   0xffffff80417dff00   0xffffff80403fa060       2 R        494   0xffffff8041e1a240            BLT -1 -1 -1    com.apple.quickl    
0xffffff80426be370   0xffffff80417dfa00   0xffffff80403fbf80       2          495   0xffffff8041e19920            BLT -1 -1 -1    silhouette          
0xffffff80403f47a0   0xffffff804333c500   0xffffff80403fa000       5 R        487   0xffffff8041d4bdb0           BLTQ -1 -1 -1    touristd            
0xffffff80428c4d88   0xffffff80417df100   0xffffff8038476ae0       4 R        496   0xffffff8041e19490            BLT -1 -1 -1    corespotlightd      
0xffffff80428c47a0   0xffffff8042669e00   0xffffff8038476ba0       3          490   0xffffff8041d4b490            BLT -1 -1 -1    DataDetectorsLoc    
0xffffff80428c41b8   0xffffff8040134400   0xffffff803a65dce0       6 R        492   0xffffff8041d4cb60           BLTQ -1 -1 -1    nbagent             
0xffffff80428c3bd0   0xffffff804333cb00   0xffffff803a65db60       2 R        486   0xffffff8041d4c240            BLT -1 -1 -1    photolibraryd       
0xffffff80428c5370   0xffffff8042669c00   0xffffff803a65c0c0       2 R        491   0xffffff8041d4b000            BLT -1 -1 -1    followupd           
0xffffff80428c5958   0xffffff8041d8b800   0xffffff80403fbce0       4 R        488   0xffffff8041d4c6d0           BLTQ -1 -1 -1    cloudphotosd        
0xffffff80428c3000   0xffffff8042dacd00   0xffffff80403fbbc0       2 R        518   0xffffff80427f2920            BLT -1 -1 -1    media-indexer       
0xffffff8041e0a370   0xffffff8042669b00   0xffffff80403fa6c0       4 D        519   0xffffff80427f2db0                -1 -1 -1    AppleSpell          
0xffffff8042daf000   0xffffff8042dac200   0xffffff8042d95440       3 D        529   0xffffff8042d98000              Q -1 -1 -1    AirPlayUIAgent      
0xffffff8042e137a0   0xffffff8042dacc00   0xffffff8042d95020       2 D        528   0xffffff8042d98490                -1 -1 -1    icdd                
0xffffff8042e131b8   0xffffff8042dac100   0xffffff8042d954a0       8 D        526   0xffffff8042d98db0             TQ -1 -1 -1    GoogleSoftwareUp    
0xffffff8042e14370   0xffffff8042dacf00   0xffffff8042d94fc0       2 D        535   0xffffff8042dd4920                -1 -1 -1    amfid               
0xffffff8042e14958   0xffffff80417df000   0xffffff8042d94f60       2 R        533   0xffffff8042dd4db0            BLT -1 -1 -1    backgroundtaskma    
0xffffff8042e12bd0   0xffffff803e030700   0xffffff8042d95260       2 R        534   0xffffff8042dd5240            BLT -1 -1 -1    ctkahp              
0xffffff8042e12000   0xffffff8042dacb00   0xffffff8042d94ea0       4 D        540   0xffffff8042dd56d0              Q -1 -1 -1    iTunesHelper        
0xffffff8042db1958   0xffffff8042dac600   0xffffff8042d95380       2 R        542   0xffffff8042dd4000            BLT -1 -1 -1    ctkd                
0xffffff8042e13d88   0xffffff8042dac000   0xffffff8042d950e0       2 R        544   0xffffff8042f45920            BLT -1 -1 -1    PrintUITool         
0xffffff8042daf5e8   0xffffff8042dac900   0xffffff8042d95140       4 D        546   0xffffff8042f46240             TQ -1 -1 -1    crashpad_handler    
0xffffff8042e125e8   0xffffff804333c900   0xffffff8042d953e0       2 R        520   0xffffff80427f3240            BLT -1 -1 -1    fileproviderd       
0xffffff803a49bd88   0xffffff804333c800   0xffffff8042d95560       2 R        527   0xffffff8042d98920            BLT -1 -1 -1    dmd                 
0xffffff8040a9c370   0xffffff804333cc00   0xffffff8042d955c0       2 R        530   0xffffff8042d99240            BLT -1 -1 -1    cloudpaird          
0xffffff8042db07a0   0xffffff804333c700   0xffffff8042d95620       3          531   0xffffff8042d996d0            BLT -1 -1 -1    diagnostics_agen    
0xffffff8042db0d88   0xffffff804333c600   0xffffff8042d94de0       3          522   0xffffff80427f3b60            BLT -1 -1 -1    ckkeyrolld          
0xffffff80433375e8   0xffffff804333ca00   0xffffff8042d94d20       2 R        524   0xffffff803e4adb60            BLT -1 -1 -1    SocialPushAgent     
0xffffff80433387a0   0xffffff804333ce00   0xffffff8042d95320       2 R        550   0xffffff8042f46b60            BLT -1 -1 -1    dmd                 
0xffffff8043337000   0xffffff804333c100   0xffffff8042d94e40       2 R        553   0xffffff8042dd5b60            BLT -1 -1 -1    com.apple.iTunes    
0xffffff8042dafbd0   0xffffff8042dac700   0xffffff8042d95080       2 D        556   0xffffff803edbc6d0             TQ -1 -1 -1    softwareupdate_n    
0xffffff8041e085e8   0xffffff803e030300   0xffffff8042d957a0       2 D        559   0xffffff803e4ad6d0             TQ -1 -1 -1    diskspaced          
0xffffff80417e8000   0xffffff8041d8b700   0xffffff8042d95680       3          560   0xffffff8040ac9490            BLT -1 -1 -1    EscrowSecurityAl    
0xffffff803e02b000   0xffffff80417dfe00   0xffffff8042d94a80       2          561   0xffffff80427f36d0            BLT -1 -1 -1    IMAutomaticHisto    
0xffffff803df14958   0xffffff80417df700   0xffffff8042d95800       2 R        565   0xffffff803b6296d0           BLTQ -1 -1 -1    storelegacy         
0xffffff8041bf0370   0xffffff804333cd00   0xffffff8042d94c00       3 R        571   0xffffff8042d99b60           BLTQ -1 -1 -1    storeassetd         
0xffffff8041e097a0   0xffffff8041d8b600   0xffffff8042d95740       3 R        575   0xffffff8040aca240            BLT -1 -1 -1    com.apple.CloudP    
0xffffff80426bc000   0xffffff8041d8ba00   0xffffff8042d94a20       2 D        577   0xffffff8043c18db0              P -1 -1 -1    sysdiagnose         
0xffffff80426bdd88   0xffffff8041d8b500   0xffffff8042d949c0       2 R        576   0xffffff8043c18920            BLT -1 -1 -1    symptomsd-diag      
0xffffff80428c35e8   0xffffff8041d8bb00   0xffffff8042d94900       2 R        578   0xffffff8043c19240            BLT -1 -1 -1    AssetCache          
0xffffff8040a9bd88   0xffffff8042dac300   0xffffff8042d94ba0       3 D        583   0xffffff803e4ac490             TQ -1 -1 -1    LaterAgent          
0xffffff804133bd88   0xffffff8042669100   0xffffff8042d94720       3 D        597   0xffffff804498c920              Q -1 -1 -1    CoreServicesUIAg    
0xffffff8041befd88   0xffffff804333c000   0xffffff8042d94780       3 D        598   0xffffff804498cdb0       RAGE BLT -1 -1 -1    mdworker            
0xffffff8041bee5e8   0xffffff803a3c9100   0xffffff8042d94ae0       3 D        599   0xffffff804498d240       RAGE BLT -1 -1 -1    mdworker            
0xffffff8043339958   0xffffff8042669000   0xffffff8042d958c0       3 D        600   0xffffff804498c490       RAGE BLT -1 -1 -1    mdworker            
0xffffff8043338d88   0xffffff8041d8b900   0xffffff8042d952c0       3 D        601   0xffffff804498d6d0       RAGE BLT -1 -1 -1    mdworker            
0xffffff8042db1370   0xffffff8042dac500   0xffffff8042d956e0       4 R        603   0xffffff804498c000       RAGE BLT -1 -1 -1    quicklookd          
0xffffff803a49a5e8   0xffffff803e90c300   0xffffff8042d94d80       3 D        605   0xffffff8042129b60       RAGE BLT -1 -1 -1    mdworker            
0xffffff803ee331b8   0xffffff804333c400   0xffffff8042d951a0       3 D        606   0xffffff8042128000       RAGE BLT -1 -1 -1    mdworker            
0xffffff8043339370   0xffffff8041d8bd00   0xffffff8042d95200       2 D        607   0xffffff803edbc240                -1 -1 -1    distnoted           
0xffffff80433381b8   0xffffff8041d8be00   0xffffff8042d94840       3 D        608   0xffffff8043c196d0       RAGE BLT -1 -1 -1    mdworker            
0xffffff8041f73bd0   0xffffff804333c300   0xffffff8042d95500       2 RB       609   0xffffff8043c19b60                -1 -1 -1    IMDMessageServic    
0xffffff803e88d1b8   0xffffff8042669f00   0xffffff8042d94b40       1          610   0xffffff8041d4b920                -1 -1 -1    launchd       

(lldb) f 0
frame #0: 0xffffff802d0b3814 kernel.development`mac_cred_check_label_update_execve [inlined] vfs_context_ucred(ctx=0xffffff90df09bd60) at kpi_vfs.c:1369 [opt]
(lldb) p/x $rbp
(unsigned long) $28 = 0xffffff90df09b950
(lldb) x/10gx 0xffffff90df09b950
0xffffff90df09b950: 0xffffff90df09bca0 0xffffff802ce6dfb3
0xffffff90df09b960: 0xffffff8041d4b920 0x0000000000000000
0xffffff90df09b970: 0xffffff90df09ba10 0xffffff802d0b88d2
0xffffff90df09b980: 0xffffff8042669500 0xffffff803a2386c8
0xffffff90df09b990: 0xffffff90df09b9c8 0x0000000000000001

(lldb) disassem -a 0xffffff802d0b3814
kernel.development`mac_cred_check_label_update_execve:
    0xffffff802d0b37f0 <+0>:   pushq  %rbp
    0xffffff802d0b37f1 <+1>:   movq   %rsp, %rbp
    0xffffff802d0b37f4 <+4>:   pushq  %r15
    0xffffff802d0b37f6 <+6>:   pushq  %r14
    0xffffff802d0b37f8 <+8>:   pushq  %r13
    0xffffff802d0b37fa <+10>:  pushq  %r12
    0xffffff802d0b37fc <+12>:  pushq  %rbx
    0xffffff802d0b37fd <+13>:  subq   $0x38, %rsp
    0xffffff802d0b3801 <+17>:  movq   %r9, -0x58(%rbp)
    0xffffff802d0b3805 <+21>:  movq   %r8, -0x50(%rbp)
    0xffffff802d0b3809 <+25>:  movq   %rcx, -0x48(%rbp)
    0xffffff802d0b380d <+29>:  movq   %rdx, -0x40(%rbp)
    0xffffff802d0b3811 <+33>:  movq   %rsi, %r13
    0xffffff802d0b3814 <+36>:  int3   
    0xffffff802d0b3815 <+37>:  movl   0x8(%rdi), %eax
    0xffffff802d0b3818 <+40>:  movq   %rax, -0x38(%rbp)
    0xffffff802d0b381c <+44>:  movl   0x3ce2b2(%rip), %eax      ; mac_policy_list + 12
    0xffffff802d0b3822 <+50>:  testl  %eax, %eax
    0xffffff802d0b3824 <+52>:  je     0xffffff802d0b38c2        ; <+210> at mac_vfs.c
    0xffffff802d0b382a <+58>:  xorl   %r12d, %r12d
    0xffffff802d0b382d <+61>:  leaq   -0x30(%rbp), %rbx
    0xffffff802d0b3831 <+65>:  xorl   %r15d, %r15d
    0xffffff802d0b3834 <+68>:  nopw   %cs:(%rax,%rax)
    0xffffff802d0b3840 <+80>:  movq   0x3ce299(%rip), %rcx      ; mac_policy_list + 24
    0xffffff802d0b3847 <+87>:  movl   %r15d, %edx
    0xffffff802d0b384a <+90>:  movq   (%rcx,%rdx,8), %rcx
    0xffffff802d0b384e <+94>:  testq  %rcx, %rcx
    0xffffff802d0b3851 <+97>:  je     0xffffff802d0b38b8        ; <+200> at mac_vfs.c:744
    0xffffff802d0b3853 <+99>:  movq   0x20(%rcx), %rdx
    0xffffff802d0b3857 <+103>: movq   0x30(%rdx), %r14
    0xffffff802d0b385b <+107>: testq  %r14, %r14
    0xffffff802d0b385e <+110>: je     0xffffff802d0b38b8        ; <+200> at mac_vfs.c:744
    0xffffff802d0b3860 <+112>: movq   $0x0, -0x30(%rbp)
    0xffffff802d0b3868 <+120>: movq   (%rcx), %rsi
    0xffffff802d0b386b <+123>: movq   0x18(%rbp), %rdi
    0xffffff802d0b386f <+127>: movq   %rbx, %rdx
    0xffffff802d0b3872 <+130>: callq  0xffffff802ce712e0        ; exec_spawnattr_getmacpolicyinfo at kern_exec.c:1954
    0xffffff802d0b3877 <+135>: movb   $0x1, %cl
    0xffffff802d0b3879 <+137>: testl  %r12d, %r12d
    0xffffff802d0b387c <+140>: jne    0xffffff802d0b38ae        ; <+190> at mac_vfs.c:756
    0xffffff802d0b387e <+142>: movq   0xe8(%r13), %r8
    0xffffff802d0b3885 <+149>: movq   -0x38(%rbp), %rdi
    0xffffff802d0b3889 <+153>: movq   %r13, %rsi
    0xffffff802d0b388c <+156>: movq   -0x40(%rbp), %rdx
    0xffffff802d0b3890 <+160>: movq   -0x48(%rbp), %rcx
    0xffffff802d0b3894 <+164>: movq   -0x50(%rbp), %r9
    0xffffff802d0b3898 <+168>: pushq  -0x30(%rbp)
    0xffffff802d0b389b <+171>: pushq  %rax
    0xffffff802d0b389c <+172>: pushq  0x10(%rbp)
    0xffffff802d0b389f <+175>: pushq  -0x58(%rbp)
    0xffffff802d0b38a2 <+178>: callq  *%r14
    0xffffff802d0b38a5 <+181>: addq   $0x20, %rsp
    0xffffff802d0b38a9 <+185>: testl  %eax, %eax
    0xffffff802d0b38ab <+187>: setne  %cl
    0xffffff802d0b38ae <+190>: movzbl %cl, %r12d
    0xffffff802d0b38b2 <+194>: movl   0x3ce21c(%rip), %eax      ; mac_policy_list + 12
    0xffffff802d0b38b8 <+200>: incl   %r15d
    0xffffff802d0b38bb <+203>: cmpl   %eax, %r15d
    0xffffff802d0b38be <+206>: jb     0xffffff802d0b3840        ; <+80> at mac_vfs.c:745
    0xffffff802d0b38c0 <+208>: jmp    0xffffff802d0b38c8        ; <+216> at mac_vfs.c:758
    0xffffff802d0b38c2 <+210>: xorl   %r15d, %r15d
    0xffffff802d0b38c5 <+213>: xorl   %r12d, %r12d
    0xffffff802d0b38c8 <+216>: callq  0xffffff802d0ac350        ; mac_policy_list_conditional_busy at mac_base.c:319
    0xffffff802d0b38cd <+221>: testl  %eax, %eax
    0xffffff802d0b38cf <+223>: je     0xffffff802d0b3974        ; <+388> at mac_vfs.c:777
    0xffffff802d0b38d5 <+229>: movl   0x3ce1f5(%rip), %eax      ; mac_policy_list + 8
    0xffffff802d0b38db <+235>: cmpl   %eax, %r15d
    0xffffff802d0b38de <+238>: ja     0xffffff802d0b396f        ; <+383> at mac_vfs.c:773
    0xffffff802d0b38e4 <+244>: leaq   -0x30(%rbp), %r14
    0xffffff802d0b38e8 <+248>: nopl   (%rax,%rax)
    0xffffff802d0b38f0 <+256>: movq   0x3ce1e9(%rip), %rcx      ; mac_policy_list + 24
    0xffffff802d0b38f7 <+263>: movl   %r15d, %edx
    0xffffff802d0b38fa <+266>: movq   (%rcx,%rdx,8), %rcx
    0xffffff802d0b38fe <+270>: testq  %rcx, %rcx
    0xffffff802d0b3901 <+273>: je     0xffffff802d0b3967        ; <+375> at mac_vfs.c:759
    0xffffff802d0b3903 <+275>: movq   0x20(%rcx), %rdx
    0xffffff802d0b3907 <+279>: movq   0x30(%rdx), %rbx
    0xffffff802d0b390b <+283>: testq  %rbx, %rbx
    0xffffff802d0b390e <+286>: je     0xffffff802d0b3967        ; <+375> at mac_vfs.c:759
    0xffffff802d0b3910 <+288>: movq   $0x0, -0x30(%rbp)
    0xffffff802d0b3918 <+296>: movq   (%rcx), %rsi
    0xffffff802d0b391b <+299>: movq   0x18(%rbp), %rdi
    0xffffff802d0b391f <+303>: movq   %r14, %rdx
    0xffffff802d0b3922 <+306>: callq  0xffffff802ce712e0        ; exec_spawnattr_getmacpolicyinfo at kern_exec.c:1954
    0xffffff802d0b3927 <+311>: movb   $0x1, %cl
    0xffffff802d0b3929 <+313>: testl  %r12d, %r12d
    0xffffff802d0b392c <+316>: jne    0xffffff802d0b395d        ; <+365> at mac_vfs.c:771
    0xffffff802d0b392e <+318>: movq   0xe8(%r13), %r8
    0xffffff802d0b3935 <+325>: movq   -0x38(%rbp), %rdi
    0xffffff802d0b3939 <+329>: movq   %r13, %rsi
    0xffffff802d0b393c <+332>: movq   -0x40(%rbp), %rdx
    0xffffff802d0b3940 <+336>: movq   -0x48(%rbp), %rcx
    0xffffff802d0b3944 <+340>: movq   -0x50(%rbp), %r9
    0xffffff802d0b3948 <+344>: pushq  -0x30(%rbp)
    0xffffff802d0b394b <+347>: pushq  %rax
    0xffffff802d0b394c <+348>: pushq  0x10(%rbp)
    0xffffff802d0b394f <+351>: pushq  -0x58(%rbp)
    0xffffff802d0b3952 <+354>: callq  *%rbx
    0xffffff802d0b3954 <+356>: addq   $0x20, %rsp
    0xffffff802d0b3958 <+360>: testl  %eax, %eax
    0xffffff802d0b395a <+362>: setne  %cl
    0xffffff802d0b395d <+365>: movzbl %cl, %r12d
    0xffffff802d0b3961 <+369>: movl   0x3ce169(%rip), %eax      ; mac_policy_list + 8
    0xffffff802d0b3967 <+375>: incl   %r15d
    0xffffff802d0b396a <+378>: cmpl   %eax, %r15d
    0xffffff802d0b396d <+381>: jbe    0xffffff802d0b38f0        ; <+256> at mac_vfs.c:760
    0xffffff802d0b396f <+383>: callq  0xffffff802d0ac3b0        ; mac_policy_list_unbusy at mac_base.c:337
    0xffffff802d0b3974 <+388>: movl   %r12d, %eax
    0xffffff802d0b3977 <+391>: addq   $0x38, %rsp
    0xffffff802d0b397b <+395>: popq   %rbx
    0xffffff802d0b397c <+396>: popq   %r12
    0xffffff802d0b397e <+398>: popq   %r13
    0xffffff802d0b3980 <+400>: popq   %r14
    0xffffff802d0b3982 <+402>: popq   %r15
    0xffffff802d0b3984 <+404>: popq   %rbp
    0xffffff802d0b3985 <+405>: retq   

```
