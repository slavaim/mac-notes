IOService registration. Here as a result of property set from a user application, there are multiple other options see below.

```
  * frame #0: 0xffffff800da155a4 kernel.development`_IOServiceJob::startJob(nub=0xffffff8018b05fa0, type=10, options=0) at IOService.cpp:956 [opt]
    frame #1: 0xffffff800da0e4e3 kernel.development`IOService::registerService(this=0xffffff8018b05fa0, options=0) at IOService.cpp:814 [opt]
    frame #2: 0xffffff800da19689 kernel.development`IOService::updateConsoleUsers(OSArray*, unsigned int) [inlined] IOService::publishResource(key=<unavailable>, value=<unavailable>) at IOService.cpp:3570 [opt]
    frame #3: 0xffffff800da19640 kernel.development`IOService::updateConsoleUsers(consoleUsers=0xffffff801cbaabc0, systemMessage=0) at IOService.cpp:5284 [opt]
    frame #4: 0xffffff800da18fc3 kernel.development`IOResources::setProperties(this=<unavailable>, properties=0xffffff801cbaa940) at IOService.cpp:5325 [opt]
    frame #5: 0xffffff800da6d141 kernel.development`::is_io_registry_entry_set_properties(registry_entry=0xffffff8018b05fa0, properties=<unavailable>, propertiesCnt=<unavailable>, result=0xffffff80216013b8) at IOUserClient.cpp:3272 [opt]
    frame #6: 0xffffff800de82948 kernel.development`_IOServiceNullNotifier::gMetaClass + 40
    frame #7: 0xffffff801ec6d5e0
    frame #8: 0xffffff800d381d07 kernel.development`ipc_kobject_server(request=0xffffff802160138c, option=<unavailable>) at ipc_kobject.c:351 [opt]
    frame #9: 0xffffff800d354d0d kernel.development`ipc_kmsg_send(kmsg=0xffffff8021022b00, option=3, send_timeout=0) at ipc_kmsg.c:1867 [opt]
    frame #10: 0xffffff800d36fafb kernel.development`mach_msg_overwrite_trap(args=<unavailable>) at mach_msg.c:570 [opt]
    frame #11: 0xffffff800d4be0ea kernel.development`mach_call_munger64(state=0xffffff801b5db5e0) at bsd_i386.c:573 [opt]
    frame #12: 0xffffff800d320a56 kernel.development`hndl_mach_scall64 + 22
```

```_IOServiceJob::startJob``` spawns a ```_IOConfigThread::main``` thread if ones doesn't exist or the system requires more threads to speed up processing.

```
  * frame #0: 0xffffff800d3b7aa1 kernel.development`kernel_thread_start [inlined] kernel_thread_start_priority(continuation=(kernel.development`_IOConfigThread::main(void*, int) at IOService.cpp:4087), parameter=0xffffff8024723850, priority=-1) at thread.c:1671 [opt]
    frame #1: 0xffffff800d3b7aa1 kernel.development`kernel_thread_start(continuation=(kernel.development`_IOConfigThread::main(void*, int) at IOService.cpp:4087), parameter=0xffffff8024723850, new_thread=0xffffff8014d0bbc0) at thread.c:1690 [opt]
    frame #2: 0xffffff800da1719f kernel.development`_IOConfigThread::configThread() at IOService.cpp:3679 [opt]
    frame #3: 0xffffff800da170b9 kernel.development`_IOServiceJob::pingConfig(job=0xffffff8018f2f4c0) at IOService.cpp:4233 [opt]
    frame #4: 0xffffff800da15619 kernel.development`_IOServiceJob::startJob(nub=0xffffff8018b05fa0, type=10, options=0) at IOService.cpp:967 [opt]
    frame #5: 0xffffff800da0e4e3 kernel.development`IOService::registerService(this=0xffffff8018b05fa0, options=0) at IOService.cpp:814 [opt]
    frame #6: 0xffffff800da19689 kernel.development`IOService::updateConsoleUsers(OSArray*, unsigned int) [inlined] IOService::publishResource(key=<unavailable>, value=<unavailable>) at IOService.cpp:3570 [opt]
    frame #7: 0xffffff800da19640 kernel.development`IOService::updateConsoleUsers(consoleUsers=0xffffff801cbaabc0, systemMessage=0) at IOService.cpp:5284 [opt]
    frame #8: 0xffffff800da18fc3 kernel.development`IOResources::setProperties(this=<unavailable>, properties=0xffffff801cbaa940) at IOService.cpp:5325 [opt]
    frame #9: 0xffffff800da6d141 kernel.development`::is_io_registry_entry_set_properties(registry_entry=0xffffff8018b05fa0, properties=<unavailable>, propertiesCnt=<unavailable>, result=0xffffff80216013b8) at IOUserClient.cpp:3272 [opt]
    frame #10: 0xffffff800de82948 kernel.development`_IOServiceNullNotifier::gMetaClass + 40
    frame #11: 0xffffff801ec6d5e0
    frame #12: 0xffffff800d381d07 kernel.development`ipc_kobject_server(request=0xffffff802160138c, option=<unavailable>) at ipc_kobject.c:351 [opt]
    frame #13: 0xffffff800d354d0d kernel.development`ipc_kmsg_send(kmsg=0xffffff8021022b00, option=3, send_timeout=0) at ipc_kmsg.c:1867 [opt]
    frame #14: 0xffffff800d36fafb kernel.development`mach_msg_overwrite_trap(args=<unavailable>) at mach_msg.c:570 [opt]
    frame #15: 0xffffff800d4be0ea kernel.development`mach_call_munger64(state=0xffffff801b5db5e0) at bsd_i386.c:573 [opt]
    frame #16: 0xffffff800d320a56 kernel.development`hndl_mach_scall64 + 22
```

A service registration by a WiFi driver

```
  * frame #0: 0xffffff800da155a4 kernel.development`_IOServiceJob::startJob(nub=0xffffff8019645800, type=10, options=0) at IOService.cpp:956 [opt]
    frame #1: 0xffffff800da0e4e3 kernel.development`IOService::registerService(this=0xffffff8019645800, options=0) at IOService.cpp:814 [opt]
    frame #2: 0xffffff7f8edeee52 IO80211Family`IO80211Interface::updateBSSIDProperty() + 206
    frame #3: 0xffffff7f8edee7e9 IO80211Family`IO80211Interface::postMessage(unsigned int, void*, unsigned long) + 2115
    frame #4: 0xffffff7f8eecd18c AirPortBrcm4360`AirPort_Brcm4360::wlEvent(char*, wlc_event*) + 8354
    frame #5: 0xffffff7f8efa4498 AirPortBrcm4360`wlc_process_eventq + 28
    frame #6: 0xffffff7f8efe2f6e AirPortBrcm4360`wlc_timer_cb + 30
    frame #7: 0xffffff7f8eed5a2c AirPortBrcm4360`wl_timer_callback(OSObject*, IOTimerEventSource*) + 94
    frame #8: 0xffffff800da42e4c kernel.development`InvokeAction(action=(AirPortBrcm4360`wl_timer_callback(OSObject*, IOTimerEventSource*)), ts=0xffffff80193a0780, owner=0xffffff8077e68000, workLoop=0xffffff8018d29aa0)(OSObject*, IOTimerEventSource*), IOTimerEventSource*, OSObject*, IOWorkLoop*) at IOTimerEventSource.cpp:109 [opt]
    frame #9: 0xffffff800da42d76 kernel.development`IOTimerEventSource::timeoutAndRelease(self=0xffffff80193a0780, c=<unavailable>) at IOTimerEventSource.cpp:167 [opt]
    frame #10: 0xffffff800d3bc0b0 kernel.development`thread_call_invoke(func=(kernel.development`IOTimerEventSource::timeoutAndRelease(void*, void*) at IOTimerEventSource.cpp:147), param0=<unavailable>, param1=<unavailable>, call=0xffffff8019397960) at thread_call.c:1387 [opt]
    frame #11: 0xffffff800d3bbc36 kernel.development`thread_call_thread(group=<unavailable>, wres=<unavailable>) at thread_call.c:1463 [opt]
    frame #12: 0xffffff800d31f5c7 kernel.development`call_continuation + 23
```
