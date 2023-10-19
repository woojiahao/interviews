---
description: >-
  Interval questions might seem hidden so good to focus on trying to identify
  the intervals and model the problem
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

1. Sort the array of intervals by the starting point or by ending point
   * Finding the maximum number of non-overlaps, see [Non-Overlapping Intervals](https://www.notion.so/Non-Overlapping-Intervals-cd21c812ea6f458fb0468566a8cd6fee?pvs=21)
     * Sort by end and if the next interval starts after the current ending, we want to extend the ending only if there isn’t any overlaps
     * To maximise the most non-overlaps, we want to schedule the intervals that end earliest first
2.  Checking if two intervals overlap

    ```python
    def is_overlap(a, b):
    	return a[0] < b[1] and b[0] < a[1]
    ```
3.  Merge intervals

    ```python
    def merge(a, b):
    	return [min(a[0], b[0]), max(a[1], b[1])]
    ```
4. Think in terms of umbrella intervals, see [Non-Overlapping Intervals](https://www.notion.so/Non-Overlapping-Intervals-cd21c812ea6f458fb0468566a8cd6fee?pvs=21)
   * Instead of trying to think of discrete intervals, think of the entire interval as one whole, then operate on it as a whole
5.  Line Sweep Algorithm

    [Lamps/Streetlights Coding Question - Sweep Line Algorithm](https://youtu.be/9wy6OA3Yvpg?si=xPp-yVMM2MGP9jb5)

    * Generally used to model 3 key events: when an interval starts, some event to happen, and when an interval closes

    [https://www.youtube.com/watch?v=phrSBwaBs7o\&list=RDQMJRqhglnbU-8\&start\_radio=1](https://www.youtube.com/watch?v=phrSBwaBs7o\&list=RDQMJRqhglnbU-8\&start\_radio=1)

    * Think of the problem in events and what happens for every interval that starts/stops
    * Normally, end events should happen before start events just to avoid double counting
    *   Questions

        [Loading...](https://leetcode.com/discuss/study-guide/2166045/line-sweep-algorithms)
