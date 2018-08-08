
```+[NSFileWrapper _canSafelyMapFilesAtPath:]``` recursively processes virtual disks(images) up to a root file system. It accepts an NSString object with a path to any file on a mounted image. Below is a pseudocode produced by Hopper Disassembler.

```
/* @class NSFileWrapper */
+(char)_canSafelyMapFilesAtPath:(void *)arg2 {
    var_30 = **___stack_chk_guard;
    r13 = [arg2 fileSystemRepresentation];
    rax = statfs$INODE64(r13, &var_8A8);
    if (rax == 0x0) goto loc_41cd2;

loc_41ca7:
    rax = 0x0;
    goto loc_41ca9;

loc_41ca9:
    if (**___stack_chk_guard == var_30) {
            rax = rax & 0xff;
    }
    else {
            rax = __stack_chk_fail();
    }
    return rax;

loc_41cd2:
    os_unfair_lock_lock(_volumeIsSafeForMapping.lock);
    r14 = &var_450;
    if ((*0x4c4b60 == 0x0) || (strcmp(*0x4c4b60, r14) != 0x0)) goto loc_41d01;

loc_41eb5:
    os_unfair_lock_unlock(_volumeIsSafeForMapping.lock);
    r12 = *(int8_t *)0x4c4b68;
    goto loc_41f6f;

loc_41f6f:
    if (r12 != 0x0) {
            if (*0x4c4b80 != 0xffffffffffffffff) {
                    dispatch_once(0x4c4b80, ^ {/* block implemented at ___fileIsSafeForMapping_block_invoke */ } });
            }
            if (*0x4c4b78 != 0x0) {
                    rax = loc_4c4b78(r13, &var_1148);
                    rax = (var_1148 != 0x5 ? 0x1 : 0x0) & (rax == 0x0 ? 0x1 : 0x0);
            }
            else {
                    rax = 0x0;
            }
    }
    else {
            rax = 0x0;
    }
    goto loc_41ca9;

loc_41d01:
    os_unfair_lock_unlock(_volumeIsSafeForMapping.lock);
    rbx = [NSString stringWithCString:r14 encoding:0x4];
    if ([rbx hasPrefix:@"/dev/"] == 0x0) goto loc_41ca7;

loc_41d4c:
    rbx = [rbx substringFromIndex:0x5, 0x4];
    if ([rbx length] == 0x0) goto loc_41ca7;

loc_41d77:
    r14 = *(int32_t *)*_kIOMasterPortDefault;
    rax = [rbx fileSystemRepresentation];
    r12 = 0x0;
    rax = IOBSDNameMatching(r14, 0x0, rax);
    if (rax != 0x0) {
            r14 = IOServiceGetMatchingService(r14, rax);
            if (r14 != 0x0) {
                    r12 = 0x0;
                    rax = IORegistryEntrySearchCFProperty(r14, "IOService", @"Protocol Characteristics", 0x0, 0x3);
                    if (rax != 0x0) {
                            var_1150 = rax;
                            rbx = CFDictionaryGetValue(rax, @"Physical Interconnect Location");
                            if (rbx != 0x0) {
                                    if (CFStringCompare(rbx, @"Internal", 0x0) != 0x0) {
                                            r12 = 0x0;
                                            rax = CFStringCompare(rbx, @"File", 0x0);
                                            rbx = var_1150;
                                            if (rax == 0x0) {
                                                    rbx = CFDictionaryGetValue(rbx, @"Virtual Interface Location Path");
                                                    if (rbx != 0x0) {
                                                            rbx = [[NSString alloc] initWithData:rbx encoding:0x4];
                                                            if ([rbx length] != 0x0) {
                                                                    r12 = [NSFileWrapper _canSafelyMapFilesAtPath:rbx];
                                                            }
                                                            else {
                                                                    r12 = 0x0;
                                                            }
                                                            rdi = rbx;
                                                            rbx = var_1150;
                                                            [rdi release];
                                                    }
                                                    else {
                                                            r12 = 0x0;
                                                            rbx = var_1150;
                                                    }
                                            }
                                    }
                                    else {
                                            r12 = 0x1;
                                            rbx = var_1150;
                                    }
                            }
                            else {
                                    r12 = 0x0;
                                    rbx = var_1150;
                            }
                            CFRelease(rbx);
                    }
                    IOObjectRelease(r14);
            }
            else {
                    r12 = 0x0;
            }
    }
    *(&var_1128 - 0x20) = *__NSConcreteStackBlock;
    *(int32_t *)(&var_1128 - 0x18) = 0xffffffffc0000000;
    *(int32_t *)(&var_1128 - 0x14) = 0x0;
    *(&var_1128 - 0x10) = ___volumeIsSafeForMapping_block_invoke;
    *(&var_1128 - 0x8) = ___block_descriptor_tmp.528;
    memcpy(&var_1128, &var_8A8, 0x878);
    *(int8_t *)(&var_1128 + 0x878) = r12;
    if (*0x4c4b70 != 0xffffffffffffffff) {
            rax = dispatch_once(0x4c4b70, &var_1148);
    }
    goto loc_41f6f;
}
```

A disassembled function is below

```
                     +[NSFileWrapper _canSafelyMapFilesAtPath:]:
0000000000041c5c         push       rbp                                         ; Objective C Implementation defined at 0x47ef68 (class method), DATA XREF=0x47ef68
0000000000041c5d         mov        rbp, rsp
0000000000041c60         push       r15
0000000000041c62         push       r14
0000000000041c64         push       r13
0000000000041c66         push       r12
0000000000041c68         push       rbx
0000000000041c69         sub        rsp, 0x1128
0000000000041c70         mov        rax, qword [___stack_chk_guard_3c7428]      ; ___stack_chk_guard_3c7428
0000000000041c77         mov        rax, qword [rax]
0000000000041c7a         mov        qword [rbp+var_30], rax
0000000000041c7e         mov        r12, qword [0x4a2648]                       ; @selector(fileSystemRepresentation)
0000000000041c85         mov        rdi, rdx                                    ; argument "instance" for method _objc_msgSend
0000000000041c88         mov        rsi, r12                                    ; argument "selector" for method _objc_msgSend
0000000000041c8b         call       qword [_objc_msgSend_3c7a08]                ; _objc_msgSend, _objc_msgSend_3c7a08,_objc_msgSend
0000000000041c91         mov        r13, rax
0000000000041c94         lea        rsi, qword [rbp+var_8A8]
0000000000041c9b         mov        rdi, r13
0000000000041c9e         call       imp___stubs__statfs$INODE64                 ; statfs$INODE64
0000000000041ca3         test       eax, eax
0000000000041ca5         je         loc_41cd2

                     loc_41ca7:
0000000000041ca7         xor        eax, eax                                    ; CODE XREF=+[NSFileWrapper _canSafelyMapFilesAtPath:]+234, +[NSFileWrapper _canSafelyMapFilesAtPath:]+277, +[NSFileWrapper _canSafelyMapFilesAtPath:]+790, +[NSFileWrapper _canSafelyMapFilesAtPath:]+816

                     loc_41ca9:
0000000000041ca9         mov        rcx, qword [___stack_chk_guard_3c7428]      ; ___stack_chk_guard_3c7428, CODE XREF=+[NSFileWrapper _canSafelyMapFilesAtPath:]+850
0000000000041cb0         mov        rcx, qword [rcx]
0000000000041cb3         cmp        rcx, qword [rbp+var_30]
0000000000041cb7         jne        loc_41fdd

0000000000041cbd         movzx      eax, al
0000000000041cc0         add        rsp, 0x1128
0000000000041cc7         pop        rbx
0000000000041cc8         pop        r12
0000000000041cca         pop        r13
0000000000041ccc         pop        r14
0000000000041cce         pop        r15
0000000000041cd0         pop        rbp
0000000000041cd1         ret
                        ; endp

                     loc_41cd2:
0000000000041cd2         lea        rdi, qword [_volumeIsSafeForMapping.lock]   ; argument "lock" for method imp___stubs__os_unfair_lock_lock, _volumeIsSafeForMapping.lock, CODE XREF=+[NSFileWrapper _canSafelyMapFilesAtPath:]+73
0000000000041cd9         call       imp___stubs__os_unfair_lock_lock            ; os_unfair_lock_lock
0000000000041cde         mov        rdi, qword [__prepareForDispatch.installedPreDispatchHandler+4096] ; 0x4c4b60
0000000000041ce5         test       rdi, rdi
0000000000041ce8         lea        r14, qword [rbp+var_450]
0000000000041cef         je         loc_41d01

0000000000041cf1         mov        rsi, r14                                    ; argument "s2" for method imp___stubs__strcmp
0000000000041cf4         call       imp___stubs__strcmp                         ; strcmp
0000000000041cf9         test       eax, eax
0000000000041cfb         je         loc_41eb5

                     loc_41d01:
0000000000041d01         lea        rdi, qword [_volumeIsSafeForMapping.lock]   ; argument "lock" for method imp___stubs__os_unfair_lock_unlock, _volumeIsSafeForMapping.lock, CODE XREF=+[NSFileWrapper _canSafelyMapFilesAtPath:]+147
0000000000041d08         call       imp___stubs__os_unfair_lock_unlock          ; os_unfair_lock_unlock
0000000000041d0d         mov        rdi, qword [objc_cls_ref_NSString]          ; argument "instance" for method _objc_msgSend, objc_cls_ref_NSString
0000000000041d14         mov        rsi, qword [0x4a7490]                       ; argument "selector" for method _objc_msgSend, @selector(stringWithCString:encoding:)
0000000000041d1b         mov        r15, qword [_objc_msgSend_3c7a08]           ; _objc_msgSend_3c7a08
0000000000041d22         mov        ecx, 0x4
0000000000041d27         mov        rdx, r14
0000000000041d2a         call       r15                                         ; Jumps to 0x6d63c0 (_objc_msgSend), _objc_msgSend
0000000000041d2d         mov        rbx, rax
0000000000041d30         mov        rsi, qword [0x4a27a0]                       ; argument "selector" for method _objc_msgSend, @selector(hasPrefix:)
0000000000041d37         lea        rdx, qword [cfstring__dev_]                 ; @"/dev/"
0000000000041d3e         mov        rdi, rbx                                    ; argument "instance" for method _objc_msgSend
0000000000041d41         call       r15                                         ; Jumps to 0x6d63c0 (_objc_msgSend), _objc_msgSend
0000000000041d44         test       al, al
0000000000041d46         je         loc_41ca7

0000000000041d4c         mov        rsi, qword [0x4a2e18]                       ; argument "selector" for method _objc_msgSend, @selector(substringFromIndex:)
0000000000041d53         mov        edx, 0x5
0000000000041d58         mov        rdi, rbx                                    ; argument "instance" for method _objc_msgSend
0000000000041d5b         call       r15                                         ; Jumps to 0x6d63c0 (_objc_msgSend), _objc_msgSend
0000000000041d5e         mov        rbx, rax
0000000000041d61         mov        rsi, qword [0x4a2998]                       ; argument "selector" for method _objc_msgSend, @selector(length)
0000000000041d68         mov        rdi, rbx                                    ; argument "instance" for method _objc_msgSend
0000000000041d6b         call       r15                                         ; Jumps to 0x6d63c0 (_objc_msgSend), _objc_msgSend
0000000000041d6e         test       rax, rax
0000000000041d71         je         loc_41ca7

0000000000041d77         mov        rax, qword [_kIOMasterPortDefault_3c7980]   ; _kIOMasterPortDefault_3c7980
0000000000041d7e         mov        r14d, dword [rax]
0000000000041d81         mov        rdi, rbx                                    ; argument "instance" for method _objc_msgSend
0000000000041d84         mov        rsi, r12                                    ; argument "selector" for method _objc_msgSend
0000000000041d87         call       qword [_objc_msgSend_3c7a08]                ; _objc_msgSend, _objc_msgSend_3c7a08,_objc_msgSend
0000000000041d8d         xor        r12d, r12d
0000000000041d90         xor        esi, esi                                    ; argument "options" for method imp___stubs__IOBSDNameMatching
0000000000041d92         mov        edi, r14d                                   ; argument "masterPort" for method imp___stubs__IOBSDNameMatching
0000000000041d95         mov        rdx, rax                                    ; argument "bsdName" for method imp___stubs__IOBSDNameMatching
0000000000041d98         call       imp___stubs__IOBSDNameMatching              ; IOBSDNameMatching
0000000000041d9d         test       rax, rax
0000000000041da0         je         loc_41f14

0000000000041da6         mov        edi, r14d                                   ; argument "masterPort" for method imp___stubs__IOServiceGetMatchingService
0000000000041da9         mov        rsi, rax                                    ; argument "matching" for method imp___stubs__IOServiceGetMatchingService
0000000000041dac         call       imp___stubs__IOServiceGetMatchingService    ; IOServiceGetMatchingService
0000000000041db1         mov        r14d, eax
0000000000041db4         test       r14d, r14d
0000000000041db7         je         loc_41ecd

0000000000041dbd         lea        rsi, qword [aIoservice]                     ; argument "plane" for method imp___stubs__IORegistryEntrySearchCFProperty, "IOService"
0000000000041dc4         lea        rdx, qword [cfstring_Protocol_Characteristics] ; argument "key" for method imp___stubs__IORegistryEntrySearchCFProperty, @"Protocol Characteristics"
0000000000041dcb         xor        r12d, r12d
0000000000041dce         xor        ecx, ecx                                    ; argument "allocator" for method imp___stubs__IORegistryEntrySearchCFProperty
0000000000041dd0         mov        r8d, 0x3                                    ; argument "options" for method imp___stubs__IORegistryEntrySearchCFProperty
0000000000041dd6         mov        edi, r14d                                   ; argument "entry" for method imp___stubs__IORegistryEntrySearchCFProperty
0000000000041dd9         call       imp___stubs__IORegistryEntrySearchCFProperty ; IORegistryEntrySearchCFProperty
0000000000041dde         test       rax, rax
0000000000041de1         je         loc_41f0c

0000000000041de7         lea        rsi, qword [cfstring_Physical_Interconnect_Location] ; argument "key" for method imp___stubs__CFDictionaryGetValue, @"Physical Interconnect Location"
0000000000041dee         mov        qword [rbp+var_1150], rax
0000000000041df5         mov        rdi, rax                                    ; argument "theDict" for method imp___stubs__CFDictionaryGetValue
0000000000041df8         call       imp___stubs__CFDictionaryGetValue           ; CFDictionaryGetValue
0000000000041dfd         mov        rbx, rax
0000000000041e00         test       rbx, rbx
0000000000041e03         je         loc_41ed2

0000000000041e09         lea        rsi, qword [cfstring_Internal]              ; argument "theString2" for method imp___stubs__CFStringCompare, @"Internal"
0000000000041e10         xor        edx, edx                                    ; argument "compareOptions" for method imp___stubs__CFStringCompare
0000000000041e12         mov        rdi, rbx                                    ; argument "theString1" for method imp___stubs__CFStringCompare
0000000000041e15         call       imp___stubs__CFStringCompare                ; CFStringCompare
0000000000041e1a         test       rax, rax
0000000000041e1d         je         loc_41ede

0000000000041e23         lea        rsi, qword [cfstring_File]                  ; argument "theString2" for method imp___stubs__CFStringCompare, @"File"
0000000000041e2a         xor        r12d, r12d
0000000000041e2d         xor        edx, edx                                    ; argument "compareOptions" for method imp___stubs__CFStringCompare
0000000000041e2f         mov        rdi, rbx                                    ; argument "theString1" for method imp___stubs__CFStringCompare
0000000000041e32         call       imp___stubs__CFStringCompare                ; CFStringCompare
0000000000041e37         test       rax, rax
0000000000041e3a         mov        rbx, qword [rbp+var_1150]
0000000000041e41         jne        loc_41f04

0000000000041e47         lea        rsi, qword [cfstring_Virtual_Interface_Location_Path] ; argument "key" for method imp___stubs__CFDictionaryGetValue, @"Virtual Interface Location Path"
0000000000041e4e         mov        rdi, rbx                                    ; argument "theDict" for method imp___stubs__CFDictionaryGetValue
0000000000041e51         call       imp___stubs__CFDictionaryGetValue           ; CFDictionaryGetValue
0000000000041e56         mov        rbx, rax
0000000000041e59         test       rbx, rbx
0000000000041e5c         je         loc_41ed2

0000000000041e5e         mov        rdi, qword [objc_cls_ref_NSString]          ; argument "instance" for method _objc_msgSend, objc_cls_ref_NSString
0000000000041e65         mov        rsi, qword [0x4a25a0]                       ; argument "selector" for method _objc_msgSend, @selector(alloc)
0000000000041e6c         call       r15                                         ; Jumps to 0x6d63c0 (_objc_msgSend), _objc_msgSend
0000000000041e6f         mov        rsi, qword [0x4a2a40]                       ; argument "selector" for method _objc_msgSend, @selector(initWithData:encoding:)
0000000000041e76         mov        ecx, 0x4
0000000000041e7b         mov        rdi, rax                                    ; argument "instance" for method _objc_msgSend
0000000000041e7e         mov        rdx, rbx
0000000000041e81         call       r15                                         ; Jumps to 0x6d63c0 (_objc_msgSend), _objc_msgSend
0000000000041e84         mov        rbx, rax
0000000000041e87         mov        rdi, rbx                                    ; argument "instance" for method _objc_msgSend
0000000000041e8a         mov        rsi, qword [0x4a2998]                       ; argument "selector" for method _objc_msgSend, @selector(length)
0000000000041e91         call       r15                                         ; Jumps to 0x6d63c0 (_objc_msgSend), _objc_msgSend
0000000000041e94         test       rax, rax
0000000000041e97         je         loc_41eea

0000000000041e99         mov        rdi, qword [objc_cls_ref_NSFileWrapper]     ; argument "instance" for method _objc_msgSend, objc_cls_ref_NSFileWrapper
0000000000041ea0         mov        rsi, qword [0x4a4380]                       ; argument "selector" for method _objc_msgSend, @selector(_canSafelyMapFilesAtPath:)
0000000000041ea7         mov        rdx, rbx
0000000000041eaa         call       qword [_objc_msgSend_3c7a08]                ; _objc_msgSend, _objc_msgSend_3c7a08,_objc_msgSend
0000000000041eb0         mov        r12b, al
0000000000041eb3         jmp        loc_41eed

                     loc_41eb5:
0000000000041eb5         lea        rdi, qword [_volumeIsSafeForMapping.lock]   ; argument "lock" for method imp___stubs__os_unfair_lock_unlock, _volumeIsSafeForMapping.lock, CODE XREF=+[NSFileWrapper _canSafelyMapFilesAtPath:]+159
0000000000041ebc         call       imp___stubs__os_unfair_lock_unlock          ; os_unfair_lock_unlock
0000000000041ec1         mov        r12b, byte [__prepareForDispatch.installedPreDispatchHandler+4104] ; 0x4c4b68
0000000000041ec8         jmp        loc_41f6f

                     loc_41ecd:
0000000000041ecd         xor        r12d, r12d                                  ; CODE XREF=+[NSFileWrapper _canSafelyMapFilesAtPath:]+347
0000000000041ed0         jmp        loc_41f14

                     loc_41ed2:
0000000000041ed2         xor        r12d, r12d                                  ; CODE XREF=+[NSFileWrapper _canSafelyMapFilesAtPath:]+423, +[NSFileWrapper _canSafelyMapFilesAtPath:]+512
0000000000041ed5         mov        rbx, qword [rbp+var_1150]
0000000000041edc         jmp        loc_41f04

                     loc_41ede:
0000000000041ede         mov        r12b, 0x1                                   ; CODE XREF=+[NSFileWrapper _canSafelyMapFilesAtPath:]+449
0000000000041ee1         mov        rbx, qword [rbp+var_1150]
0000000000041ee8         jmp        loc_41f04

                     loc_41eea:
0000000000041eea         xor        r12d, r12d                                  ; CODE XREF=+[NSFileWrapper _canSafelyMapFilesAtPath:]+571

                     loc_41eed:
0000000000041eed         mov        rdi, rbx                                    ; argument "instance" for method _objc_msgSend, CODE XREF=+[NSFileWrapper _canSafelyMapFilesAtPath:]+599
0000000000041ef0         mov        rbx, qword [rbp+var_1150]
0000000000041ef7         mov        rsi, qword [0x4a2520]                       ; argument "selector" for method _objc_msgSend, @selector(release)
0000000000041efe         call       qword [_objc_msgSend_3c7a08]                ; _objc_msgSend, _objc_msgSend_3c7a08,_objc_msgSend

                     loc_41f04:
0000000000041f04         mov        rdi, rbx                                    ; argument "cf" for method imp___stubs__CFRelease, CODE XREF=+[NSFileWrapper _canSafelyMapFilesAtPath:]+485, +[NSFileWrapper _canSafelyMapFilesAtPath:]+640, +[NSFileWrapper _canSafelyMapFilesAtPath:]+652
0000000000041f07         call       imp___stubs__CFRelease                      ; CFRelease

                     loc_41f0c:
0000000000041f0c         mov        edi, r14d                                   ; argument "object" for method imp___stubs__IOObjectRelease, CODE XREF=+[NSFileWrapper _canSafelyMapFilesAtPath:]+389
0000000000041f0f         call       imp___stubs__IOObjectRelease                ; IOObjectRelease

                     loc_41f14:
0000000000041f14         mov        rax, qword [__NSConcreteStackBlock_3c73a8]  ; __NSConcreteStackBlock_3c73a8, CODE XREF=+[NSFileWrapper _canSafelyMapFilesAtPath:]+324, +[NSFileWrapper _canSafelyMapFilesAtPath:]+628
0000000000041f1b         lea        rbx, qword [rbp+var_1128]
0000000000041f22         mov        qword [rbx-0x20], rax
0000000000041f26         mov        dword [rbx-0x18], 0xc0000000
0000000000041f2d         mov        dword [rbx-0x14], _OBJC_METACLASS_$_NSURLSession_0
0000000000041f34         lea        rax, qword [___volumeIsSafeForMapping_block_invoke] ; ___volumeIsSafeForMapping_block_invoke
0000000000041f3b         mov        qword [rbx-0x10], rax
0000000000041f3f         lea        rax, qword [___block_descriptor_tmp.528_3d6008] ; ___block_descriptor_tmp.528_3d6008
0000000000041f46         mov        qword [rbx-8], rax
0000000000041f4a         lea        rsi, qword [rbp+var_8A8]                    ; argument "src" for method imp___stubs__memcpy
0000000000041f51         mov        edx, 0x878                                  ; argument "n" for method imp___stubs__memcpy
0000000000041f56         mov        rdi, rbx                                    ; argument "dst" for method imp___stubs__memcpy
0000000000041f59         call       imp___stubs__memcpy                         ; memcpy
0000000000041f5e         mov        byte [rbx+0x878], r12b
0000000000041f65         cmp        qword [__prepareForDispatch.installedPreDispatchHandler+4112], 0xffffffffffffffff ; 0x4c4b70
0000000000041f6d         jne        loc_41fc8

                     loc_41f6f:
0000000000041f6f         test       r12b, r12b                                  ; CODE XREF=+[NSFileWrapper _canSafelyMapFilesAtPath:]+620, +[NSFileWrapper _canSafelyMapFilesAtPath:]+895
0000000000041f72         je         loc_41ca7

0000000000041f78         cmp        qword [__prepareForDispatch.installedPreDispatchHandler+4128], 0xffffffffffffffff ; 0x4c4b80
0000000000041f80         jne        loc_41fb3

                     loc_41f82:
0000000000041f82         mov        rcx, qword [__prepareForDispatch.installedPreDispatchHandler+4120] ; 0x4c4b78, CODE XREF=+[NSFileWrapper _canSafelyMapFilesAtPath:]+874
0000000000041f89         test       rcx, rcx
0000000000041f8c         je         loc_41ca7

0000000000041f92         lea        rbx, qword [rbp+var_1148]
0000000000041f99         mov        rdi, r13
0000000000041f9c         mov        rsi, rbx
0000000000041f9f         call       rcx
0000000000041fa1         test       eax, eax
0000000000041fa3         sete       cl
0000000000041fa6         cmp        dword [rbx], 0x5
0000000000041fa9         setne      al
0000000000041fac         and        al, cl
0000000000041fae         jmp        loc_41ca9

                     loc_41fb3:
0000000000041fb3         lea        rdi, qword [__prepareForDispatch.installedPreDispatchHandler+4128] ; argument "predicate" for method imp___stubs__dispatch_once, 0x4c4b80, CODE XREF=+[NSFileWrapper _canSafelyMapFilesAtPath:]+804
0000000000041fba         lea        rsi, qword [___block_literal_global_3d6048] ; argument "block" for method imp___stubs__dispatch_once, ___block_literal_global_3d6048
0000000000041fc1         call       imp___stubs__dispatch_once                  ; dispatch_once
0000000000041fc6         jmp        loc_41f82

                     loc_41fc8:
0000000000041fc8         lea        rdi, qword [__prepareForDispatch.installedPreDispatchHandler+4112] ; argument "predicate" for method imp___stubs__dispatch_once, 0x4c4b70, CODE XREF=+[NSFileWrapper _canSafelyMapFilesAtPath:]+785
0000000000041fcf         lea        rsi, qword [rbp+var_1148]                   ; argument "block" for method imp___stubs__dispatch_once
0000000000041fd6         call       imp___stubs__dispatch_once                  ; dispatch_once
0000000000041fdb         jmp        loc_41f6f

                     loc_41fdd:
0000000000041fdd         call       imp___stubs____stack_chk_fail               ; __stack_chk_fail, CODE XREF=+[NSFileWrapper _canSafelyMapFilesAtPath:]+91
                        ; endp
```
