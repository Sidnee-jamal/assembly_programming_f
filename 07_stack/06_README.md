# `06.asm`: 32 bit values on the stack

This 32 bit example pushes two register values and pops them in reverse order, then requests a normal Linux i386 exit.

Let the starting `esp` be **S**. The initial moves set `eax=0x11223344` and `ebx=0x1234` without using the stack. In 32 bit mode, each push/pop moves `esp` by four bytes. `push eax` stores `0x11223344` at `[S-4]` and sets `esp=S-4`; `push ebx` stores `0x1234` at `[S-8]` and sets `esp=S-8`. The `nop` leaves both values in place.

`pop ebx` takes the top value (the original `0x1234`) and returns `esp` to `S-4`. `pop eax` takes `0x11223344` and returns `esp` to `S`. The registers regain their original values because the pushes and pops are paired in reverse order. The program then sets `eax=1`, `ebx=0` and executes `int 0x80` to exit with status 0.

**Run observation:** I successfully assembled and linked this as ELF32. When I ran it, the environment rejected the final `int 0x80` exit syscall with “Bad system call” (shell status 159), so it did not complete with the requested exit status here. The push/pop instructions and their four-byte `esp` changes are present in the executable.
