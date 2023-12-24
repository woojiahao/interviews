---
description: >-
  Hash tables are a corner stone of data structures for most problems, they are
  useful when there are unique keys with given values
---

# Hash Tables

## Runtime analysis

1. Insert: $$O(1)$$
2. Search: $$O(1)$$
3. Delete: $$O(1)$$
4. Update: $$O(1)$$

## Take note…

1. Do we need a custom hashing function?

## Techniques

### Anagram hashing

This technique is particularly when trying to detect all anagrams and store them in buckets of same anagrams

* Multiplicative hash with prime numbers (3 onwards)
* Assign each character a prime number and calculate the hash
* Hash will be unique per anagram
* Does not scale well if more than 52 characters possible or $$n > 5000$$

```python
primes = [3, 5, 7, ...]
def hash(string):
	value = 1
	for ch in string:
		value *= primes[ord(ch) - ord('a')]
	return value
```

### Fingerprint hash tables/arrays over hash tables

I would recommend using these over hash tables where possible as they can represent the same information while reducing the complexity of the code

```python
freq = [0] * 26
# is the same as 
freq = {}
for ch in s:
    if ch not in freq:
        freq[ch] = 0
```

### Duplicate checking using bitmasks

This is a slight optimization over using hash tables/sets if the only thing that needs to be done is duplicate detection

* Not feasible if the numbers can be very large as size grows by powers of 2

```python
acc = 0
for ch in string:
	mask = 1 << (ord(ch) - ord('a'))
	if acc ^ mask < acc: # duplicate found
		return False
	acc |= mask
```

### Multiplicative hashing

$$
h(k) = \lfloor m(\phi \times k - \lfloor \phi \times k \rfloor) \rfloor
$$

* Where $$k$$ is the key to hash, $$\phi$$ is the golden ratio = $$\sqrt{5} - \frac{1}{2}$$

### Pairing with another data structure

Often used to improve performance at the cost of using more space to store the hash table

* Use [linked-lists](linked-lists/ "mention") to optimize to quickly delete/insert nodes in a certain order
  * Hash table has pointers to the nodes of the linked list

## Types of hashing

1. Chaining (linked list per bucket)
2. Open addressing (probing for a blank spot if current bucket is filled)
