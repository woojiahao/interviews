---
description: >-
  Consolidating some of the common patterns and problem solving methods you can
  try when working on problems
---

# General Problem Solving

{% hint style="info" %}
This list is not exhaustive! If you wish to contribute more techniques, please email me at [woojiahao1234@gmail.com](mailto:woojiahao1234@gmail.com)
{% endhint %}

1. Finding the median of data: focus on the definition of a median and model the solution after it
2. Sub-array problems: think about using sliding windows discussed under [arrays](../data-structures/arrays/ "mention")
3. Sub-sequence problems: think about sorting (if possible)
4. $$O(n \log n)$$ upper bound (found using [runtime-predictions.md](runtime-predictions.md "mention"))
   1. Divide and conquer, similar to merge sort
   2. [arrays](../data-structures/arrays/ "mention") manipulation + [binary-search.md](../algorithms/binary-search.md "mention") such as prefix sums + binary search
   3. [sorting.md](../algorithms/sorting.md "mention") + operations on sorted array
   4. [segment-trees.md](../data-structures/graphs/trees/segment-trees.md "mention")+ iterating over all ranges
   5. Applying the sweep line algorithm if there are [intervals.md](../algorithms/intervals.md "mention") + events
5. Minimum/maximum across queries: try using [heaps.md](../data-structures/graphs/trees/heaps.md "mention") or [double-ended-queues.md](../data-structures/queues/double-ended-queues.md "mention")
   1. [heaps.md](../data-structures/graphs/trees/heaps.md "mention") can be used to track the minimum during any query
   2. [double-ended-queues.md](../data-structures/queues/double-ended-queues.md "mention") can be used to represent the current maximum (as the front) and potential maximums (subsequent elements) in the event where the current maximum "expires"
