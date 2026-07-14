# 30-Day FAANG Interview Prep Plan

> **For:** Senior Go Developer (9 yrs exp), zero LeetCode experience
> **Goal:** Pass a FAANG first-round technical interview (LeetCode style)

---

## How To Use This Plan

Each day has its own folder (`Day01/`, `Day02/`, ... `Day30/`) containing a `README.md` with:
- **Topic** - the concept(s) covered that day
- **Strategy** - interview approach & mental models
- **Data Structures & Algorithms** - theory with Go-flavored notes
- **Problems** - specific LeetCode problems to solve (with numbers)
- **Go Tips** - idiomatic Go patterns for that topic
- **Complexity** - time/space analysis you should memorize

## Daily Routine (2-3 hours/day)

1. **Read** the day's README (15 min) - understand the pattern before touching code.
2. **Study** 1-2 example solutions from LeetCode Discuss / NeetCode (30 min).
3. **Solve** the listed problems yourself (90-120 min). Write in Go.
4. **Review** - read the top-voted solution even if you solved it. Compare approaches.
5. **Notes** - jot down the pattern name and the "trick" in a personal cheat-sheet.

## The Core Philosophy

FAANG interviews don't test 1000 random problems. They test **~15 reusable patterns**.
Master the pattern, and 80%+ of problems become recognizable. This plan is ordered
so each day builds on the previous one.

## Pattern Roadmap (the 15 patterns you MUST know)

| Pattern | Days |
|--------|------|
| Arrays & Two Pointers | 1-3 |
| Sliding Window | 3 |
| Binary Search | 4 |
| Hash Maps | 5 |
| Stacks / Monotonic Stack | 6, 23 |
| Linked Lists | 8-9 |
| Trees (BST, Traversals) | 10-11 |
| Heaps / Priority Queues | 12 |
| Backtracking / Recursion | 13, 15 |
| Graphs (BFS/DFS) | 16-17 |
| Dynamic Programming | 18-20 |
| Greedy | 22 |
| Intervals | 23 |
| Bit Manipulation | 24 |
| Trie | 25 |
| Union-Find / Advanced | 26 |

## Weekly Schedule

| Week | Theme | Days |
|------|-------|------|
| **Week 1** | Foundations & Linear Data Structures | 1-7 |
| **Week 2** | Linked Lists, Trees & Recursion | 8-14 |
| **Week 3** | Graphs & Dynamic Programming | 15-21 |
| **Week 4** | Advanced Topics & Mock Interviews | 22-30 |

## Golden Rules

1. **Never look at the solution before attempting for 20-30 min.** Struggle is where learning happens.
2. **If stuck after 30 min, read the *approach hint* only**, not the full solution. Try again.
3. **Re-implement from memory** the next day for problems you found hard.
4. **Always state time/space complexity** out loud - interviewers want to hear your reasoning.
5. **Communicate while coding** - narrate your thought process. This is a scored skill.
6. **Use Go's strengths**: slices, maps, built-in `sort`, goroutines (rarely needed), `container/heap`.
7. **Write clean code**: meaningful variable names, helper functions, no magic numbers.

## Spaced Repetition Schedule

Re-solve these days' problems on review days (Day 7, 14, 21, 30):
- **Day 7 review:** Days 1-6
- **Day 14 review:** Days 8-13 + hard ones from 1-6
- **Day 21 review:** Days 15-20 + hard ones from 8-13
- **Day 30 review:** All weak spots identified throughout

## Tools & Resources

- **NeetCode.io** - the best pattern-based problem list (free). Map its problems to these days.
- **LeetCode Discuss** - top-voted Go solutions.
- **Complexity cheat sheet** - keep it open while solving.

## LeetCode Problem Selection Criteria

Problems chosen for this plan are:
- High frequency in FAANG interviews
- Classic exemplars of each pattern
- Difficulty ramps from Easy -> Medium -> (select) Hard
- Total ~90 problems (3/day avg) - a realistic 30-day load

---

**Let's begin. Open `Day01/README.md`.**
