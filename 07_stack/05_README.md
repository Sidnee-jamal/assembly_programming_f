# `05.asm`: 64 bit values on the stack

This program pushes a 64 bit value and then a smaller value, then pops them in reverse order. It exits with status 0.

Let the starting `rsp` be **S**. The two `mov` instructions only initialize registers: `rax=0x1122334455667788`, `rbx=0x1234`. `push rax` stores the full eight-byte `rax` at `[S-8]` and changes `rsp` to `S-8`. `push rbx` stores the eight-byte stack representation of `rbx` at `[S-16]` and changes `rsp` to `S-16`. Although the number in `rbx` is small, `push rbx` still occupies eight bytes in 64 bit mode.

After `nop`, `pop rbx` retrieves the top value `0x1234` and restores `rsp` to `S-8`. `pop rax` retrieves `0x1122334455667788` and returns `rsp` to `S`. This demonstrates both LIFO order and that each 64 bit push/pop changes `rsp` by eight bytes. The program then invokes syscall 60 with exit status 0; there are no additional stack operations.

**Run observation:** I assembled and linked this as ELF64, and it ran successfully with exit status 0.
