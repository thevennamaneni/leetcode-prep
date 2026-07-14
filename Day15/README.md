# Day 15 - Backtracking Advanced

## Topic
**Advanced backtracking** - the harder constraint-satisfaction problems: **word search, N-Queens, Sudoku solver, and palindrome partitioning.** These are where Week 2's basics scale up.

---

## Why It Matters
- Word Search and Sudoku are asked regularly at Amazon, Google, Meta.
- These problems force you to manage multiple constraints simultaneously - a key interview skill.
- N-Queens is the canonical "impressive to solve" problem.

---

## Key Concepts

### The Universal Template (recap, now with constraints)
```go
func backtrack(state) {
    if isValid(state) && isComplete(state) {
        record(state); return
    }
    for choice := range choices(state) {
        if isValid(choice) {
            apply(choice)
            backtrack(newState)
            undo(choice)
        }
    }
}
```

### Word Search (Grid Backtracking)
For each cell matching the first letter, DFS in 4 directions, marking visited cells (or using a visited set). **Temporarily mutate the board** (set to a sentinel) to mark visited, then restore - this avoids allocating a visited matrix each call.

```go
func exist(board [][]byte, word string) bool {
    rows, cols := len(board), len(board[0])
    var dfs func(r, c, i int) bool
    dfs = func(r, c, i int) bool {
        if i == len(word) { return true }
        if r < 0 || r >= rows || c < 0 || c >= cols || board[r][c] != word[i] {
            return false
        }
        tmp := board[r][c]
        board[r][c] = '#'   // mark visited
        found := dfs(r+1,c,i+1) || dfs(r-1,c,i+1) ||
                 dfs(r,c+1,i+1) || dfs(r,c-1,i+1)
        board[r][c] = tmp   // restore
        return found
    }
    for r := 0; r < rows; r++ {
        for c := 0; c < cols; c++ {
            if dfs(r, c, 0) { return true }
        }
    }
    return false
}
```

### Palindrome Partitioning
Backtrack + a helper `isPalindrome(s, l, r)`. At each position, try every prefix that's a palindrome, then recurse on the suffix.

### N-Queens
Place queens row by row. For each row, try each column; check column/diagonal conflicts using sets. Record a solution when all N rows are filled.

### Sudoku Solver
For each empty cell, try digits 1-9; check row/column/3x3 box validity; recurse; if it fails, restore and try the next.

---

## Problems To Solve

| # | Problem | Difficulty | LeetCode |
|---|---------|-----------|----------|
| 1 | Word Search | Medium | [79](https://leetcode.com/problems/word-search/) |
| 2 | Palindrome Partitioning | Medium | [131](https://leetcode.com/problems/palindrome-partitioning/) |
| 3 | Letter Combinations of a Phone Number | Medium | [17](https://leetcode.com/problems/letter-combinations-of-a-phone-number/) |
| 4 | N-Queens | Hard | [51](https://leetcode.com/problems/n-queens/) |
| 5 | Sudoku Solver | Hard | [37](https://leetcode.com/problems/sudoku-solver/) |

> **#3 (Phone):** Pure combinatorial backtracking - map digit to letters, build strings. Easy Hard-leaning Medium; good warmup.
>
> **#4 (N-Queens):** Use 3 sets: `cols`, `diag1` (r+c), `diag2` (r-c). Convert the placement to a string board for output.

---

## Go Tips

- **Visited marking via mutation:** set `board[r][c] = '#'` then restore. Saves allocation. Very idiomatic for grid problems.
- `strings.Builder` for building result strings efficiently.
- Use `map[int]bool` for column/diagonal sets in N-Queens.
- Convert `int` to `byte` digit: `byte('0' + d)`.

```go
// Diagonal key helpers for N-Queens
diag1 := map[int]bool{}  // r + c
diag2 := map[int]bool{}  // r - c
cols  := map[int]bool{}
```

---

## Complexity
- Word Search: **O(N * M * 4^L)** where L = word length (4 directions, depth L).
- Palindrome partitioning: **O(n * 2^n)** in the worst case.
- N-Queens: **O(n!)**.
- Sudoku: constant bounded (~9^empty cells, but pruning makes it fast).

---

## Common Mistakes
- **Not restoring the board** after recursion in word search - corrupts other paths.
- In N-Queens: only checking the current row, not diagonals.
- In palindrome partitioning: re-checking palindromes from scratch each time (use precomputed table for optimization, though naive is fine for n <= 16).
- Returning too eagerly - in word search you need `found := a || b || c || d` (any direction works), not returning false on the first dead-end branch.

## Checklist Before Moving On
- [ ] Solved Word Search with in-place visited marking
- [ ] Solved N-Queens using column + diagonal sets
- [ ] Understand the universal backtracking template cold
- [ ] Comfortable restoring state after recursion
