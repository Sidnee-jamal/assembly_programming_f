# `03.asm`: `call` and `ret` preserve the return address

This example shows a function call that uses no explicit local stack frame. It computes 15 in `rax`, then exits with status 0.

Let `S` be `rsp` at `_start`. `mov rax,10` changes only a register. `call calculate` pushes the address labeled `after_call` onto the stack, decreasing `rsp` by eight to `S-8`, then transfers control to `calculate`. `add rax,5` changes `rax` from 10 to 15 but does not touch the stack.

`ret` pops the saved return address into the instruction pointer and raises `rsp` by eight, back to `S`. Execution resumes at `after_call`; the comment records the expected result, and `nop` does not alter either the stack or `rax`. The program then invokes the 64 bit Linux exit syscall with status 0. No other push/pop or stack allocation occurs.

**Run observation:** I assembled and linked this as ELF64, and it ran successfully with exit status 0. The `add rax,5` instruction makes the value in `rax` equal to 15 when execution returns to `_start`.
