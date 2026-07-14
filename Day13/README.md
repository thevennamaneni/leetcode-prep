# Day 13 - Recursion & Backtracking Basics

## Topic
**Recursion** fundamentals and **Backtracking** - the pattern for generating combinations, permutations, and solving constraint-satisfaction problems (subsets, N-Queens, Sudoku).

---

## Why It Matters
- Backtracking is a high-yield category - 2-3 problems per FAANG onsite.
- It builds directly on the recursive tree thinking from Days 10-11.
- Many candidates struggle with the "choose / explore / un-choose" framework - mastering it gives you an edge.

---

## Key Concepts

### Recursion Mental Model
Every recursive function answers: "Given the current state, what are my options, and what's the base case?"

```
function(state):
    if base case: record/return result
    for each choice from state:
        make the choice (mutate state)
        recurse(updated state)
        undo the choice (restore state)   ← THE KEY STEP
```

### The Backtracking Template (MEMORIZE)
```go
func backtrack(state, choices) {
    if /* base case - decision complete */ {
        // record a copy of the current state
        return
    }
    for _, choice := range choices {
        // 1. CHOOSE
        path = append(path, choice)
        // 2. EXPLORE
        backtrack(updated state, remaining choices)
        // 3. UN-CHOOSE (backtrack)
        path = path[:len(path)-1]
    }
}
```

> **Critical:** When recording a result, **copy** the current path - the path slice keeps mutating. `append([]int{}, path...)` makes a copy.

### Subsets (Power Set)
For each element, choose to include it or not. Or iterate: for each starting index, recurse with index+1.

```go
func subsets(nums []int) [][]int {
    result := [][]int{}
    var backtrack func(start int, path []int)
    backtrack = func(start int, path []int) {
        cp := make([]int, len(path))
        copy(cp, path)
        result = append(result, cp)   // every node is a valid subset
        for i := start; i < len(nums); i++ {
            path = append(path, nums[i])
            backtrack(i+1, path)
            path = path[:len(path)-1]
        }
    }
    backtrack(0, []int{})
    return result
}
```

### Permutations
Use a "used" set or swap-in-place. The swap approach is O(1) space.

### Combinations
Like subsets but only paths of a fixed length are recorded.

### Handling Duplicates
**Sort first**, then skip `nums[i] == nums[i-1]` when `i > start`. This prevents duplicate subsets/permutations without a set.

```go
for i := start; i < len(nums); i++ {
    if i > start && nums[i] == nums[i-1] { continue }  // skip dup
    ...
}
```

---

## Problems To Solve

| # | Problem | Difficulty | LeetCode |
|---|---------|-----------|----------|
| 1 | Subsets | Medium | [78](https://leetcode.com/problems/subsets/) |
| 2 | Subsets II (with duplicates) | Medium | [90](https://leetcode.com/problems/subsets-ii/) |
| 3 | Permutations | Medium | [46](https://leetcode.com/problems/permutations/) |
| 4 | Combination Sum | Medium | [39](https://leetcode.com/problems/combination-sum/) |
| 5 | Combination Sum II | Medium | [40](https://leetcode.com/problems/combination-sum-ii/) |

> **#4 (Combination Sum):** Elements are reusable (unlimited use), so recurse with `i` not `i+1`. **#5:** elements used once, sort + skip duplicates.

---

## Go Tips

- Closures for backtracking are very clean: declare `var backtrack func(int, []int)`, define it, call it. The closure captures `result`, `nums`, etc.
- **Copy slices before storing**: `cp := append([]int{}, path...)` or `make` + `copy`.
- Go slices share backing arrays - if you don't copy, your "result" entries all alias the same memory and become identical garbage.

```go
// Combination Sum - elements reusable
func combinationSum(candidates []int, target int) [][]int {
    result := [][]int{}
    var bt func(start, remaining int, path []int)
    bt = func(start, remaining int, path []int) {
        if remaining == 0 {
            cp := make([]int, len(path)); copy(cp, path)
            result = append(result, cp); return
        }
        for i := start; i < len(candidates); i++ {
            if candidates[i] > remaining { continue }
            bt(i, remaining-candidates[i], append(path, candidates[i]))
        }
    }
    bt(0, target, []int{})
    return result
}
```
> Trick: pass `append(path, x)` directly as a new slice - avoids manual un-append since the caller's `path` is unaffected (as long as capacity isn't reused). But for clarity/safety, the explicit append-then-pop pattern is safer. Pick one and be consistent.

---

## Complexity
- Subsets: 2^n subsets → **O(n * 2^n)** time and space.
- Permutations: n! → **O(n * n!)** time.
- These are inherently exponential - that's expected for backtracking.

---

## Common Mistakes
- **Not copying the path** before storing - the #1 bug. All result entries alias the same slice.
- Forgetting to skip duplicates (sort + `nums[i]==nums[i-1]` check).
- Passing `i+1` vs `i` for "use once" vs "reuse" - getting this wrong changes the answer.
- Not having a correct base case (e.g., `remaining < 0` should prune, `== 0` records).

## Checklist Before Moving On
- [ ] Solved subsets and permutations from memory
- [ ] Can explain choose/explore/un-choose in one sentence each
- [ ] Always copy the path before appending to results
- [ ] Know how to handle duplicates (sort + skip)
