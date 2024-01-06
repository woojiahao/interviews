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

## Further discussion

Actually, there is a way to implement the bottom-up solution using the "re-framing the problem" trick.

> Given that we want to solve question `i`, what's the maximum points achievable?

By thinking of the problem this way, we can devise an approach to figure out how to forcibly "solve" question `i`. We can iterate through `0..i-1` to find all days where we would have solved it and the cooldown does not exceed `i`. This results in the following recurrence instead:

$$
dp(i) = \max(q[i][0], \forall j \in [0, i), j + q[j][1] < i \land dp(j) + q[i][0])
$$

The reason we do not take $$dp(i - 1)$$ is because if $$dp(i - 1)$$ is chosen, we might not be able to solve question `i`. The only problem with this solution is that it takes $$O(n^2)$$ time to compute which does not fit within the constraints of the problem.

However, I would like to bring this up as an alternative thought process to highlight how sometimes "re-framing" the problem might not always be the optimal solution.
