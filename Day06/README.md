# Day 6 - Stacks & Queues

## Topic
**Stacks** (LIFO) and **Queues** (FIFO). Go has no generic stack/queue - you use slices. Today also covers the powerful **Monotonic Stack** pattern.

---

## Why It Matters
- Stacks are essential for parsing, parentheses matching, and expression evaluation.
- The **monotonic stack** is a high-frequency Medium pattern ("next greater element" family).
- Queues underpin BFS (Day 16) and are used in level-order traversal.

---

## Key Concepts

### Stack in Go (slice-backed)
```go
stack := []int{}
stack = append(stack, x)    // push
top := stack[len(stack)-1]   // peek
stack = stack[:len(stack)-1] // pop

// Always check empty before pop:
if len(stack) > 0 { ... }
```

### Queue in Go (slice-backed, simple)
```go
queue := []int{}
queue = append(queue, x)      // enqueue
front := queue[0]            // peek
queue = queue[1:]             // dequeue (O(n) - fine for small n)

// For efficiency with large queues, use a head index:
head := 0
queue = append(queue, x)
front := queue[head]; head++
```

### Core Stack Patterns

**1. Matching / Validation** - push openers, pop on closers.
```go
func isValid(s string) bool {
    pairs := map[byte]byte{')':'(', ']':'[', '}':'{'}
    stack := []byte{}
    for i := 0; i < len(s); i++ {
        c := s[i]
        if c == '(' || c == '[' || c == '{' {
            stack = append(stack, c)
        } else {
            if len(stack) == 0 || stack[len(stack)-1] != pairs[c] {
                return false
            }
            stack = stack[:len(stack)-1]
        }
    }
    return len(stack) == 0
}
```

**2. Monotonic Stack** - maintain a stack that's increasing or decreasing. Used to find "next greater/smaller element" in O(n).

**Key insight:** When you're about to push an element, pop all elements from the stack that violate the monotonic property. Each popped element's "next greater" is the current element.

```go
// Next Greater Element
func nextGreaterElements(nums []int) []int {
    n := len(nums)
    res := make([]int, n)
    for i := range res { res[i] = -1 }
    stack := []int{}   // stores indices
    for i := 0; i < n; i++ {
        for len(stack) > 0 && nums[stack[len(stack)-1]] < nums[i] {
            res[stack[len(stack)-1]] = nums[i]
            stack = stack[:len(stack)-1]
        }
        stack = append(stack, i)
    }
    return res
}
```

### When to Use Each
| Signal | Structure |
|--------|-----------|
| "valid parentheses" | Stack |
| "evaluate expression" / "RPN" | Stack |
| "next greater/smaller element" | Monotonic stack |
| "daily temperatures" | Monotonic decreasing stack |
| "largest rectangle in histogram" | Monotonic stack |
| BFS / level order | Queue |

---

## Problems To Solve

| # | Problem | Difficulty | LeetCode |
|---|---------|-----------|----------|
| 1 | Valid Parentheses | Easy | [20](https://leetcode.com/problems/valid-parentheses/) |
| 2 | Implement Queue using Stacks | Easy | [232](https://leetcode.com/problems/implement-queue-using-stacks/) |
| 3 | Min Stack | Easy | [155](https://leetcode.com/problems/min-stack/) |
| 4 | Daily Temperatures | Medium | [739](https://leetcode.com/problems/daily-temperatures/) |
| 5 | Evaluate Reverse Polish Notation | Medium | [150](https://leetcode.com/problems/evaluate-reverse-polish-notation/) |

> **#4 (Daily Temperatures):** Classic monotonic stack. For each day, pop while current temp > stack-top temp; the difference is the answer for the popped index. O(n).

---

## Go Tips

- For RPN: `strconv.Atoi()` converts strings to ints; handle the `error` return.
- For Min Stack: keep an **auxiliary stack** tracking the min at each level.
- Slices are fine for stacks; for very deep recursion consider iterative to avoid stack overflow (Go's goroutine stack grows but the C-style call stack is limited-ish).

```go
// Min Stack with auxiliary min tracking
type MinStack struct {
    stack []int
    mins  []int
}
```

---

## Complexity
- Stack push/pop/peek: **O(1)** time, O(1) space per op.
- Monotonic stack: **O(n)** time total (each element pushed and popped once), O(n) space.

---

## Common Mistakes
- Forgetting to check `len(stack) > 0` before popping (index out of range panic).
- In monotonic stack: using the wrong comparison (`<` vs `>`).
- In Min Stack: recomputing min with `O(n)` scan instead of the auxiliary stack.
- Using `queue = queue[1:]` in a hot loop causing O(n^2) - use a head pointer.

## Checklist Before Moving On
- [ ] Solved Daily Temperatures with the monotonic stack
- [ ] Can implement a stack and queue from a slice
- [ ] Understand why monotonic stack is O(n) not O(n^2)
