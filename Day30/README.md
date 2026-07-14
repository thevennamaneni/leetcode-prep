# Day 30 - Final Review, Weak-List & Cheat Sheet

## Topic
**Consolidation day.** Re-solve your accumulated weak-list problems, finalize your cheat sheet, and rehearse the interview meta-strategy. No new material - this is about confidence and closing gaps.

---

## Why This Day Matters
- 29 days of learning built breadth; today builds **confidence and recall**.
- Your weak list (from Days 7, 14, 21) is your highest-ROI study target.
- A polished cheat sheet and rehearsed interview protocol reduce anxiety on the real day.

---

## Part 1: Re-Solve Your Weak List (2-3 hours)

Go through every problem you scored 0 or 1 on review days. Re-solve each **from memory**.

### Weak-List Triage
| Category | Likely weak spots | Priority |
|----------|-------------------|----------|
| DP | Recurrence derivation, iteration order, space optimization | HIGH - re-derive 3 recurrences |
| Graphs | Dijkstra boilerplate, topo sort cycle check | HIGH - rewrite both |
| Backtracking | Copy-the-path bug, duplicate skipping | MEDIUM |
| Trees | Serialize/deserialize, max-path-sum | MEDIUM |
| Sliding Window | Variable window shrink with `for` vs `if` | MEDIUM |
| Binary Search | "Binary search on answer" problems | MEDIUM |
| Bit manipulation | Sign issues, `^` for NOT | LOW |
| Heap | Go `container/heap` boilerplate | MEDIUM - rewrite once |

### If your weak list is short, add these "must-knows":
- [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/) (sliding window)
- [3Sum](https://leetcode.com/problems/3sum/) (two pointers + dup skip)
- [Word Break](https://leetcode.com/problems/word-break/) (DP)
- [Course Schedule](https://leetcode.com/problems/course-schedule/) (topo sort)
- [LRU Cache](https://leetcode.com/problems/lru-cache/) (design)

---

## Part 2: Finalize Your Cheat Sheet

Your cheat sheet (started Day 7) should now contain one entry per pattern. Format:

```
PATTERN: [name]
SIGNAL: [problem keywords that trigger this pattern]
TEMPLATE: [3-5 line pseudocode skeleton]
TIME: [complexity]   SPACE: [complexity]
GO NOTES: [idiomatic Go detail]
```

### Patterns to include (15 entries):
1. Two Pointers (opposite) - sorted pair search
2. Two Pointers (fast/slow) - in-place / cycle
3. Sliding Window (fixed) - subarray of size k
4. Sliding Window (variable) - longest/shortest with constraint
5. Binary Search (standard) - sorted lookup
6. Binary Search (on answer) - minimize the maximum
7. Hash Map (complement) - two-sum family
8. Monotonic Stack - next greater / daily temperatures
9. Linked List (dummy head + reverse) - structural mods
10. Tree (BFS level-order / DFS with closure-accumulator)
11. Heap (top-k, two-heap median)
12. Backtracking (choose/explore/un-choose + copy path)
13. Graph BFS (shortest path, multi-source) / DFS (components)
14. Topological Sort (Kahn's) / Dijkstra
15. DP (1D + 2D framework, 5-step method)

---

## Part 3: Interview Meta-Strategy (rehearse this)

### The 45-Minute Interview Playbook
1. **Clarify (2-3 min):** Repeat the problem back. Ask about edge cases, input size, data types, guarantees (sorted? duplicates?).
2. **Examples (2 min):** Walk through the provided example. Invent a small one if none given.
3. **Brute force (3-5 min):** State the obvious solution and its complexity. Say you'll optimize.
4. **Optimal approach (5-10 min):** Name the pattern. Explain WHY it fits (the signal). Sketch the algorithm in prose or pseudocode. Get buy-in: "Does this approach sound reasonable?"
5. **Code (15-20 min):** Narrate. Use meaningful names. Handle edge cases as you go (or note them).
6. **Dry-run (5 min):** Trace the example through your code line by line. Fix bugs.
7. **Complexity (2 min):** State time and space. Justify briefly.
8. **Discuss (if time):** Alternatives, trade-offs, follow-ups.

### Communication Phrases to Use
- "Let me make sure I understand the problem..."
- "A brute-force approach would be... that's O(n^2). I think we can do better with..."
- "The signal here is [sorted / contiguous / k-largest], which suggests [pattern]."
- "Let me trace through with this input to verify..."
- "The time complexity is O(n) because each element is visited once; space is O(k) for the..."
- "One edge case I'm checking: empty input..."

### If You Get Stuck
- Restate the problem and your current approach out loud.
- Try a smaller example by hand.
- Consider: "What if the input were sorted? What if there were no duplicates?"
- Ask: "I'm considering between [approach A] and [approach B] - any preference?" (collaborative, not helpless).
- It's OK to ask for a hint after 15-20 min of genuine effort.

---

## Part 4: Day-Before-Interview Checklist
- [ ] Sleep 8 hours.
- [ ] Re-read your cheat sheet (no new problems).
- [ ] Do **2 warmup problems** (Easy/medium you know cold) to build momentum - don't burn energy on Hards.
- [ ] Prepare your environment: stable connection, quiet room, pen and paper.
- [ ] Have water nearby.

---

## Part 5: Mindset
- You've solved ~90 problems across 15 patterns. That's a strong foundation.
- FAANG interviewers want to **see you think**, not just produce the answer. A candidate who communicates well and solves 80% of a problem beats one who silently produces 100%.
- If you blank: breathe, restate the problem, try a small example. Pattern recognition kicks in once you relax.

---

## Final Pattern-Frequency Table (what to expect)
| Pattern | Approx. interview frequency |
|---------|------------------------------|
| Arrays / Two Pointers / Sliding Window | Very High |
| Hash Maps | Very High |
| Trees / BST | High |
| Binary Search | High |
| Graphs (BFS/DFS/Topo) | High |
| DP (1D & 2D) | High |
| Backtracking | Medium-High |
| Heaps / Top-K | Medium |
| Linked Lists | Medium |
| Stacks / Monotonic | Medium |
| Intervals / Greedy | Medium |
| Trie / Union-Find | Lower |
| Bit Manipulation | Lower |
| Math / String algos | Lower |

---

## You're Ready. Good Luck.

```
30 days. 15 patterns. ~90 problems. One senior Go engineer.
```

Open the real LeetCode tab, breathe, and trust the reps. You've got this.
