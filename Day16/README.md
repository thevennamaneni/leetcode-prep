# Day 16 - Graphs: BFS & DFS

## Topic
**Graphs** - adjacency lists, BFS (shortest path in unweighted graphs), DFS (connectivity, components). Today introduces graph representation and the two traversal engines.

---

## Why It Matters
- Graph problems appear in ~15% of FAANG interviews.
- BFS is THE algorithm for "shortest path" and "nearest X" in unweighted graphs.
- DFS is the engine for connected components, cycles, and (Day 17) topological sort.

---

## Key Concepts

### Graph Representation
**Adjacency list** is the standard: `graph [][]int` where `graph[u]` is the list of neighbors of `u`. Or a `map[int][]int` for sparse/non-contiguous node labels.

```go
// Build adjacency list from edge list [u, v]
n := numNodes
graph := make([][]int, n)
for _, e := range edges {
    graph[e[0]] = append(graph[e[0]], e[1])
    graph[e[1]] = append(graph[e[1]], e[0])  // undirected
}
```

### BFS (Breadth-First Search) - Level by Level
Use a **queue** and a **visited** set. BFS finds shortest paths in unweighted graphs.

```go
func bfs(start int, graph [][]int) {
    visited := make([]bool, len(graph))
    queue := []int{start}
    visited[start] = true
    for len(queue) > 0 {
        node := queue[0]; queue = queue[1:]
        // process node
        for _, next := range graph[node] {
            if !visited[next] {
                visited[next] = true
                queue = append(queue, next)
            }
        }
    }
}
```

**Mark visited when enqueuing** (not when dequeuing) - prevents duplicates in the queue.

### BFS for Shortest Path
Track distance: `dist[next] = dist[node] + 1`. First time you reach target = shortest.

### DFS (Depth-First Search) - Go Deep
Use recursion (implicit stack) or an explicit stack.

```go
func dfs(node int, graph [][]int, visited []bool) {
    visited[node] = true
    for _, next := range graph[node] {
        if !visited[next] {
            dfs(next, graph, visited)
        }
    }
}
```

### Connected Components
Loop over all nodes; if unvisited, DFS/BFS from it and increment component count.

### Grid as Graph
A 2D grid of '1'/'0' is a graph: cells are nodes, 4-directional neighbors are edges. **Number of Islands** = connected components.

```go
// Number of Islands - DFS on grid
func numIslands(grid [][]byte) int {
    rows, cols := len(grid), len(grid[0])
    var dfs func(r, c int)
    dfs = func(r, c int) {
        if r < 0 || r >= rows || c < 0 || c >= cols || grid[r][c] != '1' { return }
        grid[r][c] = '0'   // mark visited by sinking
        dfs(r+1,c); dfs(r-1,c); dfs(r,c+1); dfs(r,c-1)
    }
    count := 0
    for r := 0; r < rows; r++ {
        for c := 0; c < cols; c++ {
            if grid[r][c] == '1' {
                count++
                dfs(r, c)
            }
        }
    }
    return count
}
```

### When BFS vs DFS
| Need | Use |
|-----|-----|
| Shortest path (unweighted) | BFS |
| "any path" / existence | DFS |
| Connected components | Either |
| Topological sort | DFS (postorder) |
| Nearest neighbor within k steps | BFS |
| Maze solving | DFS or BFS |

---

## Problems To Solve

| # | Problem | Difficulty | LeetCode |
|---|---------|-----------|----------|
| 1 | Number of Islands | Medium | [200](https://leetcode.com/problems/number-of-islands/) |
| 2 | Clone Graph | Medium | [133](https://leetcode.com/problems/clone-graph/) |
| 3 | Rotting Oranges | Medium | [994](https://leetcode.com/problems/rotting-oranges/) |
| 4 | Flood Fill | Easy | [733](https://leetcode.com/problems/flood-fill/) |
| 5 | Walls and Gates | Medium | [286](https://leetcode.com/problems/walls-and-gates/) (premium) |

> **#3 (Rotting Oranges):** Multi-source BFS. Enqueue ALL initial rotten oranges, BFS outward, track time = BFS level. If any fresh orange remains unreachable, return -1.
>
> **#5 (Walls and Gates)** is premium - if you don't have it, use [01 Matrix (542)](https://leetcode.com/problems/01-matrix/) instead (same multi-source BFS pattern).

---

## Go Tips

- For grid BFS, the 4 directions: `dr := []int{-1,1,0,0}; dc := []int{0,0,-1,1}`.
- "Sink" visited land by setting `grid[r][c] = '0'` - no separate visited matrix needed.
- Use `[][]bool` for visited when you can't mutate the input.
- For clone graph, `map[*Node]*Node` maps original -> clone (handles cycles!).

---

## Complexity
- BFS/DFS: **O(V + E)** time and O(V) space (visited + queue/stack).
- Grid: O(rows * cols) = O(cells).

---

## Common Mistakes
- **Marking visited on dequeue instead of enqueue** - causes duplicates and wrong BFS levels.
- Forgetting to handle disconnected components (single loop start misses islands).
- In clone graph: not using a map for original->clone, leading to infinite loops on cycles.
- Not handling grid boundaries in DFS (use a bounds check at function top).

## Checklist Before Moving On
- [ ] Solved Number of Islands with in-place sink
- [ ] Solved Rotting Oranges with multi-source BFS
- [ ] Know when BFS gives shortest path
- [ ] Can build an adjacency list from an edge list
