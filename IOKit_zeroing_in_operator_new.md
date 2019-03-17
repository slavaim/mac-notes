A class inherited from OSObject has its members initialized with zeroes in operator new().

```
void *OSObject::operator new(size_t size)
{
#if IOTRACKING
    if (kIOTracking & gIOKitDebug) return (OSMetaClass::trackedNew(size));
#endif

    void * mem = kalloc_tag_bt(size, VM_KERN_MEMORY_LIBKERN);
    assert(mem);
    bzero(mem, size);
    OSIVAR_ACCUMSIZE(size);

    return (void *) mem;
}
```

The C++ standard doesn't specify default initialization for class members of the base types. IOKit always initializes all members with zeroes in operator new() for classes inherited from OSObject.
