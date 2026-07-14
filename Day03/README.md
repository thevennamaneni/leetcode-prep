# Day 3 - Sliding Window

## Topic
**Sliding Window** - a specialized two-pointer technique for problems involving contiguous subarrays or substrings.

---

## Why It Matters
- Sliding window is asked in nearly every FAANG loop.
- Converts O(n^2) / O(n*k) brute force into O(n).
- Tricky to get the window boundaries right - practice is essential.

---

## Key Concepts

### Fixed-Size Window
Window has constant size `k`. Slide it across the array, updating the result.

**Use when:** "find max/min/average of subarrays of size k".

```go
// Max average subarray of size k
func findMaxAverage(nums []int, k int) float64 {
    sum := 0
    for i := 0; i < k; i++ { sum += nums[i] }
    maxSum := sum
    for i := k; i < len(nums); i++ {
        sum += nums[i] - nums[i-k]   // add new, remove old
        if sum > maxSum { maxSum = sum }
    }
    return float64(maxSum) / float64(k)
}
```

### Variable-Size Window
Window grows/shrinks based on a condition. Use two pointers `left` and `right`.

**Use when:** "longest/shortest substring with condition X".

```go
// Longest substring without repeating characters
func lengthOfLongestSubstring(s string) int {
    seen := map[byte]int{}
    left, maxLen := 0, 0
    for right := 0; right < len(s); right++ {
        if idx, ok := seen[s[right]]; ok && idx >= left {
            left = idx + 1   // jump left past the duplicate
        }
        seen[s[right]] = right
        if right-left+1 > maxLen {
            maxLen = right - left + 1
        }
    }
    return maxLen
}
```

### The Sliding Window Template (MEMORIZE)
```go
func slidingWindow(s string) int {
    left := 0
    count := map[byte]int{}   // or a freq counter
    result := 0
    for right := 0; right < len(s); right++ {
        // 1. Expand: add s[right] to window
        count[s[right]]++
        // 2. Shrink: while window is invalid, shrink from left
        for /* window invalid */ {
            count[s[left]]--
            left++
        }
        // 3. Update result (window is valid here)
        result = max(result, right-left+1)
    }
    return result
}
```

### When to Use Sliding Window
| Signal | Type |
|--------|------|
| "subarray/substring of size k" | Fixed |
| "longest/shortest substring" + constraint | Variable |
| "max consecutive ones" + flips | Variable |
| "contains all characters of string t" | Variable (freq map) |

---

## Problems To Solve

| # | Problem | Difficulty | LeetCode |
|---|---------|-----------|----------|
| 1 | Maximum Average Subarray I | Easy | [643](https://leetcode.com/problems/maximum-average-subarray-i/) |
| 2 | Longest Substring Without Repeating Characters | Medium | [3](https://leetcode.com/problems/longest-substring-without-repeating-characters/) |
| 3 | Minimum Size Subarray Sum | Medium | [209](https://leetcode.com/problems/minimum-size-subarray-sum/) |
| 4 | Longest Repeating Character Replacement | Medium | [424](https://leetcode.com/problems/longest-repeating-character-replacement/) |
| 5 | Permutation in String | Medium | [567](https://leetcode.com/problems/permutation-in-string/) |

---

## Go Tips

- Use `map[byte]int` for character frequency (for ASCII). Use `map[rune]int` for Unicode.
- For lowercase-only strings, a `[26]int` array is faster than a map.
- `max` is built-in in Go 1.21+. Otherwise define a helper.

```go
// Freq array for lowercase letters
var freq [26]int
freq[s[i]-'a']++
```

---

## Complexity
- **Time:** O(n) - each element added once, removed once.
- **Space:** O(k) for the freq map where k = alphabet size (often O(1) for ASCII).

---

## Common Mistakes
- Forgetting to check `idx >= left` before jumping `left` (stale map entries).
- Shrinking with `if` instead of `for` - some windows need multiple shrinks.
- Updating the result in the wrong place (should be when window is valid).
- Confusing "longest" vs "shortest" - for shortest, update result inside the shrink loop.

## Checklist Before Moving On
- [ ] Solved #2 and #4 without hints (these are high-frequency)
- [ ] Can write the sliding window template from memory
- [ ] Understand when to use `for` vs `if` for the shrink step
