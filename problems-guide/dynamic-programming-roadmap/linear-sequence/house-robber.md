# House Robber

## Transitions

1. Robber chooses to rob the current house
2. Robber does not rob the current house

## Top-down

The naive top-down approach would be trying all possibilities given the transitions:

```python
def house_robber(houses):
    def rob(i):
        if i >= len(houses):
            return 0
        return max(rob(i + 1), rob(i + 2) + houses[i])
    return rob(0)
```

If house `i` isn't robbed, then we can move on to house `i + 1` to try again.

If house `i` is robbed, then we can only start robbing from house `i + 2` onwards.

This generates the following recurrence relation:

$$
dp(i) = \begin{cases}
0, i >= |houses|\\
\max(dp(i+1), dp(i+2)+houses[i])
\end{cases}
$$

## Deriving bottom-up

We will apply the trick of "re-framing the problem" to derive a bottom-up solution. Given a prefix of $$i-1$$ houses, can the optimal answer for house `i` be derived?

To rob house `i`, we must do so when we had not robbed the previous house, thus, we must have robbed at most house `i - 2`.

If house `i` is not going to be robbed, then the most amount of money we can rob from house `0` to `i` is the same as if we only looked out houses `0` to `i - 1`.

As a result, we can form a new recurrence relation:

$$
dp(i) = \begin{cases}
houses[0], i =0\\
\max(houses[0], houses[1]), i = 1\\
\max(dp(i-1), dp(i-2)+houses[i])
\end{cases}
$$

The base cases arise from the following observations:

1. If you only have 1 house, the optimal choice is always to rob it
2. If you have 2 houses, you can either rob the first but not the second or rob the second but not the first so we pick the maximum of the two

## Bottom-up

We will apply the $$n$$ state caching optimization where $$n = 2$$.

```python
def house_robber(houses):
    if len(houses) == 1: return houses[0]
    h1, h2 = houses[0], max(houses[0], houses[1])
    for i in range(2, len(houses)):
        h1, h2 = h2, max(h2, h1 + houses[i])
    return h2
```

Note that `h1` corresponds to `dp(i-2)` and `h2` corresponds to `dp(i-1)`.
