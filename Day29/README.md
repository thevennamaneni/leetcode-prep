# Day 29 - Mock Interview #2 (Graph + DP + Hard)

## Topic
**Second mock interview**, focused on the harder categories (graphs, DP, backtracking) and at least one Hard problem. This simulates the upper end of interview difficulty.

---

## Why This Day Matters
- Day 28 rehearsed process; Day 29 rehearses **difficulty**.
- Many FAANG rounds include one Hard or Hard-leaning Medium - you need reps on harder problems under time pressure.
- Two mocks give you comparison data: are you improving process or just pattern recognition?

---

## Mock Interview Protocol (same as Day 28)
- Pick **2 unseen problems** below: one Medium, one Hard.
- **35 min for the Medium, 45 min for the Hard** (Hard gets more time).
- Narrate throughout. Use the same structure: clarify -> brute -> optimal -> code -> dry-run -> complexity.

---

## Mock Interview Problems

### Medium (pick 1 unseen)
| # | Problem | Topics | LeetCode |
|---|---------|--------|----------|
| 1 | Combination Sum | Backtracking | [39](https://leetcode.com/problems/combination-sum/) |
| 2 | Letter Combinations of Phone Number | Backtracking | [17](https://leetcode.com/problems/letter-combinations-of-a-phone-number/) |
| 3 | Number of Islands | Graph DFS | [200](https://leetcode.com/problems/number-of-islands/) |
| 4 | Rotting Oranges | Graph BFS | [994](https://leetcode.com/problems/rotting-oranges/) |
| 5 | Longest Palindromic Substring | DP / Two Pointers | [5](https://leetcode.com/problems/longest-palindromic-substring/) |
| 6 | Top K Frequent Elements | Hash, Heap | [347](https://leetcode.com/problems/top-k-frequent-elements/) |
| 7 | Merge Intervals | Intervals, Sort | [56](https://leetcode.com/problems/merge-intervals/) |

### Hard (pick 1 unseen)
| # | Problem | Topics | LeetCode |
|---|---------|--------|----------|
| 1 | Median of Two Sorted Arrays | Binary Search | [4](https://leetcode.com/problems/median-of-two-sorted-arrays/) |
| 2 | Merge K Sorted Lists | Heap, Linked List | [23](https://leetcode.com/problems/merge-k-sorted-lists/) |
| 3 | Binary Tree Maximum Path Sum | Tree DFS | [124](https://leetcode.com/problems/binary-tree-maximum-path-sum/) |
| 4 | Word Search II | Trie, Backtracking | [212](https://leetcode.com/problems/word-search-ii/) |
| 5 | Edit Distance | DP | [72](https://leetcode.com/problems/edit-distance/) |
| 6 | Alien Dictionary | Graph, Topo Sort | [269](https://leetcode.com/problems/alien-dictionary/) (premium) |

> **#1 (Median of Two Sorted Arrays):** The O(log(min(m,n))) solution partitions the two arrays so left halves equal right halves in size and max(left) <= min(right). This is a famous Hard - even if you can't get the optimal, the O(n+m) merge solution shows competence.

---

## Self-Evaluation Rubric (same as Day 28)

| Dimension | Medium | Hard |
|-----------|--------|------|
| Understanding & clarifying | | |
| Approach quality | | |
| Coding correctness | | |
| Dry-run / testing | | |
| Communication | | |
| Complexity analysis | | |
| Time management | | |

**Compare to Day 28:** Did you improve in process? In pattern recognition?

---

## Hard Problem Strategy (general)

When facing a Hard:
1. **Don't panic.** Hards are often a Medium pattern with a twist or an extra constraint.
2. **Identify the core pattern** (sliding window, DP, BFS, backtracking) from the signals.
3. **Solve a simpler version first** (ignore a constraint), then add it back.
4. **Partial credit is real.** A correct O(n) solution to a Hard's simpler version + clear explanation of how you'd optimize beats silence.
5. **Communicate the gap:** "The optimal is O(log n) via binary search on the partition; I'll implement the O(n+m) merge first for correctness, then discuss the optimization."

---

## Process Weaknesses (from Day 28) - Fix Here
- Did you skip clarifying questions on Day 28? **Ask 2-3 today.**
- Did you jump to code too fast? **Spend 10 min on approach today.**
- Did you skip dry-run? **Trace the example explicitly today.**
- Did you forget complexity? **State it verbally before "done."**

---

## Post-Mock Reflection
1. Which pattern did each problem reduce to?
2. How long until you identified the pattern? (Aim: < 5 min.)
3. Did you finish each within the time budget?
4. What's the single biggest process improvement for the real interview?

---

## Checklist Before Moving On
- [ ] Completed 2 timed mocks (1 Medium + 1 Hard)
- [ ] Filled out rubrics for both
- [ ] Compared to Day 28 - noted improvement
- [ ] Identified final process adjustments for the real interview
