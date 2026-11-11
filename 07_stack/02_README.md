# `02.asm`: A 64 bit stack frame

This program calls `work`, which creates a small stack frame, uses two local slots, then restores the caller's frame.

Let `S` be `rsp` on entry to `_start`. `call work` first pushes the address of the instruction after the call (the return address), so `rsp` becomes `S-8`. At `work`, `push rbp` saves the caller's frame pointer at `[S-16]` and makes `rsp=S-16`. `mov rbp,rsp` establishes this as the frame base. `sub rsp,16` reserves 16 bytes for locals, making `rsp=S-32`.

The stores place 100 at `[rbp-8]` and 200 at `[rbp-16]`. These are ordinary memory accesses: they do not change `rsp`. The loads copy those locals into `rax` and `rbx`, also without changing the stack pointer. The frame layout, from higher to lower addresses, is the return address, saved old `rbp`, local 100, and local 200.

The epilogue undoes the allocation in a controlled order. `mov rsp,rbp` discards both local slots by restoring `rsp` to the saved-`rbp` slot. `pop rbp` restores the caller's `rbp` and advances `rsp` to the return address. `ret` takes that address from the stack and advances `rsp` back to `S`, continuing after `call`. The caller then sets up exit status 0 and invokes the 64 bit exit syscall.

**Run observation:** I assembled and linked this as ELF64, and it ran successfully with exit status 0.
