# Day 5 - Hash Maps & Sets

## Topic
**Hash Maps** (Go: `map[K]V`) and **Sets** (simulated with `map[K]bool` or `map[K]struct{}`). The Swiss Army knife of O(1) lookups.

---

## Why It Matters
- Hash maps reduce O(n) searches to O(1).
- They're the key ingredient in many patterns: counting, caching, grouping, deduplication.
- Go's `map` is built-in and hash-table-backed - no import needed.

---

## Key Concepts

### Hash Map Operations in Go
```go
// Declaration
m := map[string]int{}
m := make(map[string]int)

// Insert / Update: O(1) avg
m["key"] = 42

// Lookup: O(1) avg
val, ok := m["key"]   // ok = false if absent

// Delete: O(1) avg
delete(m, "key")

// Iterate (unordered!)
for k, v := range m {
    fmt.Println(k, v)
}

// Length
n := len(m)
```

### Set Pattern (Go has no built-in set)
```go
set := map[int]bool{}
set[1] = true
if set[1] { /* present */ }

// Alternative: struct{}{} saves a tiny bit of memory
set := map[int]struct{}{}
set[1] = struct{}{}
_, exists := set[1]
```

### Common Use Cases

| Use Case | Pattern |
|----------|---------|
| "find two numbers that sum to target" | Store seen values, check complement |
| "count frequencies" | Increment map values |
| "group items by key" | `map[K][]V` |
| "check for duplicates" | Set |
| "cache/memoize" | `map[input]result` |
| "first non-repeating char" | Freq map, then second pass |

### The Complement Pattern (Two Sum)
```go
func twoSum(nums []int, target int) []int {
    seen := map[int]int{}   // value -> index
    for i, n := range nums {
        complement := target - n
        if j, ok := seen[complement]; ok {
            return []int{j, i}
        }
        seen[n] = i
    }
    return nil
}
```

### Frequency Counting
```go
freq := map[int]int{}
for _, n := range nums {
    freq[n]++
}
```

---

## Problems To Solve

| # | Problem | Difficulty | LeetCode |
|---|---------|-----------|----------|
| 1 | Two Sum | Easy | [1](https://leetcode.com/problems/two-sum/) |
| 2 | Valid Anagram | Easy | [242](https://leetcode.com/problems/valid-anagram/) |
| 3 | Group Anagrams | Medium | [49](https://leetcode.com/problems/group-anagrams/) |
| 4 | Top K Frequent Elements | Medium | [347](https://leetcode.com/problems/top-k-frequent-elements/) |
| 5 | Longest Consecutive Sequence | Medium | [128](https://leetcode.com/problems/longest-consecutive-sequence/) |

> **#3 (Group Anagrams):** Sort each string's characters to form a key, group by that key. `sort.Strings(strings.Split(...))` is slow; instead, count chars into a `[26]int` and use a string key.
>
> **#5 (Longest Consecutive):** The O(n) trick: put all numbers in a set, then for each number that's the **start** of a sequence (num-1 not in set), count up. This avoids sorting.

---

## Go Tips

- Go maps are **not** concurrent-safe. (Rarely matters in LeetCode, but good to know.)
- Maps don't preserve insertion order. If you need order, keep a separate slice of keys.
- `sort` the keys if you need deterministic iteration: collect keys into a `[]string`, then `sort.Strings(keys)`.
- For anagram keys, build a string from char counts:

```go
func key(s string) string {
    counts := [26]int{}
    for _, c := range s {
        counts[c-'a']++
    }
    b := make([]byte, 26)
    for i := range counts {
        b[i] = byte(counts[i])
    }
    return string(b)
}
```

---

## Complexity
- **Time:** O(1) avg for insert/lookup/delete; O(n) worst case (hash collisions, rare).
- **Space:** O(n) - stores all entries.

---

## Common Mistakes
- Assuming map iteration order is stable (it's randomized in Go).
- Using a map value that's a nil pointer - dereferencing panics.
- For `map[int][]int`, forgetting to **initialize the slice** before appending: `m[k] = append(m[k], v)` works (append handles nil), but `m[k][0] = v` panics if nil.
- For #5, doing the naive O(n log n) sort instead of the O(n) set approach.

## Checklist Before Moving On
- [ ] Solved all 5 problems
- [ ] Comfortable with the complement/lookup pattern
- [ ] Know how to simulate a set in Go
- [ ] Can build an anagram-grouping key efficiently
