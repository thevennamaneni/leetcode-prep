# Day 4 - Binary Search

## Topic
**Binary Search** - the O(log n) search algorithm for sorted (or "monotonic") data. Also the basis for "binary search on the answer."

---

## Why It Matters
- Binary search shows up disguised in many problems - not just "find X in array."
- Interviewers love it because it tests edge-case handling.
- The "binary search on answer" pattern unlocks a whole category of Medium/Hard problems.

---

## Key Concepts

### Standard Binary Search
**Precondition:** the data is sorted (or monotonic - strictly increasing/decreasing in some property).

```go
func search(nums []int, target int) int {
    left, right := 0, len(nums)-1
    for left <= right {
        mid := left + (right-left)/2   // avoids overflow
        if nums[mid] == target {
            return mid
        } else if nums[mid] < target {
            left = mid + 1
        } else {
            right = mid - 1
        }
    }
    return -1
}
```

### The Two Templates (CRITICAL)
There are two common loop conditions. Pick one and stick with it.

**Template 1: `left <= right`** - finds exact match, returns inside loop.

**Template 2: `left < right`** - narrows to one element, returns after loop.
Use this when searching for a boundary/insertion point.

```go
// Find insertion point (leftmost position)
left, right := 0, len(nums)
for left < right {
    mid := left + (right-left)/2
    if nums[mid] < target {
        left = mid + 1
    } else {
        right = mid
    }
}
return left   // insertion index
```

### Key Insight: `mid = left + (right-left)/2`
- Prevents integer overflow that `(left+right)/2` can cause in other languages.
- In Go, ints are 64-bit on most platforms so overflow is unlikely, but it's a habit interviewers look for.

### Binary Search on the Answer
When the problem says "find the minimum/maximum X such that a condition holds," binary search over the **range of possible answers**.

**Signal:** "minimize the maximum" or "maximize the minimum."

```go
// Template: binary search on answer
lo, hi := minPossible, maxPossible
for lo < hi {
    mid := lo + (hi-lo)/2
    if feasible(mid) {   // can we achieve 'mid'?
        hi = mid          // searching for minimum
    } else {
        lo = mid + 1
    }
}
return lo
```

### Rotated Sorted Array
A sorted array rotated at a pivot still has a "monotonic half" - use binary search but check which half is sorted.

---

## Problems To Solve

| # | Problem | Difficulty | LeetCode |
|---|---------|-----------|----------|
| 1 | Binary Search | Easy | [704](https://leetcode.com/problems/binary-search/) |
| 2 | Search Insert Position | Easy | [35](https://leetcode.com/problems/search-insert-position/) |
| 3 | Find First and Last Position of Element | Medium | [34](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/) |
| 4 | Search in Rotated Sorted Array | Medium | [33](https://leetcode.com/problems/search-in-rotated-sorted-array/) |
| 5 | Koko Eating Bananas | Medium | [875](https://leetcode.com/problems/koko-eating-bananas/) |

> **#5 (Koko) is your intro to "binary search on the answer."** The speed ranges from 1 to max(piles). Binary search that range, checking feasibility with a helper function. This pattern reappears in many Mediums.

---

## Go Tips

- `sort.SearchInts(nums, target)` does binary search under the hood - but in interviews, implement it yourself.
- `sort.Search(n, func(i int) bool { ... })` is Go's generic binary search - useful to know.
- Always use `left + (right-left)/2` for mid.

---

## Complexity
- **Time:** O(log n)
- **Space:** O(1) iterative, O(log n) recursive.

---

## Common Mistakes
- Infinite loop: happens when `left = mid` instead of `left = mid + 1` (or `right = mid` instead of `right = mid - 1` in template 1).
- Not handling empty array.
- In rotated search: not checking `nums[mid] >= nums[left]` correctly to determine sorted half.
- For "find first/last": using a single binary search instead of two (one biased left, one biased right).

## Checklist Before Moving On
- [ ] Solved #4 (rotated search) - know the "which half is sorted" check cold
- [ ] Solved #5 (Koko) - understand binary search on answer space
- [ ] Can write binary search without an infinite loop bug
- [ ] Know the difference between the two templates
