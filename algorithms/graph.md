---
description: >-
  Graph algorithms are quite common and usually fall under the following
  algorithms. You will usually be required to modify these algorithms to fit the
  problem but it's good to know the fundamentals
---

# Graph

## Time complexities

<table><thead><tr><th width="125">Algorithm</th><th width="176">Time Complexity</th><th width="156">Space Complexity</th><th>Remarks</th></tr></thead><tbody><tr><td>BFS</td><td><span class="math">O(E + V)</span></td><td><span class="math">O(V)</span></td><td></td></tr><tr><td>DFS</td><td><span class="math">O(E + V)</span></td><td><span class="math">O(V)</span></td><td>Space accounting for stack frames used in recursion</td></tr><tr><td>Topological Sorting</td><td><span class="math">O(E + V)</span></td><td><span class="math">O(V)</span> for storing the edges and frontier</td><td>Note that when iterating over every node, we only decrease the edges at most <span class="math">O(E)</span> times</td></tr><tr><td>Dijkstra</td><td><span class="math">O((V + E) \log V)</span></td><td><span class="math">O(E)</span> for priority queue</td><td></td></tr><tr><td>Bellman-Ford</td><td><span class="math">O(EV)</span></td><td></td><td>For <span class="math">O(V)</span> vertices, we iterate through all <span class="math">O(E)</span> edges and compute the SSP</td></tr><tr><td>Prim’s Algorithm</td><td><span class="math">O(E \log V)</span> or <span class="math">O(V^2)</span></td><td></td><td>Time complexity achieved if using Fibonacci heap AND iterating over the entire priority queue</td></tr><tr><td>Kruskal’s Algorithm</td><td><span class="math">O(E \log V)</span></td><td></td><td></td></tr></tbody></table>

## Frequency in interviews

1. Common: BFS, DFS
2. Uncommon:  Topological sort, Dijkstra
3. Almost never: Bellman-Ford, Floyd Warshall, Prim’s, Kruskal’s

## BFS

{% hint style="info" %}
Trees do not require a `visited` set since there is only 1 path between nodes (property of trees)
{% endhint %}

```python
from collections import deque

def bfs(matrix):
  # Check for an empty matrix/graph.
  if not matrix:
    return []

  rows, cols = len(matrix), len(matrix[0])
  visited = set()
  directions = ((0, 1), (0, -1), (1, 0), (-1, 0))

  def traverse(i, j):
    queue = deque([(i, j)])
    while queue:
      curr_i, curr_j = queue.popleft()
      if (curr_i, curr_j) not in visited:
        visited.add((curr_i, curr_j))
        # Traverse neighbors.
        for direction in directions:
          next_i, next_j = curr_i + direction[0], curr_j + direction[1]
          if 0 <= next_i < rows and 0 <= next_j < cols:
            # Add in question-specific checks, where relevant.
            queue.append((next_i, next_j))

  for i in range(rows):
    for j in range(cols):
      traverse(i, j)
```

## DFS

```python
def dfs(matrix):
  # Check for an empty matrix/graph.
  if not matrix:
    return []

  rows, cols = len(matrix), len(matrix[0])
  visited = set()
  directions = ((0, 1), (0, -1), (1, 0), (-1, 0))

  def traverse(i, j):
    if (i, j) in visited:
      return

    visited.add((i, j))
    # Traverse neighbors.
    for direction in directions:
      next_i, next_j = i + direction[0], j + direction[1]
      if 0 <= next_i < rows and 0 <= next_j < cols:
        # Add in question-specific checks, where relevant.
        traverse(next_i, next_j)

  for i in range(rows):
    for j in range(cols):
      traverse(i, j)
```

## Topological sorting

* Used for job scheduling a sequence of jobs or tasks that have dependencies on other jobs/tasks
  * Jobs represent vertices and edges from X to Y (directed) if X depends on Y

```python
def graph_topo_sort(num_nodes, edges):
    from collections import deque
    nodes, order, queue = {}, [], deque()
    
    # O(V)
    for node_id in range(num_nodes):
        nodes[node_id] = { 'in': 0, 'out': set() }
		
    # O(E)
    for node_id, pre_id in edges:
        nodes[node_id]['in'] += 1
        nodes[pre_id]['out'].add(node_id)

    # O(V)
    for node_id in nodes.keys():
        if nodes[node_id]['in'] == 0:
            queue.append(node_id)

    # O(E), total number of decreases happen O(E) times at most
    while len(queue):  # At most O(V) elements
        node_id = queue.pop()
        for outgoing_id in nodes[node_id]['out']:  # At most O(V - 1) edges
            nodes[outgoing_id]['in'] -= 1
            if nodes[outgoing_id]['in'] == 0:
                queue.append(outgoing_id)
        order.append(node_id)
    return order if len(order) == num_nodes else None

print(graph_topo_sort(4, [[0, 1], [0, 2], [2, 1], [3, 0]]))
# [1, 2, 0, 3]
```

## Dijkstra

{% hint style="info" %}
Only works with non-negative edge weights. If the edges have a negative weight, use [#bellman-ford](graph.md#bellman-ford "mention") instead
{% endhint %}

### Optimizations

1. If target node known, once we process target node (i.e. pop is target node), we can early return

```python
import heapq
import math

class Solution:
    def networkDelayTime(self, times: List[List[int]], n: int, k: int) -> int:
        graph = {}
        # O(E)
        # Maintain the edge costs + edges
        for [source, target, time] in times:
            if (source - 1) not in graph:
                graph[source - 1] = []
            graph[source - 1].append((target - 1, time))
	       
	# All nodes start with inf cost to reach
        costs = [math.inf] * n
	# Start node has cost of 0 (naturally)
        costs[k - 1] = 0
        q = [(0, k - 1)]
        visited = set()
				
	# O(V log V) + O(E log V)
	# O((E + V) log V)
	# O(E log V)
        while q:  # At most O(V)
	    # O(log V)
            node_cost, node = heapq.heappop(q)
            if node in visited:
                continue
            visited.add(node)
            if node not in graph:
                continue
	    # O(E log V)
            for neighbor, time in graph[node]: # Visit at most O(E) nodes
		# Only update cost and re-queue if the cost to reach neighbor decreases
                if node_cost + time < costs[neighbor]:
                    costs[neighbor] = node_cost + time
                    heapq.heappush(q, (node_cost + time, neighbor))
        
        if len(visited) != n:
            # Disjoint component
	    # Alternative way to check is to check if any cost is still inf
            return -1

        return max(costs)
```

## Bellman-Ford

{% hint style="info" %}
Works with negative weight edges but does not work if there are negative weight **cycles.** For those cases, we can detect that a cycle exists but cannot do anything about it
{% endhint %}

Loop for `v - 1` times and for each loop, relax all edges

To detect negative weight edges, check if the cost of the same node decreases twice

### Optimizations

1. Track if any costs decreased, if none did, then we can early terminate since that means we found the lowest possible cost for all edges

```python
import heapq
import math

class Solution:
    def networkDelayTime(self, times: List[List[int]], n: int, k: int) -> int:
        costs = [math.inf] * n
        costs[k - 1] = 0
        # O(V)
        for _ in range(n - 1):
            has_change = False

	# O(E)
            for source, target, weight in times:
                if costs[target - 1] > costs[source - 1] + weight:
                    costs[target - 1] = costs[source - 1] + weight
                    has_change = True

            if not has_change:
                break
        
        if any([cost == math.inf for cost in costs]):
            return -1
        
        return max(costs) 
```

## Prim’s algorithm

Finds the Minimum Spanning Tree of a graph and is easier to implement than [#kruskals-algorithm](graph.md#kruskals-algorithm "mention")

{% hint style="info" %}
The general intuition for Prim's algorithm is as such:\
\
Given a node `i`, go through all connected points and add the edge weights to a min heap `(weight, other point)`. For every element in the min heap, if visited before, discard, otherwise, use the topmost point’s edge. Iterate at most `N - 1` times as that is the maximum number of edges of a tree.
{% endhint %}

For this specific implementation, the time complexity is $$O(EV \log V)$$

```python
import heapq

# edges are: [from, to, weight]
# n points in total
def prim(n: int, edges: List[List[int]]):
	graph = {}
	# O(E)
	for f, t, w in edges:
		if f not in graph: graph[f] = []
		if t not in graph: graph[t] = []
		graph[f].append((t, w))
		graph[t].append((f, w))

	visited = [False] * n
	h = []
	node = 0
	min_weight = 0
	
	# O(VE log V)
	for i in range(N - 1):
		visited[node] = True
		# O(E log V)
		for neighbor, weight in graph[node]: # Total O(E)
			if not visited[neighbor]:
				heapq.heappush(h, (weight, neighbor))

		# O(log V)
		while visited[h[0][1]]:
			heapq.heappop(h)

		# O(log V)
		weight, node = heapq.heappop(h)
		min_weight += weight

	return min_weight
```

### Fully-connected graphs

For fully connected graphs, instead of using a heap, use a `min_d` array, tracking the minimum weight to reach each point in the graph.

{% hint style="info" %}
The general intuition for this variation is:\
\
For each node `node`, we update the edge weight to all connected points. Then the point we choose to use would be the one that has not yet been visited and has the shortest edge weight.

Once a `node` is visited, set the weight to reach to be `inf` so no extra `visited` is needed
{% endhint %}

```python
import math

def prim(n: int, edges: List[List[int]]):
	graph = {}
	for f, t, w in edges:
		if f not in graph: graph[f] = []
		if t not in graph: graph[t] = []
		graph[f].append((t, w))
		graph[t].append((f, w))

	min_d = [10*8] * n
	node = 0
	min_weight = 0
	for i in range(N - 1):
		min_d[node] = math.inf
		min_j = node
		for neighbor, weight in graph[node]:
			if min_d[neighbor] != math.inf:
				min_d[neighbor] = min(min_d[neighbor], weight)
				min_j = neighbor if min_d[neighbor] < min_d[min_j] else min_j
		min_weight += min_d[min_j]
		node = min_j
	
	return min_weight
```

## Kruskal’s algorithm

Use [union-find-disjoint-set-ufds.md](../data-structures/union-find-disjoint-set-ufds.md "mention") to determine which edges are redundant and use a min heap to store the weights of the edges. Redundant edges are those whose points already exist in the same set (meaning that there exists another path between these two points in the MST so far).

This implementation has a time complexity of $$O(E \log E + E \log V)$$ because we’re not sorting the entire graph at once. If we sorted, the time complexity would be similar but $$O(\log E) = O(\log V^2) = O(2 \log V) = O(\log V)$$ dominates the term so we take that instead

```python
import heapq

def find(ds, i):
	# O(log V)
	if ds[i] == i:
		return i
	
	ds[i] = find(ds, ds[i])
	return ds[i]

def union(ds, p, q):
	# O(log V)
	root_p = find(p)
	root_q = find(q)
	ds[root_p] = root_q

def kruskal(n: int, edges: List[List[int]]):
	ds = list(range(n))
	h = []
	min_weight = 0
	# O(E)
	for f, t, w in edges:
		h.append((w, f, t))
	# O(E)
	heapq.heapify(h)
	used = 0	

	# O(E log E + E log V)
	# O(E)
	while h:
		# O(log E)
		weight, i, j = heapq.heappop(h)
		# O(log V)
		i = find(ds, i)
		j = find(ds, j)
		if i != j:
			min_weight += weight
			union(ds, i, j)
			used += 1
			if used == n - 1:
				break

	return min_weight
			
```
