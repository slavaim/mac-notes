A kernel reloads CR3 register on each system call.

```
Entry(hi64_sysenter)
Entry(idt64_sysenter)
	/* Synthesize an interrupt stack frame onto the
	 * exception stack.
	 */
	push	$(USER_DS)		/* ss */
	push	%rcx			/* uesp */
	pushf				/* flags */
	/*
	 * Clear, among others, the Nested Task (NT) flags bit;
	 * this is zeroed by INT, but not by SYSENTER.
	 */
	push	$0
	popf
	push	$(SYSENTER_CS)		/* cs */ 
L_sysenter_continue:
	push	%rdx			/* eip */
	push	%rax			/* err/eax - syscall code */
	pushq	$(HNDL_SYSENTER)
	pushq	$(T_SYSENTER)
	orl	$(EFL_IF), ISF64_RFLAGS(%rsp)
	jmp L_dispatch

/*
 * Common dispatch point.
 * Determine what mode has been interrupted and save state accordingly.
 * Here with:
 *	rsp	from user-space:   interrupt state in PCB, or
 *		from kernel-space: interrupt state in kernel or interrupt stack
 *	GSBASE	from user-space:   pthread area, or
 *		from kernel-space: cpu_data
 */

L_dispatch:
	pushq	%rax
	testb	$3, 8+ISF64_CS(%rsp)
	jz	1f
     	swapgs
	leaq	EXT(idt64_hndl_table0)(%rip), %rax
     	mov	16(%rax), %rax

	mov	%gs:CPU_TASK_CR3(%rax), %rax 
	mov	%rax, %cr3
  #if	DEBUG
	mov	%rax, %gs:CPU_ENTRY_CR3
#endif
1:
	/* The text/data relationship here must be preserved in the doublemap, and the contents must be remapped */
	leaq	EXT(idt64_hndl_table0)(%rip), %rax
	/* Indirect branch to non-doublemapped trampolines */
	jmp *(%rax)
/* User return: register restoration and address space switch sequence */
```

The ```testb	$3, 8+ISF64_CS(%rsp)``` instruction checks that previous mode was a user mode by testing the least significant two bits of the CS selector saved on a stack as ```L_dispatch``` is a common code for various type of traps and can be called from a kernel mode. The both user code and sysenter selectors have these bits set to 1 (0x*b pattern).
```
#define KERNEL64_CS 	0x08		/* 1:  K64 code */
#define SYSENTER_CS 	0x0b 		/*     U32 sysenter pseudo-segment */
#define	KERNEL64_SS	0x10		/* 2:  KERNEL64_CS+8 for syscall */
#define USER_CS		0x1b		/* 3:  U32 code */
#define USER_DS		0x23		/* 4:  USER_CS+8 for sysret */
#define USER64_CS	0x2b		/* 5:  USER_CS+16 for sysret */
#define USER64_DS	USER_DS		/*     U64 data pseudo-segment */
#define KERNEL_LDT	0x30		/* 6:  */
					/* 7:  other 8 bytes of KERNEL_LDT */
#define KERNEL_TSS	0x40		/* 8:  */
					/* 9:  other 8 bytes of KERNEL_TSS */
#define KERNEL32_CS	0x50		/* 10: */
#define USER_LDT	0x58		/* 11: */
					/* 12: other 8 bytes of USER_LDT */
#define KERNEL_DS	0x68		/* 13: 32-bit kernel data */
```
