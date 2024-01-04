# Climbing Stairs

## Transitions

Observe that for every stair, you can make two decisions:

1. Climb one step
2. Climb two steps

## Recurrence tree

<figure><img src="../../../.gitbook/assets/image (1).png" alt="" width="563"><figcaption></figcaption></figure>

We don't count leaves that end with values where `v > n`. We can also memoize the results of sub-trees such as `2`, reducing the computation of the right sub-tree to a $$O(1)$$ lookup.

## Top-down

{% hint style="info" %}
Top-down can often be implemented by directly modelling the state transitions
{% endhint %}

```python
def climb_stairs(n):
    memo = {}
    def climb(m):
        if m in memo: return memo[m]
        if m == n: return 1
        if m > n: return 0
        memo[m] = climb(m + 1) + climb(m + 2)
        return memo[m]
```

The recurrence relation above looks like this:

$$
dp(m) = \begin{cases}
1, m == n\\
0, m > n \\
dp(m + 1) + dp(m + 2)
\end{cases}
$$

## Bottom-up

{% hint style="success" %}
**Trick:** Re-framing the problem\
\
A way to convert top-down to bottom-up solutions is to **re-frame the problem** in a way that only **restricts your view of the data to a subset**, such as the prefix/suffix/sub-array. Solving this subset gives you the tools to solve larger subsets.\
\
A good way to do this is to ask: "Given index i, how do I use the solutions of 0 to i-1 (prefix) or i+1 to n (suffix) solve for index i?"
{% endhint %}

By re-framing the problem, we will notice that if we are index $$m$$ and we have computed the optimal solution for $$0..m-1$$ (prefix), then we can obtain the optimal solution for $$m$$ using the following recurrence:

$$
dp(m) = \begin{cases}
1, m = 1\\
2, m = 2\\
dp(m-1) + dp(m - 2)
\end{cases}
$$

{% hint style="success" %}
**Trick:** Interpreting recurrence relations\
\
Define what $$dp(i)$$ means and read the recurrence in terms of that definition.\
\
For instance, the above can be interpreted as:\
\
$$dp(m)$$ is the number of ways to reach step $$m$$\
If $$m = 1$$, then it means we can only take 1 step to reach it (i.e. 1 way)\
If $$m = 2$$, then we can either take 2x1 step or 1x2 steps to reach it (i.e. 2 ways)\
Otherwise, the number of ways to reach step $$m$$ is by stepping once from $$m-1$$ or stepping twice from $$m-2$$
{% endhint %}

```python
def climb_stairs(n):
    if n == 1: return 1 # corner case
    stairs = [0] * (n + 1)
    stairs[1] = 1 # reaching stair 1 takes 1 way
    stairs[2] = 2 # reaching stair 2 takes 2 ways
    for i in range(3, n + 1):
        stairs[i] = stairs[i - 1] + stairs[i - 2]
    return stairs[n]
```

## Optimization

From the recurrence relation, we only ever require the previous 2 states, `dp(m - 1)` and `dp(m - 2)` to compute `dp(m)`. So, we can store these 2 states as variables instead of using an array.

{% hint style="success" %}
**Optimization:** $$n$$-state caching\
\
If your recurrence relies on a finite number of $$n$$ states only, we can use $$n$$ variables to represent these states, removing the need for an array
{% endhint %}

```python
def climb_stairs(n):
    if n == 1: return 1 # corner case
    s1, s2 = 1, 2 # s1 -> dp(m-2), s2 -> dp(m-1) 
    for i in range(3, n + 1):
        s1, s2 = s2, s1 + s2
    return s2
```

{% hint style="info" %}
Keen observers will notice that this is basically the Fibonacci sequence
{% endhint %}
