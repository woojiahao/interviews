# Perfect Squares

{% hint style="info" %}
There is a purely [mathematical solution](https://leetcode.com/problems/perfect-squares/solutions/71488/summary-of-4-different-solutions-bfs-dp-static-dp-and-mathematics/) for this as well but I won't cover it here
{% endhint %}

## Observations

* The number of perfect square numbers (PSN) that sum to a perfect square is `1`
* The minimum number of PSNs to form `n` is found by trying all possible combinations of perfect square numbers
* Using PSN `p` means we have to find the minimum number of PSNs to form `n - p` after

## Recurrence relation

$$
dp(n) = \begin{cases}
1, \lfloor{\sqrt{n}}\rfloor \times \lfloor{\sqrt{n}\rfloor} = n\\
\forall p \in Z, p^2 < n \land \min(1+dp(n-p))
\end{cases}
$$

We add $$1$$ to $$dp(n-p)$$ because we are using $$p$$ as the first perfect square.

{% hint style="success" %}
**Recurrence pattern:** past ~~lives~~ states\


Some states rely on multiple previously computed state. This contrasts the $$n$$ state caching optimization as $$n$$ is not fixed.&#x20;
{% endhint %}

## Bottom-up

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
**Trick:** using impossibly high/low numbers to initialize $$dp(i)$$\
\
If `min(dp[i], ...)` is used, then use an impossibly high number to initialize `dp[i]`\
\
Otherwise, if `max(dp[i], ...)` is used, then use an impossibly low number or `0` if no other states can be `0`.
{% endhint %}
