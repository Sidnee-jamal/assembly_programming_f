# Multiplication

For unsigned `MUL`, CF and OF are both clear when the upper half of the full product is zero, and both set otherwise. SF, ZF, AF, and PF are undefined after `MUL`; they must not be interpreted as meaningful outcomes.

## `mul1.asm`

`25 × 10` produces the 16-bit product `250` (`AX = 0x00FA`, so `AH = 0`).

- **CF = 0 and OF = 0:** the upper half (`AH`) is zero, so the product fits entirely in the original 8-bit operand width.
- **SF, ZF, AF, PF = undefined:** `MUL` does not define these flags. Their displayed values would not describe whether the product is negative, zero, or has a particular parity.

## `mul2.asm`

`3000 × 200` produces the 32-bit product `600000` (`DX:AX = 0x0009:0x27C0`).

- **CF = 1 and OF = 1:** the upper half `DX` is nonzero (`0x0009`), so the product does not fit in the 16-bit destination half `AX` alone.
- **SF, ZF, AF, PF = undefined:** `MUL` leaves these flags undefined.

## Run note

Both programs assembled and linked. Execution in this container is blocked at the 32-bit `int 0x80` exit syscall (`Bad system call`); GDB is also blocked from launching the inferior by the container's `ptrace` restriction.
