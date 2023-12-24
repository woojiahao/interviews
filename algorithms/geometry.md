---
description: >-
  Geometry problems are quite niche and I have really only ever encountered a
  small handful of them myself. Advent of Code 2023 was where I developed these
  pointers
---

# Geometry

## Techniques

### Check if point is inside polygon

Otherwise known to be raycasting, crossing number algorithm, or the even-odd rule algorithm

* From point $$(x, y)$$, draw a line infinitely long in 1 direction and check how many sides of the polygon it intersects
  * If intersect odd number of points, inside polygon
  * Otherwise, outside of polygon
* Ensure to check for corner pieces such as `L` and `7` pieces and do not count those
