# Day 2 - Two Pointers

## Topic
**Two Pointers** - using two indices to traverse an array, usually moving toward each other or in the same direction.

This is THE most common Easy/Medium pattern. Learn to recognize it instantly.

---

## Why It Matters
- Two pointers turns many O(n^2) brute-force solutions into O(n).
- Appears in ~20% of all interview problems.
- It's the gateway to understanding Sliding Window and Fast/Slow pointers.

---

## Key Concepts

### Pattern 1: Opposite-Direction Pointers
Two pointers starting at opposite ends, moving toward the center.
**Use when:** input is sorted and you're searching for a pair.

```go
func twoSumSorted(nums []int, target int) []int {
    left, right := 0, len(nums)-1
    for left < right {
        sum := nums[left] + nums[right]
        if sum == target {
            return []int{left, right}
        } else if sum < target {
            left++
        } else {
            right--
        }
    }
    return nil
}
```

### Pattern 2: Same-Direction Pointers (Fast & Slow)
One pointer moves faster than the other.
**Use when:** in-place removal, deduplication, finding cycles.

```go
// Remove duplicates in-place, return new length
func removeDuplicates(nums []int) int {
    if len(nums) == 0 { return 0 }
    slow := 0
    for fast := 1; fast < len(nums); fast++ {
        if nums[fast] != nums[slow] {
            slow++
            nums[slow] = nums[fast]
        }
    }
    return slow + 1
}
```

### When to Reach for Two Pointers
| Signal in the problem | Pattern |
|----------------------|---------|
| "sorted array" + "find pair" | Opposite direction |
| "in-place" + "remove/dedupe" | Same direction |
| "palindrome" | Opposite direction from ends |
| "reverse" | Opposite direction |
| "container with most water" | Opposite direction with greedy |

---

## Problems To Solve

| # | Problem | Difficulty | LeetCode |
|---|---------|-----------|----------|
| 1 | Two Sum II - Input Sorted | Easy | [167](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/) |
| 2 | Remove Duplicates from Sorted Array | Easy | [26](https://leetcode.com/problems/remove-duplicates-from-sorted-array/) |
| 3 | Move Zeroes | Easy | [283](https://leetcode.com/problems/move-zeroes/) |
| 4 | Valid Palindrome | Easy | [125](https://leetcode.com/problems/valid-palindrome/) |
| 5 | Container With Most Water | Medium | [11](https://leetcode.com/problems/container-with-most-water/) |
| 6 | 3Sum | Medium | [15](https://leetcode.com/problems/3sum/) |

> **3Sum is a classic.** The trick: sort the array, fix one element, then use two pointers for the remaining two. Watch out for duplicate triplets - skip duplicates on all three pointers.

---

## Go Tips

- `strings.ToLower()` for case-insensitive palindrome checks.
- Use `unicode.IsLetter()` and `unicode.IsDigit()` for alphanumeric filtering.
- For 3Sum, sort first: `sort.Ints(nums)`.

```go
// Palindrome with two pointers
func isPalindrome(s string) bool {
    l, r := 0, len(s)-1
    for l < r {
        for l < r && !isAlnum(rune(s[l])) { l++ }
        for l < r && !isAlnum(rune(s[r])) { r-- }
        if toLower(rune(s[l])) != toLower(rune(s[r])) {
            return false
        }
        l++; r--
    }
    return true
}
```

---

## Complexity
- **Time:** O(n) - each element visited at most once.
- **Space:** O(1) - in-place, no extra structures.

---

## Common Mistakes
- Forgetting to sort before opposite-direction pointers (Two Sum II is pre-sorted; 3Sum you must sort).
- Not skipping duplicates in 3Sum -> produces duplicate triplets.
- Off-by-one: `left < right` not `left <= right` for pair-finding.
- Overwriting values when you shouldn't in "move zeroes".

## Checklist Before Moving On
- [ ] Solved 3Sum with the two-pointer approach (not brute force)
- [ ] Can explain why two pointers works only on sorted arrays for pair-finding
- [ ] Know the difference between the two sub-patterns and when to use each
