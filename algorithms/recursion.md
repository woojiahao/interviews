---
description: >-
  Recursion is quite a typical problem and can be rather tricky to get right on
  the first go. Try visualizing recursive algorithms by tracing them out on
  paper with simpler recursions.
---

# Recursion

## Runtime analysis

### Time complexity

The runtime of recursive algorithms can often be determined by looking at the branching factor and using things like [master theorem](https://en.wikipedia.org/wiki/Master\_theorem\_\(analysis\_of\_algorithms\)) to calculate accurately.

### Space complexity

There is always space used for recursive calls in Python. But in languages with tail-call optimization like Elixir, the space can be $$O(1)$$

## Take note...

1. Define base cases to terminate the recursion
2. Recursion implies using a stack to model the problem so a stack can be used to replace recursive methods
3. Recursion is particularly useful for permutation problems

## Corner cases

1. `n = 0`
2. `n = 1`
3. `n >= len(arr)`

## Techniques

### Reducing space

Rather than doing `acc + [nums[i]]` for each recursive call (this creates a new array every time), use `.append()` before the recursive call and `.pop()` after. Then, when a base case is reached, use `acc[::]` to duplicate the array just once

```python
def fn(i, acc):
    if i >= len(arr):
        ans.append(acc[::])
        return
    for j in range(i, len(arr)):
        acc.append(arr[i])
        fn(j + 1, acc)
        acc.pop()
```

### Memoization

Commonly used to optimize repeated computations. The memoization stores the result of a branch and allows future calls to that branch to re-use the memoized values instead of performing the computation again

* Commonly associated with bottom-up approach

### Tabulation

Storing the result of computations and using them for later computations

* Commonly associated with top-down approach

### Converting to iterative

Store the previous iteration's answer and use it as the basis for the next iteration, performing operations to modify it

* Base case is usually `[]` but it may vary

### Dealing with duplicates

Duplicate values can be particularly annoying to deal with because they can cause repeated and wrong computation. To resolve this, rather than iterating over the entire array directly, iterate over a frequency map of the elements, controlling how many elements appear in each recursive call

* Particularly useful for problems like [Combination Sum 2](https://leetcode.com/problems/combination-sum-ii/) where the duplicates can cause overcounting if naively recursed

For questions about **permutations** where the order of the elements matter, we can continue to use the above strategy or create an auxiliary array that maintains the order of the elements so that the first `1` cannot be used after the second `1`, thus enforcing the order of the elements
