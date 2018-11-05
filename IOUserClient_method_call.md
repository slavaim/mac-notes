
```
  * frame #0: 0xffffff7fa9902d93 XXXX
    frame #1: 0xffffff8026c67275 kernel.development`::shim_io_connect_method_scalarI_scalarO(method=<unavailable>, object=<unavailable>, input=0xffffff803d0e8518, inputCount=0, output=0xffffff90cb2d3d10, outputCount=0xffffff90cb2d3bd0) at IOUserClient.cpp:4171 [opt]
    frame #2: 0xffffff8026c65ae8 kernel.development`IOUserClient::externalMethod(this=<unavailable>, selector=<unavailable>, args=<unavailable>, dispatch=0x0000000000000000, target=0x0000000000000000, reference=<unavailable>) at IOUserClient.cpp:5423 [opt]
    frame #3: 0xffffff8026c6e7b7 kernel.development`::is_io_connect_method(connection=0xffffff803be806d0, selector=5, scalar_input=<unavailable>, scalar_inputCnt=<unavailable>, inband_input=<unavailable>, inband_inputCnt=0, ool_input=<unavailable>, ool_input_size=<unavailable>, inband_output=<unavailable>, inband_outputCnt=<unavailable>, scalar_output=<unavailable>, scalar_outputCnt=<unavailable>, ool_output=<unavailable>, ool_output_size=<unavailable>) at IOUserClient.cpp:3971 [opt]
    frame #4: 0xffffff802668a284 kernel.development`_Xio_connect_method(InHeadP=<unavailable>, OutHeadP=0xffffff803c71bde0) at device_server.c:8379 [opt]
    frame #5: 0xffffff8026581d07 kernel.development`ipc_kobject_server(request=0xffffff803d0e8480, option=<unavailable>) at ipc_kobject.c:351 [opt]
    frame #6: 0xffffff8026554d0d kernel.development`ipc_kmsg_send(kmsg=0xffffff803d0e8480, option=3, send_timeout=0) at ipc_kmsg.c:1867 [opt]
    frame #7: 0xffffff802656fafb kernel.development`mach_msg_overwrite_trap(args=<unavailable>) at mach_msg.c:570 [opt]
    frame #8: 0xffffff80266be0ea kernel.development`mach_call_munger64(state=0xffffff803a3800a0) at bsd_i386.c:573 [opt]
    frame #9: 0xffffff8026520a56 kernel.development`hndl_mach_scall64 + 22
```
