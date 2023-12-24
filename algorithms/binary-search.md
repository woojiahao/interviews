---
description: >-
  Binary search typically appears when the array is sorted or its left and right
  halves both possess the same properties
---

# Binary Search

## Runtime analysis

$$
T(n)=T(\frac{n}{2})+O(1)=O(\log n)
$$

## Take note…

* Is array sorted?
  * If not, can you sort it and still preserve information?
* Can the array be separated into two distinct parts where the first half satisfies a condition while the other does not?

## Techniques

### Peak finding

Move towards the direction of larger/smaller values relative to the mid-point chosen.

* This does not work with arrays that can have duplicates (naively picking a direction to move in could be disastrous)
* This can only produce local peaks

### Peak finding in rotated arrays

{% hint style="info" %}
Rotated arrays refer to arrays where the values after index `i` are moved to the front of the array instead, preserving their order\
\
`[1, 2, 3, 4, 5]` can be rotated to become `[3, 4, 5, 1, 2]`
{% endhint %}

The arrays always exhibit the following properties:

* `nums[0] > nums[-1]`
* There are essentially 2 sorted arrays, the boundary can be found by comparing `nums[i]` against `nums[0]`
  * If `nums[i] > nums[0]`, `i` is in the first half, otherwise, it is in the second half

Problems commonly test to see if you can identify these properties (such as [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/) or [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/))

* Use `nums[0]` as a marker for determining where `i` is in the array

### Apply binary search to matrices

Try using the values of the arrays as a range or treating using the pure index like `(0, 0)` to `(n - 1, n - 1)` has the last index as `(n * n) - 1`

* These problems are harder to notice but a common property that can be found is when the matrix is sorted in some order (row)

Problems can include [searching a fully sorted 2D matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/description/) or modifying binary search to search the matrix in a consistent manner such as [Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/solutions/1322101/c-java-python-maxheap-minheap-binary-search-picture-explain-clean-concise/)

## General template

{% tabs %}
{% tab title="Template" %}
{% code lineNumbers="true" %}
```python
left, right = 0, len(nums) - 1  # start to end of array
while left < right:  # terminates when left = right
	mid = left + (right - left) // 2  # avoids overflow
	if nums[mid] < target:  # mid is left leaning
		left = mid + 1  # mid cannot be the answer
	else:
		right = mid  # mid might be the answer
return left if nums[left] == target else -1
```
{% endcode %}
{% endtab %}

{% tab title="Variations" %}
```python
# Search for index of target, else -1 if target does not exist
def search(nums, target):
    lo, hi = 0, len(nums) - 1
    while lo <= hi: # Use <= here since the target index could be at lo == hi
        mid = (lo + hi) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] > target: # Mid is too large, move search space to left
            hi = mid - 1
        else: # Mid is too small, move search space to right
            lo = mid + 1
    return -1 # Not found

# Search for the smallest index i such that nums[i] >= target
def search(nums, target):
    lo, hi = 0, len(nums) - 1
    while lo < hi: # not using <= since if our answer is at lo == hi, infinite loop occurs due to floor division
        mid = (lo + hi) // 2
        if nums[mid] >= target: # mid could be the index we want since nums[mid] >= target
            hi = mid 
        else: # nums[mid] < target, move search space to right
            lo = mid + 1
    return lo # smallest index i that satisfies nums[i] >= target if nums[lo] >= target. If nums[lo] < target, all numbers are < target.

# Search for largest index i such that nums[i] <= target
def search(nums, target):
    lo, hi = 0, len(nums) - 1
    while lo < hi: # not using <= since if our answer is at lo == hi, infinite loop occurs
        mid = (lo + hi) // 2
        if nums[mid] <= target: # mid could be the index we want since nums[mid] <= target
            lo = mid 
        else: # nums[mid] > target, move search space to left
            hi = mid - 1
    return hi # largest index i that satisfies nums[i] <= target if nums[hi] <= target. If nums[hi] > target, all numbers are > target.
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Range" %}
Always from `[0, n)`
{% endtab %}

{% tab title="Condition" %}
`<` vs `<=`

* Former is often used for approximate answers while latter is used for guaranteed answers
* Consider the case when `l == r` and we can still move, where would `l` be? Where would `r` be? Is that information useful to us?
*   If `<=` used, must include the following to avoid infinite loop

    * Must track the answer internally
    * Must move pointers by `m - 1` and `m + 1` to avoid infinite looping

    ```python
    l, r = i + 1, n - 1
    j = -1
    while l <= r:
        m = l + (r - l) // 2
        if values[m][0] >= values[i][1]:
            j = m
            r = m - 1
        else:
            l = m + 1
    ```
{% endtab %}

{% tab title="Pointer Shifts" %}
`mid` is **left leaning:**

`mid = left + (right - left) // 2`&#x20;

* Set `right = mid` and `left = mid + 1`
* Useful when dealing with cases where the `right` could also be the answer
* I.e. finding first instance of element

`mid` is **right leaning**:&#x20;

`mid = left + (right - left) // 2 + 1`&#x20;

* Set `left = mid` and `right = mid - 1`
* Useful when dealing with cases where `left` could also be the answer
* I.e. finding last instance of element
{% endtab %}

{% tab title="Return Value" %}
Run through some examples to figure out where the `left` lies (usually we care only about `left`)
{% endtab %}

{% tab title="Considerations" %}
* When do we move towards the left?
* When do we move towards the right?
* What assumptions can we make about the way the data is arranged?
* Does the middle element tell us anything about everything on the left/right?
{% endtab %}
{% endtabs %}
