# Day 1 - Arrays, Big-O & Complexity Analysis

## Topic
**Arrays** - the most fundamental data structure - and **Big-O notation**, the language interviewers use to evaluate your code.

Every FAANG interview starts with array questions or reduces to array manipulation. Master this first.

---

## Why It Matters
- ~40% of interview problems involve arrays in some form.
- Arrays are the foundation for Two Pointers, Sliding Window, and Hash Maps.
- Interviewers expect you to **state complexity immediately** after writing code.

---

## Key Concepts

### Array Basics (in Go)
Go has no generic "ArrayList" - you use **slices** (`[]int`). A slice is a reference to an underlying array with length + capacity.

```go
// Declaration
nums := []int{1, 2, 3}
var nums []int              // nil slice
nums = make([]int, 5)       // zero-valued len 5
nums = make([]int, 0, 10)   // len 0, cap 10

// Access: O(1)
x := nums[0]

// Append: amortized O(1), worst case O(n) on resize
nums = append(nums, 4)

// Insert at index i: O(n)
nums = append(nums[:i], append([]int{val}, nums[i:]...)...)

// Delete at index i: O(n)
nums = append(nums[:i], nums[i+1:]...)

// Length & Capacity
len(nums)   // current element count
cap(nums)   // underlying array capacity
```

### Big-O Cheat Sheet (MEMORIZE)
| Operation | Time |
|-----------|------|
| Access by index | O(1) |
| Search (unsorted) | O(n) |
| Search (sorted, binary) | O(log n) |
| Insert/Delete at end | O(1) amortized |
| Insert/Delete at front/middle | O(n) |

### Common Complexities
- **O(1)** - constant
- **O(log n)** - logarithmic (binary search, halving)
- **O(n)** - linear (single pass)
- **O(n log n)** - linearithmic (optimal sorting)
- **O(n^2)** - quadratic (nested loops over same array)
- **O(2^n)** - exponential (naive recursion, subsets)

### Space Complexity
- Count **extra** data structures, not input/output.
- Recursive call stack counts as space.
- In-place = O(1) auxiliary space.

---

## Interview Strategy

1. **Clarify the input**: sorted? duplicates allowed? empty array? negative numbers?
2. **Brute force first**: state the obvious O(n^2) solution out loud, then optimize.
3. **State complexity before coding** - shows you think ahead.
4. **Edge cases**: empty array, single element, all duplicates, already sorted.

---

## Problems To Solve

| # | Problem | Difficulty | LeetCode |
|---|---------|-----------|----------|
| 1 | Contains Duplicate | Easy | [217](https://leetcode.com/problems/contains-duplicate/) |
| 2 | Maximum Subarray (Kadane's) | Easy | [53](https://leetcode.com/problems/maximum-subarray/) |
| 3 | Two Sum | Easy | [1](https://leetcode.com/problems/two-sum/) |
| 4 | Best Time to Buy and Sell Stock | Easy | [121](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) |

---

## Go Tips

- Use `map[int]bool` for membership checks (set replacement).
- `sort.Ints(nums)` sorts in place - O(n log n).
- `math.MaxInt32` / `math.MinInt32` for sentinel values.
- Initialize large result vars with `math.MinInt` when finding max.

```go
// Set pattern in Go
seen := map[int]bool{}
for _, n := range nums {
    if seen[n] { return true }
    seen[n] = true
}
```

---

## Common Mistakes
- Forgetting that `append` may allocate a new backing array.
- Off-by-one errors in loops - always check `<` vs `<=`.
- Not considering the empty-input case.
- Using `math.Max` which needs float64 conversion for ints (Go < 1.21). In Go 1.21+, `max()` is built-in.

---

## Complexity To Memorize
- Hash map lookup/insert: **O(1)** avg, O(n) worst.
- Sort: **O(n log n)** time, O(n) space (or O(1) in-place).
- Kadane's algorithm: **O(n)** time, O(1) space.

## Checklist Before Moving On
- [ ] Can state Big-O of any loop nest on sight
- [ ] Solved all 4 problems without looking at solutions
- [ ] Can explain Kadane's algorithm in one sentence
- [ ] Know the difference between slice length and capacity
