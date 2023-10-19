---
description: Sorting algorithms are fun
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
{% endtab %}

{% tab title="Quick Sort" %}
$$
O(n\log n)
$$

* Pick a pivot, partition array around pivot value and repeat for all halves till 1 element
* Same recurrence as merge sort
* If duplicates allowed, use dutch flag algorithm
* Worst case can be $$O(n^2)$$ if array already sorted
{% endtab %}

{% tab title="Counting Sort" %}
$$
O(n+k)
$$

* $$k$$ is the number of distinct elements
* Count frequency of each element
* Reconstruct the array from the frequency
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
