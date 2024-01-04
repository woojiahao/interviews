# Nth Tribonacci Number

This problem is basically [climbing-stairs.md](climbing-stairs.md "mention") but using three states instead.

## Bottom-up

$$
dp(m) = \begin{cases}
0, m = 0\\
1, m = 1\\
1, m = 2\\
dp(m-2) + dp(m - 1) + dp(m)
\end{cases}
$$

As discussed in [climbing-stairs.md](climbing-stairs.md "mention"), since we only rely on the past `3` states, we can use $$n$$ state caching and store them as variables instead.

```python
def tribonacci(n):
    if n == 0: return 0 # base case
    t0, t1, t2 = 0, 1, 1
    for i in range(3, n + 1):
        t0, t1, t2 = t1, t2, t0 + t1 + t2
    return t2
```
