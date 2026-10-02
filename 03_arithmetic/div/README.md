# Division

For unsigned `DIV`, the quotient and remainder are written to the instruction's implicit registers. **All arithmetic condition flags (CF, OF, SF, ZF, AF, PF) are undefined** after a successful division. Division does not set or clear them to report quotient or remainder properties, so GDB's displayed flag bits cannot be interpreted as results of the divide.

## `div1.asm`

The 16-bit dividend `AX = 100` divided by the 8-bit divisor `BL = 7` yields `AL = 14` and `AH = 2` (quotient and remainder). The divisor is nonzero and the quotient fits in AL, so the division completes without divide error. **CF, OF, SF, ZF, AF, and PF are all undefined.**

## `div2.asm`

The 32-bit dividend `DX:AX = 50000` divided by the 16-bit divisor `BX = 300` yields `AX = 166` and `DX = 200`. The divisor is nonzero and the quotient fits in AX. **CF, OF, SF, ZF, AF, and PF are all undefined.**

## Run note

Both programs assembled and linked. Execution in this container is blocked at the 32-bit `int 0x80` exit syscall (`Bad system call`); GDB is also blocked from launching the inferior by the container's `ptrace` restriction.
