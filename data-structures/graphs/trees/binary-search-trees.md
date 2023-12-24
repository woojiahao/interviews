---
description: Binary Search Trees are the first of many specialized trees
---

# Binary Search Trees

{% hint style="info" %}
Binary search trees have the core property where `left <= node < right` thus allowing elements to be stored in sorted fashion easily
{% endhint %}

## Runtime analysis

BSTs have the same runtime as [.](./ "mention") but if the tree is balanced, it implies that insertions of elements in sorted order can be done in $$O(\log n)$$ which is faster than what inserting into a sorted array can achieve

## Core properties

1. `left <= node < right`
2. In-order traversal produces a sorted array
3. The maximum value exists on the rightmost leaf of the right sub-tree
4. The minimum value exists on the leftmost leaf of the left sub-tree

## Techniques

Most of the techniques of a BST is the same as those [.](./ "mention"), however, there is a technique that only BSTs can achieve

### Using in-order traversal

The core property of BSTs allow in-order traversal to become a sequential traversal of the elements in sorted order. Once a problem mentions that the tree is a BST, try thinking of how to exploit this property to solve the problem
