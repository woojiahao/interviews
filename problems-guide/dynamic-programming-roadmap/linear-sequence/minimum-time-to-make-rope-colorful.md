# Minimum Time to Make Rope Colorful

{% hint style="info" %}
There is are approaches that uses [#two-pointers](../../../data-structures/arrays/#two-pointers "mention") or greedy instead but I will not cover those.
{% endhint %}

## Clarification

Consecutive balloons do not count the popped balloons, so popping the middle `R` in `RRR` will still violate the criteria of having no consecutive colors.

## Observations

Let's start with an arbitrary rope with balloons: `RRRGGBB` and an arbitrary cost of `[2, 1, 5, 3, 4, 1, 2]`. From hand-simulation, we know that the ultimate "optimal" answer is `XXRXGBX` where `X` are the popped balloons.

1. For every segment of like colors, we never pop the highest cost balloon to achieve the lowest possible cost within the segment (the mathematical proof is relatively easy to come up with)
2. When encountering a same color, a decision between popping the current balloon or the previously seen, unpopped, same balloon should be made, with the decision made to be the smallest of the two (see (1))
3. For every segment of same color, once a balloon encountered is of a different color, there's no need to pop anymore of the previous color so the color tracked can be changed

## Recurrence relation

$$
dp(i) = \begin{cases}
dp(i-1), r[i] \neq r[prev]\\
dp(i-1)+\min(time[i], time[prev])
\end{cases}
$$

`prev` is needed alongside the recurrence.

Processing from left to right, $$dp(i)$$ can be interpreted as the minimum time to make the rope $$r[:i+1]$$ colorful. This means we are solving for the prefix of the array.

If $$r[i]$$ is the same as the previous color, we need to make the decision to pop based on observation (1) using the minimum between the current cost to pop and the cost to pop the previous same color balloon.

If $$r[i]$$ is not the same, then we simply need to update the "previous color" and we know that $$dp(i) = dp(i-1)$$ since there is no balloons to pop.

## Bottom-up

We will apply the $$n$$ state caching optimization, where $$n = 1$$, only storing the previous time to make the rope colorful as $$t$$.

```python
def min_time(r, time):
    n = len(time)
    t = 0
    prev = 0
    for i in range(1, n):
        if r[i] == r[prev]:
            t += min(time[i], time[prev])
            if time[prev] < time[i]:
                prev = i # since we pop the prev balloon, we have a new prev
        else:
            prev = i # we don't need to track the same color anymore
    return t
```
