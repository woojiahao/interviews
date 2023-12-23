---
description: >-
  Binary problems are quite rare in interviews and even online assessments but
  it's always good to know
---

# Binary

## Corner cases

1. Check for overflow/underflow
2. Negative numbers (they use the [twos complement system](https://en.wikipedia.org/wiki/Two's\_complement))

## XOR behavior

The XOR (^) operator is quite commonly used to solve binary problems, these are some important properties you should familiarize yourself with:

1. `n ^ n = 0`
2. `n ^ m == m ^ n`
3. `n ^ (m ^ k) == (n ^ m) ^ k`
4. `n ^ 0 = n`

## Common bitmasks

Bitmasks are commonly used to "force" a certain set of bits to be used. They are also used to constraint Python's numbers as Python doesn't use 32 bits for integers so using a manual bitmask is necessary for constraining it

1. Retrieving the upper 16 bits: `0xffff0000`
2. Retrieving the lower 16 bits: `0x0000ffff`
3. Retrieving all bits in groups of 4: `0xff00ff00`
4. Retrieving all bits in groups of 2: `0xcccccccc`
5. Retrieving all single bits: `0xaaaaaaaa`

## Techniques

1. Test is bit K is set: `num & (1 << k) != 0`
2. Set bit K: `num |= (1 << k)`
3. Turn off bit K: `num &= ~(1 << k)`
4. Toggle bit K: `num ^= (1 << k)`
5. Multiply by $$2^K$$: `num << k`
6. Divide by $$2^K$$: `num >> k`
7. Check if number is power of 2: `(num & num - 1) == 0` or `num & (-num) == num`
8. Remove rightmost set bit: `num & (num - 1)`
9. Swapping two variables (only positive numbers): `num1 ^= num2; num2 ^= num1; num1 ^= num2`

For more information and tricks, refer to this [post.](https://leetcode.com/problems/sum-of-two-integers/solutions/84278/a-summary-how-to-use-bit-manipulation-to-solve-problems-easily-and-efficiently/)
