```
extern	uint64_t physmap_base, physmap_max;

...

static	inline void * PHYSMAP_PTOV_check(void *paddr) {
	uint64_t pvaddr = (uint64_t)paddr + physmap_base;

	if (__improbable(pvaddr >= physmap_max))
		panic("PHYSMAP_PTOV bounds exceeded, 0x%qx, 0x%qx, 0x%qx",
		      pvaddr, physmap_base, physmap_max);

	return (void *)pvaddr;
}

#define PHYSMAP_PTOV(x)	(PHYSMAP_PTOV_check((void*) (x)))
```

Usage in kernel debug (```KDK/System/Library/Kernels/kernel.development.dSYM/Contents/Resources/Python/lldbmacros/core/kernelcore.py```)

```
    def PhysToKernelVirt(self, addr):
        if self.arch == 'x86_64':
            return (addr + unsigned(self.GetGlobalVariable('physmap_base')))
        elif self.arch.startswith('arm64'):
            return self.PhysToKVARM64(addr)
        elif self.arch.startswith('arm'):
            return (addr - unsigned(self.GetGlobalVariable("gPhysBase")) + unsigned(self.GetGlobalVariable("gVirtBase")))
        else:
            raise ValueError("PhysToVirt does not support {0}".format(self.arch))
```
