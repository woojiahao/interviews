# House Robber

## Transitions

The problem provides the two transitions available:

1. Robber chooses to rob the current house
2. Robber does not rob the current house

This gives rise to the naive top-down approach.

## Top-down

```python
def house_robber(houses):
    def rob(i):
        if i >= len(houses):
            return 0
        return max(rob(i + 1), rob(i + 2) + houses[i])
    return rob(0)
```

The top-down is a direct replication of the transitions where the optimal choice for each house is to either rob it and skip to the $$i+2$$ house or to not rob it and move to the $$i+1$$ house.

This generates the following recurrence relation:

$$
dp(i) = \max(dp(i+1), dp(i+2)+houses[i])
$$

Much like [climbing-stairs.md](../warmup/climbing-stairs.md "mention"), we can try converting this recursive solution to be iterative by flipping the question to be

> Given houses $$0..i$$, what is the most money that the robber can steal?

By inverting the question, we don't try "looking ahead" for the next answer, but rather, start with thinking of $$dp(i)$$ as assuming there was only $$houses[:i+1]$$ houses available.

## Recurrence relation

The implement the newly thought of idea, we can then try to think about the case where $$house[i]$$ is the last house available.

As a result, we can form a new recurrence relation:

$$
dp(i) = \begin{cases}
houses[0], i =0\\
\max(houses[0], houses[1]), i = 1\\
\max(dp(i-1), dp(i-2)+houses[i])
\end{cases}
$$

At $$house[i]$$, if we want to rob it, we can only do so when we are guaranteed that we had not robbed the previous house, which is only achievable by looking at $$houses[i-2]$$. Since we might have chosen to rob $$houses[i-1]$$ so we cannot guarantee that it has not been robbed

If we do not want to rob $$house[i]$$, we can simply infer the most amount of money we could rob from the previous house instead $$houses[i-1]$$ since there would be no change.

The base cases arise from the following observations:

1. If you only have 1 house, the optimal choice is always to rob it
2. If you have 2 houses, you can either rob the first but not the second or rob the second but not the first so we pick the maximum of the two

Given this new recurrence, we can implement the next step in bottom-up fashion.

{% hint style="info" %}
This highlights how defining state transitions are so important as defining broad transitions like "rob" or "not rob" lets us look at the various recurrences without any anticipated outcomes
{% endhint %}

Another thing we can do is to apply the $$n$$ state optimization, where $$n = 2$$.

## Bottom-up

```python
def house_robber(houses):
    if len(houses) == 1: return houses[0]
    h1, h2 = houses[0], max(houses[0], houses[1])
    for i in range(2, len(houses)):
        h1, h2 = h2, max(h2, h1 + houses[i])
    return h2
```

Note that `h1` corresponds to `dp(i-2)` and `h2` corresponds to `dp(i-1)`.
