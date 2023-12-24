---
description: >-
  Segment trees are a unique application of trees that help to solve problems
  that involve many range queries over an array
---

# Segment Trees

{% hint style="info" %}
Whenever you see problems that involve multiple range queries like finding the maximum across ranges, explore using segment trees
{% endhint %}

The fundamental idea of segment trees is that each leaf corresponds to an element in the array (by index) and each subsequent parent represents a range of that array, `i..j`. As a result, while segment trees can be represented as an actual tree, it can also just be represented directly using an array.

## Implementation

{% hint style="info" %}
For this implementation of segment trees, updates are relatively expensive because they have to traverse the entire tree to ensure that all nodes are updated accordingly. To optimize updates, you can look into [lazy propagation on segment trees](https://www.geeksforgeeks.org/lazy-propagation-in-segment-tree/)
{% endhint %}

### Building

Build from bottom up using merge sort like algorithms

```cpp
void build(int node, int start, int end)
{
    if(start == end)
    {
        // Leaf node will have a single element
        tree[node] = A[start];
    }
    else
    {
        int mid = (start + end) / 2;
        // Recurse on the left child
        build(2*node, start, mid);
        // Recurse on the right child
        build(2*node+1, mid+1, end);
        // Internal node will have the sum of both of its children
        tree[node] = tree[2*node] + tree[2*node+1];
    }
}
```

### Updating

```cpp
void update(int node, int start, int end, int idx, int val)
{
    if(start == end)
    {
        // Leaf node
        A[idx] += val;
        tree[node] += val;
    }
    else
    {
        int mid = (start + end) / 2;
        if(start <= idx and idx <= mid)
        {
            // If idx is in the left child, recurse on the left child
            update(2*node, start, mid, idx, val);
        }
        else
        {
            // if idx is in the right child, recurse on the right child
            update(2*node+1, mid+1, end, idx, val);
        }
        // Internal node will have the sum of both of its children
        tree[node] = tree[2*node] + tree[2*node+1];
    }
}
```

### Querying

When querying for a range, we're simply looking for the ranges (represented by our nodes) that fully encompass our search range

```cpp
int query(int node, int start, int end, int l, int r)
{
    if(r < start or end < l)
    {
        // range represented by a node is completely outside the given range
        return 0;
    }
    if(l <= start and end <= r)
    {
        // range represented by a node is completely inside the given range
        return tree[node];
    }
    // range represented by a node is partially inside and partially outside the given range
    int mid = (start + end) / 2;
    int p1 = query(2*node, start, mid, l, r);
    int p2 = query(2*node+1, mid+1, end, l, r);
    return (p1 + p2);
}
```
