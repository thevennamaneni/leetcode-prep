# Day 7 - Review & Reinforce (Week 1 Consolidation)

## Topic
**Spaced repetition review.** No new concepts today. Re-solve the hardest problems from Days 1-6 *from memory* and identify weak spots.

This day is non-negotiable. Cramming without review = forgetting 70% within a week.

---

## Why This Day Matters
- Research shows spaced repetition is the most effective retention technique.
- Interviews require recall under pressure. Re-solving builds that muscle.
- You'll discover which patterns didn't "click" - fix them now before stacking more.

---

## Review Problems (Re-solve from memory, no solution peeking)

Re-implement these from scratch in a blank file. Time yourself (target: solve in <60% of your original time).

| # | Problem | Day | LeetCode |
|---|---------|-----|----------|
| 1 | Two Sum (hash map version) | 1/5 | [1](https://leetcode.com/problems/two-sum/) |
| 2 | Maximum Subarray (Kadane's) | 1 | [53](https://leetcode.com/problems/maximum-subarray/) |
| 3 | 3Sum (two pointers) | 2 | [15](https://leetcode.com/problems/3sum/) |
| 4 | Longest Substring Without Repeating Characters | 3 | [3](https://leetcode.com/problems/longest-substring-without-repeating-characters/) |
| 5 | Search in Rotated Sorted Array | 4 | [33](https://leetcode.com/problems/search-in-rotated-sorted-array/) |
| 6 | Group Anagrams | 5 | [49](https://leetcode.com/problems/group-anagrams/) |
| 7 | Daily Temperatures (monotonic stack) | 6 | [739](https://leetcode.com/problems/daily-temperatures/) |

---

## Review Strategy

### For each problem:
1. **Name the pattern** out loud before coding (e.g., "This is two pointers on a sorted array").
2. **State the time/space complexity** before writing code.
3. **Write the solution** in one pass without running it.
4. **Dry-run** on a small example by hand.
5. Only then run/test. If it fails, debug - don't start over.

### Self-Assessment Rubric
Rate yourself on each problem:

| Score | Meaning |
|-------|---------|
| 3 | Solved cleanly, no hints, < 10 min |
| 2 | Solved with minor bugs, < 20 min |
| 1 | Needed to peek at notes/solution |
| 0 | Couldn't solve |

**Anything scored 0-1 goes on your "weak list"** - re-solve it again on Day 14 and Day 30.

---

## Pattern Recognition Drill

For each scenario below, name the pattern and sketch the approach (don't code):

1. "Find two numbers in a sorted array that sum to target" → ?
2. "Longest substring with at most k distinct characters" → ?
3. "Find the first bad version in 1..n" → ?
4. "Check if a string of brackets is valid" → ?
5. "Group strings that are anagrams" → ?
6. "Next warmer day for each day" → ?
7. "Find minimum in rotated sorted array" → ?

<details>
<summary>Answers</summary>

1. Two pointers (opposite direction)
2. Sliding window (variable) with a freq map
3. Binary search
4. Stack
5. Hash map with sorted-char key
6. Monotonic decreasing stack
7. Binary search (modified)
</details>

---

## Week 1 Cheat Sheet (build your own)

Create a personal cheat sheet file with these entries. Fill in the template:

```
PATTERN: Two Pointers (opposite)
WHEN: sorted array, find pair
TEMPLATE: l=0, r=n-1; while l<r: adjust based on sum
TIME: O(n)  SPACE: O(1)
GO: sort.Ints first if unsorted
```

Do this for: Two Pointers, Sliding Window (fixed & variable), Binary Search (both templates), Hash Map complement, Monotonic Stack.

---

## Bonus (if time allows)
- [1480. Running Sum of 1D Array](https://leetcode.com/problems/running-sum-of-1d-array/) (Easy) - prefix sum warmup
- [724. Find Pivot Index](https://leetcode.com/problems/find-pivot-index/) (Easy) - prefix sums

> Prefix sums are a small but useful tool - we'll use them more in upcoming days.

---

## Checklist Before Moving On
- [ ] Re-solved all 7 review problems
- [ ] Identified and noted weak patterns
- [ ] Started a personal cheat sheet
- [ ] Pattern recognition drill answered correctly
