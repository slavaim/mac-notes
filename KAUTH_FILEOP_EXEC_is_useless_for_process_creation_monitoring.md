There is a believe that ```KAUTH_SCOPE_FILEOP``` scope can be used to track process creation ( e.g. see "Monitoring Process Creation via the Kernel (Part II)" at https://objective-see.com/blog/blog_0x0A.html ). Actually ```KAUTH_SCOPE_FILEOP``` scope misses an important part to be useful for process creation monitoring - ```vfs_context_t``` pointer for a processs in which context an image is being loaded. Another type of callback - ```KAUTH_SCOPE_VNODE``` - provide it as a callback parameter but this callback is not of much use to detect process creation. A callback for ```KAUTH_FILEOP_EXEC``` can be called in a context of a process that called ```posix_spawn``` to create a new process. A missing ```vfs_context_t``` or ```proc_t``` pointer for a new process makes it impossible to get any information about a new process except having its image file vnode. Below is a snapshot from a mentioned above "Monitoring Process Creation via the Kernel (Part II)" article for a KAUTH listener output when ```proc_selfpid()``` is used inside ```KAUTH_SCOPE_FILEOP``` callback
  
![alt text](https://objective-see.com/images/blog/blog_0x0A/procNotifications.png)
   
The reported PID for ```xpcproxy``` processes is 1 which is ```launchd``` PID - a parent for ```xpcproxy```. The reported PID for a spindump process is 356 which is a correct one because this is a PID of a process that called ```execve()``` (this process is ```xpcproxy``` in this case for which a wrong PID was reported). When process calls ```execve()``` its address space is being replaced with a new address space but it saves its PID ( it also saves BSD process structure but get a new Mach task ) so ```proc_selfpid()``` reports a correct PID. That said, the reported PID is not consistent - it is correct PID in one case and incorrect in another.

Below is a call stack for a KAUTH callback callled from ```posix_spawn```

```
frame #2: 0xffffff8007ee2e93 kernel`kauth_authorize_fileop [inlined] kauth_authorize_action(scope=<unavailable>, credential=0xffffff801374f5f0, action=<unavailable>, arg0=<unavailable>, arg3=0x0000000000000000) at kern_authorization.c:422 [opt]
frame #3: 0xffffff8007ee2e16 kernel`kauth_authorize_fileop(credential=0xffffff801374f5f0, action=6, arg0=0xffffff801429fd10, arg1=0xffffff8013325000) at kern_authorization.c:567 [opt]
frame #4: 0xffffff8007f0e141 kernel`exec_activate_image(imgp=0xffffff8018adb080) at kern_exec.c:1526 [opt]
frame #5: 0xffffff8007f0ccb7 kernel`posix_spawn(ap=0xffffff8019fcf910, uap=<unavailable>, retval=0xffffff8016f44058) at kern_exec.c:2790 [opt]
frame #6: 0xffffff8007ffb3e8 kernel`unix_syscall64(state=<unavailable>) at systemcalls.c:382 [opt]
frame #7: 0xffffff8007a02906 kernel`hndl_unix_scall64 + 22
 ```
