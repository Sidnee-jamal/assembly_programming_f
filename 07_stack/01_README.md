# `01.asm`: Push and pop in LIFO order

This 64 bit Linux program demonstrates that the stack is last in, first out. I followed the values in the registers as they were pushed and popped.

At `_start`, let the initial stack pointer be **S**. `mov rax, 100` and `mov rbx, 200` only set registers and do not touch the stack. Each `push` uses eight bytes in 64 bit mode and moves `rsp` down by eight:

1. `push rax` stores 100 at `[S-8]`; `rsp` becomes `S-8`.
2. `push rbx` stores 200 at `[S-16]`; `rsp` becomes `S-16`.
3. `nop` has no effect, so 200 is still the top value.
4. `pop rcx` loads 200 into `rcx` and advances `rsp` to `S-8`.
5. `pop rdx` loads 100 into `rdx` and advances `rsp` back to `S`.

The values therefore come back in reverse order: the last value pushed (200) is the first popped. The second `nop` also leaves the stack unchanged. Finally, the program puts Linux's 64 bit exit number (60) in `rax`, status 0 in `rdi`, and invokes `syscall`; it makes no further stack operations.

**Run observation:** I assembled and linked this as ELF64, and it ran successfully with exit status 0.
