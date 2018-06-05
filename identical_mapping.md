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
