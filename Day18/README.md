# Day 18 - Dynamic Programming: 1D Fundamentals

## Topic
**Dynamic Programming (DP)** - the pattern that scares candidates most. Start with 1D DP: **Fibonacci, climbing stairs, house robber, coin change.** Master the **state -> transition -> base case** framework.

---

## Why It Matters
- DP is asked in ~15-20% of FAANG interviews - and it's the most failed category.
- The hard part isn't the code; it's **identifying the recurrence**. This skill transfers to recursion, divide-and-conquer, and optimization.
- Today builds the mental model; Days 19-20 expand to 2D and advanced DP.

---

## Key Concepts

### What Is DP?
DP = recursion + memoization (or iteration + tabulation). Use it when a problem has:
1. **Overlapping subproblems** - same subproblem solved repeatedly.
2. **Optimal substructure** - the optimal solution includes optimal solutions to subproblems.

### The 5-Step DP Framework (MEMORIZE)
1. **Define the state:** what does `dp[i]` represent? (Be precise.)
2. **Identify the transition:** how does `dp[i]` relate to earlier states?
3. **Set base cases:** `dp[0]`, `dp[1]`, etc.
4. **Choose iteration order:** usually left-to-right; sometimes reverse.
5. **Identify the answer:** usually `dp[n]` or `max(dp[...])`.

### Two Implementation Styles

**Top-Down (Memoization):** recursive, with a cache.
```go
func fib(n int) int {
    memo := map[int]int{}
    var f func(int) int
    f = func(n int) int {
        if n <= 1 { return n }
        if v, ok := memo[n]; ok { return v }
        memo[n] = f(n-1) + f(n-2)
        return memo[n]
    }
    return f(n)
}
```

**Bottom-Up (Tabulation):** iterative, fill an array.
```go
func fib(n int) int {
    if n <= 1 { return n }
    dp := make([]int, n+1)
    dp[0], dp[1] = 0, 1
    for i := 2; i <= n; i++ { dp[i] = dp[i-1] + dp[i-2] }
    return dp[n]
}
```
> **Bottom-up is preferred in interviews** - no stack overflow, cleaner, often O(1) space via rolling variables.

### Space Optimization
If `dp[i]` only depends on a few previous values, drop the array and use rolling variables:
```go
// Fibonacci with O(1) space
prev, curr := 0, 1
for i := 2; i <= n; i++ { prev, curr = curr, prev+curr }
```

### Classic 1D DP Problems

**House Robber:** `dp[i] = max(dp[i-1], dp[i-2] + nums[i])` - rob current (skip previous) or skip current.

**Climbing Stairs:** `dp[i] = dp[i-1] + dp[i-2]` - same as Fibonacci.

**Coin Change:** `dp[amount] = min(dp[amount-coin] + 1)` over all coins. Bottom-up from 0 to amount.

**Longest Increasing Subsequence (LIS):** `dp[i] = max(dp[j]+1)` for all `j < i` with `nums[j] < nums[i]`. (O(n^2); there's an O(n log n) variant with binary search - Day 30 bonus.)

---

## Problems To Solve

| # | Problem | Difficulty | LeetCode |
|---|---------|-----------|----------|
| 1 | Climbing Stairs | Easy | [70](https://leetcode.com/problems/climbing-stairs/) |
| 2 | House Robber | Medium | [198](https://leetcode.com/problems/house-robber/) |
| 3 | House Robber II (circular) | Medium | [213](https://leetcode.com/problems/house-robber-ii/) |
| 4 | Coin Change | Medium | [322](https://leetcode.com/problems/coin-change/) |
| 5 | Longest Increasing Subsequence | Medium | [300](https://leetcode.com/problems/longest-increasing-subsequence/) |

> **#3 (Robber II):** Trick - run the linear robber on `nums[0:n-1]` and `nums[1:n]`, take the max. (Can't rob both first and last.)
>
> **#5 (LIS):** Learn the O(n^2) DP first; the O(n log n) binary-search version is a Day 30 advanced topic.

---

## Go Tips

- Initialize `dp` slice: `dp := make([]int, n+1)`.
- Use `math.MaxInt` as "infinity" for min-comparison DP (coin change).
- For "impossible" results, return -1 after the DP loop.
- Closures for top-down memoization capture the memo map cleanly.

```go
// Coin Change - bottom up
func coinChange(coins []int, amount int) int {
    dp := make([]int, amount+1)
    for i := 1; i <= amount; i++ {
        dp[i] = math.MaxInt
        for _, c := range coins {
            if c <= i && dp[i-c] != math.MaxInt {
                dp[i] = min(dp[i], dp[i-c]+1)
            }
        }
    }
    if dp[amount] == math.MaxInt { return -1 }
    return dp[amount]
}
```

---

## Complexity
- Fibonacci/stairs: O(n) time, O(1) space (rolling).
- House Robber: O(n) time, O(1) space.
- Coin Change: O(amount * coins) time, O(amount) space.
- LIS (DP): O(n^2) time, O(n) space.

---

## Common Mistakes
- **Not defining the state clearly** - the #1 cause of DP confusion. Write it in a comment.
- Wrong iteration order (coin change must go amount 0 -> n; if iterating coins outer, it's combinations not permutations - for coin change both give correct min, but be careful with variants).
- Forgetting the "impossible" check in coin change (dp[amount] still infinity).
- Using O(n) space when O(1) rolling suffices - interviewers notice.

## Checklist Before Moving On
- [ ] Can state the 5-step DP framework from memory
- [ ] Solved House Robber and Coin Change independently
- [ ] Understand top-down vs bottom-up and their trade-offs
- [ ] Can space-optimize a 1D DP to O(1) when only k previous values matter
