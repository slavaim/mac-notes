```
  * frame #0: 0xffffff800d3acdd8 kernel.development`task_wait [inlined] task_wait_locked(task=0xffffff801bce87a0, until_not_runnable=<unavailable>) at task.c:2650 [opt]
    frame #1: 0xffffff800d3acdd8 kernel.development`task_wait(task=0xffffff801bce87a0, until_not_runnable=0) at task.c:2628 [opt]
    frame #2: 0xffffff800d8902a7 kernel.development`postsig_locked [inlined] sig_lock_to_exit at kern_sig.c:3583 [opt]
    frame #3: 0xffffff800d890278 kernel.development`postsig_locked(signum=2) at kern_sig.c:3095 [opt]
    frame #4: 0xffffff800d8909d7 kernel.development`bsd_ast(thread=<unavailable>) at kern_sig.c:3420 [opt]
    frame #5: 0xffffff800d372ea4 kernel.development`ast_taken_user at ast.c:207 [opt]
    frame #6: 0xffffff800d32021c kernel.development`return_from_trap + 172
```
