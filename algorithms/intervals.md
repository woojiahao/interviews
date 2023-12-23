---
description: >-
  Interval problems aren't easy to spot and require some practice, try looking
  for ways to model the problem as a set of ranges/intervals.
---

# Intervals

## Take note…

1. Clarify if `[1, 2]` and `[2, 3]` are considered overlapping intervals
2. Clarify if `[a, b]` will strictly follow `a < b`

## Corner cases

1. No intervals
2. Single interval
3. Two intervals
4. Non-overlapping intervals
5. Interval totally consuming within another
6. Duplicate intervals
7. Intervals which start right where another ends

## Techniques

### Overlap checking

{% hint style="info" %}
Try to remember it as checking`0 < 1` in both intervals
{% endhint %}

```python
def is_overlap(a, b):
	return a[0] < b[1] and b[0] < a[1]
```

### Merge intervals

This is commonly used when looking to combine a bunch of intervals into a giant interval as long as there is some overlap between them

```python
def merge(a, b):
	return [min(a[0], b[0]), max(a[1], b[1])]
```

### Sorting first

Sort the array of intervals by the starting point or by ending point first

{% hint style="info" %}
It is useful to think of why we need to sort and what information can be gathered from sorting first (i.e. what guarantees do we have once we sort)
{% endhint %}

* Used to find the maximum number of non-overlaps, see [Non-Overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)
  * Sort by end and if the next interval starts after the current ending, we want to extend the ending only if there isn’t any overlaps
  * To maximize the most non-overlaps, we want to schedule the intervals that end earliest first

### Umbrella intervals

Instead of trying to think of discrete intervals, think of the entire interval as one whole, then operate on it as a whole

* This can be useful when we don't actually care about the interval spans but rather whether or not the interval can reach a certain point like in [Jump Game 2](https://leetcode.com/problems/jump-game-ii/description/)

### Line sweep

{% embed url="https://www.youtube.com/watch?v=phrSBwaBs7o&list=RDQMJRqhglnbU-8&start_radio=1" %}

A relatively interesting class of problems where each interval represents a duration of an event occurring. When the interval starts, the event starts and when the interval ends, so does the event.

* To model these problems, create events for the start/end of intervals and any additional intermediate events (usually arranged as `(point, event_type)`)
  * The order of the events is dependent on how they are calculated, for instance, if the end of an interval should be counted before starting another interval
  * However, typically, `end interval` events should occur before `start interval` ones
* Sort this array and then simulate by running through it linearly

Common problems are the [streetlights problem](https://www.youtube.com/watch?v=9wy6OA3Yvpg\&feature=youtu.be)

### Operate at the interval level

Rather than focusing on individual values within an interval, try solving by breaking intervals into sub-intervals

* Useful when optimizing problems that have too many values inside an interval
* Take extra care when dealing with leading and trailing intervals that may be leftover from doing the partitioning

A good example of this is part 2 of Advent of Code 2023 Day 5
