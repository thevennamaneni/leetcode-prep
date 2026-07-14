# Day 24 - Bit Manipulation

## Topic
**Bit manipulation** - bitwise operators, common tricks (XOR, masks), and problems that hinge on bit-level reasoning (single number, counting bits, reverse bits).

---

## Why It Matters
- Bit problems appear in ~10% of interviews; they're often "Easy if you know the trick, impossible if you don't."
- They test low-level reasoning that senior engineers are expected to have.
- A few memorized tricks unlock most of the category.

---

## Key Concepts

### Go Bitwise Operators
```go
&    // AND        both bits set
|    // OR         either bit set
^    // XOR        bits differ     (also unary: ^x = bitwise NOT in Go)
&^   // AND NOT    bit clear (a &^ b = a & ~b)
<<   // left shift (multiply by 2)
>>   // right shift (divide by 2)
```

> **Go quirk:** `^x` (unary) is bitwise NOT. Most languages use `~`. Go has no `~`.

### The Essential Bit Tricks (MEMORIZE)

| Trick | Code | Meaning |
|-------|------|---------|
| Check bit i | `(n >> i) & 1` | Is bit i set? |
| Set bit i | `n \| (1 << i)` | Turn bit i on |
| Clear bit i | `n &^ (1 << i)` | Turn bit i off |
| Toggle bit i | `n ^ (1 << i)` | Flip bit i |
| Lowest set bit | `n & -n` | Isolate rightmost 1 (e.g., 6 & -6 = 2) |
| Clear lowest set bit | `n & (n - 1)` | Remove rightmost 1 |
| Count set bits | Brian Kernighan's: loop `n &= (n-1)` | Each iteration removes one 1 |
| Check power of 2 | `n > 0 && (n & (n-1)) == 0` | Powers of 2 have one 1-bit |
| XOR all | `a ^ a == 0`, `a ^ 0 == a` | Pairs cancel - finds the unique one |

### XOR: The Canceling Property
XOR of a number with itself = 0. So XOR-ing all elements of an array where every element appears twice except one yields the unique element.

```go
func singleNumber(nums []int) int {
    result := 0
    for _, n := range nums { result ^= n }
    return result
}
```

### Brian Kernighan's Bit Counting
```go
func hammingWeight(n int) int {
    count := 0
    for n != 0 {
        n &= (n - 1)  // clears the lowest set bit
        count++
    }
    return count
}
```

### Missing Number (0..n)
XOR all numbers 0..n with all array elements; the missing one remains. Or use the sum formula `n*(n+1)/2 - sum(nums)`.

---

## Problems To Solve

| # | Problem | Difficulty | LeetCode |
|---|---------|-----------|----------|
| 1 | Single Number | Easy | [136](https://leetcode.com/problems/single-number/) |
| 2 | Number of 1 Bits | Easy | [191](https://leetcode.com/problems/number-of-1-bits/) |
| 3 | Missing Number | Easy | [268](https://leetcode.com/problems/missing-number/) |
| 4 | Counting Bits | Easy | [338](https://leetcode.com/problems/counting-bits/) |
| 5 | Reverse Bits | Easy | [190](https://leetcode.com/problems/reverse-bits/) |

> **#4 (Counting Bits):** DP + bits: `dp[i] = dp[i >> 1] + (i & 1)`. The count for `i` = count for `i/2` (shifted off the low bit) plus the low bit. O(n).
>
> **#5 (Reverse Bits):** Process 32 bits: build result by shifting left and adding the LSB of n, then shifting n right.

---

## Go Tips

- Go integers: `int` is platform-dependent (usually 64-bit). For bit problems with fixed width, use `uint32`.
- `fmt.Sprintf("%b", n)` for debugging binary representation.
- For "reverse bits," loop exactly 32 times using `uint32`.
- Go's `math/bits` package has `bits.OnesCount`, `bits.TrailingZeros`, etc. - know they exist, but implement manually in interviews unless told otherwise.

```go
// Reverse bits
func reverseBits(num uint32) uint32 {
    var result uint32
    for i := 0; i < 32; i++ {
        result = (result << 1) | (num & 1)
        num >>= 1
    }
    return result
}
```

---

## Complexity
- Single number: **O(n)** time, O(1) space.
- Bit counting: O(1) for fixed-width ints (32 iterations max).
- Counting bits (DP): O(n).

---

## Common Mistakes
- Using `~` (doesn't exist in Go) instead of `^` for NOT.
- Sign issues: right-shifting a negative number is arithmetic (sign-extends). Use unsigned types for pure bit work.
- Forgetting that `n & -n` relies on two's complement (true in Go).
- In counting bits DP: wrong transition - it's `dp[i>>1]` (not `dp[i-1]`).

## Checklist Before Moving On
- [ ] Memorized the bit tricks table
- [ ] Solved Single Number with XOR
- [ ] Solved Counting Bits with the DP approach
- [ ] Know that Go uses `^` for NOT, not `~`
