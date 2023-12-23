---
description: >-
  Sorting algorithms aren't usually tested alone. They are often paired with
  another problem type and the goal is to really see if you're able to think
  about the algorithms in a wider context.
---

# Sorting

{% tabs %}
{% tab title="Bubble Sort" %}
$$
O(n^2)
$$

* Traverse from left to right, swap element with neighbor is neighbor is less than element
* Repeat till no more swaps are needed
* Invariant: after every iteration, the largest elements are arranged at the end in ascending order
{% endtab %}

{% tab title="Selection Sort" %}
$$
O(n^2)
$$

* Traverse from left to right, maintaining a “sorted” section in the front of the array
* Every iteration, we seek the next smallest element in the “unsorted” section and insert to the front
* Invariant: after iteration $$j$$, the front $$j$$ elements are arranged the smallest elements sorted in ascending order
{% endtab %}

{% tab title="Insertion Sort" %}
$$
O(n^2)
$$

* Traverse from left to right
* When encountering a number less than the current element, we move backwards, finding elements before the current element that is greater than the number
* Repeat till a smaller number found, and then swap the elements into position
* Invariant: after iteration $$j$$, the front $$j$$ elements are arranged in ascending order (no guarantee to be the smallest $$j$$
{% endtab %}

{% tab title="Merge Sort" %}
$$
O(n \log n)
$$

* $$T(n) = 2T(n/2) + O(n)$$
* Break up the array into half and merge at the end
* Stop breaking it up once only 1 element is left
* Invariant: the leftmost set of elements will be sorted before the rightmost

```python
# [left, right)
def merge_sort(arr, left, right):
    if left == right:
        return arr[left]
    elif left < right:
        mid = left + (right - left) // 2
        left_merged = merge_sort(arr, left, mid)   # [left, mid)
        right_merged = merge_sort(arr, mid, right) # [mid, right)
        return merge(left_merged, right_merged)
        
def merge(a, b):
    c = []
    i, j = 0, 0
    while i < len(a) and j < len(b):
        if a[i] < b[j]:
            c.append(a[i])
            i += 1
        else:
            c.append(b[j])
            j += 1
    idx, remaining = (i, a) if i < len(a) else (j, b)
    for k in range(idx, len(remaining)):
        c.append(remaining[k])
        
    return c
```
{% endtab %}

{% tab title="Quick Sort" %}
$$
O(n\log n)
$$

* Pick a pivot, partition array around pivot value, and repeat for all halves till 1 element
* Same recurrence as merge sort
* If duplicates allowed, use [dutch flag algorithm](https://www.geeksforgeeks.org/sort-an-array-of-0s-1s-and-2s/), otherwise, can refer to the partitioning algorithms used in [Quick Select](quick-select.md)
* Worst case can be $$O(n^2)$$ if array already sorted
{% endtab %}

{% tab title="Counting Sort" %}
$$
O(n+k)
$$

* $$k$$ is the number of distinct elements
* Count frequency of each element
* Reconstruct the array from the frequency
* Can appear as ways to re-construct an array that has very little elements but many duplicates

```python
def counting_sort(arr):
    offset = min(arr)
    freq = [0] * (max(arr) - offset + 1)
    for num in arr:
        freq[num - offset] += 1
    result = []
    for i in range(freq):
        while freq[i] > 0:
            result.append(i + offset)
    return result
    
```
{% endtab %}

{% tab title="Bucket Sort" %}
$$
O(n + \frac{n^2}{k} + k)
$$

* For every element in the array, create buckets that correspond to some property (such as value and each bucket is a range)
* Sort elements of each individual bucket
* Join all buckets to form result
* Usually best if $$k \approx n$$ so the overall runtime could be $$O(n)$$
{% endtab %}
{% endtabs %}
