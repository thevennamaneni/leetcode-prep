# Day 25 - Trie & Advanced Data Structures

## Topic
**Trie** (prefix tree) - the data structure for prefix-based string queries. Plus a survey of other interview-relevant structures: **LRU/LFU cache**, **disjoint intervals**.

---

## Why It Matters
- Trie problems (word search II, autocomplete) are common at Google/Amazon.
- "Design LRU Cache" is one of the most frequently asked design problems across all FAANG.
- Understanding these structures shows senior-level depth.

---

## Key Concepts

### Trie Structure
A tree where each path from root spells out a string. Each node has children (often a `[26]*Trie` array) and an `isEnd` flag.

```go
type TrieNode struct {
    children [26]*TrieNode
    isEnd    bool
}

type Trie struct {
    root *TrieNode
}

func Constructor() Trie { return Trie{root: &TrieNode{}} }

func (t *Trie) Insert(word string) {
    node := t.root
    for i := 0; i < len(word); i++ {
        idx := word[i] - 'a'
        if node.children[idx] == nil {
            node.children[idx] = &TrieNode{}
        }
        node = node.children[idx]
    }
    node.isEnd = true
}

func (t *Trie) Search(word string) bool {
    node := t.searchPrefix(word)
    return node != nil && node.isEnd
}

func (t *Trie) StartsWith(prefix string) bool {
    return t.searchPrefix(prefix) != nil
}

func (t *Trie) searchPrefix(s string) *TrieNode {
    node := t.root
    for i := 0; i < len(s); i++ {
        idx := s[i] - 'a'
        if node.children[idx] == nil { return nil }
        node = node.children[idx]
    }
    return node
}
```

### LRU Cache (Doubly-Linked List + HashMap)
**Requirements:** O(1) get and put.
- **HashMap** maps key -> node for O(1) lookup.
- **Doubly-linked list** maintains recency order: most recent at head, least at tail.
- On `get`: move node to head. On `put`: add to head; if over capacity, remove tail.

```go
type Node struct {
    key, val  int
    prev, next *Node
}
type LRUCache struct {
    cap       int
    cache     map[int]*Node
    head, tail *Node  // sentinels
}
```
> Using **sentinel head/tail nodes** eliminates nil checks for edges - highly recommended.

### LFU Cache
Similar idea but track frequency counts; evict the least frequently used (and least recently among ties). Harder - know the structure but don't over-invest.

---

## Problems To Solve

| # | Problem | Difficulty | LeetCode |
|---|---------|-----------|----------|
| 1 | Implement Trie (Prefix Tree) | Medium | [208](https://leetcode.com/problems/implement-trie-prefix-tree/) |
| 2 | Design Add and Search Words | Medium | [211](https://leetcode.com/problems/design-add-and-search-words-data-structure/) |
| 3 | Word Search II | Hard | [212](https://leetcode.com/problems/word-search-ii/) |
| 4 | LRU Cache | Medium | [146](https://leetcode.com/problems/lru-cache/) |
| 5 | Implement Queue using Stacks | Easy | [232](https://leetcode.com/problems/implement-queue-using-stacks/) |

> **#2 (Add & Search Words):** Like Trie, but `.` matches any char. On `.`, recurse into all children.
>
> **#3 (Word Search II):** Build a trie of all words; DFS the grid, pruning when no trie child matches. Backtrack with board mutation (Day 15 technique).

---

## Go Tips

- Go has no generics-based Trie in the stdlib; implement per problem.
- For LRU, define the doubly-linked list with `prev`/`next` and helper `remove`/`addToFront` methods.
- Use sentinel nodes (`head`/`tail`) to avoid nil-pointer edge cases in the DLL.
- Maps in Go can hold `*Node` pointers; mutation through the pointer is automatic.

```go
// LRU helpers with sentinels
func (c *LRUCache) remove(n *Node) {
    n.prev.next = n.next
    n.next.prev = n.prev
}
func (c *LRUCache) addFront(n *Node) {
    n.next = c.head.next
    n.prev = c.head
    c.head.next.prev = n
    c.head.next = n
}
```

---

## Complexity
- Trie insert/search: **O(L)** where L = word length.
- LRU get/put: **O(1)**.
- Word Search II: **O(M * N * 4^L)** but trie pruning dramatically cuts this.

---

## Common Mistakes
- Trie: not handling lowercase assumption; if input has uppercase, normalize first or use a `map[byte]*TrieNode`.
- LRU: not using sentinels -> verbose nil-checking and bugs.
- LRU: forgetting to update the map when removing the tail node.
- Word Search II: not pruning (re-running full DFS for every word separately -> TLE).

## Checklist Before Moving On
- [ ] Implemented Trie from scratch
- [ ] Solved LRU Cache with O(1) get/put using DLL + map
- [ ] Solved Word Search II with trie pruning
- [ ] Comfortable with sentinel node technique
