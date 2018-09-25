A step by step description to find a vnode backing a memory mapping for a particular address. Lets find a vnode behind ```0x0007fff51b41000``` address which according to ```vmmap``` output belongs to ```/usr/lib/libSystem.B.dylib``` mapping for a ```__TEXT``` segment

Let's locate a task, ```ping``` in this case.

```
(lldb) showalltasks
task                 vm_map               ipc_space            #acts flags    pid       process             io_policy  wq_state  command             
....
0xffffff803e0ce7a0   0xffffff803e957200   0xffffff803e93a9c0       1 D        594   0xffffff803fc986d0                -1 -1 -1    ping   
```

Next we inspect its ```vmmap``

```
(lldb) showmapvme 0xffffff803e957200
vm_map             pmap               size                    #ents              rsize              start:end               
0xffffff803e957200 0xffffff803e954c90 0x00000001051cd000         37                202 0x00000001087f3000:0x00007fffffe00000
entry                           start:end                      #pgs tag.kmod prot&flags object             offset            
0xffffff803eb47640 0x00000001087f3000:0x00000001087f9000          6   0      57         0xffffff8045dd5700 0x0               
0xffffff803eb47910 0x00000001087f9000:0x00000001087fa000          1   0      37         0xffffff8044538800 0x0               
0xffffff803eb47050 0x00000001087fa000:0x000000010880a000         16   0      37         0xffffff8044538000 0x0               
0xffffff803f789550 0x000000010880a000:0x000000010880e000          4   0      17n        0xffffff8044538500 0x7000            
0xffffff803f789be0 0x000000010880e000:0x0000000108810000          2  73      37         0xffffff8044538200 0x0               
0xffffff803ef8e550 0x0000000108810000:0x0000000108811000          1   1      17n        0xffffff8044538900 0x0               
0xffffff803ad2b320 0x0000000108811000:0x0000000108812000          1   1      37n        0xffffff8044538900 0x1000            
0xffffff803f789e10 0x0000000108812000:0x0000000108813000          1   1      07         0xffffff8044538900 0x2000            
0xffffff803690c820 0x0000000108813000:0x0000000108817000          4   1      37n        0xffffff8044538900 0x3000            
0xffffff803f789000 0x0000000108817000:0x0000000108818000          1   1      07         0xffffff8044538900 0x7000            
0xffffff803690ca00 0x0000000108818000:0x0000000108819000          1   1      07         0x0000000000000000 0x0               
0xffffff803ef8eb90 0x0000000108819000:0x000000010881d000          4   1      37n        0xffffff8044538100 0x0               
0xffffff803ef8e230 0x000000010881d000:0x000000010881e000          1   1      07         0x0000000000000000 0x5000            
0xffffff80367b6140 0x000000010881e000:0x000000010881f000          1   1      17         0xffffff8044538a00 0x0               
0xffffff803d1e72d0 0x000000010881f000:0x0000000108820000          1   0      13         0xffffff80368b8400 0x0               
------------------ 0x0000000108820000:0x000000011852a000      64778
0xffffff803f789230 0x000000011852a000:0x0000000118575000         75   0      57         0xffffff8045dd5b00 0x0               
0xffffff803f789dc0 0x0000000118575000:0x0000000118578000          3   0      37         0xffffff8044538700 0x0               
0xffffff803f7895f0 0x0000000118578000:0x00000001185ad000         53   0      37         0xffffff8044538400 0x0               
0xffffff803f7895a0 0x00000001185ad000:0x00000001185c8000         27   0      17n        0xffffff8035f2e300 0x4f000           
------------------ 0x00000001185c8000:0x00007fb925c00000 34284295736
0xffffff80367b6230 0x00007fb925c00000:0x00007fb925d00000        256   7      37n        0xffffff8044538b00 0x0               
0xffffff803d1e7320 0x00007fb925d00000:0x00007fb925e00000        256   7      37n        0xffffff8044537f00 0x0               
0xffffff803f9ee3c0 0x00007fb925e00000:0x00007fb925f00000        256   7      37         0xffffff8044537e00 0x0               
------------------ 0x00007fb925f00000:0x00007fb926000000        256
0xffffff803ad2bd70 0x00007fb926000000:0x00007fb926800000       2048   2      37         0xffffff8044538c00 0x0               
0xffffff803f9eeaf0 0x00007fb926800000:0x00007fb927000000       2048   2      37n        0xffffff8044537200 0x0               
------------------ 0x00007fb927000000:0x00007ffee340d000   73122829
0xffffff803f789730 0x00007ffee340d000:0x00007ffee6c0d000      14336  30      07         0x0000000000000000 0x0               
0xffffff803f789a50 0x00007ffee6c0d000:0x00007ffee740d000       2048  30      37         0xffffff8044538600 0x0               
------------------ 0x00007ffee740d000:0x00007fff00000000     101363
0xffffff803f789d20 0x00007fff00000000:0x00007fff80000000     524288  32      17snp      submap:0xffffff8035fa3600 0x0               
0xffffff803ad2b000 0x00007fff80000000:0x00007fff8be00000      48640  35      17sn       submap:0xffffff8035fa3600 0x80000000        
0xffffff803ad2b2d0 0x00007fff8be00000:0x00007fff8c000000        512  35      37         0xffffff8044538e00 0x0               
0xffffff803ad2b780 0x00007fff8c000000:0x00007fff8c200000        512  35      17sn       submap:0xffffff8035fa3600 0x8c000000        
0xffffff803ad2b280 0x00007fff8c200000:0x00007fff8c400000        512  35      37         0xffffff8044538f00 0x0               
0xffffff803f7899b0 0x00007fff8c400000:0x00007fff8c600000        512  35      37         0xffffff8044538d00 0x0               
0xffffff803f789d70 0x00007fff8c600000:0x00007fff8c69b000        155  35      37         0xffffff8044538300 0x0               
0xffffff803f789960 0x00007fff8c69b000:0x00007fffc0000000     211301  35      17sn       submap:0xffffff8035fa3600 0x8c69b000        
0xffffff803f789aa0 0x00007fffc0000000:0x00007fffffe00000     261632  32      17snp      submap:0xffffff8035fa3600 0xc0000000        
0xffffff803f789fa0 0x00007fffffe00000:0x00007fffffe01000          1   0      11s        submap:0xffffff802d4e1a20 0x0               
------------------ 0x00007fffffe01000:0x00007fffffeb6000        181
0xffffff803f7892d0 0x00007fffffeb6000:0x00007fffffeb7000          1   0      55s        submap:0xffffff802d4e1c20 0x0
```

The address we are looking for belongs to a range backed by a submap ```0xffffff8035fa3600```. Let's inspect this submap.

```
(lldb) showmapvme -v 0xffffff8035fa3600
vm_map             pmap               size                    #ents              rsize              start:end               
0xffffff8035fa3600 0xffffff80353d9598 0x0000000045704000         62              34581 0x0000000000000000:0x00000000ffe00000
entry                           start:end                      #pgs tag.kmod prot&flags object             offset            
------------------ 0x0000000000000000:0x0000000024192000     147858
0xffffff8036052780 0x0000000024192000:0x000000005421f000     196749   0      55n        0xffffff8035fa4500 0x0               
------------------ 0x000000005421f000:0x0000000084192000     196467
0xffffff803b1f9690 0x0000000084192000:0x0000000084200000        110   0      33n        0xffffff8035fa4500 0x3008d000        
0xffffff803b1f9f50 0x0000000084200000:0x0000000084400000        512   0      33n        0xffffff8035fa4500 0x300fb000        
0xffffff803621f960 0x0000000084400000:0x0000000084600000        512   0      33n        0xffffff8035fa4500 0x302fb000        
0xffffff803b24baa0 0x0000000084600000:0x0000000084800000        512   0      33n        0xffffff8035fa4500 0x304fb000        
0xffffff803b24bb40 0x0000000084800000:0x0000000084a00000        512   0      33n        0xffffff8035fa4500 0x306fb000        
0xffffff803b24b910 0x0000000084a00000:0x0000000085400000       2560   0      33n        0xffffff8035fa4500 0x308fb000        
0xffffff803b24b8c0 0x0000000085400000:0x0000000085600000        512   0      33n        0xffffff8035fa4500 0x312fb000        
0xffffff8036b19410 0x0000000085600000:0x0000000085800000        512   0      33n        0xffffff8035fa4500 0x314fb000        
0xffffff803c9b10f0 0x0000000085800000:0x0000000085a00000        512   0      33n        0xffffff8035fa4500 0x316fb000        
0xffffff803621fb40 0x0000000085a00000:0x0000000085c00000        512   0      33n        0xffffff8035fa4500 0x318fb000        
0xffffff803621f690 0x0000000085c00000:0x0000000085e00000        512   0      33n        0xffffff8035fa4500 0x31afb000        
0xffffff80361cbc80 0x0000000085e00000:0x0000000086000000        512   0      33n        0xffffff8035fa4500 0x31cfb000        
0xffffff803621f370 0x0000000086000000:0x0000000086200000        512   0      33n        0xffffff8035fa4500 0x31efb000        
0xffffff803621f6e0 0x0000000086200000:0x0000000086400000        512   0      33n        0xffffff8035fa4500 0x320fb000        
0xffffff803621f0f0 0x0000000086400000:0x0000000086600000        512   0      33n        0xffffff8035fa4500 0x322fb000        
0xffffff8036052d20 0x0000000086600000:0x0000000086800000        512   0      33n        0xffffff8035fa4500 0x324fb000        
0xffffff803b24b9b0 0x0000000086800000:0x0000000086e00000       1536   0      33n        0xffffff8035fa4500 0x326fb000        
0xffffff8037bd0f00 0x0000000086e00000:0x0000000087000000        512   0      33n        0xffffff8035fa4500 0x32cfb000        
0xffffff8037fb1dc0 0x0000000087000000:0x0000000087200000        512   0      33n        0xffffff8035fa4500 0x32efb000        
0xffffff803687b190 0x0000000087200000:0x0000000087400000        512   0      33n        0xffffff8035fa4500 0x330fb000        
0xffffff80390b2f00 0x0000000087400000:0x0000000087600000        512   0      33n        0xffffff8035fa4500 0x332fb000        
0xffffff803621f550 0x0000000087600000:0x0000000087800000        512   0      33n        0xffffff8035fa4500 0x334fb000        
0xffffff803908a0a0 0x0000000087800000:0x0000000087c00000       1024   0      33n        0xffffff8035fa4500 0x336fb000        
0xffffff80375d5820 0x0000000087c00000:0x0000000087e00000        512   0      33n        0xffffff8035fa4500 0x33afb000        
0xffffff8037f0fb90 0x0000000087e00000:0x0000000088000000        512   0      33n        0xffffff8035fa4500 0x33cfb000        
0xffffff803724ddc0 0x0000000088000000:0x0000000088200000        512   0      33n        0xffffff8035fa4500 0x33efb000        
0xffffff803b11ac30 0x0000000088200000:0x0000000088400000        512   0      33n        0xffffff8035fa4500 0x340fb000        
0xffffff8036c39870 0x0000000088400000:0x0000000088600000        512   0      33n        0xffffff8035fa4500 0x342fb000        
0xffffff8038010500 0x0000000088600000:0x0000000088800000        512   0      33n        0xffffff8035fa4500 0x344fb000        
0xffffff8036c390f0 0x0000000088800000:0x0000000088a00000        512   0      33n        0xffffff8035fa4500 0x346fb000        
0xffffff8037c577d0 0x0000000088a00000:0x0000000088c00000        512   0      33n        0xffffff8035fa4500 0x348fb000        
0xffffff80390b2be0 0x0000000088c00000:0x0000000088e00000        512   0      33n        0xffffff8035fa4500 0x34afb000        
0xffffff803908a820 0x0000000088e00000:0x0000000089000000        512   0      33n        0xffffff8035fa4500 0x34cfb000        
0xffffff803b11ac80 0x0000000089000000:0x0000000089200000        512   0      33n        0xffffff8035fa4500 0x34efb000        
0xffffff803b24ba00 0x0000000089200000:0x0000000089400000        512   0      33n        0xffffff8035fa4500 0x350fb000        
0xffffff803621f410 0x0000000089400000:0x0000000089600000        512   0      33n        0xffffff8035fa4500 0x352fb000        
0xffffff803b24ba50 0x0000000089600000:0x0000000089800000        512   0      33n        0xffffff8035fa4500 0x354fb000        
0xffffff8037c57410 0x0000000089800000:0x0000000089a00000        512   0      33n        0xffffff8035fa4500 0x356fb000        
0xffffff8036922280 0x0000000089a00000:0x0000000089c00000        512   0      33n        0xffffff8035fa4500 0x358fb000        
0xffffff803843de10 0x0000000089c00000:0x0000000089e00000        512   0      33n        0xffffff8035fa4500 0x35afb000        
0xffffff803908a730 0x0000000089e00000:0x000000008a000000        512   0      33n        0xffffff8035fa4500 0x35cfb000        
0xffffff8037f42410 0x000000008a000000:0x000000008a200000        512   0      33n        0xffffff8035fa4500 0x35efb000        
0xffffff803cd14a50 0x000000008a200000:0x000000008a400000        512   0      33n        0xffffff8035fa4500 0x360fb000        
0xffffff803a407a00 0x000000008a400000:0x000000008a600000        512   0      33n        0xffffff8035fa4500 0x362fb000        
0xffffff803b24bbe0 0x000000008a600000:0x000000008a800000        512   0      33n        0xffffff8035fa4500 0x364fb000        
0xffffff803908aaa0 0x000000008a800000:0x000000008aa00000        512   0      33n        0xffffff8035fa4500 0x366fb000        
0xffffff803b24b4b0 0x000000008aa00000:0x000000008ac00000        512   0      33n        0xffffff8035fa4500 0x368fb000        
0xffffff80390b2140 0x000000008ac00000:0x000000008ae00000        512   0      33n        0xffffff8035fa4500 0x36afb000        
0xffffff803ad2ba00 0x000000008ae00000:0x000000008b200000       1024   0      33n        0xffffff8035fa4500 0x36cfb000        
0xffffff80390b2820 0x000000008b200000:0x000000008b400000        512   0      33n        0xffffff8035fa4500 0x370fb000        
0xffffff8037cc2280 0x000000008b400000:0x000000008b600000        512   0      33n        0xffffff8035fa4500 0x372fb000        
0xffffff80361cb6e0 0x000000008b600000:0x000000008b800000        512   0      33n        0xffffff8035fa4500 0x374fb000        
0xffffff80390b2af0 0x000000008b800000:0x000000008ba00000        512   0      33n        0xffffff8035fa4500 0x376fb000        
0xffffff803690c1e0 0x000000008ba00000:0x000000008bc00000        512   0      33n        0xffffff8035fa4500 0x378fb000        
0xffffff803680e460 0x000000008bc00000:0x000000008be00000        512   0      33n        0xffffff8035fa4500 0x37afb000        
0xffffff80361cbd20 0x000000008be00000:0x000000008c000000        512   0      33n        0xffffff8035fa4500 0x37cfb000        
0xffffff8036052a50 0x000000008c000000:0x000000008c200000        512   0      33n        0xffffff8035fa4500 0x37efb000        
0xffffff8037ce4c30 0x000000008c200000:0x000000008c400000        512   0      33n        0xffffff8035fa4500 0x380fb000        
0xffffff80360521e0 0x000000008c400000:0x000000008c600000        512   0      33n        0xffffff8035fa4500 0x382fb000        
0xffffff803621f5a0 0x000000008c600000:0x000000008c69b000        155   0      33n        0xffffff8035fa4500 0x384fb000        
------------------ 0x000000008c69b000:0x00000000c4192000     228087
0xffffff80360526e0 0x00000000c4192000:0x00000000d1300000      53614   0      11n        0xffffff8035fa4500 0x38596000        
------------------ 0x00000000d1300000:0x00000000ffe00000     191232

```

The address we are looking for is backed by a ```vm_object``` 0xffffff8035fa4500. Let's print it out.

```
(lldb) p *(vm_object_t)0xffffff8035fa4500
(vm_object) $49 = {
  memq = (next = 47704340, prev = 47704340)
  Lock = {
     = {
      lck_rw_shared_count = 0
      lck_rw_interlock = '\0'
      lck_rw_priv_excl = '\x01'
      lck_rw_want_upgrade = '\0'
      lck_rw_want_write = '\0'
      lck_r_waiting = '\0'
      lck_w_waiting = '\0'
      lck_rw_can_sleep = '\x01'
      lck_rw_padb6 = '\0'
      lck_rw_tag = 0
      lck_rw_owner = 0x0000000000000000
    }
     = (data = 553648128, lck_rw_pad4 = 0, lck_rw_pad8 = 0, lck_rw_pad12 = 0)
  }
  Lock_owner = 0x0000000000000000
  vo_un1 = (vou_size = 1164984320, vou_cache_pages_to_scan = 1164984320)
  memq_hint = 0x0000000000000000
  ref_count = 4877
  resident_page_count = 0
  wired_page_count = 0
  reusable_page_count = 0
  copy = 0x0000000000000000
  shadow = 0xffffff8035fa4300
  vo_un2 = {
    vou_shadow_offset = 0
    vou_cache_ts = 0
    vou_purgeable_owner = 0x0000000000000000
    vou_slide_info = 0x0000000000000000
  }
  pager = 0x0000000000000000
  paging_offset = 0
  pager_control = 0x0000000000000000
  copy_strategy = 4
  paging_in_progress = 0
  __object1_unused_bits = 0
  activity_in_progress = 0
  all_wanted = 0
  pager_created = 0
  pager_initialized = 0
  pager_ready = 0
  pager_trusted = 0
  can_persist = 0
  internal = 1
  private = 0
  pageout = 0
  alive = 1
  purgable = 3
  purgeable_only_by_kernel = 0
  purgeable_when_ripe = 0
  shadowed = 1
  true_share = 0
  terminating = 0
  named = 0
  shadow_severed = 0
  phys_contiguous = 0
  nophyscache = 0
  _object5_unused_bits = 0
  cached_list = {
    next = 0x0000000000000000
    prev = 0x0000000000000000
  }
  last_alloc = 0
  sequential = 0
  pages_created = 0
  pages_used = 0
  cow_hint = 0xffffffffffffffff
  wimg_bits = 128
  code_signed = 0
  transposed = 0
  mapping_in_progress = 0
  phantom_isssd = 0
  volatile_empty = 0
  volatile_fault = 0
  all_reusable = 0
  blocked_access = 0
  set_cache_attr = 0
  object_slid = 0
  purgeable_queue_type = 3
  purgeable_queue_group = 0
  io_tracking = 0
  no_tag_update = 0
  __object3_unused_bits = 0
  __object2_unused_bits = 0
  scan_collisions = '\0'
  wire_tag = 0
  __object4_unused_bits = ([0] = '\0', [1] = '\0')
  phantom_object_id = 0
  uplq = {
    next = 0xffffff8035fa45c0
    prev = 0xffffff8035fa45c0
  }
  objq = {
    next = 0x0000000000000000
    prev = 0x0000000000000000
  }
  task_objq = {
    next = 0x0000000000000000
    prev = 0x0000000000000000
  }
}
```

The pager is NULL and this object in turn is backed by a shadow object 0xffffff8035fa4300. Switch to this object.

```
(lldb) p *(vm_object_t)0xffffff8035fa4300
(vm_object) $50 = {
  memq = (next = 2148243131, prev = 2148236198)
  Lock = {
     = {
      lck_rw_shared_count = 0
      lck_rw_interlock = '\0'
      lck_rw_priv_excl = '\x01'
      lck_rw_want_upgrade = '\0'
      lck_rw_want_write = '\0'
      lck_r_waiting = '\0'
      lck_w_waiting = '\0'
      lck_rw_can_sleep = '\x01'
      lck_rw_padb6 = '\0'
      lck_rw_tag = 0
      lck_rw_owner = 0x0000000000000000
    }
     = (data = 553648128, lck_rw_pad4 = 0, lck_rw_pad8 = 0, lck_rw_pad12 = 0)
  }
  Lock_owner = 0x0000000000000000
  vo_un1 = (vou_size = 0, vou_cache_pages_to_scan = 0)
  memq_hint = 0xffffff80301495d0
  ref_count = 3
  resident_page_count = 116984
  wired_page_count = 0
  reusable_page_count = 0
  copy = 0xffffff8035fa4500
  shadow = 0x0000000000000000
  vo_un2 = {
    vou_shadow_offset = 18446743524858593968
    vou_cache_ts = 18446743524858593968
    vou_purgeable_owner = 0xffffff8035ef02b0
    vou_slide_info = 0xffffff8035ef02b0
  }
  pager = 0xffffff8035f3b8c0
  paging_offset = 0
  pager_control = 0xffffff80359d65a0
  copy_strategy = 2
  paging_in_progress = 0
  __object1_unused_bits = 0
  activity_in_progress = 0
  all_wanted = 0
  pager_created = 1
  pager_initialized = 1
  pager_ready = 1
  pager_trusted = 0
  can_persist = 1
  internal = 0
  private = 0
  pageout = 0
  alive = 1
  purgable = 3
  purgeable_only_by_kernel = 0
  purgeable_when_ripe = 0
  shadowed = 0
  true_share = 0
  terminating = 0
  named = 1
  shadow_severed = 0
  phys_contiguous = 0
  nophyscache = 0
  _object5_unused_bits = 0
  cached_list = {
    next = 0x0000000000000000
    prev = 0x0000000000000000
  }
  last_alloc = 836317184
  sequential = 0
  pages_created = 123988
  pages_used = 50061
  cow_hint = 0xffffffffffffffff
  wimg_bits = 128
  code_signed = 1
  transposed = 0
  mapping_in_progress = 0
  phantom_isssd = 0
  volatile_empty = 0
  volatile_fault = 0
  all_reusable = 0
  blocked_access = 0
  set_cache_attr = 0
  object_slid = 1
  purgeable_queue_type = 3
  purgeable_queue_group = 0
  io_tracking = 0
  no_tag_update = 0
  __object3_unused_bits = 0
  __object2_unused_bits = 0
  scan_collisions = '\0'
  wire_tag = 0
  __object4_unused_bits = ([0] = '\0', [1] = '\0')
  phantom_object_id = 5
  uplq = {
    next = 0xffffff8035fa43c0
    prev = 0xffffff8035fa43c0
  }
  objq = {
    next = 0x0000000000000000
    prev = 0x0000000000000000
  }
  task_objq = {
    next = 0x0000000000000000
    prev = 0x0000000000000000
  }
}
```

Now we have a valid pager. Let's inspect it.

```
(lldb) p *(memory_object_t)0xffffff8035f3b8c0
(memory_object) $51 = {
  mo_ikot = 11
  mo_pager_ops = 0xffffff80298cae70
  mo_control = 0xffffff80359d65a0
}
(lldb) p *((memory_object_t)0xffffff8035f3b8c0)->mo_pager_ops
(const memory_object_pager_ops) $52 = {
  memory_object_reference = 0xffffff8028dfcc10 (kernel.development`vnode_pager_reference at bsd_vm.c:648)
  memory_object_deallocate = 0xffffff8028dfcc80 (kernel.development`vnode_pager_deallocate at bsd_vm.c:663)
  memory_object_init = 0xffffff8028dfccf0 (kernel.development`vnode_pager_init at bsd_vm.c:402)
  memory_object_terminate = 0xffffff8028dfcd90 (kernel.development`vnode_pager_terminate at bsd_vm.c:688)
  memory_object_data_request = 0xffffff8028dfcda0 (kernel.development`vnode_pager_data_request at bsd_vm.c:620)
  memory_object_data_return = 0xffffff8028dfce70 (kernel.development`vnode_pager_data_return at bsd_vm.c:449)
  memory_object_data_initialize = 0xffffff8028dfcef0 (kernel.development`vnode_pager_data_initialize at bsd_vm.c:464)
  memory_object_data_unlock = 0xffffff8028dfcf10 (kernel.development`vnode_pager_data_unlock at bsd_vm.c:475)
  memory_object_synchronize = 0xffffff8028dfcf20 (kernel.development`vnode_pager_synchronize at bsd_vm.c:703)
  memory_object_map = 0xffffff8028dfcf40 (kernel.development`vnode_pager_map at bsd_vm.c:715)
  memory_object_last_unmap = 0xffffff8028dfcf90 (kernel.development`vnode_pager_last_unmap at bsd_vm.c:738)
  memory_object_data_reclaim = 0x0000000000000000
  memory_object_pager_name = 0xffffff8029515488 "vnode pager"
}
(lldb) p *((memory_object_t)0xffffff8035f3b8c0)->mo_control
(memory_object_control) $53 = {
  moc_ikot = 32
  moc_object = 0xffffff8035fa4300
}
```

This is a ```vnode_pager``` object. Now we cast the pager to ```vnode_pager``` type.

```
(lldb) p *(vnode_pager*)0xffffff8035f3b8c0
(vnode_pager) $55 = {
  vn_pgr_hdr = {
    mo_ikot = 11
    mo_pager_ops = 0xffffff80298cae70
    mo_control = 0xffffff80359d65a0
  }
  ref_count = 1
  vnode_handle = 0xffffff8035f112e8
}
```

Then print the vnode path.

```
(lldb) showvnodepath 0xffffff8035f112e8
/private/var/db/dyld/dyld_shared_cache_x86_64
```

As it was expected this is actually a vnode for a prelinked shared libraries cache file.
