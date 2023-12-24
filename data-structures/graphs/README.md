---
description: >-
  Graph problems are plentiful but the core data structure does have some unique
  techniques that can be applied to it
---

# Graphs

{% hint style="info" %}
Most of the algorithms used for graph problems are found under [graph.md](../../algorithms/graph.md "mention") instead
{% endhint %}

## Runtime analysis

* DFS/BFS/Topological sort: $$O(|V| + |E|)$$
* Number of vertices is $$O(V)$$
* Number of edges is $$O(E)$$

## Graph representations

1. Adjacency matrix
2. Adjacency list
3. Hash table of hash tables

## Take note…

* Tree like diagrams could be a graph with cycles so clarify before assuming
* Correctly track visited nodes and not visit each node more than once

## Corner cases

1. Empty graphs
2. Graph with one or two nodes
3. Disconnected graphs
4. Graphs with cycles

## Techniques

### Shifting the goalpost

Rather than starting from the original problem, try reversing the problem and start with the end state in mind or the other set of elements

### "Virus" traversal

This method of traversal is also known as "level-order BFS" but I like to name is as a "virus" traversal as you can imagine it as a virus spreading across the graph.

The infected nodes spread to their neighbors at once, must like how level-order BFS works

* Queue all elements that fit a criteria and BFS from that queue at once
* Ensure that `visited` set is maintained so no duplicate elements are queued
* Useful for propagating an operation to all neighbors

Each batch of processing of processing is often seen as "1 day" of virus spreading of one step.
