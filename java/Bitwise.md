# Explain through examples for the Bitwise operations

## & (and)

On each byte string position, if both value is 1, than the result at that position will be one, else it stays 0.


| num    | as binary string |
| ------ | ---------------- |
| 8      | 00001000         |
| 24     | 00011000         |
| 8 & 24 | 00001000         |

## | (or)

On each byte string position, if any of the value is 1, result at that position will be one.

| num     | as binary string |
| ------- | ---------------- |
| 8       | 00001000         |
| 24      | 00011000         |
| 8 \| 24 | 00011000         |

## ^ (xor)

On eacn byte string position if both are 1, it stays as 0, and else it behaves as the OR operator

| num    | as binary string |
| ------ | ---------------- |
| 8      | 00001000         |
| 24     | 00011000         |
| 8 ^ 24 | 00010000         |

## ~ (NOT)

On each byte string position change the bit position to the opposite one.

| num | as binary string |
| --- | ---------------- |
| 8   | 00001000         |
| ~8  | 11110111         |

## << (Left shift)

shifts the bits in the left direction by a given number.

| num    | as binary string |
| ------ | ---------------- |
| 8      | 00001000         |
| 8 << 2 | 00100000         |


## >> (Right shift)

shifts the bits in the right direction by a given number.

| num    | as binary string |
| ------ | ---------------- |
| 8      | 00001000         |
| 8 >> 0 | 00001000         |
| 8 >> 1 | 00000100         |
| 8 >> 2 | 00000010         |
| 8 >> 3 | 00000001         |
| 8 >> 4 | 00000000         |
