A KEXT can be unloaded if it has only `IKOT_IOKIT_CONNECT` port type references. `IKOT_IOKIT_CONNECT` references `IOUserClass` object and is created by a call to `IOServiceOpen`. There is special processing for `IKOT_IOKIT_CONNECT` ports to synchronize with ongoing calls to `IOUserClass` methods and to invalidate a port object. In contrast `IOServiceGetMatchingServices` creates a port object of type `IKOT_IOKIT_OBJECT` and it references the related object preventing a KEXT from unloading.
An `IKOT_IOKIT_CONNECT` reference to `IOUserClass` object is released by a call to `iokit_destroy_object_port` from `IOMachPort::free` which in turn is called as a result of releasing `IOMachPort` by `IOUserClient::destroyUserReferences`. A task (i.e. a client) still have a reference to a port object but the port doesn't reference `IOUserClass` object. This allows a `KEXT` to be unloaded with open connections. A relevant call stack is as follows.

```
  * frame #0: 0xffffff800f8840e9 kernel`IOMachPort::free(this=0xffffff803805d480) at IOUserClient.cpp:368:6 [opt]
    frame #1: 0xffffff800f889f18 kernel`IOUserClient::destroyUserReferences(obj=0xffffff803845e900) at IOUserClient.cpp:351:10 [opt]
    frame #2: 0xffffff800f82ee31 kernel`IOService::terminatePhase1(this=<unavailable>, options=7) at IOService.cpp:2274:4 [opt]
    frame #3: 0xffffff800f8502dd kernel`IOCatalogue::_terminateDrivers(this=<unavailable>, matching=0xffffff803cb0f700) at IOCatalogue.cpp:603:18 [opt]
    frame #4: 0xffffff800f8504e4 kernel`IOCatalogue::terminateDriversForModule(this=0xffffff80316d2fc0, moduleName=0xffffff8031b63f20, unload=false) at IOCatalogue.cpp:703:8 [opt]
    frame #5: 0xffffff800f7df9c1 kernel`OSKext::removeKext(OSKext*, bool) [inlined] IOCatalogue::terminateDriversForModule(this=0xffffff80316d2fc0, moduleName=<unavailable>, unload=false) at IOCatalogue.cpp:738:8 [opt]
    frame #6: 0xffffff800f7df9a0 kernel`OSKext::removeKext(aKext=0xffffff803733b100, terminateServicesAndRemovePersonalitiesFlag=true) at OSKext.cpp:3613 [opt]
    frame #7: 0xffffff800f7e62fd kernel`OSKext::handleRequest(hostPriv=<unavailable>, clientLogFilter=<unavailable>, requestBuffer=<unavailable>, requestLength=<unavailable>, responseOut=0xffffff91437d3cd8, responseLengthOut=0xffffff91437d3ce8, logInfoOut=0xffffff91437d3ce0, logInfoLengthOut=0xffffff91437d3cf4) at OSKext.cpp:8177:13 [opt]
    frame #8: 0xffffff800f7f627c kernel`::kext_request(hostPriv=0xffffff800fa9ab08, clientLogSpec=4082, requestIn=<unavailable>, requestLengthIn=222, responseOut=<unavailable>, responseLengthOut=<unavailable>, logDataOut=0xffffff803d65441c, logDataLengthOut=0xffffff803d654438, op_result=0xffffff803d65443c) at OSKextLib.cpp:296:16 [opt]
    frame #9: 0xffffff800f20e9b7 kernel`_Xkext_request(InHeadP=0xffffff803bbe1478, OutHeadP=0xffffff803d6543e8) at host_priv_server.c:2785:12 [opt]
    frame #10: 0xffffff800f1b506c kernel`ipc_kobject_server(request=0xffffff803bbe1400, option=<unavailable>) at ipc_kobject.c:361:4 [opt]
    frame #11: 0xffffff800f18fa91 kernel`ipc_kmsg_send(kmsg=0xffffff803bbe1400, option=3, send_timeout=0) at ipc_kmsg.c:1868:10 [opt]
    frame #12: 0xffffff800f1a42fe kernel`mach_msg_overwrite_trap(args=<unavailable>) at mach_msg.c:553:8 [opt]
    frame #13: 0xffffff800f2c3e87 kernel`mach_call_munger64(state=0xffffff803ac5a1c0) at bsd_i386.c:599:24 [opt]
    frame #14: 0xffffff800f15d2c6 kernel`hndl_mach_scall64 + 22
```

`IOMachPort::free` is a result of  a call to `OSDictionary::removeObject` which calls `IOMachPort::taggedRelease`. These calls are not visible on the stack because of the compiler optimization.

A port object is also released from `IOMachPort::noMoreSendersForObject` when a client task terminates or closes a connection. When a client (i.e. task) closes a connection with a call to `IOServiceClose` a port is destroyed and a `IOUserClass` reference is released. The relevant call stack is

```
  * frame #0: 0xffffff800f8840e9 kernel`IOMachPort::free(this=0xffffff80380fe7e0) at IOUserClient.cpp:368:6 [opt]
    frame #1: 0xffffff800f889a51 kernel`IOMachPort::noMoreSendersForObject(obj=0xffffff803733b280, type=30, mscount=0xffffff912d983cec) at IOUserClient.cpp:271:11 [opt]
    frame #2: 0xffffff800f88a329 kernel`::iokit_client_died(obj=0xffffff803733b280, (null)=<unavailable>, type=30, mscount=<unavailable>) at IOUserClient.cpp:609:7 [opt]
    frame #3: 0xffffff800f290e23 kernel`iokit_notify at iokit_rpc.c:363:24 [opt]
    frame #4: 0xffffff800f290dcd kernel`iokit_notify(msg=0xffffff803d58b890) at iokit_rpc.c:389 [opt]
    frame #5: 0xffffff800f1b50d4 kernel`ipc_kobject_server(request=0xffffff803d58b800, option=327681) at ipc_kobject.c:373:9 [opt]
    frame #6: 0xffffff800f18fa91 kernel`ipc_kmsg_send(kmsg=0xffffff803d58b800, option=327681, send_timeout=0) at ipc_kmsg.c:1868:10 [opt]
    frame #7: 0xffffff800f1b577c kernel`mach_msg_send_from_kernel_proper(msg=<unavailable>, send_size=44) at ipc_mig.c:191:7 [opt]
    frame #8: 0xffffff800f19ba37 kernel`ipc_right_dealloc [inlined] mach_notify_no_senders(notify=0xffffff803d4d61f8, mscount=1) at mach_notify_user.c:378:15 [opt]
    frame #9: 0xffffff800f19b9f7 kernel`ipc_right_dealloc [inlined] ipc_notify_no_senders(port=0xffffff803d4d61f8, mscount=1) at ipc_notify.c:144 [opt]
    frame #10: 0xffffff800f19b9f7 kernel`ipc_right_dealloc(space=0xffffff803164e4a0, name=5383, entry=0xffffff80382181f8) at ipc_right.c:1004 [opt]
    frame #11: 0xffffff800f1a4e8d kernel`mach_port_deallocate(space=0xffffff803164e4a0, name=5383) at mach_port.c:829:7 [opt]
    frame #12: 0xffffff800f1a2983 kernel`_kernelrpc_mach_port_deallocate_trap(args=0xffffff912d983f10) at mach_kernelrpc.c:220:7 [opt]
    frame #13: 0xffffff800f2c3e87 kernel`mach_call_munger64(state=0xffffff8037cb6e20) at bsd_i386.c:599:24 [opt]
    frame #14: 0xffffff800f15d2c6 kernel`hndl_mach_scall64 + 22
```
