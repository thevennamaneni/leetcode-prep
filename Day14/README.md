# Day 14 - Review & Reinforce (Week 2 Consolidation)

## Topic
**Spaced repetition review.** Re-solve the hardest Week 2 problems from memory. Trees, lists, and backtracking are the most testable Week 2 themes.

---

## Why This Day Matters
- Week 2 introduced recursion-heavy patterns that are easy to forget.
- The dummy-head trick, closure-accumulator, and copy-the-path pattern all need repetition.
- You're at the halfway point - solidify before graphs & DP (the hardest material).

---

## Review Problems (Re-solve from memory)

| # | Problem | Day | LeetCode |
|---|---------|-----|----------|
| 1 | Reverse Linked List | 8 | [206](https://leetcode.com/problems/reverse-linked-list/) |
| 2 | Copy List with Random Pointer | 9 | [138](https://leetcode.com/problems/copy-list-with-random-pointer/) |
| 3 | Validate Binary Search Tree | 10 | [98](https://leetcode.com/problems/validate-binary-search-tree/) |
| 4 | Binary Tree Level Order Traversal | 10 | [102](https://leetcode.com/problems/binary-tree-level-order-traversal/) |
| 5 | Binary Tree Maximum Path Sum | 11 | [124](https://leetcode.com/problems/binary-tree-maximum-path-sum/) |
| 6 | Serialize and Deserialize Binary Tree | 11 | [297](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/) |
| 7 | Kth Largest Element in an Array | 12 | [215](https://leetcode.com/problems/kth-largest-element-in-an-array/) |
| 8 | Subsets | 13 | [78](https://leetcode.com/problems/subsets/) |
| 9 | Combination Sum | 13 | [39](https://leetcode.com/problems/combination-sum/) |
| 10 | Reverse Nodes in k-Group | 9 | [25](https://leetcode.com/problems/reverse-nodes-in-k-group/) |

> Time-box: spend max 20 min per problem. If stuck, mark it and move on; revisit at day's end.

---

## Self-Assessment Rubric (same as Day 7)

| Score | Meaning |
|-------|---------|
| 3 | Solved cleanly, no hints, < 10 min |
| 2 | Solved with minor bugs, < 20 min |
| 1 | Needed to peek at notes/solution |
| 0 | Couldn't solve |

**Add all 0-1 scores to your weak list** for Day 30 review.

---

## Pattern Recognition Drill

Name the pattern + sketch the approach (no coding):

1. "Generate all subsets of a set with duplicates" → ?
2. "Find the kth smallest in a BST" → ?
3. "Lowest common ancestor of two nodes in a binary tree (not BST)" → ?
4. "Merge k sorted linked lists" → ?
5. "Serialize a tree to a string and back" → ?
6. "Find median of a stream of numbers" → ?
7. "Reverse nodes in groups of k" → ?

<details>
<summary>Answers</summary>

1. Backtracking + sort + skip duplicates
2. Inorder traversal (BST inorder = sorted), return kth; or iterative stack
3. Recursive LCA: if node == p or q return it; recurse both sides; if both non-nil, node is LCA
4. Min-heap of list heads; pop smallest, push its next
5. Preorder with `#` nil markers + index-based deserialization
6. Two heaps (max for lower half, min for upper half), rebalance
7. Sublist reverse in a loop, k at a time
</details>

---

## Weak-List Audit

Review your Day 7 weak list. Re-attempt those problems. If still weak after two attempts, schedule them for Day 30 with extra time.

---

## Heap Boilerplate Check
From memory, write out a complete Go heap implementation for `[]int` (min-heap): all 5 methods. This is rote memorization that pays off in interviews - you don't want to fumble `Push`/`Pop` signatures under pressure.

---

## Bonus (if time allows)
- [701. Insert into a BST](https://leetcode.com/problems/insert-into-a-binary-search-tree/) (Medium) - BST insert/delete
- [230. Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/) (Medium) - inorder

---

## Checklist Before Moving On
- [ ] Re-solved all 10 review problems
- [ ] Updated weak list with new entries
- [ ] Wrote heap boilerplate from memory
- [ ] Pattern drill answered correctly
- [ ] Ready for graphs & DP (the heaviest week)
