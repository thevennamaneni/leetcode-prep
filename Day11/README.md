# Day 11 - Tree DFS, BFS & Recursive Thinking

## Topic
Advanced tree problems: **right-side view, zigzag, serialize/deserialize, balanced-tree check, and path problems**. Deepen recursive "return value + side effect" thinking.

---

## Why It Matters
- These combine traversals with logic - closer to real interview questions.
- Serialize/deserialize is a **very common** FAANG question (LinkedIn, Google).
- Path problems introduce the "global vs. local" distinction critical for DP later.

---

## Key Concepts

### The Two Recursive Return Patterns

Trees are recursive. Master two patterns:

**Pattern A - Return a value, parent combines it:**
```go
func maxDepth(root *TreeNode) int {
    if root == nil { return 0 }
    return 1 + max(maxDepth(root.Left), maxDepth(root.Right))
}
```

**Pattern B - Accumulate into a global/closure, recursion explores:**
```go
func diameterOfBinaryTree(root *TreeNode) int {
    diameter := 0
    var depth func(*TreeNode) int
    depth = func(node *TreeNode) int {
        if node == nil { return 0 }
        l := depth(node.Left)
        r := depth(node.Right)
        if l+r > diameter { diameter = l + r }   // side effect
        return 1 + max(l, r)
    }
    depth(root)
    return diameter
}
```
> **Go closures for globals:** declare `var result int`, then define a recursive `func` that captures it. This is idiomatic Go for "recursive function with accumulator."

### Right-Side View (BFS, last node per level)
```go
func rightSideView(root *TreeNode) []int {
    if root == nil { return nil }
    result := []int{}
    queue := []*TreeNode{root}
    for len(queue) > 0 {
        n := len(queue)
        for i := 0; i < n; i++ {
            node := queue[0]; queue = queue[1:]
            if i == n-1 { result = append(result, node.Val) }   // last of level
            if node.Left != nil { queue = append(queue, node.Left) }
            if node.Right != nil { queue = append(queue, node.Right) }
        }
    }
    return result
}
```

### Path Problems
- **Path sum (root-to-leaf):** subtract node.Val, check if leaf and target==0.
- **Maximum path sum (any path):** at each node, best "through node" = node.Val + leftBranch + rightBranch; return only one branch upward (node.Val + max(left, right)).

### Serialize / Deserialize
- **Preorder with markers:** output `Val,L,R`; use a sentinel (e.g., `"#"`) for nil. Deserialize by splitting and using an index pointer.

---

## Problems To Solve

| # | Problem | Difficulty | LeetCode |
|---|---------|-----------|----------|
| 1 | Diameter of Binary Tree | Easy | [543](https://leetcode.com/problems/diameter-of-binary-tree/) |
| 2 | Balanced Binary Tree | Easy | [110](https://leetcode.com/problems/balanced-binary-tree/) |
| 3 | Binary Tree Right Side View | Medium | [199](https://leetcode.com/problems/binary-tree-right-side-view/) |
| 4 | Path Sum | Easy | [112](https://leetcode.com/problems/path-sum/) |
| 5 | Binary Tree Maximum Path Sum | Hard | [124](https://leetcode.com/problems/binary-tree-maximum-path-sum/) |
| 6 | Serialize and Deserialize Binary Tree | Hard | [297](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/) |

> **#6 (Serialize):** The preorder-with-`#`-markers approach is the cleanest. For deserialization, split the string and use a closure with an index pointer that recurses. This is a Top-10 FAANG question - master it.

---

## Go Tips

- The closure-with-accumulator pattern is Go's answer to "pass-by-reference globals" in recursion. Define `var dfs func(*TreeNode) int` then assign `dfs = func(...) { ... }`.
- For serialize, `strconv.Itoa` converts int to string; `strconv.Atoi` the reverse. Use a delimiter like `,`.
- Use `strings.Split(data, ",")` to parse serialized form.

```go
// Serialize (preorder with # markers)
func serialize(root *TreeNode) string {
    var b strings.Builder
    var enc func(*TreeNode)
    enc = func(node *TreeNode) {
        if node == nil {
            b.WriteString("#,"); return
        }
        b.WriteString(strconv.Itoa(node.Val) + ",")
        enc(node.Left); enc(node.Right)
    }
    enc(root)
    return b.String()
}
```

---

## Complexity
- All today's problems: **O(n)** time.
- Space: O(h) recursive DFS, O(n) BFS queue or serialize output.

---

## Common Mistakes
- In diameter: counting edges vs. nodes (off by one). Define clearly.
- In max-path-sum: returning the "through node" sum upward instead of only one branch (creates invalid paths).
- In balanced: recomputing height twice (O(n^2)) instead of returning -1 to signal imbalance (O(n)).
- In serialize: using a mutable slice index without capturing it in the closure.

## Checklist Before Moving On
- [ ] Solved serialize/deserialize independently
- [ ] Comfortable with the closure-accumulator pattern
- [ ] Can distinguish "return value for parent" vs. "side-effect global"
- [ ] Solved max-path-sum (Hard)
