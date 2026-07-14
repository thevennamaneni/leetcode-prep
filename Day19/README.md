# Day 19 - Dynamic Programming: 2D & Grid

## Topic
**2D Dynamic Programming** - grid path problems, the 0/1 knapsack, and 2D state problems (unique paths, edit distance). Yesterday's framework scales to 2D states.

---

## Why It Matters
- 2D DP is where problems get "interview-hard" - exactly the level FAANG targets.
- Edit distance and knapsack are classic algorithms worth knowing by name.
- Many string DP problems reduce to `dp[i][j]` = "answer for s1[0..i], s2[0..j]".

---

## Key Concepts

### 2D DP State Definition
`dp[i][j]` typically represents "the answer for the subproblem considering the first `i` rows/elements of X and first `j` of Y" (or a grid cell at row `i`, col `j`).

### Grid Path DP (Unique Paths)
From top-left to bottom-right, moving only down/right. `dp[i][j] = dp[i-1][j] + dp[i][j-1]`.

```go
func uniquePaths(m, n int) int {
    dp := make([][]int, m)
    for i := range dp {
        dp[i] = make([]int, n)
        dp[i][0] = 1
    }
    for j := 0; j < n; j++ { dp[0][j] = 1 }
    for i := 1; i < m; i++ {
        for j := 1; j < n; j++ {
            dp[i][j] = dp[i-1][j] + dp[i][j-1]
        }
    }
    return dp[m-1][n-1]
}
```

### Grid with Obstacles / Min Path Sum
- With obstacles: `dp[i][j] = 0` if the cell is blocked.
- Min path sum: `dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])`.

### 0/1 Knapsack
Given weights/values and a capacity, maximize value with each item used at most once.
`dp[i][w]` = max value using first `i` items with capacity `w`.
Transition: `dp[i][w] = max(dp[i-1][w], dp[i-1][w - weight[i]] + value[i])`.

**Space optimization:** use a 1D array, iterate capacity **backwards** (to avoid reusing an item in the same pass).

### Longest Common Subsequence (LCS)
`dp[i][j]` = LCS length of `s1[0..i]` and `s2[0..j]`.
- If `s1[i-1] == s2[j-1]`: `dp[i][j] = dp[i-1][j-1] + 1`.
- Else: `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`.

### Edit Distance (Levenshtein)
`dp[i][j]` = min edits to convert `word1[0..i]` to `word2[0..j]`.
- If chars match: `dp[i][j] = dp[i-1][j-1]`.
- Else: `dp[i][j] = 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])` (delete, insert, replace).

---

## Problems To Solve

| # | Problem | Difficulty | LeetCode |
|---|---------|-----------|----------|
| 1 | Unique Paths | Medium | [62](https://leetcode.com/problems/unique-paths/) |
| 2 | Minimum Path Sum | Medium | [64](https://leetcode.com/problems/minimum-path-sum/) |
| 3 | Longest Common Subsequence | Medium | [1143](https://leetcode.com/problems/longest-common-subsequence/) |
| 4 | Edit Distance | Medium | [72](https://leetcode.com/problems/edit-distance/) |
| 5 | Word Break | Medium | [139](https://leetcode.com/problems/word-break/) |

> **#5 (Word Break):** `dp[i]` = can segment `s[0..i]`. For each `i`, try every word in dict: if `s[i-len(word):i] == word` and `dp[i-len(word)]` is true, then `dp[i] = true`. Alternatively iterate j < i and check `s[j:i]` in a set.

---

## Go Tips

- 2D slice allocation:
```go
dp := make([][]int, m)
for i := range dp { dp[i] = make([]int, n) }
```
- For LCS/edit distance, the DP table is `(len1+1) x (len2+1)` - the +1 is for the empty-prefix base case. Don't forget it.
- Space-optimizing to 1D + a "prev diagonal" variable is common - mention it to the interviewer even if you implement the 2D version first.

---

## Complexity
- Unique paths / min path sum: **O(m * n)** time and space (O(n) space optimized).
- LCS: **O(len1 * len2)** time and space.
- Edit distance: **O(len1 * len2)**.
- Knapsack: **O(n * capacity)**.
- Word break: **O(n^2 * wordLen)** with substring checks, or O(n^2) with a set.

---

## Common Mistakes
- **Off-by-one in table size**: forgetting the +1 for empty prefixes in string DP.
- Wrong base case (e.g., `dp[0][0]` should be 0 for edit distance, 1 for unique paths).
- In knapsack space optimization: iterating capacity **forwards** allows reusing items (that's unbounded knapsack, a different problem!).
- In word break: not handling the empty string / not seeding `dp[0] = true`.

## Checklist Before Moving On
- [ ] Solved LCS and Edit Distance independently
- [ ] Understand the +1 table-size convention for string DP
- [ ] Can explain the knapsack transition and why capacity iterates backwards
- [ ] Solved Word Break (high-frequency)
