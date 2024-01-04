# Minimum Cost for Tickets

## Transitions

1. On day `i`, if there is an existing ticket covering today, then there is no need to buy any more
2. On day `i`, if there are not existing tickets covering today, then we can try buying all three kinds of tickets

A part of the transitions involves keeping track of the duration of the ticket bought.

## Top-down

Modelling the transitions, we get:

```python
def cost_tickets(days, prices):
    def solve(i, expiry):
        if i >= len(days): return 0
        if days[i] < expiry: return solve(i + 1, expiry)
        buy_one = solve(i + 1, days[i] + 1) + prices[0]
        buy_seven = solve(i + 1, days[i] + 7) + prices[1]
        buy_thirty = solve(i + 1, days[i] + 30) + prices[2]
        return min(buy_one, buy_seven, buy_thirty)
    return solve(0, 0)
```

The recurrence relation is:

$$
dp(i, e) = \begin{cases}
0, i \geq |days|\\
dp(i+1, e), days[i] < e\\
\min \begin{cases}
dp(i+1, days[i]+1)+prices[0]\\
dp(i+1, days[i]+7)+prices[1]\\
dp(i+1, days[i]+30)+prices[2]
\end{cases}
\end{cases}
$$

Where we would be trying to buy all types of tickets whenever we aren't being covered by a ticket. However, this means that we are trying about $$3^N$$ possibilities. While memoization can help, it may be better to try reducing the trial-and-error.

## Bottom-up

> Given days $$[0, i)$$, what is the minimum cost to travel on day `i`?

If we intend to buy a ticket on day `i`, we cannot be under an existing ticket. So we have to look backwards by (1/7/30) days to find the last time that ticket was bought.

To determine what's the cheapest, we would simply pick the cheapest among the three possible choices when we are looking back.

Hence, the following recurrence relation:

$$
dp(i) = \begin{cases}
0, i-1 < 0 \lor i -7 < 0 \lor i -30 < 0\\
dp(i-1), i \not\in days\\
\min \begin{cases}
dp(i-1) + prices[0]\\
dp(i-7) + prices[1]\\
dp(i-30) + prices[2]
\end{cases}
\end{cases}
$$

If we aren't going to travel on day `i`, then we should not try purchasing a ticket, and instead defer the computation via $$dp(i) = dp(i - 1)$$.

```python
def cost_tickets(days, prices):
    last_day = days[-1]
    dp = [0] * (last_day + 1)
    days = set(days)
    for i in range(1, last_day + 1):
        if i not in days: 
            dp[i] = dp[i - 1]
        else:
            one_day = dp[max(0, i - 1)] + prices[0]
            seven_days = dp[max(0, i - 7)] + prices[1]
            thirty_days = dp[max(0, i - 30)] + prices[2]
            dp[i] = min(one_day, seven_days, thirty_days)
    return dp[last_day]
```
