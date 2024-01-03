# Min Cost to Climb Stairs

## State transitions

Much like [climbing-stairs.md](../warmup/climbing-stairs.md "mention"), there are two ways to progress the state. The first is by taking one step and incurring the cost of that step and the second is by taking two steps and incurring the cost of that step instead.&#x20;

## Observations

In order to minimize the cost to climb all of the steps, we can start smaller and try to find the minimum cost it takes to climb to the steps before the top. If we want to reach step `n`, we first have to reach either steps `n - 1` or `n - 2` and then incur the cost of step `n`.

To minimize this cost, we have to pick out the `min(step(n-1), step(n-2))`.

This gives rise to the recurrence relation.

## Recurrence relation

$$
dp(n) = \begin{cases}
cost(0), n = 0\\
cost(1), n = 1\\
\min(dp(n-1),dp(n-2)) + cost(n)
\end{cases}
$$

Before we even code it out, we realize that we can optimize it by storing only the past $$n$$ states, with $$n = 2$$ in this case.

{% hint style="warning" %}
Normally, you should not prematurely optimize for DP problems but since this is a recurring optimization that we have dealt with before, it does not really matter
{% endhint %}

## Bottom-up

It's relatively straightforward to implement this recurrence relation iteratively from the get go. You can try implementing this recursively if you want to gain more experience (hint: try starting from the end).

```python
def climb_stairs(costs):
    costs += [0] # account for when we are on the final step that has no cost (top)
    s1, s2 = costs[0], costs[1]
    for i in range(2, len(costs)):
        s1, s2 = s2, min(s1, s2) + costs[i]
    return s2
```

{% hint style="success" %}
A trick for many DP problems is to add "boundaries" or "padding" to the existing array/string/number to account for overflows. In this problem, we append`0` to the `costs` to represent the final step that you take (i.e. the goal) that has no cost to reach.\
\
These boundaries are often inspired by the nature of the problem and its scope.
{% endhint %}
