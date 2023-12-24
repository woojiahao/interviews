---
description: >-
  Tries are special types of trees that make searching and storing strings more
  efficient
---

# Tries

```python
class Trie:
	def __init__(self):
		self.ch = [None] * 26
		self.end = False
	
	def insert(self, word):
		if not word:
			self.end = True
			return

		cur = self
		for ch in word:
			if not cur.ch[ord(ch) - ord('a')]:
				cur.ch[ord(ch) - ord('a')] = Trie()
			cur = cur.ch[ord(ch) - ord('a')]
		cur.end = True

	def word(self, target):
		if not target: return self.end
		
		cur = self
		for ch in target:
			if not cur.ch[ord(ch) - ord('a')]: return False
			cur = cur.ch[ord(ch) - ord('a')]
		return cur.end
```

## Runtime analysis

### Time complexity

`m` is the length of the string

* Search: $$O(m)$$
* Insert: $$O(m)$$
* Remove: $$O(m)$$

### Space complexity

$$O(m \times n)$$ where $$n$$ is the number of strings and $$m$$ is the length of the longest string

## Corner cases

1. Searching for a string in an empty trie
2. Inserting empty strings into a trie

## Techniques

### Preprocessing list of words

Use a trie to store a list of words to improve the efficiency for searching for a word of length `k` among `n` words

* It takes only $$O(k)$$ over $$O(n)$$

### Using DFS for pattern matching

Pattern matching with values like `*` and `.` can be performed using DFS where branching is done to all valid "next" characters when these characters are encountered, otherwise normal traversal is used

### Inverting what is stored

Some problems will use tries by storing a different set of data available, try out various inputs to see what works best

* See problems like [Word Search 2](https://leetcode.com/problems/word-search-ii/) for reference

### Augmenting nodes to store words that end at the node

This is a slight optimization to reduce the need to accumulate the strings as the Trie traversal is occurring

* See problems like [Word Search 2](https://leetcode.com/problems/word-search-ii/) for reference
