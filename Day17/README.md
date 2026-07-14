# Day 17 - Graphs: Topological Sort, Cycles & Dijkstra

## Topic
Advanced graph algorithms: **topological sort** (for DAGs), **cycle detection**, **Dijkstra's** (shortest path with weights), and **union-find** for connectivity.

---

## Why It Matters
- Topological sort answers "course schedule" / "build order" questions - extremely common.
- Dijkstra is THE shortest-path algorithm for weighted graphs.
- Union-Find is a powerful disjoint-set tool that reappears on Day 26.

---

## Key Concepts

### Topological Sort (Kahn's Algorithm - BFS)
**Use when:** ordering matters (dependencies), graph is a DAG.
1. Compute in-degree of every node.
2. Enqueue all nodes with in-degree 0.
3. Dequeue, "remove" it: decrement neighbors' in-degrees; enqueue any that hit 0.
4. If processed count < total nodes → cycle exists.

```go
func canFinish(numCourses int, prerequisites [][]int) bool {
    graph := make([][]int, numCourses)
    indegree := make([]int, numCourses)
    for _, p := range prerequisites {
        graph[p[1]] = append(graph[p[1]], p[0])
        indegree[p[0]]++
    }
    queue := []int{}
    for i := 0; i < numCourses; i++ {
        if indegree[i] == 0 { queue = append(queue, i) }
    }
    count := 0
    for len(queue) > 0 {
        node := queue[0]; queue = queue[1:]
        count++
        for _, next := range graph[node] {
            indegree[next]--
            if indegree[next] == 0 { queue = append(queue, next) }
        }
    }
    return count == numCourses
}
```

### Cycle Detection (DFS)
For directed graphs, use 3 states: unvisited (0), visiting (1), visited (2). If DFS reaches a "visiting" node → cycle.

```go
state := make([]int, n)  // 0=unvisited, 1=visiting, 2=visited
var dfs func(int) bool
dfs = func(node int) bool {
    if state[node] == 1 { return false }  // cycle
    if state[node] == 2 { return true }
    state[node] = 1
    for _, next := range graph[node] {
        if !dfs(next) { return false }
    }
    state[node] = 2
    return true
}
```

### Dijkstra's Algorithm (Priority Queue)
Shortest path in a **weighted** graph with non-negative weights.

```go
// dist[i] = shortest distance from source to i
dist := make([]int, n)
for i := range dist { dist[i] = math.MaxInt }
dist[source] = 0

h := &minHeap{}  // (distance, node)
heap.Push(h, pair{0, source})
for h.Len() > 0 {
    cur := heap.Pop(h).(pair)
    if cur.dist > dist[cur.node] { continue }  // stale entry
    for _, e := range graph[cur.node] {
        nd := cur.dist + e.weight
        if nd < dist[e.to] {
            dist[e.to] = nd
            heap.Push(h, pair{nd, e.to})
        }
    }
}
```

### Union-Find (preview - full treatment Day 26)
Disjoint-set for connectivity queries. `find` with path compression, `union` by rank. Near O(1) per op.

---

## Problems To Solve

| # | Problem | Difficulty | LeetCode |
|---|---------|-----------|----------|
| 1 | Course Schedule | Medium | [207](https://leetcode.com/problems/course-schedule/) |
| 2 | Course Schedule II (return order) | Medium | [210](https://leetcode.com/problems/course-schedule-ii/) |
| 3 | Network Delay Time | Medium | [743](https://leetcode.com/problems/network-delay-time/) |
| 4 | Cheapest Flights Within K Stops | Medium | [787](https://leetcode.com/problems/cheapest-flights-within-k-stops/) |
| 5 | Alien Dictionary | Hard | [269](https://leetcode.com/problems/alien-dictionary/) (premium) |

> **#3 (Network Delay Time):** Classic Dijkstra from source `k`. Answer = max over all distances (if any unreachable, return -1).
>
> **#4 (Cheapest Flights):** Bellman-Ford style - relax edges up to k+1 times. Or BFS with a `stops` counter. NOT plain Dijkstra (the "k stops" constraint complicates it).
>
> **#5 (Alien Dictionary)** is premium - if unavailable, focus on #1-4 which cover the patterns.

---

## Go Tips

- For Dijkstra, define a heap of `pair{dist, node}` structs with `Less` comparing dist.
- `math.MaxInt` for "infinity" sentinel.
- Use `container/heap` (Day 12 boilerplate) for the priority queue.
- For Bellman-Ford (#4), iterate over all edges k+1 times, relaxing distances.

---

## Complexity
- Topological sort: **O(V + E)**.
- Cycle detection (DFS): **O(V + E)**.
- Dijkstra with binary heap: **O((V + E) log V)**.
- Bellman-Ford: **O(V * E)**.

---

## Common Mistakes
- In Kahn's: not detecting the cycle (processed count < n means cycle, return false).
- In Dijkstra: not skipping stale heap entries (`cur.dist > dist[cur.node]`).
- In alien dictionary: building the graph incorrectly from adjacent differing characters.
- Using Dijkstra for "k stops" problem without the stop constraint - wrong answer.

## Checklist Before Moving On
- [ ] Solved Course Schedule I & II
- [ ] Implemented Dijkstra with a Go heap
- [ ] Understand the 3-state cycle detection
- [ ] Know when to use Bellman-Ford vs Dijkstra (negative weights / k-hop constraint)
