# Subtraction

The following two programs assembled and linked successfully. Values and flags below are the state immediately after `SUB`.

## `sub1.asm`

`50 − 80` in 8 bits produces `−30`, represented by `0xD2` (210 unsigned).

- **CF = 1:** unsigned subtraction needs a borrow because 50 is less than 80.
- **OF = 0:** the signed result −30 fits in the 8-bit signed range. The operands have the same sign, so signed overflow cannot occur here.
- **SF = 1:** result bit 7 is set.
- **ZF = 0:** the result is nonzero.
- **AF = 0:** the low nibbles are `2 − 0`, which requires no borrow across bit 3.
- **PF = 1:** the low result byte `0xD2` contains four set bits, an even count.

## `sub2.asm`

`1000 − 2000` in 16 bits produces `−1000`, represented by `0xFC18` (64536 unsigned).

- **CF = 1:** unsigned 1000 is less than 2000, so the subtraction borrows.
- **OF = 0:** signed −1000 is within the 16-bit signed range. Both source values are positive, and their difference is representable.
- **SF = 1:** result bit 15 is set in `0xFC18`.
- **ZF = 0:** the result is nonzero.
- **AF = 0:** the low nibbles `8 − 0` do not require a borrow from bit 4.
- **PF = 1:** parity uses the low byte `0x18`, which contains two set bits.

## Run note

Both programs assembled and linked. Execution in this container is blocked at the 32-bit `int 0x80` exit syscall (`Bad system call`); GDB is also blocked from launching the inferior by the container's `ptrace` restriction.
