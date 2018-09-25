
A return from an interrupt to a kernel mode

```
trap_from_kernel:
	movq	%r15, %rdi		/* saved state addr */
	pushq   R64_RIP(%r15)           /* Simulate a CALL from fault point */
	pushq   %rbp                    /* Extend framepointer chain */
	movq    %rsp, %rbp
	CCALLWITHSP(kernel_trap)	/* to kernel trap routine */
	popq    %rbp
	addq    $8, %rsp
	mov	%rsp, %r15		/* DTrace slides stack/saved-state */
	cli

	movl	%gs:CPU_PENDING_AST,%eax	/* get pending asts */
	testl	$(AST_URGENT),%eax		/* any urgent preemption? */
	je	ret_to_kernel			/* no, nothing to do */
	cmpl	$(T_PREEMPT),R64_TRAPNO(%r15)
	je	ret_to_kernel			/* T_PREEMPT handled in kernel_trap() */
	testl	$(EFL_IF),R64_RFLAGS(%r15)	/* interrupts disabled? */
	je	ret_to_kernel
	cmpl	$0,%gs:CPU_PREEMPTION_LEVEL	/* preemption disabled? */
	jne	ret_to_kernel
	movq	%gs:CPU_KERNEL_STACK,%rax
	movq	%rsp,%rcx
	xorq	%rax,%rcx
	andq	EXT(kernel_stack_mask)(%rip),%rcx
	testq	%rcx,%rcx		/* are we on the kernel stack? */
	jne	ret_to_kernel		/* no, skip it */

	CCALL(ast_taken_kernel)         /* take the AST */

	mov	%rsp, %r15		/* AST changes stack, saved state */
	jmp	ret_to_kernel
```

The ```ast_taken_kernel``` routine handles preemption. Note that if a thread called ```assert_wait``` and it returned ```THREAD_WAITING``` this thread can't be preempted until it calls ```thread_block``` explicitly.

```
/*
 * AST_URGENT was detected while in kernel mode
 * Called with interrupts disabled, returns the same way
 * Must return to caller
 */
void
ast_taken_kernel(void)
{
	assert(ml_get_interrupts_enabled() == FALSE);

	thread_t thread = current_thread();

	/* Idle threads handle preemption themselves */
	if ((thread->state & TH_IDLE)) {
		ast_off(AST_PREEMPTION);
		return;
	}

	/*
	 * It's possible for this to be called after AST_URGENT
	 * has already been handled, due to races in enable_preemption
	 */
	if (ast_peek(AST_URGENT) != AST_URGENT)
		return;

	/*
	 * Don't preempt if the thread is already preparing to block.
	 * TODO: the thread can cheese this with clear_wait()
	 */
	if (waitq_wait_possible(thread) == FALSE) {
		/* Consume AST_URGENT or the interrupt will call us again */
		ast_consume(AST_URGENT);
		return;
	}

	/* TODO: Should we csw_check again to notice if conditions have changed? */

	ast_t urgent_reason = ast_consume(AST_PREEMPTION);

	assert(urgent_reason & AST_PREEMPT);

	counter(c_ast_taken_block++);

	thread_block_reason(THREAD_CONTINUE_NULL, NULL, urgent_reason);

	assert(ml_get_interrupts_enabled() == FALSE);
}
```

This ```AST_URGENT``` is set for a current processor(!) by ```csw_check``` routine called from multiple places but the most important is a call from ```thread_quantum_expire``` routine invoked by a timer interrupt.

