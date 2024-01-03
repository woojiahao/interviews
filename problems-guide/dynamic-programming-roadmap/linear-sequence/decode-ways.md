# Decode Ways

## Transitions

The problem actually outlines the transitions rather clearly:

1. If `s[i] = '0'` , then invalid ways (0)
2. If `s[i] = '1'`, then we can choose to take `s[i]` as it is, or pair it with the next digit (no matter what, it will form a valid number)
3. If `s[i] = '2'`, then we can choose to take `s[i]` as it is, or pair it with the next digit as long as the next digit is from `'0'` to `'6'`
4. Any other digits have to be taken as it is

This gives rise to the naive recurrence relation and recursive solution.

## Top-down

$$
dp(i) = \begin{cases}
0, s[i] = 0\\
1, i \geq |s|\\
dp(i+1), s[i] \not \in [1,2]\\
dp(i+1) + dp(i+2), s[i] = 1 \lor (s[i] = 2 \land s[i+1] < 6)
\end{cases}
$$

```python
def decode_ways(s):
    def ways(i):
        if i >= len(s): return 1
        if s[i] == '0': return 0
        ans = ways(i + 1)
        if i + 1 < len(s) and (s[i] == '1' or (s[i] == '2' and s[i + 1] <= '6')):
            ans += ways(i + 2)
        return ans
    return ways(0)
```

We can then memoize the value of each recursive call by index `i`.

We can then turn our attention to converting this to an iterative solution.

## Bottom-up

We will employ the same way we have been converting top-down solutions to bottom-up, by re-framing the question we are asking ourselves. However, this time, there is a crucial observation to be made:&#x20;

We CANNOT figure out the ways to form $$s[:i+1]$$ (much like what we have been trying to do with the prefixes of the previous problems). This is because the nature of decoding involves looking forward to see the next character. Thus, we ask ourselves:

> Given $$[i, n)$$, can we find out how many ways there are to form $$s[i-1]$$?

Notice that we are now looking at the SUFFIX of the string, not the prefix. So we can think of $$dp(i)$$ as the number of ways it takes to form $$s[i:]$$. This is useful to know as we move backwards as it lets us model the ways we can form $$dp(i-1)$$.

For instance, if $$s = `12325276$$ and we are currently looking at $$123\text{ } 2 \text{ } 5276$$, we would have computed the number of ways to form $$5276$$. Given that the current character is $$2$$, we can either take it as it is (i.e. number of ways to form $$5276$$) or take it with the next character $$5$$. If we do the latter, we would instead be looking at the number of ways to form $$276$$. This is why it is so useful that we have processed the suffix. The same cases as [#top-down](decode-ways.md#top-down "mention") but this time, we are looking at the string in reverse.

This gives us the recurrence:

$$
dp(i) = \begin{cases}
1, i = |s|\\
0, s[i] = 0\\
dp(i+1), s[i] \not\in [1, 2]\\
dp(i+1) + dp(i+2), s[i] = 1 \lor (s[i] = 2 \land s[i+1] < 6)
\end{cases}
$$

Notice that it looks very similar to the original recurrence as we are essentially doing the same operations. Additionally, we can apply the $$n$$ state optimization, where $$n = 2$$ given that we are only ever using the next 2 states.

```python
def decode_ways(s):
    n = len(s)
    if n == 1: return 1
    s1, s2 = 1, 0 # s1 -> dp(i+1), s2 -> dp(i+2)
    for i in range(n - 1, -1, -1):
        si = 0 if s[i] == '0' else s1 # equivalent to collapsing the first 3 cases
        if i < n - 1 and (s[i] == '1' or (s[i] == '2' and s[i + 1] <= '6')): 
            si += s2
        s1, s2 = si, s1
    return s1
```

{% hint style="success" %}
This gives rise to another common trick in solving DP, trying to solve the problem backwards (i.e. using suffix over prefix). This is especially useful when each $$dp(i)$$ is dependent on the next character (which makes using the prefix impossible)
{% endhint %}
