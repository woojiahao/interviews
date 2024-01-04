# Min Cost to Climb Stairs

## Observations

* On each step `n`, we must incur the cost since we have to decide whether or not to move one or two steps up
* To reach step `n`, you must move from step `n - 1` or step `n - 2`
* To minimize the cost to reach step `n`, we need to minimize the cost it takes to reach it

## Recurrence relation

$$
dp(n) = \begin{cases}
cost(0), n = 0\\
cost(1), n = 1\\
\min(dp(n-1),dp(n-2)) + cost(n)
\end{cases}
$$

We can define $$dp(n)$$ as the minimum cost it takes to climb from step $$n$$. If we start at the first step, the cost is $$cost(0)$$ and if we start at the second step, the cost is $$cost(1)$$. Otherwise, for every step $$n$$, the cost to climb from it is the cost it took to reach it with the cost of leaving the step.

## Bottom-up

The $$n$$ state caching optimization can be applied here again, given that each state depends only on the previous 2 states.

```python
def climb_stairs(costs):
    s1, s2 = costs[0], costs[1]
    for i in range(2, len(costs)):
        s1, s2 = s2, min(s1, s2) + costs[i]
    return min(s1, s2)
```
