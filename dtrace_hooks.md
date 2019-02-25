
A syscall hook activation

```
  * frame #0: 0xffffff8005dc4154 kernel`systrace_enable(arg=0x0000000000000000, id=162, parg=0x0000000000010005) at systrace.c:476 [opt]
    frame #1: 0xffffff8005b6ea27 kernel`dtrace_ecb_create_enable at dtrace.c:10312 [opt]
    frame #2: 0xffffff8005b6ea03 kernel`dtrace_ecb_create_enable(probe=<unavailable>, arg1=<unavailable>, arg2=<unavailable>) at dtrace.c:11118 [opt]
    frame #3: 0xffffff8005b61402 kernel`dtrace_match(pkp=0xffffff90ab08b898, priv=31, uid=0, zoneid=0, matched=(kernel`dtrace_ecb_create_enable at dtrace.c:11098), arg1=0xffffff80210c5fc0, arg2=0xffffff801e5674b0) at dtrace.c:7764 [opt]
    frame #4: 0xffffff8005b6418f kernel`_dtrace_ioctl [inlined] dtrace_probe_enable(desc=<unavailable>, enab=<unavailable>, ep=0xffffff801e5674b0) at dtrace.c:8583 [opt]
    frame #5: 0xffffff8005b64125 kernel`_dtrace_ioctl at dtrace.c:12003 [opt]
    frame #6: 0xffffff8005b640fb kernel`_dtrace_ioctl at dtrace.c:17264 [opt]
    frame #7: 0xffffff8005b63f0d kernel`_dtrace_ioctl(dev=<unavailable>, cmd=<unavailable>, data=<unavailable>, fflag=<unavailable>, p=<unavailable>) at dtrace.c:18287 [opt]
    frame #8: 0xffffff8005e38caa kernel`spec_ioctl(ap=0xffffff90ab08bb90) at spec_vnops.c:694 [opt]
    frame #9: 0xffffff8005e2d43f kernel`VNOP_IOCTL(vp=0xffffff80141d2648, command=536896518, data="", fflag=3, ctx=0xffffff90ab08be58) at kpi_vfs.c:3600 [opt]
    frame #10: 0xffffff8005e1f17b kernel`vn_ioctl(fp=0xffffff801b291fc0, com=536896518, data="", ctx=0xffffff90ab08be58) at vfs_vnops.c:1524 [opt]
    frame #11: 0xffffff8006098f0b kernel`fo_ioctl(fp=0xffffff801b291fc0, com=536896518, data="", ctx=0xffffff90ab08be58) at kern_descrip.c:5979 [opt]
    frame #12: 0xffffff80060eee8c kernel`ioctl(p=0xffffff8014501630, uap=<unavailable>, retval=<unavailable>) at sys_generic.c:921 [opt]
    frame #13: 0xffffff80061b619b kernel`unix_syscall64(state=<unavailable>) at systemcalls.c:381 [opt]
    frame #14: 0xffffff8005b5c466 kernel`hndl_unix_scall64 + 22
```

Processing a hook in a victim's context

```
  * frame #0: 0xffffff8005b668aa kernel`dtrace_dif_emulate(difo=0xffffff801ac27ac0, mstate=0xffffff90c455bab0, vstate=0xffffff801695ac30, state=0xffffff801695ac00) at dtrace.c:5062 [opt]
    frame #1: 0xffffff8005b5d470 kernel`__dtrace_probe(id=<unavailable>, arg0=4, arg1=<unavailable>, arg2=4, arg3=18446743524332645376, arg4=<unavailable>) at dtrace.c:6569 [opt]
    frame #2: 0xffffff8005b5ceaf kernel`dtrace_probe(id=163, arg0=4, arg1=4, arg2=0, arg3=0, arg4=0) at dtrace.c:7000 [opt]
    frame #3: 0xffffff8005dc3851 kernel`dtrace_systrace_syscall(pp=0xffffff80198561c0, uap=0xffffff8015325760, rv=<unavailable>) at systrace.c:297 [opt]
    frame #4: 0xffffff80061b619b kernel`unix_syscall64(state=<unavailable>) at systemcalls.c:381 [opt]
    frame #5: 0xffffff8005b5c466 kernel`hndl_unix_scall64 + 22
```

Getting a string from a victim's address space

```
  * frame #0: 0xffffff8005dbec64 kernel`dtrace_copycheck [inlined] get_active_thread at cpu_data.h:394 [opt]
    frame #1: 0xffffff8005dbec64 kernel`dtrace_copycheck [inlined] current_thread at machine_routines.c:803 [opt]
    frame #2: 0xffffff8005dbec64 kernel`dtrace_copycheck(uaddr=123145422286672, kaddr=0x0000000000000000, size=256) at dtrace_glue.c:1028 [opt]
    frame #3: 0xffffff8005dbed5b kernel`dtrace_copyinstr(src=123145422286672, dst=0xffffff90c7665239, len=256, flags=<unavailable>) at dtrace_glue.c:1064 [opt]
    frame #4: 0xffffff8005b6bad9 kernel`dtrace_dif_subr(subr=3345371705, rd=1, regs=0xffffff800de33950, tupregs=0xffffff800de33990, nargs=302477248, mstate=0xffffff800de33ab0, state=0xffffff8012ba8300) at dtrace.c:3868 [opt]
    frame #5: 0xffffff8005b67484 kernel`dtrace_dif_emulate(difo=<unavailable>, mstate=0xffffff800de33ab0, vstate=0xffffff8012ba8330, state=0xffffff8012ba8300) at dtrace.c:5623 [opt]
    frame #6: 0xffffff8005b5d553 kernel`__dtrace_probe(id=<unavailable>, arg0=18446744073709551615, arg1=<unavailable>, arg2=18446744073709551615, arg3=18446743524267950848, arg4=<unavailable>) at dtrace.c:6697 [opt]
    frame #7: 0xffffff8005b5ceaf kernel`dtrace_probe(id=163, arg0=18446744073709551615, arg1=18446744073709551615, arg2=18446744073709551615, arg3=16, arg4=0) at dtrace.c:7000 [opt]
    frame #8: 0xffffff8005dc3851 kernel`dtrace_systrace_syscall(pp=0xffffff80125a2b70, uap=0xffffff8013ec0418, rv=<unavailable>) at systrace.c:297 [opt]
    frame #9: 0xffffff80061b619b kernel`unix_syscall64(state=<unavailable>) at systemcalls.c:381 [opt]
    frame #10: 0xffffff8005b5c466 kernel`hndl_unix_scall64 + 22
```
