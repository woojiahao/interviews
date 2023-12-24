---
description: >-
  UFDS is a straightforward but powerful data structure often used to determine
  when data is in the same set as one another
---

# Union-Find Disjoint Set (UFDS)

## Runtime analysis

| Type                              | Find(p)  | Union(p, q) |
| --------------------------------- | -------- | ----------- |
| Quick Find                        | O(1)     | O(n)        |
| Quick Union                       | O(n)     | O(n)        |
| Weighted union (union by rank)    | O(log n) | O(log n)    |
| Path compression                  | O(log n) | O(log n)    |
| Weighted union + path compression | a(m, n)  | a(m, n)     |

## Implementation

### Key variables

```python
parent = list(range(n + 1))
rank = [0] * (n + 1)
```

### Find with path compression

```python
def find(p):
	if p == parent[p]:
		return p
	
	parent[p] = find(parent[p])
	return parent[p]
```

### Union by rank

```python
def union(p, q):
	root_p = find(p)
	root_q = find(q)
	if rank[root_p] > rank[root_q]:
		parent[root_q] = root_p
		rank[root_p] += 1
	else:
		parent[root_p] = root_q
		rank[root_q] += 1
```
