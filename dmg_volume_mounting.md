A collection of kernel stacks showing internal operations while creating/mounting a virtual volume from a dmg file.

```
(lldb) c
Process 1 resuming
Process 1 stopped
* thread #1210, name = '0xffffff801d729420', queue = '0x0', stop reason = breakpoint 1.1
    frame #0: 0xffffff800da155a4 kernel.development`_IOServiceJob::startJob(nub=0xffffff801f383e80, type=10, options=0) at IOService.cpp:956 [opt]
Target 0: (kernel.development) stopped.
(lldb) bt
* thread #1210, name = '0xffffff801d729420', queue = '0x0', stop reason = breakpoint 1.1
  * frame #0: 0xffffff800da155a4 kernel.development`_IOServiceJob::startJob(nub=0xffffff801f383e80, type=10, options=0) at IOService.cpp:956 [opt]
    frame #1: 0xffffff800da0e4e3 kernel.development`IOService::registerService(this=0xffffff801f383e80, options=0) at IOService.cpp:814 [opt]
    frame #2: 0xffffff7f8eacacef IOHDIXController`IOHDIXController::createDrive(OSDictionary*, int*, task*) + 1111
    frame #3: 0xffffff7f8eacc269 IOHDIXController`IOHDIXControllerUserClient::_createDriveOutKernel(void const*, int*, unsigned long) + 157
    frame #4: 0xffffff7f8eacc18d IOHDIXController`IOHDIXControllerUserClient::createDrive64(HDIImageCreateBlock64 const*, int*) + 113
    frame #5: 0xffffff800da67a29 kernel.development`::shim_io_connect_method_structureI_structureO(method=<unavailable>, object=<unavailable>, input=<unavailable>, inputCount=<unavailable>, output=<unavailable>, outputCount=0xffffff8014cebb38) at IOUserClient.cpp:0 [opt]
    frame #6: 0xffffff800da65bb0 kernel.development`IOUserClient::externalMethod(this=<unavailable>, selector=<unavailable>, args=0xffffff8014cebb80, dispatch=0x0000000000000000, target=0x0000000000000000, reference=<unavailable>) at IOUserClient.cpp:5436 [opt]
    frame #7: 0xffffff800da6e7b7 kernel.development`::is_io_connect_method(connection=0xffffff801bccde00, selector=0, scalar_input=<unavailable>, scalar_inputCnt=<unavailable>, inband_input=<unavailable>, inband_inputCnt=0, ool_input=<unavailable>, ool_input_size=<unavailable>, inband_output=<unavailable>, inband_outputCnt=<unavailable>, scalar_output=<unavailable>, scalar_outputCnt=<unavailable>, ool_output=<unavailable>, ool_output_size=<unavailable>) at IOUserClient.cpp:3971 [opt]
    frame #8: 0xffffff800d48a284 kernel.development`_Xio_connect_method(InHeadP=<unavailable>, OutHeadP=0xffffff802466bde0) at device_server.c:8379 [opt]
    frame #9: 0xffffff800d381d07 kernel.development`ipc_kobject_server(request=0xffffff8026a26000, option=<unavailable>) at ipc_kobject.c:351 [opt]
    frame #10: 0xffffff800d354d0d kernel.development`ipc_kmsg_send(kmsg=0xffffff8026a26000, option=3, send_timeout=0) at ipc_kmsg.c:1867 [opt]
    frame #11: 0xffffff800d36fafb kernel.development`mach_msg_overwrite_trap(args=<unavailable>) at mach_msg.c:570 [opt]
    frame #12: 0xffffff800d4be0ea kernel.development`mach_call_munger64(state=0xffffff801d77a420) at bsd_i386.c:573 [opt]
    frame #13: 0xffffff800d320a56 kernel.development`hndl_mach_scall64 + 22
(lldb) c
Process 1 resuming
Process 1 stopped
* thread #1210, name = '0xffffff801d729420', queue = '0x0', stop reason = breakpoint 1.1
    frame #0: 0xffffff800da155a4 kernel.development`_IOServiceJob::startJob(nub=0xffffff801bcaf460, type=10, options=0) at IOService.cpp:956 [opt]
Target 0: (kernel.development) stopped.
(lldb) bt
* thread #1210, name = '0xffffff801d729420', queue = '0x0', stop reason = breakpoint 1.1
  * frame #0: 0xffffff800da155a4 kernel.development`_IOServiceJob::startJob(nub=0xffffff801bcaf460, type=10, options=0) at IOService.cpp:956 [opt]
    frame #1: 0xffffff800da0e4e3 kernel.development`IOService::registerService(this=0xffffff801bcaf460, options=0) at IOService.cpp:814 [opt]
    frame #2: 0xffffff7f8eacd962 IOHDIXController`IOHDIXHDDrive::createNub() + 920
    frame #3: 0xffffff7f8eac8c3f IOHDIXController`IOHDIXHDDriveOutKernelUserClient::activateDrive() + 85
    frame #4: 0xffffff800da67275 kernel.development`::shim_io_connect_method_scalarI_scalarO(method=<unavailable>, object=<unavailable>, input=0xffffff8026a084d8, inputCount=0, output=0xffffff8014cebd10, outputCount=0xffffff8014cebbd0) at IOUserClient.cpp:4171 [opt]
    frame #5: 0xffffff800da65ae8 kernel.development`IOUserClient::externalMethod(this=<unavailable>, selector=<unavailable>, args=<unavailable>, dispatch=0x0000000000000000, target=0x0000000000000000, reference=<unavailable>) at IOUserClient.cpp:5423 [opt]
    frame #6: 0xffffff800da6e7b7 kernel.development`::is_io_connect_method(connection=0xffffff801bccd400, selector=0, scalar_input=<unavailable>, scalar_inputCnt=<unavailable>, inband_input=<unavailable>, inband_inputCnt=0, ool_input=<unavailable>, ool_input_size=<unavailable>, inband_output=<unavailable>, inband_outputCnt=<unavailable>, scalar_output=<unavailable>, scalar_outputCnt=<unavailable>, ool_output=<unavailable>, ool_output_size=<unavailable>) at IOUserClient.cpp:3971 [opt]
    frame #7: 0xffffff800d48a284 kernel.development`_Xio_connect_method(InHeadP=<unavailable>, OutHeadP=0xffffff8026a15de0) at device_server.c:8379 [opt]
    frame #8: 0xffffff800d381d07 kernel.development`ipc_kobject_server(request=0xffffff8026a08440, option=<unavailable>) at ipc_kobject.c:351 [opt]
    frame #9: 0xffffff800d354d0d kernel.development`ipc_kmsg_send(kmsg=0xffffff8026a08440, option=3, send_timeout=0) at ipc_kmsg.c:1867 [opt]
    frame #10: 0xffffff800d36fafb kernel.development`mach_msg_overwrite_trap(args=<unavailable>) at mach_msg.c:570 [opt]
    frame #11: 0xffffff800d4be0ea kernel.development`mach_call_munger64(state=0xffffff801d77a420) at bsd_i386.c:573 [opt]
    frame #12: 0xffffff800d320a56 kernel.development`hndl_mach_scall64 + 22
(lldb) c
Process 1 resuming
Process 1 stopped
* thread #1211, name = '0xffffff8020a30e30', queue = '0x0', stop reason = breakpoint 3.1
    frame #0: 0xffffff800da16694 kernel.development`IOService::startCandidate(this=0xffffff801bcaf460, service=0xffffff8026a838f0) at IOService.cpp:3510 [opt]
Target 0: (kernel.development) stopped.
(lldb) bt
* thread #1211, name = '0xffffff8020a30e30', queue = '0x0', stop reason = breakpoint 3.1
  * frame #0: 0xffffff800da16694 kernel.development`IOService::startCandidate(this=0xffffff801bcaf460, service=0xffffff8026a838f0) at IOService.cpp:3510 [opt]
    frame #1: 0xffffff800da16413 kernel.development`IOService::probeCandidates(this=<unavailable>, matches=<unavailable>) at IOService.cpp:3448 [opt]
    frame #2: 0xffffff800da158cd kernel.development`IOService::doServiceMatch(this=0xffffff801bcaf460, options=<unavailable>) at IOService.cpp:3758 [opt]
    frame #3: 0xffffff800da17366 kernel.development`_IOConfigThread::main(arg=0xffffff8023bfd430, result=<unavailable>) at IOService.cpp:4130 [opt]
    frame #4: 0xffffff800d31f5c7 kernel.development`call_continuation + 23
(lldb) c
Process 1 resuming
Process 1 stopped
* thread #1211, name = '0xffffff8020a30e30', queue = '0x0', stop reason = breakpoint 1.1
    frame #0: 0xffffff800da155a4 kernel.development`_IOServiceJob::startJob(nub=0xffffff8024f32bc0, type=10, options=8) at IOService.cpp:956 [opt]
Target 0: (kernel.development) stopped.
(lldb) bt
* thread #1211, name = '0xffffff8020a30e30', queue = '0x0', stop reason = breakpoint 1.1
  * frame #0: 0xffffff800da155a4 kernel.development`_IOServiceJob::startJob(nub=0xffffff8024f32bc0, type=10, options=8) at IOService.cpp:956 [opt]
    frame #1: 0xffffff800da0e4e3 kernel.development`IOService::registerService(this=0xffffff8024f32bc0, options=8) at IOService.cpp:814 [opt]
    frame #2: 0xffffff7f8dc55905 IOStorageFamily`IOBlockStorageDriver::acceptNewMedia(this=<unavailable>) at IOBlockStorageDriver.cpp:842 [opt]
    frame #3: 0xffffff7f8dc55af9 IOStorageFamily`IOBlockStorageDriver::mediaStateHasChanged(this=0xffffff8026a838f0, state=<unavailable>) at IOBlockStorageDriver.cpp:909 [opt]
    frame #4: 0xffffff7f8dc559d5 IOStorageFamily`IOBlockStorageDriver::checkForMedia(this=0xffffff8026a838f0) at IOBlockStorageDriver.cpp:866 [opt]
    frame #5: 0xffffff7f8dc565cb IOStorageFamily`IOBlockStorageDriver::handleStart(this=0xffffff8026a838f0, provider=<unavailable>) at IOBlockStorageDriver.cpp:1353 [opt]
    frame #6: 0xffffff7f8dc54a87 IOStorageFamily`IOBlockStorageDriver::start(this=0xffffff8026a838f0, provider=0xffffff801bcaf460) at IOBlockStorageDriver.cpp:199 [opt]
    frame #7: 0xffffff800da166ed kernel.development`IOService::startCandidate(this=0xffffff801bcaf460, service=0xffffff8026a838f0) at IOService.cpp:3529 [opt]
    frame #8: 0xffffff800da16413 kernel.development`IOService::probeCandidates(this=<unavailable>, matches=<unavailable>) at IOService.cpp:3448 [opt]
    frame #9: 0xffffff800da158cd kernel.development`IOService::doServiceMatch(this=0xffffff801bcaf460, options=<unavailable>) at IOService.cpp:3758 [opt]
    frame #10: 0xffffff800da17366 kernel.development`_IOConfigThread::main(arg=0xffffff8023bfd430, result=<unavailable>) at IOService.cpp:4130 [opt]
    frame #11: 0xffffff800d31f5c7 kernel.development`call_continuation + 23
(lldb) c
Process 1 resuming
Process 1 stopped
* thread #1211, name = '0xffffff8020a30e30', queue = '0x0', stop reason = breakpoint 1.1
    frame #0: 0xffffff800da155a4 kernel.development`_IOServiceJob::startJob(nub=0xffffff8026a838f0, type=10, options=0) at IOService.cpp:956 [opt]
Target 0: (kernel.development) stopped.
(lldb) bt
* thread #1211, name = '0xffffff8020a30e30', queue = '0x0', stop reason = breakpoint 1.1
  * frame #0: 0xffffff800da155a4 kernel.development`_IOServiceJob::startJob(nub=0xffffff8026a838f0, type=10, options=0) at IOService.cpp:956 [opt]
    frame #1: 0xffffff800da0e4e3 kernel.development`IOService::registerService(this=0xffffff8026a838f0, options=0) at IOService.cpp:814 [opt]
    frame #2: 0xffffff7f8dc54aae IOStorageFamily`IOBlockStorageDriver::start(this=0xffffff8026a838f0, provider=0xffffff801bcaf460) at IOBlockStorageDriver.cpp:213 [opt]
    frame #3: 0xffffff800da166ed kernel.development`IOService::startCandidate(this=0xffffff801bcaf460, service=0xffffff8026a838f0) at IOService.cpp:3529 [opt]
    frame #4: 0xffffff800da16413 kernel.development`IOService::probeCandidates(this=<unavailable>, matches=<unavailable>) at IOService.cpp:3448 [opt]
    frame #5: 0xffffff800da158cd kernel.development`IOService::doServiceMatch(this=0xffffff801bcaf460, options=<unavailable>) at IOService.cpp:3758 [opt]
    frame #6: 0xffffff800da17366 kernel.development`_IOConfigThread::main(arg=0xffffff8023bfd430, result=<unavailable>) at IOService.cpp:4130 [opt]
    frame #7: 0xffffff800d31f5c7 kernel.development`call_continuation + 23
(lldb) c
```
