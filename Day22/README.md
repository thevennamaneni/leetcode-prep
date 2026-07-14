# Day 22 - Greedy Algorithms & Intervals

## Topic
**Greedy algorithms** - make the locally optimal choice at each step - and **interval problems** (merging, inserting, meeting rooms). Intervals are often solved with greedy + sorting.

---

## Why It Matters
- Interval problems are a distinct, common category with recognizable patterns.
- Greedy thinking is tested implicitly in many problems (jump game, gas station).
- Intervals are easy once you know the sort-by-start/end trick; they're free points if you spot them.

---

## Key Concepts

### Greedy: When Does It Work?
Greedy works when the problem has the **greedy-choice property** (a local optimum leads to a global optimum) and **optimal substructure**. Proving greedy correctness is hard; in interviews, you often justify intuitively or by counterexample elimination.

### Interval Patterns

**Sort by start time** when you need chronological processing (meeting rooms, insert interval).
**Sort by end time** when you want to maximize count (non-overlapping intervals, scheduling).

#### Merge Intervals
```go
func merge(intervals [][]int) [][]int {
    sort.Slice(intervals, func(i, j int) bool {
        return intervals[i][0] < intervals[j][0]
    })
    merged := [][]int{intervals[0]}
    for i := 1; i < len(intervals); i++ {
        last := merged[len(merged)-1]
        if intervals[i][0] <= last[1] {
            last[1] = max(last[1], intervals[i][1])  // overlap -> extend
        } else {
            merged = append(merged, intervals[i])
        }
    }
    return merged
}
```

#### Non-overlapping Intervals (min to remove)
Sort by **end time**; greedily keep the interval that ends earliest. Count removals.

#### Meeting Rooms
Sort by start; if any `intervals[i][0] < intervals[i-1][1]`, there's an overlap (return false for "can attend all").

#### Meeting Rooms II (min rooms)
Two approaches:
1. **Min-heap of end times:** sort by start; for each meeting, pop all ended ones, push current's end; heap size = rooms needed.
2. **Two sorted arrays (starts, ends):** two-pointer sweep.

### Jump Game
Greedy: track the farthest reachable index. If at any point `i > farthest`, you can't proceed. Update `farthest = max(farthest, i + nums[i])`.

---

## Problems To Solve

| # | Problem | Difficulty | LeetCode |
|---|---------|-----------|----------|
| 1 | Merge Intervals | Medium | [56](https://leetcode.com/problems/merge-intervals/) |
| 2 | Insert Interval | Medium | [57](https://leetcode.com/problems/insert-interval/) |
| 3 | Non-overlapping Intervals | Medium | [435](https://leetcode.com/problems/non-overlapping-intervals/) |
| 4 | Meeting Rooms II | Medium | [253](https://leetcode.com/problems/meeting-rooms-ii/) (premium) |
| 5 | Jump Game | Medium | [55](https://leetcode.com/problems/jump-game/) |
| 6 | Gas Station | Medium | [134](https://leetcode.com/problems/gas-station/) |

> **#4 (Meeting Rooms II)** is premium; free alternatives: [1353. Maximum Number of Events That Can Be Attended](https://leetcode.com/problems/maximum-number-of-events-that-can-be-attended/) or implement the two-sorted-arrays sweep on a test case by hand.
>
> **#6 (Gas Station):** If total gas >= total cost, a solution exists. Find the starting index: reset start whenever the running tank goes negative.

---

## Go Tips

- Sort slices with a custom comparator: `sort.Slice(s, func(i, j int) bool { ... })`.
- For 2D intervals `[][]int`, sort by `intervals[i][0]`.
- Use `max` (built-in Go 1.21+) for extending interval ends.

---

## Complexity
- Merge/insert/non-overlapping: **O(n log n)** (sort dominates).
- Meeting rooms II (heap): **O(n log n)**.
- Jump game: **O(n)**.
- Gas station: **O(n)**.

---

## Common Mistakes
- Sorting by the wrong field (start vs end) for the specific sub-pattern.
- In merge: comparing `intervals[i][0] < last[1]` with `<=` vs `<` (overlapping vs touching - read the problem's definition).
- In jump game: using DP/BFS when greedy O(n) suffices (mention both, implement greedy).
- In gas station: not checking the total feasibility first (if total gas < total cost, return -1 immediately).

## Checklist Before Moving On
- [ ] Solved Merge Intervals and Non-overlapping from memory
- [ ] Know when to sort by start vs end
- [ ] Solved Jump Game with the greedy O(n) approach
- [ ] Understand the gas station feasibility check
