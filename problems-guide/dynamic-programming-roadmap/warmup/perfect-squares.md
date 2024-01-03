# Perfect Squares

A slight departure from [climbing-stairs.md](climbing-stairs.md "mention") and [nth-tribonacci-number.md](nth-tribonacci-number.md "mention") but this problem is still pretty doable with the following observations.

{% hint style="info" %}
There is a purely [mathematical solution](https://leetcode.com/problems/perfect-squares/solutions/71488/summary-of-4-different-solutions-bfs-dp-static-dp-and-mathematics/) for this as well but I won't cover it as the focus of this guide is to examine how we derive DP relationships.
{% endhint %}

## Observations

First of all, an obvious but key observation is that the number of perfect square numbers that sum to a perfect square is `1`.

Next, observe that for `n` when `n` is not a perfect square number, we will need to try all possible combinations of perfect square numbers to form `n`. If we have tried perfect square number `p`, we will have to try to find the least number of perfect square numbers to form `n - p` instead.

This means, if we process `n - p` in an earlier iteration, we can reuse this value, rather than performing the computation again. This is exactly what DP is useful for.

## Recurrence relation

Given the [#observations](perfect-squares.md#observations "mention"), we can form the recurrence relation:

$$
dp(n) = \begin{cases}
1, \lfloor{\sqrt{n}}\rfloor \times \lfloor{\sqrt{n}\rfloor} = n\\
\forall p \in Z, p^2 < n \land \min(1+dp(n-p))
\end{cases}
$$

We add $$1$$ to $$dp(n-p)$$ because we are using $$p$$ as the first perfect square.

{% hint style="info" %}
This is a very common type of recurrence where the optimal answer relies on various past states that have already been computed. The answer can be derived from fixing a chosen value and then relying on previous states. More of this pattern will be seen in the coming problems.\
\
As a result, we cannot optimize it the way we did with [climbing-stairs.md](climbing-stairs.md "mention") where we store only the past $$n$$ states because $$n$$ can be infinitely large (in theory)
{% endhint %}

## Bottom-up

We can start with the iterative bottom-up approach because it is more intuitive that way:

```python
def perfect_squares(n):
    dp = {}
    for i in range(1, n + 1): # we want to iterate from [1, n]
        root = i**0.5
        if int(root)**2 == i:
            dp[i] = 1
        else:
            p = 1
            dp[i] = 10**9 # we set it to 10^9 because we are using min()
            while p**2 < i:
                dp[i] = min(dp[i], 1 + dp[i - p**2])
                p += 1
    return dp[n]
```

{% hint style="success" %}
A common trick we have here is to set the value of the $$dp$$ to an impossibly high/low number, depending on the operation performed.\
\
If `min(dp[i], ...)` is used, then use an impossibly high number.\
Otherwise, if `max(dp[i], ...)` is used, then use an impossibly low number or `0` if no other states can be `0`.
{% endhint %}
