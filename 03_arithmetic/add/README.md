# Addition

The following two programs assembled and linked successfully. Values and flags below are the state immediately after `ADD`, before subsequent instructions can change flags.

## `add1.asm`

`120 + 10` in 8 bits produces `130` (`0x82`, stored as a signed interpretation of −126).

- **CF = 0:** the unsigned sum 130 fits in 8 bits; there is no carry out of bit 7.
- **OF = 1:** both signed operands are positive, but the 8-bit signed result has its sign bit set. +120 + +10 exceeds +127.
- **SF = 1:** result bit 7 is 1.
- **ZF = 0:** the result is nonzero.
- **AF = 1:** adding the low nibbles, `8 + A`, carries from bit 3 into bit 4.
- **PF = 1:** the low result byte `0x82` has two set bits, an even count.

## `add2.asm`

`32000 + 500` in 16 bits produces `32500` (`0x7EF4`).

- **CF = 0:** the result fits in 16 unsigned bits, with no carry out of bit 15.
- **OF = 0:** both operands and result fit in the signed 16-bit positive range (through 32767).
- **SF = 0:** result bit 15 is clear.
- **ZF = 0:** the result is nonzero.
- **AF = 0:** the low nibbles `0 + 4` do not carry into bit 4.
- **PF = 0:** parity is computed from the low byte `0xF4`; it has five set bits, an odd count.

## Run note

Both programs assembled and linked. Execution in this container is blocked at the 32-bit `int 0x80` exit syscall (`Bad system call`); GDB is also blocked from launching the inferior by the container's `ptrace` restriction.
