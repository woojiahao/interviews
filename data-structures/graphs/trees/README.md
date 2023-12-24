---
description: >-
  Trees are special types of graphs that have exactly N - 1 edges with exactly
  one path between nodes. They are one of the most common category of problems
  so it is a must to master them
---

# Trees

## Terminology

1. **Neighbor:** parent or child of a node
2. **Ancestor:** node reachable by traversing its parent chain
3. **Descendant:** node in the node’s subtree
4. **Degree:** number of children of a node
5. **Degree of a tree:** maximum degree of nodes in the tree
6. **Distance:** number of edges along the shortest path between two nodes
7. **Level/depth:** number of edges along the unique path between a node and the root node
8. **Height:** maximum number of edges between node till last leaf
9. **Width:** number of nodes in a level
10. **Complete binary tree:** every level except possibly last is completely filled, left first
11. **Balanced:** height differs no more than 1

## Traversals

1. Pre-order: SELF, left, right
2. In-order: left, SELF, right
3. Post-order: left, right, SELF

## Runtime analysis

Unbalanced trees have `h = n` in worst case, balanced trees have `h = log n`

1. Access: $$O(h)$$
2. Search: $$O(h)$$
3. Insert: $$O(h)$$
4. Remove: $$O(h)$$

## Corner cases

1. Empty tree
2. Single node
3. Two nodes
4. Skewed tree

## Take note…

1. When using BFS/DFS, `visited` array is not needed since tree has only 1 direction (assuming directed tree)

## Common operations

1. Insert value
   * Find appropriate position and insert
2. Delete value
   * Cases:
     1. No children: delete as per usual
     2. 1 child: swap node with child and delete
     3. 2 child: swap with successor and delete
3. Count number of nodes in tree
4. Whether value is in tree
   * Tree searching: think of conditions to branch left and right
5. Calculate height of the tree
   * `NULL` nodes have height of `-1` while leafs have height of `0`

{% code fullWidth="false" %}
```python
def height(node):
    if not node:
        return -1
    return max(height(node.left), height(node.right)) + 1
```
{% endcode %}

## Techniques

### Recursion

Most tree problems will require [recursion.md](../../../algorithms/recursion.md "mention") to some degree. Some tips to think about recursion for trees:

* Think of each recursive call as returning the value we want for all subsequent calls (often thinking of it as returning value from the left/right sub-tree)
* Try replacing the return result with multiple values (using a tuple or array) or returning another tree node

### Level-order traversal

Some problems can also be solved by traversing in level-order, much like the "virus" traversal for graphs

### Summation of nodes

Nodes can be summed to verify if nodes are negative

### Modifying existing traversal algorithms

Some problems such as [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/) requires some modification to the predefined traversal orders in [#traversals](./#traversals "mention"). As such, think about possibilities of reversing the order of traversal to start from the right first

### Merkle hashing

This is a relatively unused technique but it is useful when you're trying to do tree comparisons to detect mismatches

* `SHA-256` is used to hash every leaf
* Combining the hashes of the left and right forms the hash of the parent's hash
* Repeat this process till the root of the tree to form the tree hash

I have only seen this technique used for [Subtree of Another Tree](https://leetcode.com/problems/subtree-of-another-tree/)

### Split tree problems

This is a unique class of tree problems where we break the problem into two smaller sub-problems:

1. Finding the overall answer
2. Finding the recursive answer

These problems often arise when the optimal answer can be found by either taking a path through a node (thus voiding the remaining path) or by continuing upwards through the traversal (as seen in [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/) and [Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/))

The first sub-problem focuses on solving the overall answer (by assuming a path is taken through the current node) while the second sub-problem focuses on solving the recursive answer, often optimizing the optimal value along a linear path (i.e. does not pass through a sub-tree's root)

```python
ans = 0
def split(node):
    nonlocal ans
    if not node:
        return -10**7
    
    left = split(node.left)
    right = split(node.right)
    ans = max(ans, left + right + node.val)
    return max(left, right) + node.val    
```

### Tree construction

For most tree construction questions, you will need both the pre-order/post-order and in-order traversal to re-construct the tree:

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        indices = {}
        N = len(preorder)
        for i in range(N):
            indices[inorder[i]] = i

        def build(i, l, r):
            if l > r:
                return None

            if l == r:
                return TreeNode(preorder[i])
            
            root_value = preorder[i]
            node = TreeNode(preorder[i])
            node.left = build(i + 1, l, indices[root_value] - 1)
            node.right = build(i + indices[root_value] - l + 1, indices[root_value] + 1, r)
            return node
        
        return build(0, 0, N - 1)
```

This is based on the property that the values in a pre-order traversal represent the root of each sub-tree throughout the traversal. The in-order traversal is split along the root's value, the left being the values found in the left sub-tree and the right being the values found in the right sub-tree

Another interesting property is that if given the choice to serialize a tree, you can achieve a complete reconstruction using only the pre-order traversal given that you also serialize the `NULL` nodes into fixed characters like `#`. This way, we can just continue removing elements from the top traversal and taking those as the left and right values

### Finding the center of a tree

A center node of a tree is one whose sub-trees have the minimum height, causing nodes to be distributed evenly on the left and right. Additionally, a tree can contain at most two centers

To find the center of a tree, topological sort (found under [graph.md](../../../algorithms/graph.md "mention")) can be used, eliminating all leaf nodes at most two nodes remain

```python
class Solution:
    def findMinHeightTrees(self, n: int, edges: List[List[int]]) -> List[int]:
        if n <= 2:
            return list(range(n))

        graph = {i: set() for i in range(n)}
        for a, b in edges:
            graph[a].add(b)
            graph[b].add(a)
        
        frontier = []
        for i, s in graph.items():
            if len(s) == 1:
                frontier.append(i)
            
        while n > 2:
            n -= len(frontier)
            next_frontier = []
            for i in frontier:
                for o in graph[i]:
                    graph[o].remove(i)
                    if len(graph[o]) == 1:
                        next_frontier.append(o)
            frontier = next_frontier

        return frontier
```
