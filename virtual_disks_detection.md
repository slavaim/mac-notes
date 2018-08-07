
```+[NSFileWrapper _canSafelyMapFilesAtPath:]``` recursively process virtual disks(images) up to a root file system. It accepts an NSString object with a path to any file on a mounted image. Below is a psedocode produced by Hopper Disassembler.

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
