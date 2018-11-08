
A call stack for manually registering a new IOKit kext with the kernel using ```kextutil```.

```
* thread #1206, name = '0xffffff80228fa840', queue = '0x0', stop reason = breakpoint 1.1
  * frame #0: 0xffffff800da155a4 kernel.development`_IOServiceJob::startJob(nub=0xffffff8018b05fa0, type=10, options=8) at IOService.cpp:956 [opt]
    frame #1: 0xffffff800da1c56d kernel.development`IOService::catalogNewDrivers(newTables=0xffffff801cb35960) at IOService.cpp:940 [opt]
    frame #2: 0xffffff800da352ef kernel.development`IOCatalogue::addDrivers(this=<unavailable>, drivers=<unavailable>, doNubMatching=<unavailable>) at IOCatalogue.cpp:378 [opt]
    frame #3: 0xffffff800d9c361a kernel.development`OSKext::sendPersonalitiesToCatalog(this=0xffffff8024740a00, startMatching=true, personalityNames=<unavailable>) at OSKext.cpp:10355 [opt]
    frame #4: 0xffffff800d9bfdc7 kernel.development`OSKext::load(this=0xffffff8024740a00, startOpt='\0', startMatchingOpt='\0', personalityNames=0xffffff801d1476c0) at OSKext.cpp:4889 [opt]
    frame #5: 0xffffff800d9cc924 kernel.development`OSKext::loadKextWithIdentifier(kextIdentifier=0xffffff8018c7a7e0, allowDeferFlag=false, delayAutounloadFlag=<unavailable>, startOpt='\0', startMatchingOpt='\0', personalityNames=0xffffff801d1476c0) at OSKext.cpp:4513 [opt]
    frame #6: 0xffffff800d9cc73d kernel.development`OSKext::loadFromMkext(clientLogFilter=<unavailable>, mkextBuffer=<unavailable>, mkextBufferLength=<unavailable>, logInfoOut=0xffffff80a08f3d30, logInfoLengthOut=0xffffff80a08f3d44) at OSKext.cpp:3134 [opt]
    frame #7: 0xffffff800d9dd642 kernel.development`::kext_request(hostPriv=<unavailable>, clientLogSpec=4082, requestIn=<unavailable>, requestLengthIn=<unavailable>, responseOut=0xffffff8023b9a9ac, responseLengthOut=0xffffff8023b9a9d4, logDataOut=<unavailable>, logDataLengthOut=<unavailable>, op_result=<unavailable>) at OSKextLib.cpp:280 [opt]
    frame #8: 0xffffff800d3de805 kernel.development`_Xkext_request(InHeadP=<unavailable>, OutHeadP=0xffffff8023b9a988) at host_priv_server.c:2785 [opt]
    frame #9: 0xffffff800d381d07 kernel.development`ipc_kobject_server(request=0xffffff802311a400, option=<unavailable>) at ipc_kobject.c:351 [opt]
    frame #10: 0xffffff800d354d0d kernel.development`ipc_kmsg_send(kmsg=0xffffff802311a400, option=3, send_timeout=0) at ipc_kmsg.c:1867 [opt]
    frame #11: 0xffffff800d36fafb kernel.development`mach_msg_overwrite_trap(args=<unavailable>) at mach_msg.c:570 [opt]
    frame #12: 0xffffff800d4be0ea kernel.development`mach_call_munger64(state=0xffffff8022a24d40) at bsd_i386.c:573 [opt]
    frame #13: 0xffffff800d320a56 kernel.development`hndl_mach_scall64 + 22
```

A calling process is ```kextutil```.

```
Processor 0xffffff800de0ba40 cpu_id  0x0 AST: P      State RUNNING      Preemption Disabled
task                 vm_map               ipc_space            #acts flags    pid       process             io_policy  wq_state  command             
0xffffff8021e007a0   0xffffff80238be500   0xffffff802389b5a0       3 D        670   0xffffff8020635240                -1 -1 -1    kextutil            
	thread                   thread_id  processor            base   pri    sched_mode      io_policy       state    ast          waitq                            wait_event           wmesg                thread_name         
	0xffffff80228fa840       0x2f6c     0xffffff800de0ba40   31     31     timeshare                       R                                                                                                                    
	Backtrace:
	kernel_stack = 0xffffff80a08f0000
	stacktop = 0xffffff80a08f3ad0
	0xffffff80a08f3ad0 0xffffff800da155a4 _IOServiceJob::startJob(IOService*, int, unsigned int)((IOService *) nub = 0xffffff8018b05fa0, (int) type = 10, (IOOptionBits) options = 8) 
	0xffffff80a08f3b20 0xffffff800da1c56d IOService::catalogNewDrivers(OSOrderedSet*)((OSOrderedSet *) newTables = 0xffffff801cb35960) 
	0xffffff80a08f3b80 0xffffff800da352ef IOCatalogue::addDrivers(OSArray*, bool)((IOCatalogue *) this = <>, , (OSArray *) drivers = <>, , (bool) doNubMatching = <>, ) 
	0xffffff80a08f3be0 0xffffff800d9c361a OSKext::sendPersonalitiesToCatalog(bool, OSArray*)((OSKext *) this = 0xffffff8024740a00, (bool) startMatching = true, (OSArray *) personalityNames = <>, ) 
	0xffffff80a08f3c50 0xffffff800d9bfdc7 OSKext::load(unsigned char, unsigned char, OSArray*)((OSKext *) this = 0xffffff8024740a00, (OSKextExcludeLevel) startOpt = '\0', (OSKextExcludeLevel) startMatchingOpt = '\0', (OSArray *) personalityNames = 0xffffff801d1476c0) 
	0xffffff80a08f3ca0 0xffffff800d9cc924 OSKext::loadKextWithIdentifier(OSString*, bool, bool, unsigned char, unsigned char, OSArray*)((OSString *) kextIdentifier = 0xffffff8018c7a7e0, (Boolean) allowDeferFlag = false, (Boolean) delayAutounloadFlag = <unable to extract DW_OP_bit_piece(bit_size = 1, bit_offset = 0) from an addresss value.>, , (OSKextExcludeLevel) startOpt = '\0', (OSKextExcludeLevel) startMatchingOpt = '\0', (OSArray *) personalityNames = 0xffffff801d1476c0) 
	0xffffff80a08f3d00 0xffffff800d9cc73d OSKext::loadFromMkext(unsigned int, char*, unsigned int, char**, unsigned int*)((OSKextLogSpec) clientLogFilter = <>, , (char *) mkextBuffer = <>, , (uint32_t) mkextBufferLength = <>, , (char **) logInfoOut = 0xffffff80a08f3d30, (uint32_t *) logInfoLengthOut = 0xffffff80a08f3d44) 
	0xffffff80a08f3d70 0xffffff800d9dd642 ::kext_request(host_priv_t, uint32_t, vm_offset_t, mach_msg_type_number_t, vm_offset_t *, mach_msg_type_number_t *, vm_offset_t *, mach_msg_type_number_t *, kern_return_t *)((host_priv_t) hostPriv = <>, , (uint32_t) clientLogSpec = 4082, (vm_offset_t) requestIn = <>, , (mach_msg_type_number_t) requestLengthIn = <>, , (vm_offset_t *) responseOut = 0xffffff8023b9a9ac, (mach_msg_type_number_t *) responseLengthOut = 0xffffff8023b9a9d4, (vm_offset_t *) logDataOut = <>, , (mach_msg_type_number_t *) logDataLengthOut = <>, , (kern_return_t *) op_result = <>, ) 
	0xffffff80a08f3dc0 0xffffff800d3de805 _Xkext_request((mach_msg_header_t *) InHeadP = <>, , (mach_msg_header_t *) OutHeadP = 0xffffff8023b9a988) 
	0xffffff80a08f3e10 0xffffff800d381d07 ipc_kobject_server((ipc_kmsg_t) request = 0xffffff802311a400, (mach_msg_option_t) option = <>, ) 
	0xffffff80a08f3e60 0xffffff800d354d0d ipc_kmsg_send((ipc_kmsg_t) kmsg = 0xffffff802311a400, (mach_msg_option_t) option = 3, (mach_msg_timeout_t) send_timeout = 0) 
	0xffffff80a08f3ef0 0xffffff800d36fafb mach_msg_overwrite_trap((mach_msg_overwrite_trap_args *) args = <>, ) 
	0xffffff80a08f3fa0 0xffffff800d4be0ea mach_call_munger64((x86_saved_state_t *) state = 0xffffff8022a24d40) 
	0x0000000000000000 0xffffff800d320a56 kernel.development`hndl_mach_scall64 + 0x16 
	stackbottom = 0xffffff80a08f3fa0
  ```
  
  The nub belongs to the IOService plan's IOResource root (the output was cut for brevity)
  
  ```
  (lldb) showregistryentry 0xffffff8018b05fa0
+-o ??  <object 0xffffff8018b05fa0, id 0x100000113, vtable 0xffffff800dedf078, registered, matched, active, busy 1, retain count 41>
  | {
  |   `object 0xffffff8018b17d40, vt 0xffffff800dedc6a0, retain count 3, container retain 3` "intel_cpupm_matching" = `object 0xffffff8018789ed0, vt 0xffffff800dedb518` 0
  |   `object 0xffffff8018be9960, vt 0xffffff800dedc6a0, retain count 62, container retain 62` "IOKit" = `object 0xffffff8018b17760, vt 0xffffff800dedc6a0` "IOService"
  |   `object 0xffffff8018b8ac40, vt 0xffffff800dedc6a0, retain count 2, container retain 2` "AKSFileSystemKeyServices" = `object 0xffffff8077e59000, vt 0xffffff7f8fbcf2e8 <vtable for AppleKeyStore>` 
  |   `object 0xffffff8018b8aae0, vt 0xffffff800dedc6a0, retain count 2, container retain 2` "AKSKernelServices" = `object 0xffffff8077e59000, vt 0xffffff7f8fbcf2e8 <vtable for AppleKeyStore>` 
  |   `object 0xffffff8018b8a540, vt 0xffffff800dedc6a0, retain count 2, container retain 2` "AKSFileVaultServices" = `object 0xffffff8077e59000, vt 0xffffff7f8fbcf2e8 <vtable for AppleKeyStore>` 
  |   `object 0xffffff8018b17520, vt 0xffffff800dedc6a0, retain count 3, container retain 2` "IOResourceMatched" = `object 0xffffff801e314f00, vt 0xffffff800ded9ce0` (`object 0xffffff8018b17d40, vt 0xffffff800dedc6a0` "intel_cpupm_matching",`object 0xffffff8018be9960, vt 0xffffff800dedc6a0` "IOKit",`object 0xffffff8018b8ac40, vt 0xffffff800dedc6a0` "AKSFileSystemKeyServices",`object 0xffffff8018b8aae0, vt 0xffffff800dedc6a0` "AKSKernelServices",`object 0xffffff8018b8a540, vt 0xffffff800dedc6a0` "AKSFileVaultServices",`object 0xffffff8018b17520, vt 0xffffff800dedc6a0` "IOResourceMatched",`object 0xffffff8018b234a0, vt 0xffffff800dedc6a0` "ACPI",`object 0xffffff8018b9bd80, vt 0xffffff800dedc6a0` "SMBIOS",`object 0xffffff8018c9ee00, vt 0xffffff800dedc6a0` "efi-runtime",`object 0xffffff8018b21d00, vt 0xffffff800dedc6a0` "IORTC",`object 0xffffff8018cca8e0, vt 0xffffff800dedc6a0` "IOPlatformUUID",`object 0xffffff8018bb73c0, vt 0xffffff800dedc6a0` "IONVRAM",`object 0xffffff8018ccfd60, vt 0xffffff800dedc6a0` "com.apple.AppleFSCompression.Type1",`object 0xffffff8018b17420, vt 0xffffff800dedc6a0` "IOBSD",`object 0xffffff8018d045e0, vt 0xffffff800dedc6a0` "com.apple.AppleFSCompression.Type3",`object 0xffffff8018d04860, vt 0xffffff800dedc6a0` "com.apple.AppleFSCompression.Type4",`object 0xffffff8018d04880, vt 0xffffff800dedc6a0` "com.apple.AppleFSCompression.Type7",`object 0xffffff8018d04840, vt 0xffffff800dedc6a0` "com.apple.AppleFSCompression.Type8",`object 0xffffff8018d045c0, vt 0xffffff800dedc6a0` "com.apple.AppleFSCompression.Type9",`object 0xffffff8018d04760, vt 0xffffff800dedc6a0` "com.apple.AppleFSCompression.Type10",`object 0xffffff8018d04780, vt 0xffffff800dedc6a0` "com.apple.AppleFSCompression.Type11",`object 0xffffff8018d04620, vt 0xffffff800dedc6a0` "com.apple.AppleFSCompression.Type12",`object 0xffffff8018d047a0, vt 0xffffff800dedc6a0` "com.apple.AppleFSCompression.Type5",`object 0xffffff8018c1ba00, vt 0xffffff800dedc6a0` "CCPipe",`object 0xffffff8018cc1380, vt 0xffffff800dedc6a0` "boot-uuid-media",`object 0xffffff8018bd1080, vt 0xffffff800dedc6a0` "IOTimeSyncClockManager",`object 0xffffff8018be99c0, vt 0xffffff800dedc6a0` "com.apple.iokit.SCSISubsystemGlobals",`object 0xffffff801b713880, vt 0xffffff800dedc6a0` "IOSerialManagerMustLoad",`object 0xffffff8018b49c20, vt 0xffffff800dedc6a0` "IOPlatformDeviceASPMEnable",`object 0xffffff8018b4bc60, vt 0xffffff800dedc6a0` "IOPlatformPlugin",`object 0xffffff801ca6c720, vt 0xffffff800dedc6a0` "IOAVBNub",`object 0xffffff8018b17540, vt 0xffffff800dedc6a0` "IOConsoleUsersSeed",`object 0xffffff8018b17c60, vt 0xffffff800dedc6a0` "IOConsoleUsers",`object 0xffffff8018f3bf80, vt 0xffffff800dedc6a0` "WindowServer",`object 0xffffff801ca795e0, vt 0xffffff800dedc6a0` "ComodoVfsDeactivation",`object 0xffffff8018c20e00, vt 0xffffff800dedc6a0` "CCCapture")
  |   `object 0xffffff8018b234a0, vt 0xffffff800dedc6a0, retain count 7, container retain 7` "ACPI" = `object 0xffffff801905aa00, vt 0xffffff7f8fdb42a0 <vtable for AppleACPIPlatformExpert>` 
  |   `object 0xffffff8018b9bd80, vt 0xffffff800dedc6a0, retain count 3, container retain 3` "SMBIOS" = `object 0xffffff8018b17760, vt 0xffffff800dedc6a0` "IOService"
  |   `object 0xffffff8018c9ee00, vt 0xffffff800dedc6a0, retain count 3, container retain 3` "efi-runtime" = `object 0xffffff8018cee0e0, vt 0xffffff7f8e9d7068 <vtable for AppleEFIRuntime>` 
  |   `object 0xffffff8018b21d00, vt 0xffffff800dedc6a0, retain count 6, container retain 5` "IORTC" = `object 0xffffff80190f2c90, vt 0xffffff7f8fac9088 <vtable for AppleRTC>` 
  |   `object 0xffffff8018cca8e0, vt 0xffffff800dedc6a0, retain count 3, container retain 3` "IOPlatformUUID" = `object 0xffffff8018cca580, vt 0xffffff800dedc380` "0665A1C1-BB65-5EAC-ADEB-D4A91694FC6B"
  |   `object 0xffffff8018bb73c0, vt 0xffffff800dedc6a0, retain count 2, container retain 2` "IONVRAM" = `object 0xffffff8018b17760, vt 0xffffff800dedc6a0` "IOService"
  |   `object 0xffffff8018ccfd60, vt 0xffffff800dedc6a0, retain count 2, container retain 2` "com.apple.AppleFSCompression.Type1" = `object 0xffffff80186f7730, vt 0xffffff800deda030` Yes
  |   `object 0xffffff8018b17420, vt 0xffffff800dedc6a0, retain count 71, container retain 70` "IOBSD" = `object 0xffffff8018b17760, vt 0xffffff800dedc6a0` "IOService"
  |   `object 0xffffff8018d045e0, vt 0xffffff800dedc6a0, retain count 2, container retain 2` "com.apple.AppleFSCompression.Type3" = `object 0xffffff80186f7730, vt 0xffffff800deda030` Yes
  |   `object 0xffffff8018d04860, vt 0xffffff800dedc6a0, retain count 2, container retain 2` "com.apple.AppleFSCompression.Type4" = `object 0xffffff80186f7730, vt 0xffffff800deda030` Yes
  |   `object 0xffffff8018d04880, vt 0xffffff800dedc6a0, retain count 2, container retain 2` "com.apple.AppleFSCompression.Type7" = `object 0xffffff80186f7730, vt 0xffffff800deda030` Yes
  |   `object 0xffffff8018d04840, vt 0xffffff800dedc6a0, retain count 2, container retain 2` "com.apple.AppleFSCompression.Type8" = `object 0xffffff80186f7730, vt 0xffffff800deda030` Yes
  |   `object 0xffffff8018d045c0, vt 0xffffff800dedc6a0, retain count 2, container retain 2` "com.apple.AppleFSCompression.Type9" = `object 0xffffff80186f7730, vt 0xffffff800deda030` Yes
  |   `object 0xffffff8018d04760, vt 0xffffff800dedc6a0, retain count 2, container retain 2` "com.apple.AppleFSCompression.Type10" = `object 0xffffff80186f7730, vt 0xffffff800deda030` Yes
  |   `object 0xffffff8018d04780, vt 0xffffff800dedc6a0, retain count 2, container retain 2` "com.apple.AppleFSCompression.Type11" = `object 0xffffff80186f7730, vt 0xffffff800deda030` Yes
  |   `object 0xffffff8018d04620, vt 0xffffff800dedc6a0, retain count 2, container retain 2` "com.apple.AppleFSCompression.Type12" = `object 0xffffff80186f7730, vt 0xffffff800deda030` Yes
  |   `object 0xffffff8018d047a0, vt 0xffffff800dedc6a0, retain count 2, container retain 2` "com.apple.AppleFSCompression.Type5" = `object 0xffffff80186f7730, vt 0xffffff800deda030` Yes
  |   `object 0xffffff8018c1ba00, vt 0xffffff800dedc6a0, retain count 6, container retain 3` "CCPipe" = `object 0xffffff8018b17760, vt 0xffffff800dedc6a0` "IOService"
  |   `object 0xffffff8018cc1380, vt 0xffffff800dedc6a0, retain count 2, container retain 2` "boot-uuid-media" = `object 0xffffff8019248640, vt 0xffffff7f8dc68a90 <vtable for IOMedia>, retain count 9, container retain 8` 
  |   `object 0xffffff8018bd1080, vt 0xffffff800dedc6a0, retain count 13, container retain 12` "IOTimeSyncClockManager" = `object 0xffffff8018b17760, vt 0xffffff800dedc6a0` "IOService"
  |   `object 0xffffff8018be99c0, vt 0xffffff800dedc6a0, retain count 4, container retain 4` "com.apple.iokit.SCSISubsystemGlobals" = `object 0xffffff80186f7730, vt 0xffffff800deda030` Yes
  |   `object 0xffffff801b713880, vt 0xffffff800dedc6a0, retain count 4, container retain 4` "IOSerialManagerMustLoad" = `object 0xffffff80186f7730, vt 0xffffff800deda030` Yes
  |   `object 0xffffff8018b49c20, vt 0xffffff800dedc6a0, retain count 6, container retain 2` "IOPlatformDeviceASPMEnable" = `object 0xffffff801df7e000, vt 0xffffff7f904610d8 <vtable for X86PlatformPlugin>` 
  |   `object 0xffffff8018b4bc60, vt 0xffffff800dedc6a0, retain count 2, container retain 2` "IOPlatformPlugin" = `object 0xffffff801df7e000, vt 0xffffff7f904610d8 <vtable for X86PlatformPlugin>` 
  |   `object 0xffffff801ca6c720, vt 0xffffff800dedc6a0, retain count 9, container retain 8` "IOAVBNub" = `object 0xffffff8018b17760, vt 0xffffff800dedc6a0` "IOService"
  |   `object 0xffffff8018b17540, vt 0xffffff800dedc6a0, retain count 3, container retain 2` "IOConsoleUsersSeed" = `object 0xffffff8018789e40, vt 0xffffff800deda7e0` 
  ....
  ```
  
  _IOServiceJob::startJob spawns a _IOConfigThread::main thread or uses an existing one. In turn ```_IOConfigThread::main``` starts object matching.
  
 ```
 * thread #1207, name = '0xffffff801b64a338', queue = '0x0', stop reason = breakpoint 3.1
  * frame #0: 0xffffff800da16694 kernel.development`IOService::startCandidate(this=0xffffff8018b05fa0, service=0xffffff8024741180) at IOService.cpp:3510 [opt]
    frame #1: 0xffffff800da16413 kernel.development`IOService::probeCandidates(this=<unavailable>, matches=<unavailable>) at IOService.cpp:3448 [opt]
    frame #2: 0xffffff800da158cd kernel.development`IOService::doServiceMatch(this=0xffffff8018b05fa0, options=<unavailable>) at IOService.cpp:3758 [opt]
    frame #3: 0xffffff800da17366 kernel.development`_IOConfigThread::main(arg=0xffffff802474df90, result=<unavailable>) at IOService.cpp:4130 [opt]
    frame #4: 0xffffff800d31f5c7 kernel.development`call_continuation + 23
 ```
