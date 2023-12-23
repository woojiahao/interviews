---
description: >-
  Quick select often comes up when the question asks for the first/last K
  elements or the Kth largest/smallest element
---

# Quick Select

## Runtime

$$
T(n) = T(\frac{n}{2}) + O(n)
$$

* Best/Average: $$O(n)$$ can guaranteed by shuffling the array
* Worst: $$O(n^2)$$if sorted

## Techniques

### Kth Element

Arrange largest elements in front by inverting the pivot comparison. This is a common problem type that tests if you know exactly how the partitioning algorithm works.

## Partitioning Algorithms

{% hint style="info" %}
Pick a pivot, partition elements smaller than the pivot to the left, and elements larger to the right of the pivot
{% endhint %}

{% tabs %}
{% tab title="Lomuto" %}
{% hint style="info" %}
`idx-1` is the pivot index after performing the partitioning the array. Think of `idx` as the position for the next element that is smaller than the pivot value
{% endhint %}

* Easier to implement

{% code lineNumbers="true" %}
```python
def select(start, end):
    # Randomly choose a pivot value to guarantee O(n)
    random = randint(start, end)
    # Swap the pivot value to the end
    points[end], points[random] = points[random], points[end]
    # Calculate the value of the pivot
    pivot_value = calculate(points[end])
    idx = start
    for i in range(start, end + 1):
        # If current value less than pivot, then swap to idx position
        if calculate(points[i]) <= pivot_value:
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
{% endcode %}
{% endtab %}

{% tab title="Hoare" %}
{% hint style="info" %}
Choose first element as pivot
{% endhint %}

* Faster in theory

Two pointers, find 2 elements where `arr[left] > pivot` and `arr[right] < pivot`, swap and continue. When pointers converge, they converge at where the pivot should be so swap `arr[start]` and `arr[left]`.

{% code lineNumbers="true" %}
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
{% endcode %}
{% endtab %}
{% endtabs %}
