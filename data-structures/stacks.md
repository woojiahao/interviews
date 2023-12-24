---
description: Stack problems usually come in the form of monotonic stack problems
---

# Stacks

## Runtime analysis

1. Top/peek: $$O(1)$$
2. Push: $$O(1)$$
3. Pop: $$O(1)$$
4. Search: $$O(n)$$
5. isEmpty: $$O(1)$$

## Corner cases

1. Empty stack
2. Stack with one item
3. Stack with two items

## Take note…

1. `pop()` is LIFO so if order of insertion matters, reverse the popped values

## Techniques

### Monotonic stack&#x20;

Push elements while monotonically increasing/decreasing. Then, keep popping the elements once the reverse occurs until either empty or incoming element is eventually increasing/decreasing

* The final stack can represent the overall values

### Mathematical equation parsing

Stacks are useful for problems like evaluating equations as you can push numbers onto the stack and operations just need to pop the top two elements of the stack
