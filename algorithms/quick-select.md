---
description: >-
  Quick select is typically useful for questions that ask for the top K elements
  or some variation of that question
---

# Quick Select

## Runtime

$T(n) = T(\frac{n}{2}) + O(n)$

Best/Average: $O(n)$

Worst: $O(n^2)$ if sorted

## Techniques

1. Arrange largest elements in front

## Partitioning algorithm

General idea: pick a pivot, partition elements smaller than the pivot to the left and elements larger to the right of the pivot

1.  Lomuto’s algorithm (easier to implement)

    \<aside> 💡 Treat `idx` as a pseudo pivot position so every element that is less than the actual pivot moves before it.

    \</aside>

    ```python
    select(start, end):
      pivot = arr[end - 1]
      idx = start
      for i in range(start, end):
        if arr[i] <= pivot:  # arranges smallest first (flip to get largest)
          arr[idx], arr[i] = arr[i], arr[idx]
          idx += 1
      if idx == k - 1:  # idx is where the pivot is
        return k
      elif idx < k - 1:
        return select(idx, end)  # search from [idx, end)
      else:
        return select(start, idx - 1)  # search from [start, idx - 1) (don't want to repeat pivot)

    select(0, len(arr))
    ```

    Alternative implementation: have noticed that any other variations if we use `i - 1` will not work so should try to follow this implementation strictly

    ```python
    def select(start, end):
        random = randint(start, end)
        points[end], points[random] = points[random], points[end]
        pivot_distance = euclidean(points[end]) # fix the value
        idx = start
        for i in range(start, end + 1):
            if euclidean(points[i]) <= pivot_distance:
                points[i], points[idx] = points[idx], points[i]
                idx += 1
        return idx - 1

    s, e, pos = 0, len(points) - 1, len(points)
    while pos != k:
        pos = select(s, e)
        if pos < k:
            s = pos + 1
        else:
            e = pos - 1
    return points[:k]
    ```
2.  Hoare’s algorithm (faster)

    \<aside> 💡 Choose first element as pivot

    \</aside>

    Two pointers, find 2 elements where `arr[left] > pivot` and `arr[right] < pivot`, swap and continue. When pointers converge, they converge at where the pivot should be so swap `arr[start]` and `arr[left]`.

    ```python
    partition(start, end):
    	i, j = start, end + 1
    	while True:
    		while i < end and arr[i] < arr[start]:
    			i += 1
    		while j > start and arr[i] > arr[start]:
    			j -= 1
    		if i >= j:
    			break
    		swap(arr[i], arr[j])
    	swap(arr[start], arr[j])
    	return j
    ```

## Optimisations

1. Guarantee $O(n)$: shuffle array
