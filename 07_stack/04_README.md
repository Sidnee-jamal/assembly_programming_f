# `04.asm`: A 32 bit stack frame

This is the 32 bit counterpart to `02.asm`: `work` saves `ebp`, reserves two local slots, reads them, and returns. It uses the Linux i386 `int 0x80` exit convention.

Let `S` be `esp` at `_start`. In 32 bit mode, `call work` pushes a four-byte return address, so `esp=S-4`. `push ebp` saves the caller's frame pointer at `[S-8]` and moves `esp` to `S-8`. `mov ebp,esp` marks the frame base. `sub esp,8` reserves eight bytes for two four-byte locals, giving `esp=S-16`.

The program writes 100 to `[ebp-4]` and 200 to `[ebp-8]`, then loads them into `eax` and `ebx`. Those memory reads and writes leave `esp` unchanged. `mov esp,ebp` drops the local area by restoring the stack pointer to the saved-`ebp` slot. `pop ebp` restores the old frame pointer and advances `esp` to the return address. `ret` consumes that address, advances `esp` to `S`, and resumes after `call` in `_start`.

The caller sets `eax=1` (i386 `exit`) and `ebx=0` (exit status), then executes `int 0x80`. This final kernel call does not return to the program.

**Run observation:** I successfully assembled and linked this as ELF32. When I ran it, the environment rejected the final `int 0x80` exit syscall with “Bad system call” (shell status 159), so it did not complete with the requested exit status here.
