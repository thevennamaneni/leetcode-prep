# Day 23 - Stacks (Advanced) & Monotonic Queue

## Topic
**Monotonic stack/queue** (advanced), **largest rectangle in histogram**, **daily temperatures** (recap), and **evaluate expression** with precedence. Deepens your stack mastery.

---

## Why It Matters
- "Largest rectangle in histogram" is a classic Hard that's pure monotonic stack.
- Expression parsing (infix -> postfix, eval) is asked at Amazon/Google.
- These problems cement the "stack as stateful boundary keeper" mental model.

---

## Key Concepts

### Monotonic Stack Recap (Day 6)
A stack that maintains increasing or decreasing order. When pushing, pop everything that violates the order. Each pop resolves a "next greater/smaller" query.

### Largest Rectangle in Histogram (THE classic)
For each bar, find the widest rectangle using that bar's height as the minimum. The width extends left and right until bars shorter than it. Use a **monotonic increasing stack of indices**.

```go
func largestRectangleArea(heights []int) int {
    stack := []int{}  // indices, increasing heights
    maxArea := 0
    for i := 0; i <= len(heights); i++ {
        h := 0
        if i < len(heights) { h = heights[i] }
        for len(stack) > 0 && heights[stack[len(stack)-1]] > h {
            height := heights[stack[len(stack)-1]]
            stack = stack[:len(stack)-1]
            width := i
            if len(stack) > 0 { width = i - stack[len(stack)-1] - 1 }
            if height*width > maxArea { maxArea = height * width }
        }
        stack = append(stack, i)
    }
    return maxArea
}
```

> **The sentinel trick:** iterate `i` from `0` to `len(heights)` (inclusive), treating a virtual bar of height 0 at the end. This flushes the stack so every bar gets its rectangle computed. Cleanly handles the last element.

### Expression Evaluation
**Infix to Postfix (Shunting Yard):** use a stack for operators, respecting precedence (`*`/`/` > `+`/`-`).
**Evaluate Postfix:** operand stack; on operator, pop two operands, apply, push result.

### Asteroid Collision
Stack-based simulation: right-moving asteroids push; on a left-moving one, resolve collisions with the stack top until stable.

---

## Problems To Solve

| # | Problem | Difficulty | LeetCode |
|---|---------|-----------|----------|
| 1 | Daily Temperatures | Medium | [739](https://leetcode.com/problems/daily-temperatures/) |
| 2 | Largest Rectangle in Histogram | Hard | [84](https://leetcode.com/problems/largest-rectangle-in-histogram/) |
| 3 | Basic Calculator II | Medium | [227](https://leetcode.com/problems/basic-calculator-ii/) |
| 4 | Asteroid Collision | Medium | [735](https://leetcode.com/problems/asteroid-collision/) |
| 5 | Online Stock Span | Medium | [901](https://leetcode.com/problems/online-stock-span/) |

> **#5 (Stock Span):** Monotonic decreasing stack of (price, span). On a new price, pop smaller entries accumulating their spans; push (price, accumulated span). O(n) amortized.

---

## Go Tips

- For expression evaluation, `strconv.Atoi` parses numbers; handle multi-digit by accumulating `num = num*10 + int(c-'0')`.
- Operator precedence as a map: `prec := map[byte]int{'+':1, '-':1, '*':2, '/':2}`.
- For Basic Calculator II, a simpler approach: process operators in a single pass, pushing to a stack for `*`/`/`, summing for `+`/`-`.

```go
// Basic Calculator II - stack for * and /
func calculate(s string) int {
    stack := []int{}
    num := 0
    op := byte('+')
    for i := 0; i < len(s); i++ {
        if s[i] >= '0' && s[i] <= '9' {
            num = num*10 + int(s[i]-'0')
        }
        if isOp(s[i]) || i == len(s)-1 {
            switch op {
            case '+': stack = append(stack, num)
            case '-': stack = append(stack, -num)
            case '*': stack[len(stack)-1] *= num
            case '/': stack[len(stack)-1] /= num
            }
            op, num = s[i], 0
        }
    }
    sum := 0
    for _, v := range stack { sum += v }
    return sum
}
```

---

## Complexity
- All today's problems: **O(n)** time (each element pushed/popped once), O(n) space.

---

## Common Mistakes
- In histogram: not using the sentinel to flush the stack (last bars never evaluated).
- In histogram: computing width wrong when stack is non-empty (`i - top - 1`).
- In calculator: not handling multi-digit numbers or trailing spaces.
- In asteroid collision: only handling adjacent pairs (need a stack to resolve cascading collisions).

## Checklist Before Moving On
- [ ] Solved Largest Rectangle in Histogram with the sentinel trick
- [ ] Solved Basic Calculator II
- [ ] Understand the monotonic stack's "pop resolves a query" insight
- [ ] Can explain why these are all O(n) despite nested-looking loops
