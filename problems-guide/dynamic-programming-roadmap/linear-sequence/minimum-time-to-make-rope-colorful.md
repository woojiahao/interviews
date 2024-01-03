# Minimum Time to Make Rope Colorful

{% hint style="info" %}
There is are approaches that uses [#two-pointers](../../../data-structures/arrays/#two-pointers "mention") or greedy instead but I will not cover that here. However, I will note that the DP approach for this problem is the least optimal approach since both the [#two-pointers](../../../data-structures/arrays/#two-pointers "mention")and greedy solutions are more intuitive to implement
{% endhint %}

The problem is actually relatively confusing but to clarify, consecutive balloons excludes the ones being popped so given the following sequence: `RRRGG`, if we popped the middle `R` as such `RXRGG`, there will still be consecutive `R` and `G`.

## Observations

We can actually try solving this by simulating the operation and then noting down some observations.

Let's start with an arbitrary rope with balloons: `RRRGGBB` and an arbitrary cost of `[2, 1, 5, 3, 4, 1, 2]`. To simulate this operation, we can start from the first balloon, `R` and work our way to the last. We know that the ultimate "optimal" answer is `XXRXGBX` where `X` are the popped balloons.

The first observation we can make is that we are popping the lowest cost balloons in each segment of like colors. This is because if we had included the largest value in the popping, we would not have achieved the smallest possible cost to pop (I will leave the mathematical proof for this up to you).

The next observation is that as we are iterating from left to right, if we are encountering the same color, we should make a decision between popping the current same color or the previously seen same color. This is fueled by the first observation, where the optimal choice is always the smaller cost.

A third observation is that after each segment of same colors, we don't need to track that color anymore and can switch to tracking the next color. For instance, within the `R` segment, we need to track where the previous `R` was but once we start the `G` segment, we no longer need to do so.

These three observations give us the approach we can take to solve this.

## Approach

If we process the rope from left to right, we can denote $$dp(i)$$ as the minimum time to make the rope $$r[:i+1]$$ colorful. We are also assured that $$\forall j \in [0, i), dp(j)$$ is optimal as well. Then, for each $$r[i]$$, we can determine what we need to do:

If $$r[i]$$ is the same as the previous color, we need to make the decision to pop based on observation (1) using the minimum between the current cost to pop and the cost to pop the previous same color balloon.

If $$r[i]$$ is not the same, then we simply need to update the "previous color" and we know that $$dp(i) = dp(i-1)$$ since there is no balloons to pop.

## Recurrence relation

$$
dp(i) = \begin{cases}
dp(i-1), r[i] \neq r[prev]\\
dp(i-1)+\min(time[i], time[prev])
\end{cases}
$$

And alongside the recurrence, we need to update the `prev` value depending on $$r[i]$$

## Bottom-up

The approach is quite straightforward to implement from bottom-up. We can also apply the $$n$$ state optimization by using 1 variable to represent $$dp(i - 1)$$.

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
