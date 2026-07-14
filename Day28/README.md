# Day 28 - Mock Interview #1 (Array + Trees + Two Pointers)

## Topic
**Simulated interview practice.** Today you run a full mock interview, timed and narrated, on a problem you haven't seen. The goal is to rehearse the *process*, not just the solution.

---

## Why This Day Matters
- Solving LeetCode alone ≠ interview performance. Interviews add: time pressure, communication requirements, and unseen problems.
- Mock interviews train you to **think out loud** - a scored skill interviewers explicitly evaluate.
- Doing this twice (Day 28 and Day 29) builds the composure muscle.

---

## The Interview Simulation Protocol

### Setup
- Pick **one problem you have NOT solved** from the list below (or any Medium you haven't seen).
- Set a timer for **45 minutes**.
- Use a blank editor (no autocomplete hints, no solution tabs open).
- Have a notepad for scratch work.

### The 45-Minute Structure (follow this!)
| Time | Activity |
|------|----------|
| 0-5 min | Read the problem. Ask clarifying questions out loud (edge cases, input guarantees, output format). |
| 5-15 min | Discuss approach(es). Start with brute force, state its complexity, then optimize. Agree on the approach before coding. |
| 15-35 min | Code the solution. Narrate as you type. |
| 35-40 min | Dry-run on the example by hand. Check edge cases. Fix bugs. |
| 40-45 min | State final time/space complexity. Discuss extensions/alternatives. |

### What to Narrate (interviewers score this)
- "The brute force is O(n^2) - I can do better with [pattern] because [signal]."
- "I'll use a hash map to track X, giving O(1) lookups."
- "Let me trace through with input [1,2,3]... at i=1, the pointer is here..."
- "Edge case: empty input - I'll return early."
- "Final complexity: O(n) time, O(k) space."

### Red Flags to Avoid
- Silence (interviewers can't read your mind).
- Jumping to code before discussing approach.
- Not testing / dry-running.
- Saying "it works" without tracing a concrete example.

---

## Mock Interview Problems (pick 1-2 unseen)

| # | Problem | Difficulty | Topics | LeetCode |
|---|---------|-----------|--------|----------|
| 1 | 3Sum | Medium | Two Pointers | [15](https://leetcode.com/problems/3sum/) |
| 2 | Container With Most Water | Medium | Two Pointers | [11](https://leetcode.com/problems/container-with-most-water/) |
| 3 | Product of Array Except Self | Medium | Array, Prefix | [238](https://leetcode.com/problems/product-of-array-except-self/) |
| 4 | Construct Binary Tree from Preorder+Inorder | Medium | Tree, Recursion | [105](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/) |
| 5 | Kth Smallest Element in BST | Medium | Tree, Inorder | [230](https://leetcode.com/problems/kth-smallest-element-in-a-bst/) |
| 6 | Word Break | Medium | DP | [139](https://leetcode.com/problems/word-break/) |
| 7 | Course Schedule | Medium | Graph, Topo Sort | [207](https://leetcode.com/problems/course-schedule/) |

> **#3 (Product Except Self):** Classic prefix-product / suffix-product. No division allowed. O(n) time, O(1) extra space (output array doesn't count). High-frequency at Amazon/Meta.

---

## Self-Evaluation Rubric (be honest)

| Dimension | Score (1-5) | Notes |
|-----------|-------------|-------|
| Problem understanding & clarifying Qs | | |
| Approach quality (brute -> optimal) | | |
| Coding speed & correctness | | |
| Dry-run / testing | | |
| Communication / narration | | |
| Complexity analysis | | |
| Time management (done in 45 min) | | |

**Total: ___ / 35.** Target 25+ before the real interview.

---

## If You Get Stuck (realistic simulation)

In a real interview, you can ask for a hint. Simulate this:
1. Struggle for **20 minutes** first.
2. If truly stuck, allow yourself to read the **approach** (not code) from Discuss.
3. Resume coding.
4. Note what tripped you up - add to weak list.

---

## Post-Mock Reflection (write answers in your notes)
1. Did I understand the problem fully before coding?
2. Did I state the brute force first?
3. Did I communicate throughout?
4. Did I dry-run before saying "done"?
5. Did I state complexity at the end?
6. Where did I lose the most time?

---

## Bonus (if finished early)
Do a **second** mock with a different problem. The more reps, the calmer you'll be on the real day.

---

## Checklist Before Moving On
- [ ] Completed at least one full timed mock interview
- [ ] Filled out the self-evaluation rubric
- [ ] Identified one process weakness to fix on Day 29
