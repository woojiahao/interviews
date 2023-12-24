---
description: >-
  Queue problems are less common but it is still a data structure that is worth
  understanding well
---

# Queues

## Runtime analysis

1. Enqueue/offer: $$O(1)$$
2. Dequeue/poll: $$O(1)$$
3. Front: $$O(1)$$
4. Back: $$O(1)$$
5. isEmpty: $$O(1)$$

## Take note…

1. Built in data structure like `[]` in Python use $$O(n)$$, not $$O(1)$$
   * Check if can assume data structure is optimal
   * To optimize for Python, use `from collections import deque` instead to `popleft()`

## Corner cases

1. Empty queues
2. Queue with one item
3. Queue with two items

## Techniques

### Rotating the queue on itself

Pop the queue and push back onto itself to bring the last element to the front

