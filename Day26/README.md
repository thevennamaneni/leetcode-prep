# Day 26 - Union-Find & Matrix/Advanced Patterns

## Topic
**Union-Find** (Disjoint Set Union) for connectivity, plus **matrix traversal patterns** (spiral, rotate, set matrix zeroes) and **reservoir sampling** if time permits.

---

## Why It Matters
- Union-Find elegantly solves "are these in the same component?" and "number of connected components" - especially when edges are added dynamically.
- Matrix problems are common Easys/Mediums that test careful indexing.
- Union-Find is a senior-level tool that impresses when applied correctly.

---

## Key Concepts

### Union-Find (DSU)
Two operations:
- `find(x)`: return the root representative of x's set.
- `union(x, y)`: merge x's and y's sets.

**Optimizations (both needed for near-O(1)):**
1. **Path compression:** in `find`, point each node directly to the root as you recurse up.
2. **Union by rank/size:** always attach the smaller tree under the larger.

```go
type DSU struct {
    parent []int
    rank   []int
}

func NewDSU(n int) *DSU {
    d := &DSU{parent: make([]int, n), rank: make([]int, n)}
    for i := range d.parent { d.parent[i] = i }
    return d
}

func (d *DSU) Find(x int) int {
    if d.parent[x] != x {
        d.parent[x] = d.Find(d.parent[x])  // path compression
    }
    return d.parent[x]
}

func (d *DSU) Union(x, y int) bool {
    px, py := d.Find(x), d.Find(y)
    if px == py { return false }  // already same set
    if d.rank[px] < d.rank[py] { px, py = py, px }
    d.parent[py] = px
    if d.rank[px] == d.rank[py] { d.rank[px]++ }
    return true
}
```

> `Union` returns `true` if a merge happened - useful for counting components (decrement count on each successful union).

### Number of Connected Components
Start with `count = n`; each successful `union` decrements `count`. Final `count` = components.

### Matrix Patterns

**Spiral Order:** layer by layer, four directional sweeps (right, down, left, up), shrinking boundaries.

**Rotate Image 90°:** transpose (swap `m[i][j]` and `m[j][i]`), then reverse each row.

**Set Matrix Zeroes (in-place):** use the first row and first column as markers; do two passes (mark, then zero). Handle the corner cell specially.

---

## Problems To Solve

| # | Problem | Difficulty | LeetCode |
|---|---------|-----------|----------|
| 1 | Number of Connected Components (UF) | Medium | [323](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/) (premium) |
| 2 | Number of Islands II | Hard | [305](https://leetcode.com/problems/number-of-islands-ii/) (premium) |
| 3 | Redundant Connection | Medium | [684](https://leetcode.com/problems/redundant-connection/) |
| 4 | Spiral Matrix | Medium | [54](https://leetcode.com/problems/spiral-matrix/) |
| 5 | Set Matrix Zeroes | Medium | [73](https://leetcode.com/problems/set-matrix-zeroes/) |
| 6 | Rotate Image | Medium | [48](https://leetcode.com/problems/rotate-image/) |

> **Free alternatives for premium #1/#2:** implement Union-Find and apply it to [547. Number of Provinces](https://leetcode.com/problems/number-of-provinces/) (free) - same pattern. For #2, the concept: as you add land cells one by one, union them with any land neighbors; count goes down on each merge.
>
> **#3 (Redundant Connection):** Union edges in order; the first edge that connects two already-connected nodes is the redundant one. Pure Union-Find.

---

## Go Tips

- DSU with path compression is naturally recursive. For deep sets, Go's stack might overflow - but LeetCode inputs are fine.
- Use iterative `Find` if recursion depth is a concern:
```go
func (d *DSU) Find(x int) int {
    for d.parent[x] != x {
        d.parent[x] = d.parent[d.parent[x]]  // path halving
        x = d.parent[x]
    }
    return x
}
```
- Matrix rotation: `m[i][j], m[j][i] = m[j][i], m[i][j]` transposes in place (only upper triangle).

---

## Complexity
- Union-Find: **O(alpha(n))** per operation (inverse Ackermann, effectively O(1)).
- Matrix problems: **O(m * n)**.

---

## Common Mistakes
- Forgetting path compression or union-by-rank (degrades to O(n) per op).
- In Set Matrix Zeroes: zeroing in the first pass instead of just marking (corrupts the markers).
- In Spiral Matrix: off-by-one on the layer boundaries; handle single-row/column cases.
- In Rotate Image: transposing the full matrix (double-swaps) instead of just upper triangle.

## Checklist Before Moving On
- [ ] Implemented DSU with both optimizations
- [ ] Solved Redundant Connection
- [ ] Solved Spiral Matrix and Rotate Image
- [ ] Know the transpose-then-reverse trick for 90° rotation
