---
description: >-
  String problems are very similar to array problems as they both use very
  similar techniques but there are some slight differences
---

# Strings

## Runtime analysis

Similar to [arrays](arrays/ "mention")with some differences:

1. Concatenation: $$O(m + n)$$
2. Find substring: $$O(m \times n)$$ (can be improved using Rabin Karp)
3. Slice: $$O(m)$$
4. Split by token: $$O(m + n)$$
5. Strip: $$O(n)$$

## Corner cases

1. Empty strings
2. Strings with 1 or 2 characters
3. Strings with repeated characters
4. Strings with only distinct characters

## Take note...

1. Input character set (ASCII, UTF-8, all lowercase alphabets, etc.)
2. Case sensitivity

## Techniques

Similar to [arrays](arrays/ "mention")with some additions:

1. Think about using two pointers more
2. Count the frequency of characters using a fingerprint frequency array over a hash table if input character set is fixed size
3. Bitmasking to find duplicates
4. Anagram checking using frequency over sorting
5. Palindrome checking using converging two pointers
6. Counting the number of palindromes using two pointers diverging from middle
