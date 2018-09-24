A memory map for a process before a first user mode instruction being executed.

```
slavas-MacBook-Pro:~ slava$ lldb /work/tmp/ping
(lldb) process launch --stop-at-entry -- www.microsoft.com
(lldb) process status
Process 1722 stopped
* thread #1, stop reason = signal SIGSTOP
    frame #0: 0x000000010001c19c dyld`_dyld_start
dyld`_dyld_start:
->  0x10001c19c <+0>: popq   %rdi
    0x10001c19d <+1>: pushq  $0x0
    0x10001c19f <+3>: movq   %rsp, %rbp
    0x10001c1a2 <+6>: andq   $-0x10, %rsp
(lldb) apropos shell
The following commands may relate to 'shell':
  platform shell -- Run a shell command on the current platform.
(lldb) platform shell
platform shell <cmd-options>
(lldb) platform shell vmmap 1722
2018-09-24 17:51:47.617 vmmap[1736:137850] *** task_malloc_get_all_zones: couldn't find libsystem_malloc dylib in target task
2018-09-24 17:51:47.668 vmmap[1736:137850] *** task_malloc_get_all_zones: couldn't find libsystem_malloc dylib in target task
Process:         ping [1722]
Path:            /work/tmp/ping
Load Address:    0x100000000
Identifier:      ping
Version:         ???
Code Type:       X86-64
Parent Process:  debugserver [1723]

Date/Time:       2018-09-24 17:51:47.507 +1000
Launch Time:     2018-09-24 17:37:09.703 +1000
OS Version:      Mac OS X 10.13.6 (17G65)
Report Version:  7
Analysis Tool:   /usr/bin/vmmap

Physical footprint:         12K
Physical footprint (peak):  12K
----

Virtual Memory Map of process 1722 (ping)
Output report format:  2.4  -- 64-bit process
VM page size:  4096 bytes

==== Non-writable regions for process 1722
REGION TYPE                      START - END             [ VSIZE  RSDNT  DIRTY   SWAP] PRT/MAX SHRMOD PURGE    REGION DETAIL
__TEXT                 0000000100000000-0000000100006000 [   24K    24K     0K     0K] r-x/rwx SM=COW          /work/tmp/ping
__LINKEDIT             0000000100017000-000000010001b000 [   16K    16K     0K     0K] r--/rwx SM=COW          /work/tmp/ping
__TEXT                 000000010001b000-0000000100066000 [  300K   300K     4K     0K] r-x/rwx SM=COW          /usr/lib/dyld
__LINKEDIT             000000010009e000-00000001000b9000 [  108K   108K     0K     0K] r--/rwx SM=COW          /usr/lib/dyld
STACK GUARD            00007ffeebc00000-00007ffeef400000 [ 56.0M     0K     0K     0K] ---/rwx SM=NUL          stack guard for thread 0
shared memory          00007fffffe00000-00007fffffe01000 [    4K     4K     4K     0K] r--/r-- SM=SHM          
shared memory          00007fffffe13000-00007fffffe14000 [    4K     4K     4K     0K] r-x/r-x SM=SHM          

==== Writable regions for process 1722
REGION TYPE                      START - END             [ VSIZE  RSDNT  DIRTY   SWAP] PRT/MAX SHRMOD PURGE    REGION DETAIL
__DATA                 0000000100006000-0000000100007000 [    4K     4K     0K     0K] rw-/rwx SM=COW          /work/tmp/ping
__DATA                 0000000100007000-0000000100017000 [   64K     0K     0K     0K] rw-/rwx SM=NUL          /work/tmp/ping
__DATA                 0000000100066000-0000000100069000 [   12K    12K     4K     0K] rw-/rwx SM=COW          /usr/lib/dyld
__DATA                 0000000100069000-000000010009e000 [  212K     0K     0K     0K] rw-/rwx SM=NUL          /usr/lib/dyld
Stack                  00007ffeef400000-00007ffeefc00000 [ 8192K     4K     4K     0K] rw-/rwx SM=PRV          thread 0

==== Legend
SM=sharing mode:  
	COW=copy_on_write PRV=private NUL=empty ALI=aliased 
	SHM=shared ZER=zero_filled S/A=shared_alias
PURGE=purgeable mode:  
	V=volatile N=nonvolatile E=empty   otherwise is unpurgeable

==== Summary for process 1722
ReadOnly portion of Libraries: Total=448K resident=448K(100%) swapped_out_or_unallocated=0K(0%)
Writable regions: Total=8468K written=4K(0%) resident=4K(0%) swapped_out=0K(0%) unallocated=8464K(100%)

                                VIRTUAL RESIDENT    DIRTY  SWAPPED VOLATILE   NONVOL    EMPTY   REGION 
REGION TYPE                        SIZE     SIZE     SIZE     SIZE     SIZE     SIZE     SIZE    COUNT (non-coalesced) 
===========                     ======= ========    =====  ======= ========   ======    =====  ======= 
STACK GUARD                       56.0M       0K       0K       0K       0K       0K       0K        2 
Stack                             8192K       4K       4K       0K       0K       0K       0K        2 
__DATA                             292K      16K       4K       0K       0K       0K       0K        5 
__LINKEDIT                         124K     124K       0K       0K       0K       0K       0K        3 
__TEXT                             324K     324K       4K       0K       0K       0K       0K        3 
shared memory                        8K       8K       8K       0K       0K       0K       0K        3 
===========                     ======= ========    =====  ======= ========   ======    =====  ======= 
TOTAL                             64.7M     476K      20K       0K       0K       0K       0K       12 

    VIRTUAL   RESIDENT      DIRTY    SWAPPED ALLOCATION      BYTES DIRTY+SWAP          REGION
MALLOC ZONE       SIZE       SIZE       SIZE       SIZE      COUNT  ALLOCATED  FRAG SIZE  % FRAG   COUNT
===========    =======  =========  =========  =========  =========  =========  =========  ======  ======

(lldb) 
```
