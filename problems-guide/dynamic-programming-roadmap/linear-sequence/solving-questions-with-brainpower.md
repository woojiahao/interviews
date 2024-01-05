# Solving Questions with Brainpower

## Transitions

For question `i`, either:

1. Solve the question, gain the points, and only be able to answer the `i + cooldown` problem onwards
2. Skip the question

## Top-down

```python
def brainpower(questions):
    def solve(i):
        if i >= len(questions): return 0
        return max(solve(i + 1), solve(i + questions[i][1] + 1) + questions[i][0])
    return solve(0)
```

The recurrence relation, much like other [.](./ "mention") problems is obtained by directly mapping the transitions.

$$
dp(i) = \begin{cases}
0, i \geq |q|\\
\max(dp(i+1), dp(i+q[i][1]+1)  +q[i][0])
\end{cases}
$$

## Bottom-up

The recurrence is the same as the top-down approach as we need to have information about the "next" state, so prefixes will not be an ideal solution for this.

This means we will have to iterate from the back. We also add a padding of `0` towards the end of the array to represent the case when `i + 1` or `i + questions[i][1] + 1` exceed `N` (length of questions).

{% hint style="success" %}
**Trick:** Boundary conditions\
\
Very often you will encounter state transitions that can cause out of bounds such as `i + 1 >= N` above. In these scenarios, you can introduction boundary conditions by modifying the `dp` array to include additional leading/trailing elements.\
\
If the state transition may go _very_ out of bounds like `i + questions[i][1] + 1`, then you can use `min(transition, N)` to default it to use the boundary condition.\
\
The value of the boundary condition is often the case when there is nothing to process. In this case, we add `0` as that's the default value that exists when there's "no questions".
{% endhint %}

```python
def brainpower(questions):
    N = len(questions)
    dp = [0] * (N + 1)
    for i in range(N - 1, -1, -1):
        dp[i] = max(dp[i + 1], dp[min(i + questions[i][1] + 1, N)] + questions[i][0]) 
    return dp[0]
```
