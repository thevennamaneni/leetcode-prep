# Day 10 - Binary Trees & BSTs

## Topic
**Binary Trees** - structure, traversals (DFS preorder/inorder/postorder, BFS), and **Binary Search Trees** (BSTs). The tree node is the next essential struct after `ListNode`.

---

## Why It Matters
- Tree problems are a FAANG staple - they test recursion fluency.
- Many graph problems are just tree traversals in disguise.
- BSTs are the basis for understanding `map`, sorted sets, and range queries.

---

## Key Concepts

### The TreeNode (LeetCode definition)
```go
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}
```

### Tree Traversals

| Traversal | Order | Use Case |
|-----------|-------|----------|
| Preorder | Root, Left, Right | Copy tree, top-down processing |
| Inorder | Left, Root, Right | Sorted output of BST |
| Postorder | Left, Right, Root | Bottom-up (height, delete) |
| Level-order (BFS) | Top to bottom, left to right | Closest nodes, serialize |

**Recursive DFS (most common):**
```go
func inorder(root *TreeNode) {
    if root == nil { return }
    inorder(root.Left)
    // visit root
    inorder(root.Right)
}
```

**Iterative with a stack (interviewers may ask for this):**
```go
func inorderIterative(root *TreeNode) {
    stack := []*TreeNode{}
    curr := root
    for curr != nil || len(stack) > 0 {
        for curr != nil {
            stack = append(stack, curr)
            curr = curr.Left
        }
        curr = stack[len(stack)-1]
        stack = stack[:len(stack)-1]
        // visit curr
        curr = curr.Right
    }
}
```

**Level-order (BFS) with a queue:**
```go
func levelOrder(root *TreeNode) [][]int {
    if root == nil { return nil }
    result := [][]int{}
    queue := []*TreeNode{root}
    for len(queue) > 0 {
        levelSize := len(queue)
        level := []int{}
        for i := 0; i < levelSize; i++ {
            node := queue[0]
            queue = queue[1:]
            level = append(level, node.Val)
            if node.Left != nil { queue = append(queue, node.Left) }
            if node.Right != nil { queue = append(queue, node.Right) }
        }
        result = append(result, level)
    }
    return result
}
```

### Binary Search Tree (BST) Properties
- Left subtree < node < right subtree (for all nodes).
- Inorder traversal of a BST yields sorted values.
- Search, insert, delete: O(log n) average, O(n) worst (degenerate).

### Key Subproblems
- **Height/depth:** `max(height(left), height(right)) + 1`.
- **Balanced check:** check balance recursively, return -1 if unbalanced to short-circuit.
- **LCA (Lowest Common Ancestor):** if node is between p and q, it's the LCA. Recurse left/right.

---

## Problems To Solve

| # | Problem | Difficulty | LeetCode |
|---|---------|-----------|----------|
| 1 | Maximum Depth of Binary Tree | Easy | [104](https://leetcode.com/problems/maximum-depth-of-binary-tree/) |
| 2 | Binary Tree Inorder Traversal | Easy | [94](https://leetcode.com/problems/binary-tree-inorder-traversal/) |
| 3 | Binary Tree Level Order Traversal | Medium | [102](https://leetcode.com/problems/binary-tree-level-order-traversal/) |
| 4 | Validate Binary Search Tree | Medium | [98](https://leetcode.com/problems/validate-binary-search-tree/) |
| 5 | Lowest Common Ancestor of BST | Medium | [235](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/) |

> **#4 (Validate BST):** Don't just check `left < node < right` locally - pass down min/max bounds. Use `(int64)` or send `nil` bounds to handle `math.MinInt32` edge cases. The classic bug: a node's value satisfies local constraints but violates a global bound higher up.

---

## Go Tips

- `math.MinInt64` / `math.MaxInt64` for sentinel bounds (use these to avoid the `MinInt32` edge case in BST validation).
- Use `math.MinInt64` only as a marker; better to pass `*int` bounds or `int64`.
- Slices make decent queues/stacks for trees (interview scale is small).

---

## Complexity
- All traversals: **O(n)** time.
- Space: O(h) for recursive DFS (h = height), O(n) worst for BFS queue (widest level).
- Balanced BST search: O(log n); degenerate: O(n).

---

## Common Mistakes
- In validate-BST: only checking immediate children, not the entire subtree's bounds.
- In level-order: not snapshotting `levelSize` before the inner loop (queue grows during iteration).
- Off-by-one in depth (empty tree = 0, single node = 1).
- In LCA: not handling when p or q is the root itself (then root is the LCA).

## Checklist Before Moving On
- [ ] Can write all 4 traversals (3 DFS + BFS) from memory
- [ ] Solved validate-BST with min/max bound passing
- [ ] Understand when to use BFS vs DFS
- [ ] Know the BST search/insert/delete complexity
