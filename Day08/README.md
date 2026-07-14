# Day 8 - Linked Lists Basics

## Topic
**Linked Lists** - singly and doubly. LeetCode uses `ListNode` structs. Master pointer manipulation, the dummy-head trick, and cycle detection.

---

## Why It Matters
- Linked list problems test your ability to reason about pointers carefully.
- They're common in FAANG interviews (especially reversing and merging).
- Many candidates freeze here because pointer bugs are subtle - practice makes them routine.

---

## Key Concepts

### The ListNode (LeetCode definition)
```go
type ListNode struct {
    Val  int
    Next *ListNode
}
```

### Traversal
```go
curr := head
for curr != nil {
    // process curr
    curr = curr.Next
}
```

### The Dummy Head Technique (MEMORIZE)
When the operation might change the head (insertion, deletion, merging), use a **dummy node** that points to the real head. This eliminates special-casing the head.

```go
dummy := &ListNode{Next: head}
prev := dummy
// ... operate using prev, prev.Next, etc.
return dummy.Next   // new head
```

### Deleting a Node
```go
prev.Next = prev.Next.Next   // unlink the node after prev
```
You need a reference to the **previous** node, not the node to delete.

### Reversing a Linked List (THE fundamental)
```go
func reverseList(head *ListNode) *ListNode {
    var prev *ListNode  // nil
    curr := head
    for curr != nil {
        next := curr.Next   // save
        curr.Next = prev    // reverse link
        prev = curr         // advance prev
        curr = next         // advance curr
    }
    return prev
}
```

### Fast & Slow Pointers (Floyd's)
- **Find middle:** slow moves 1, fast moves 2. When fast hits end, slow is at middle.
- **Detect cycle:** if slow and fast meet, there's a cycle.
- **Find cycle start:** after meeting, reset one pointer to head; move both by 1 until they meet again.

---

## Problems To Solve

| # | Problem | Difficulty | LeetCode |
|---|---------|-----------|----------|
| 1 | Reverse Linked List | Easy | [206](https://leetcode.com/problems/reverse-linked-list/) |
| 2 | Merge Two Sorted Lists | Easy | [21](https://leetcode.com/problems/merge-two-sorted-lists/) |
| 3 | Linked List Cycle | Easy | [141](https://leetcode.com/problems/linked-list-cycle/) |
| 4 | Remove Nth Node From End | Medium | [19](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) |
| 5 | Middle of the Linked List | Easy | [876](https://leetcode.com/problems/middle-of-the-linked-list/) |

> **#4 (Remove Nth from End):** Use two pointers. Advance the first by `n`, then move both until first hits the end. The second is now at the node before the one to delete. Use a dummy head to handle deleting the head itself.

---

## Go Tips

- Go has no null; use `nil` for pointers.
- `var prev *ListNode` initializes to `nil`.
- Be careful: Go's pointers are real pointers. `curr.Next = prev` mutates the actual node.
- For cycle detection, LeetCode sometimes hides cycles via the test harness - Floyd's algorithm handles it.

```go
// Merge two sorted lists with dummy
func mergeTwoLists(l1, l2 *ListNode) *ListNode {
    dummy := &ListNode{}
    tail := dummy
    for l1 != nil && l2 != nil {
        if l1.Val <= l2.Val {
            tail.Next = l1; l1 = l1.Next
        } else {
            tail.Next = l2; l2 = l2.Next
        }
        tail = tail.Next
    }
    if l1 != nil { tail.Next = l1 }
    if l2 != nil { tail.Next = l2 }
    return dummy.Next
}
```

---

## Complexity
- Traversal/reverse/merge: **O(n)** time, **O(1)** space.
- Floyd's cycle detection: **O(n)** time, **O(1)** space.

---

## Common Mistakes
- Losing the head reference - always use a dummy node when modifying structure.
- Forgetting to save `curr.Next` before reassigning it in reverse (infinite loop / lost nodes).
- Off-by-one in "remove nth from end": advancing the first pointer by `n` then moving both.
- Not handling the empty list (`head == nil`).
- Memory leak: not nulling out the deleted node's `Next` (less important in Go due to GC, but good practice).

## Checklist Before Moving On
- [ ] Can reverse a linked list from memory in under 5 min
- [ ] Understand the dummy-head pattern and when to use it
- [ ] Solved cycle detection with Floyd's algorithm
- [ ] Know the two-pointer "n-ahead" trick for nth-from-end
