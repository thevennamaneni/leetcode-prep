# Day 12 - Heaps & Priority Queues

## Topic
**Heaps** (binary heap) and **Priority Queues**. Go provides `container/heap`. Master the top-K pattern and the "k-way merge."

---

## Why It Matters
- The **Top-K** pattern is one of the most frequently asked Medium categories.
- Heaps enable efficient streaming algorithms (process data without sorting everything).
- Many graph algorithms (Dijkstra, Day 17) require a priority queue.

---

## Key Concepts

### Binary Heap Properties
- **Min-heap:** parent <= children. **Max-heap:** parent >= children.
- Stored as an array: children of index `i` are at `2i+1` and `2i+2`; parent at `(i-1)/2`.
- Insert (push up): O(log n). Extract-min (pop, sift down): O(log n). Peek: O(1).

### Go's `container/heap` Package
Go's heap is an **interface** - you implement a type with `Len`, `Less`, `Swap`, `Push`, `Pop`. Then call `heap.Init`, `heap.Push`, `heap.Pop`.

```go
import "container/heap"

type IntHeap []int
func (h IntHeap) Len() int           { return len(h) }
func (h IntHeap) Less(i, j int) bool { return h[i] < h[j] }  // min-heap
func (h IntHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *IntHeap) Push(x any)        { *h = append(*h, x.(int)) }
func (h *IntHeap) Pop() any {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[:n-1]
    return x
}

// Usage:
h := &IntHeap{3, 1, 2}
heap.Init(h)
heap.Push(h, 0)
min := heap.Pop(h).(int)
```

> **Note:** `Push`/`Pop` have **pointer receivers** because they mutate the slice. `Len`/`Less`/`Swap` use value receivers.

### The Top-K Pattern
**"Find the k largest/smallest elements."**
- Min-heap of size k: push elements; when size > k, pop the min. Remaining = k largest. **O(n log k)**.
- For k smallest: use a max-heap of size k.

```go
// K largest using a min-heap of size k
h := &IntHeap{}
heap.Init(h)
for _, n := range nums {
    heap.Push(h, n)
    if h.Len() > k { heap.Pop(h) }
}
```

### K-Way Merge
Merge k sorted lists: push each list's head into a min-heap by value; repeatedly pop the smallest, append to result, push its next.

### When Heap vs. Sorting
| Situation | Use |
|-----------|-----|
| Need all elements sorted | `sort.Ints` - O(n log n), simpler |
| Top-k of large/streaming data | Heap of size k - O(n log k) |
| Need min repeatedly during processing | Heap |

---

## Problems To Solve

| # | Problem | Difficulty | LeetCode |
|---|---------|-----------|----------|
| 1 | Kth Largest Element in an Array | Medium | [215](https://leetcode.com/problems/kth-largest-element-in-an-array/) |
| 2 | Top K Frequent Elements | Medium | [347](https://leetcode.com/problems/top-k-frequent-elements/) |
| 3 | Find Median from Data Stream | Hard | [295](https://leetcode.com/problems/find-median-from-data-stream/) |
| 4 | Merge K Sorted Lists | Hard | [23](https://leetcode.com/problems/merge-k-sorted-lists/) |

> **#3 (Median stream):** Use TWO heaps - a max-heap for the lower half, a min-heap for the upper half. Keep them balanced (sizes differ by at most 1). Median = top of the larger heap (or average of both tops).
>
> **#4 (Merge K Lists):** Define a heap of `*ListNode` ordered by `Val`. Push all list heads. Pop smallest, push its next. O(N log k) where N = total nodes, k = lists.

---

## Go Tips

- For a heap of structs (e.g., `*ListNode`), implement `Less` comparing the relevant field.
- Go's `heap` package requires `Push`/`Pop` to take `any` (interface{}). Use type assertions `x.(int)`.
- For max-heap in Go, just invert `Less`: `return h[i] > h[j]`.

```go
// Heap of ListNodes
type ListNodeHeap []*ListNode
func (h ListNodeHeap) Len() int { return len(h) }
func (h ListNodeHeap) Less(i, j int) bool { return h[i].Val < h[j].Val }
func (h ListNodeHeap) Swap(i, j int) { h[i], h[j] = h[j], h[i] }
func (h *ListNodeHeap) Push(x any) { *h = append(*h, x.(*ListNode)) }
func (h *ListNodeHeap) Pop() any {
    old := *h; n := len(old); x := old[n-1]; *h = old[:n-1]; return x
}
```

---

## Complexity
- Building a heap: O(n).
- Push/Pop: O(log n).
- Top-K: O(n log k) time, O(k) space.
- Merge k lists: O(N log k) time, O(k) space.
- Median stream: O(log n) per insert, O(1) median query.

---

## Common Mistakes
- Using `sort.Ints` then indexing for "kth largest" - works but interviewers expect the heap (or quickselect). Know both.
- Forgetting pointer receivers on `Push`/`Pop` (compile error).
- In two-heap median: not rebalancing when sizes differ by more than 1.
- Pushing nodes into the heap by value instead of pointer (loses `Next` linkage).

## Checklist Before Moving On
- [ ] Implemented a heap type in Go from scratch (the boilerplate)
- [ ] Solved top-K with a size-k heap
- [ ] Solved median-from-data-stream with two heaps
- [ ] Understand quickselect as an O(n) avg alternative for kth-largest
