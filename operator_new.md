
```
  * frame #0: 0xffffff8023bd5981 kernel.development`try_alloc_from_zone(zone=0x0000000000000000, tag=0, check_poison=0x0000000000000000) at zalloc.c:1263 [opt]
    frame #1: 0xffffff8023bd3df1 kernel.development`zalloc_internal(zone=0xffffff80244c0a10, canblock=1, nopagewait=0, reqsize=0x0000000000000047, tag=<unavailable>) at zalloc.c:3084 [opt]
    frame #2: 0xffffff8023b8878c kernel.development`kalloc_canblock [inlined] zalloc_canblock_tag(zone=<unavailable>, canblock=1, reqsize=<unavailable>, tag=<unavailable>) at zalloc.c:3370 [opt]
    frame #3: 0xffffff8023b88778 kernel.development`kalloc_canblock(psize=<unavailable>, canblock=1, site=0xffffff80244b24b8) at kalloc.c:693 [opt]
    frame #4: 0xffffff80241d3a84 kernel.development`operator new(unsigned long) [inlined] kern_os_malloc(size=71) at OSRuntime.cpp:100 [opt]
    frame #5: 0xffffff80241d3a6a kernel.development`operator new(size=<unavailable>) at OSRuntime.cpp:535 [opt]
    ....
```
