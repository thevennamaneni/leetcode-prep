# Day 9 - Linked Lists Advanced

## Topic
Advanced linked-list operations: **in-place reversal of sublists, palindrome checking, intersection, and copy with random pointers**. These combine yesterday's fundamentals into harder problems.

---

## Why It Matters
- These problems are direct combinations of patterns you now know - they test whether you can compose primitives.
- "Copy list with random pointer" is a classic FAANG question (Microsoft, Google).
- "Reverse nodes in k-group" is a Hard that's very approachable once you can reverse a sublist.

---

## Key Concepts

### Reverse a Sublist [left, right]
Use the dummy-head + three-pointer technique:
1. Walk to the node before `left` (call it `prev`).
2. Reverse `right - left + 1` nodes starting at `prev.Next`.
3. Reconnect the tail.

```go
func reverseBetween(head *ListNode, left, right int) *ListNode {
    dummy := &ListNode{Next: head}
    prev := dummy
    for i := 1; i < left; i++ { prev = prev.Next }
    // prev.Next is the start of the section to reverse
    curr := prev.Next
    for i := 0; i < right-left; i++ {
        next := curr.Next
        curr.Next = next.Next
        next.Next = prev.Next
        prev.Next = next
    }
    return dummy.Next
}
```

### Linked List Palindrome
**Approach:** find middle (fast/slow), reverse second half, compare the two halves.

### Intersection of Two Lists
**Trick:** walk both lists; when one reaches the end, redirect to the other list's head. They'll meet at the intersection (or both nil) after equal total distance.

### Copy List with Random Pointer
**Two-pass approach:**
1. First pass: insert a copy of each node right after it (A -> A' -> B -> B' -> ...).
2. Second pass: set the `random` of each copy = original's random's next.
3. Third pass: detach the copies into a new list.

This is O(n) time, O(1) space (vs. a hash-map approach which is O(n) space).

---

## Problems To Solve

| # | Problem | Difficulty | LeetCode |
|---|---------|-----------|----------|
| 1 | Reverse Linked List II (sublist) | Medium | [92](https://leetcode.com/problems/reverse-linked-list-ii/) |
| 2 | Palindrome Linked List | Easy | [234](https://leetcode.com/problems/palindrome-linked-list/) |
| 3 | Intersection of Two Linked Lists | Easy | [160](https://leetcode.com/problems/intersection-of-two-linked-lists/) |
| 4 | Copy List with Random Pointer | Medium | [138](https://leetcode.com/problems/copy-list-with-random-pointer/) |
| 5 | Reverse Nodes in k-Group | Hard | [25](https://leetcode.com/problems/reverse-nodes-in-k-group/) |

> **#5 (k-Group) is Hard but very doable:** count the list length; in a loop, reverse k nodes at a time using the sublist-reverse technique, decrementing the remaining count. Stop when fewer than k remain.

---

## Go Tips

- For "copy with random pointer," define the struct as LeetCode gives it (with `Random *Node`).
- Use the dummy-head pattern consistently - it simplifies reconnection logic.
- Drawing the pointer arrows on paper for these problems is HIGHLY recommended before coding.

```go
// Random pointer node
type Node struct {
    Val    int
    Next   *Node
    Random *Node
}
```

---

## Complexity
- All today's problems: **O(n)** time.
- Space:
  - Sublist reverse, palindrome, intersection: **O(1)**.
  - Copy with random (hash map approach): **O(n)**. The interleave approach: **O(1)**.

---

## Common Mistakes
- In k-group: reversing when fewer than k nodes remain (must check length first).
- In palindrome: not restoring the reversed half (interviewers may ask you to; practice it).
- In intersection: not handling the no-intersection case (both become nil).
- In copy-random: setting random before all copies exist (use the interleave method to avoid this).

## Checklist Before Moving On
- [ ] Solved reverse-between from memory
- [ ] Solved copy-with-random-pointer with the O(1) space interleave method
- [ ] Attempted k-group (Hard) - even if you peek, re-solve independently
