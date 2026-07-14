# Day 20 - Dynamic Programming: Advanced Patterns

## Topic
**Advanced DP patterns** - subsequence DP (palindromes, LIS variants), interval DP (matrix chain / burst balloons), and 2D state DP with constraints (interleaving, distinct subsequences).

---

## Why It Matters
- These are the "Hards that become Mediums once you see the pattern."
- Palindromic DP is a small family with a distinctive state definition - very high ROI.
- This day solidifies your ability to **derive** a recurrence, not just memorize one.

---

## Key Concepts

### Palindromic Subsequence / Substring DP
State: `dp[i][j]` = answer for substring `s[i..j]`.

**Longest Palindromic Substring:** expand around centers OR `dp[i][j] = s[i]==s[j] && (j-i < 2 || dp[i+1][j-1])`. Iterate by **length** of substring (shortest first), since longer ranges depend on shorter ones.

**Longest Palindromic Subsequence (count):**
- `s[i] == s[j]`: `dp[i][j] = dp[i+1][j-1] + 2`.
- else: `dp[i][j] = max(dp[i+1][j], dp[i][j-1])`.

> **Iteration order matters:** for substring DP, you must iterate by **increasing length** (or by `i` descending and `j` ascending) so that `dp[i+1][j-1]` is computed before `dp[i][j]`.

```go
// Longest Palindromic Subsequence
func longestPalindromeSubseq(s string) int {
    n := len(s)
    dp := make([][]int, n)
    for i := range dp { dp[i] = make([]int, n) }
    for i := n-1; i >= 0; i-- {
        dp[i][i] = 1
        for j := i+1; j < n; j++ {
            if s[i] == s[j] {
                dp[i][j] = dp[i+1][j-1] + 2
            } else {
                dp[i][j] = max(dp[i+1][j], dp[i][j-1])
            }
        }
    }
    return dp[0][n-1]
}
```

### Counting Distinct Subsequences
`dp[i][j]` = number of distinct subsequences of `s[0..i]` equal to `t[0..j]`.
- `s[i] == t[j]`: `dp[i][j] = dp[i-1][j-1] + dp[i-1][j]` (use s[i] or skip).
- else: `dp[i][j] = dp[i-1][j]` (must skip).

### Interleaving String
`dp[i][j]` = can `s3[0..i+j]` be formed by interleaving `s1[0..i]` and `s2[0..j]`.
- `dp[i][j] = (dp[i-1][j] && s1[i-1]==s3[i+j-1]) || (dp[i][j-1] && s2[j-1]==s3[i+j-1])`.

### Stock DP (State Machine)
"Best Time to Buy and Sell Stock with Cooldown/Transaction Fee/k transactions." Use a state machine: `dp[i][holding?]` or `dp[i][transactions][holding?]`. Transitions encode buy/sell/rest.

---

## Problems To Solve

| # | Problem | Difficulty | LeetCode |
|---|---------|-----------|----------|
| 1 | Longest Palindromic Substring | Medium | [5](https://leetcode.com/problems/longest-palindromic-substring/) |
| 2 | Palindromic Substrings | Medium | [647](https://leetcode.com/problems/palindromic-substrings/) |
| 3 | Longest Palindromic Subsequence | Medium | [516](https://leetcode.com/problems/longest-palindromic-subsequence/) |
| 4 | Best Time to Buy and Sell Stock IV | Hard | [188](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/) |
| 5 | Decode Ways | Medium | [91](https://leetcode.com/problems/decode-ways/) |

> **#5 (Decode Ways):** `dp[i]` = ways to decode `s[0..i]`. If `s[i]` is a valid single digit ('1'-'9'), add `dp[i-1]`. If `s[i-1..i]` is a valid two-digit number ("10"-"26"), add `dp[i-2]`. Watch leading zeros and "0" handling.

---

## Go Tips

- For substring DP, allocate `dp[n][n]` and iterate `i` from `n-1` down to `0`, `j` from `i` up to `n-1`. This guarantees `dp[i+1][j-1]` is ready.
- Expand-around-center is an alternative for #1 and #2 that's O(1) space - know both; mention it as an optimization.
- For stock DP, model states explicitly as variables: `hold`, `cash`, `cooldown`.

```go
// Stock with cooldown (state machine)
sold, held, rest := 0, math.MinInt, 0
for _, p := range prices {
    prevSold := sold
    sold = held + p
    held = max(held, rest - p)
    rest = max(rest, prevSold)
}
```

---

## Complexity
- Palindrome substring/subsequence: **O(n^2)** time and space.
- Distinct subsequences: **O(len1 * len2)**.
- Stock k-transactions: **O(n * k)**.
- Decode ways: **O(n)**.

---

## Common Mistakes
- **Wrong iteration order** in substring DP - `dp[i+1][j-1]` used before it's computed. Always iterate by length or by `i` descending.
- In decode ways: not handling "0" (a single '0' is invalid; only "10"/"20" work as two-digit).
- In stock DP: confusing the state transitions (buying should come from `cash - price`, not `cash`).
- Forgetting base cases on the diagonal (`dp[i][i] = 1` for palindromes).

## Checklist Before Moving On
- [ ] Solved palindromic substring and subsequence (know the iteration order!)
- [ ] Solved Decode Ways (watch the edge cases)
- [ ] Understand state-machine DP for stock problems
- [ ] Can derive a 2D recurrence from a problem statement, not just recall it
