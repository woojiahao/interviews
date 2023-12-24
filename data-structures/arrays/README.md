---
description: >-
  Array problems are by far the most common type of algorithm problems I have
  encountered. There are a few techniques that are used very commonly so it is
  best to familiarize yourself with all of them
---

# Arrays

## Runtime analysis

* Insert
  * Start/End: $$O(1)$$
  * Middle: $$O(n)$$
* Search
  * Sorted (binary search): $$O(\log n)$$
  * Unsorted (linear search): $$O(n)$$
* Delete: similar to insert
* Access: $$O(1)$$

## Corner cases

1. Empty array
2. Array of size 1 or 2
3. Monotonically increasing/decreasing
4. Duplicate values
5. Consecutive repeated elements
6. Odd number of elements

## Take note…

1. Index out of bounds, check with: $$0 \leq |x| < len(arr)$$
2. Handling duplicate entries
3. Is the array sorted?
   * If so, can you use [binary-search.md](../../algorithms/binary-search.md "mention")instead?
   * If not, can you sort it while preserving information?
4. Do not over-concatenate
   * Use `append` and then copy the accumulation using `acc[::]` to reduce the amount of concatenation, refer to [recursion.md](../../algorithms/recursion.md "mention")for more information on this technique
5. Relationship between elements in an array such as the information that can be gathered based on the values/indices of the elements

## Techniques

### Sorting first

This depends entirely on whether or not the information in the array can be preserved after sorting (take note of the effects of stable vs unstable sorting).

If the array can be sorted first, [binary-search.md](../../algorithms/binary-search.md "mention")could be applied afterwards

* Useful when finding the minimum difference between all elements and finding the lowest possible running total using prefix sums (commonly found in subsequence problems)

If the array cannot be sorted without losing information, then try other solutions

### Two pointers

Pointers that point to different/same points in the array and move (often) independently of one another

* Can be used to traverse across two arrays simultaneously
* Pointers can cross one another
* Commonly used for problems that involve palindromes

When solving two pointer problems, focus on realizing the following conditions (similar to [binary-search.md](../../algorithms/binary-search.md "mention")):

1. When to move the left pointer?
2. When to move the right pointer?
3. Where to move if neither condition is met?

### Sliding window

Sliding windows are simply two pointers that never cross one another. They can be of a fixed size (useful if the window size is pre-defined) or a flexible size (resizing depending on conditions)

To understand how the window moves, first answer the following questions:

1. Is the window fixed?
2. When to expand right?
3. When to retract left? (some problems don't require retracting the left at all)
4. What does each window represent?
5. What to do when an element enters/leaves the window?

Sliding window problems are often paired with frequency arrays like [Longest Substring Without Repeating Character](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

There are two categories of problem that can be solved using sliding windows, namely:

#### Maximizing problems

The sliding window never shrinks, meaning that it is only extending by one or shifting by one, such as in [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

The final answer is usually the size of the sliding window since the starting position or contents of the window does not matter

```python
start = 0
for i in range(len(nums)):
	# Always extend
	window.append(nums[i])
	# Shift to the right
	if not condition:
		window.remove(nums[start])
		start += 1
return len(nums) - start
```

#### Minimizing problems

The sliding window retracts until the window no longer satisfies a given condition, such as [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

The size of the window should be tracked after each retraction and that represents the final answer

```python
window, i, j = 0, 0, 0
while j < len(nums):
	# Extend first
	window += nums[j]
	# Retract till the current window is just right
	while condition:
		window -= nums[i]
		i += 1
		ans = min(ans, j - i + 1) # window size
	j += 1
```

### Reverse traversal

Rather than traversing from left to right, try traversing from right to left and see if new information can be gathered instead

* Try combining both normal and reverse traversal together to see if the problem can be solved that way

### Prefix/Suffix sums

Often used to reduce repeated computation that requires the prefix/suffix of the array

* Not limited to the sum, product and other properties are possible as well
* May require both sums so try that out as well
  * If both are required, there's a chance that the one of the arrays can be converted to an accumulating variable that is computed to save both time and space
* Pay extra attention to whether or not the prefix should include the value at `i`

```python
# Prefix sums
prefix = [0] * len(arr)
prefix[0] = arr[0]
for i in range(1, len(arr)):
    prefix[i] = prefix[i - 1] + arr[i]
    
# Suffix sums
suffix = [0] * len(arr)
suffix[-1] = arr[-1]
for i in range(len(arr) - 2, -1, -1):
    suffix[i] = suffix[i + 1] + arr[i]
```

### Using index as key/Treating indices as buckets

Sometimes, you can visualize the array as a set of buckets where the index corresponds to a key/bucket and allows you to use this information to perform computation. This pattern is quite hard to spot so I have included some questions you can try to develop the intuition for

* Each index represents an element in the array relative to that position (like index 0 is supposed to be element 1)
* Algorithms like cyclic sorting use this assumption
* Common manipulations: negate the value in the cell to indicate that the value is present in some other part of the array or adding the `MAX + 1` to those values

Practice questions:

* First Missing Positive
* Find the Duplicate Number
* Missing Number

### Kadane's algorithm

This is a relatively niche algorithm used in problems like [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/) and [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/). However, it can still be generalized to solve other problems

Focus on understanding how the accumulation of values can be generalized across elements

* Often involves tracking a local value (running variable) and global value (final result)
* Update the global value after every iteration to avoid missing the last element

### Divide and conquer

Divide the existing array into two/`N` parts and perform simpler work on each part. This technique is the corner stone of merge sort

### Iteration patterns

* Iterate over one array while moving the other with a pointer
* Jumping around the array given the current value
