---
description: >-
  Matrix problems are quite common and are relatively straightforward, just
  focus on understanding the techniques required
---

# Matrices

## Runtime analysis

Most matrix problems have a run time of $$O(mn)$$for a $$m \times n$$matrix

## Corner cases

* Empty matrix (ensure none of the array length is 0)
* 1 by 1 matrix
* Row/column matrix

## Techniques

### Creating an empty M x N matrix

Typically used to initialize the values for traversal or dynamic programming.

* Make a copy of the matrix with the same dimensions with empty values to store the state
* When initializing the rows, must use `for _ in range` rather than `* M` instead, otherwise, any changes to any of the columns affects every single row

```python
# Create zero matrix
# Inner array is the columns
# Outer array is the rows
zero_matrix = [[0] * len(matrix[0]) for _ in range(len(matrix))]

# Copy matrix
copied_matrix = [row[:] for row in matrix]
```

### Matrix transposition

Rows of a matrix becomes the columns and vice versa.

* Used when single direction verification can be repeated (verify horizontally, transpose, verify horizontally again)

{% code fullWidth="false" %}
```python
# *matrix -> return the rows
# zip -> pairs every value in the same position with each other
transposed_matrix = zip(*matrix)

# Bruteforce
transposed = [[0] * len(matrix[0]) for _ in range(len(matrix))]
for i in range(len(matrix)):
	for j in range(len(matrix[0])):
		tranposed[i][j] = matrix[i][j]
```
{% endcode %}

### Traversing common shapes

Commonly used when the elements of a matrix follow a given property and these elements create a common shape that can be traversed

* Common shapes include stairs (as seen in [Count Negative Numbers in a Sorted Matrix](https://leetcode.com/problems/count-negative-numbers-in-a-sorted-matrix/)) or pairs
  * Stair traversal: either move 1 row up/down or move 1 col left/right
* See if can make assumptions about the entire row based on some indicated value (like if first value is < 0, then rest of the row is 0
* Given the shape or pattern, how do we traverse the matrix (when do we move 1 row up/down, when do we move 1 col left/right)

### Treating matrix as flat array&#x20;

Rather than trying to traverse using two for-loops, traverse in one from 0 to $$mn$$. See [searching a fully sorted 2D matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/description/) for an example of such traversal

* Given an index `i` where $$i \in [0, mn]$$, then
  * The equivalent row is `i // n`
  * The equivalent column is `i % n`

### Creating sub-grids

Given a matrix of size `N x N`, break up the matrix into sub-grids of size `M x M`&#x20;

* Given a coordinate `(r, c)`, then associated sub-grid is `(r // 3 * 3, c // 3)`
* Commonly used when trying to calculate a property within sub-grids such as [Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)
