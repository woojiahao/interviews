# Nth Tribonacci Number

This problem is actually very similar [climbing-stairs.md](climbing-stairs.md "mention") (Fibonacci Sequence) and is a simple extension.

However, the question actually removes any guesswork on what pattern we are relying on and so we can focus on the implementation instead.

{% hint style="info" %}
I won't cover all three implementations, if you want to see how to simplify this type of DP relationship, refer to [climbing-stairs.md](climbing-stairs.md "mention")
{% endhint %}

## Bottom-up

The bottom up DP has the following recurrence relation:

$$
dp(m) = \begin{cases}
0, m = 0\\
1, m = 1\\
1, m = 2\\
dp(m-2) + dp(m - 1) + dp(m)
\end{cases}
$$

As discussed in [climbing-stairs.md](climbing-stairs.md "mention"), since we only rely on the past `3` states, we can use variables to represent them instead, rather than using an array to store all past states.

```python
def tribonacci(n):
    if n == 0: return 0 # base case
    t0, t1, t2 = 0, 1, 1
    for i in range(3, n + 1):
        t0, t1, t2 = t1, t2, t0 + t1 + t2
    return t2
```
