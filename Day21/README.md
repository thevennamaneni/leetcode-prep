# Day 21 - Review & Reinforce (Week 3 Consolidation)

## Topic
**Spaced repetition review.** Week 3 was the hardest material (graphs + DP). Re-solve the highest-yield problems from memory. DP especially requires repetition to "stick."

---

## Why This Day Matters
- DP and graphs have the steepest learning curve - one pass is not enough.
- Many candidates "understand" DP solutions when reading but blank out when writing. Re-solving from memory builds the recall you need under interview pressure.
- You're 70% through the plan - this review cements the foundation for the final stretch.

---

## Review Problems (Re-solve from memory)

| # | Problem | Day | LeetCode |
|---|---------|-----|----------|
| 1 | Word Search | 15 | [79](https://leetcode.com/problems/word-search/) |
| 2 | Number of Islands | 16 | [200](https://leetcode.com/problems/number-of-islands/) |
| 3 | Rotting Oranges | 16 | [994](https://leetcode.com/problems/rotting-oranges/) |
| 4 | Course Schedule | 17 | [207](https://leetcode.com/problems/course-schedule/) |
| 5 | Network Delay Time | 17 | [743](https://leetcode.com/problems/network-delay-time/) |
| 6 | Coin Change | 18 | [322](https://leetcode.com/problems/coin-change/) |
| 7 | Longest Increasing Subsequence | 18 | [300](https://leetcode.com/problems/longest-increasing-subsequence/) |
| 8 | Longest Common Subsequence | 19 | [1143](https://leetcode.com/problems/longest-common-subsequence/) |
| 9 | Edit Distance | 19 | [72](https://leetcode.com/problems/edit-distance/) |
| 10 | Word Break | 19 | [139](https://leetcode.com/problems/word-break/) |
| 11 | Longest Palindromic Subsequence | 20 | [516](https://leetcode.com/problems/longest-palindromic-subsequence/) |
| 12 | Decode Ways | 20 | [91](https://leetcode.com/problems/decode-ways/) |

> Time-box: 15-20 min per problem. If stuck, mark and move on.

---

## Self-Assessment Rubric

| Score | Meaning |
|-------|---------|
| 3 | Solved cleanly, no hints, < 10 min |
| 2 | Solved with minor bugs, < 20 min |
| 1 | Needed to peek at notes/solution |
| 0 | Couldn't solve |

**DP problems are the most likely to score 0-1.** Add them to your weak list for Day 30.

---

## Pattern Recognition Drill

For each, name the pattern and write the recurrence (no full code):

1. "Fewest coins to make amount X" → ?  (state + transition)
2. "Number of ways to decode a digit string" → ?
3. "Longest common subsequence of two strings" → ?
4. "Min edits to transform word1 to word2" → ?
5. "Can you finish all courses given prerequisites?" → ?
6. "Shortest time for signal to reach all nodes" → ?
7. "Longest palindromic subsequence" → ?
8. "All fresh oranges rot - how many minutes?" → ?

<details>
<summary>Answers</summary>

1. Coin change: `dp[a] = min(dp[a-coin]+1)` over coins
2. Decode ways: `dp[i] = (valid single? dp[i-1]) + (valid pair? dp[i-2])`
3. LCS: `match ? dp[i-1][j-1]+1 : max(dp[i-1][j], dp[i][j-1])`
4. Edit: `match ? dp[i-1][j-1] : 1 + min(del, ins, rep)`
5. Topological sort / cycle detection (Kahn's)
6. Dijkstra from source k; answer = max distance
7. `s[i]==s[j] ? dp[i+1][j-1]+2 : max(dp[i+1][j], dp[i][j-1])`
8. Multi-source BFS; answer = BFS levels
</details>

---

## DP Recurrence Derivation Practice
Pick a problem you haven't solved and **derive** the recurrence before coding:
- [416. Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/) (Medium) - reduces to 0/1 knapsack with target = sum/2
- [198 (already solved)](https://leetcode.com/problems/house-robber/) - re-derive, don't recall

---

## Heap & Dijkstra Boilerplate Check
From memory:
1. Write a Go min-heap of `pair{dist, node}` structs (all 5 methods).
2. Write Dijkstra's main loop using that heap.

This combination appears in many graph problems - have it memorized.

---

## Bonus (if time allows)
- [746. Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/) (Easy) - gentle DP warmup
- [695. Max Area of Island](https://leetcode.com/problems/max-area-of-island/) (Medium) - DFS with area tracking

---

## Checklist Before Moving On
- [ ] Re-solved all 12 review problems
- [ ] Updated weak list (especially DP entries)
- [ ] Wrote Dijkstra + heap boilerplate from memory
- [ ] Pattern drill answered correctly
- [ ] Ready for the final week (advanced + mocks)
