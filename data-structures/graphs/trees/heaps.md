---
description: >-
  Heaps or priority queues are a very powerful data structure that can be used
  to easily store information that has some intrinsic order
---

# Heaps

{% hint style="info" %}
Heaps can be implemented as trees under the hood, but some are also implemented using arrays. I have chosen to stick to the implementation I was taught and park heaps under Trees
{% endhint %}

## Runtime analysis

1. Find min/max: $$O(1)$$
2. Insert: $$O(\log n)$$
3. Remove: $$O(\log n)$$
4. Heapify: $$O(n)$$

## Techniques

### Max heaps

In Python, `heapq` is defaults to using a min heap. This might not be what you want by default. The only way to make `heapq` work as a max heap is by inverting the values inserted

### K-smallest/largest

K-smallest implies using a k-sized max heap, removing the maximum element if the size becomes `> k`

K-largest implies using a k-sized min heap instead

### Using quick select instead

Sometimes, the information required from a k-sized heap can be replicated by using [quick-select.md](../../../algorithms/quick-select.md "mention"). The benefit of doing so is that the runtime can be reduced to $$O(n)$$ whereas with heaps, it would be $$O(n \log n)$$

### Finding the median of data

Simulate the median by having a max heap for the elements to the left of the median and a min heap for the elements to the right of the median
