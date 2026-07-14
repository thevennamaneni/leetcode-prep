# Day 27 - String Algorithms & Math

## Topic
**String algorithms** (Rabin-Karp rolling hash, KMP basics) and **math problems** (modulo, primes, fast exponentiation). These round out edge categories interviewers may pull from.

---

## Why It Matters
- String matching (find substring) is fundamental; interviewers sometimes ask you NOT to use `strings.Index`.
- Math problems test whether you can avoid overflow and reason about number theory.
- These are "icing" topics - less frequent but high-value when they appear.

---

## Key Concepts

### Rabin-Karp (Rolling Hash)
Compute a hash of the pattern and a rolling hash of the window; compare hashes, then verify on match.

```go
func strStr(haystack, needle string) int {
    if len(needle) == 0 { return 0 }
    if len(needle) > len(haystack) { return -1 }
    base, mod := 256, int(1e9 + 7)
    nhash, hhash, power := 0, 0, 1
    for i := 0; i < len(needle); i++ {
        nhash = (nhash*base + int(needle[i])) % mod
        hhash = (hhash*base + int(haystack[i])) % mod
        power = (power * base) % mod
    }
    if nhash == hhash && haystack[:len(needle)] == needle { return 0 }
    for i := len(needle); i < len(haystack); i++ {
        hhash = (hhash*base + int(haystack[i]) -
                 int(haystack[i-len(needle)])*power%mod + mod) % mod
        if hhash == nhash && haystack[i-len(needle)+1:i+1] == needle {
            return i - len(needle) + 1
        }
    }
    return -1
}
```

> **Mod arithmetic trick:** when subtracting, add `mod` before `%mod` to avoid negative results: `(a - b + mod) % mod`.

### Math: Fast Exponentiation (Binary Exponentiation)
Compute `x^n` in O(log n):
```go
func myPow(x float64, n int) float64 {
    if n < 0 { x = 1 / x; n = -n }
    result := 1.0
    for n > 0 {
        if n & 1 == 1 { result *= x }
        x *= x
        n >>= 1
    }
    return result
}
```

### Math: GCD (Euclid's Algorithm)
```go
func gcd(a, b int) int {
    for b != 0 { a, b = b, a%b }
    return a
}
```

### Roman to Integer / Integer to Roman
Map-based: process symbols; subtract when a smaller value precedes a larger one (e.g., IV = 4).

### Happy Number / Power of Three
Detect cycles (slow/fast pointer or a seen-set) for happy number. For "power of three," the largest power of 3 fitting in an int is divisible by all smaller powers.

---

## Problems To Solve

| # | Problem | Difficulty | LeetCode |
|---|---------|-----------|----------|
| 1 | Find the Index of the First Occurrence | Easy | [28](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/) |
| 2 | Roman to Integer | Easy | [13](https://leetcode.com/problems/roman-to-integer/) |
| 3 | Pow(x, n) | Medium | [50](https://leetcode.com/problems/powx-n/) |
| 4 | Multiply Strings | Medium | [43](https://leetcode.com/problems/multiply-strings/) |
| 5 | Happy Number | Easy | [202](https://leetcode.com/problems/happy-number/) |
| 6 | Reverse Integer | Medium | [7](https://leetcode.com/problems/reverse-integer/) |

> **#4 (Multiply Strings):** Simulate grade-school multiplication. `result[i+j] += digit_i * digit_j`. Handle carries at the end. Watch for leading zeros.
>
> **#6 (Reverse Integer):** Must detect 32-bit overflow. In Go (64-bit int), check if reversed value exceeds `math.MaxInt32` before returning.

---

## Go Tips

- For overflow-safe 32-bit checks: `if result > math.MaxInt32/10 || (result == math.MaxInt32/10 && digit > 7)`.
- `strings.Repeat`, `strconv.Itoa`, `strconv.Atoi` for string-number conversions.
- For Roman numerals, define `values := map[byte]int{'I':1, 'V':5, 'X':10, ...}`.

---

## Complexity
- Rabin-Karp: average **O(n + m)**, worst O(nm) with hash collisions.
- Fast exponentiation: **O(log n)**.
- GCD: **O(log(min(a,b)))**.
- Multiply strings: **O(len1 * len2)**.

---

## Common Mistakes
- Negative modulo results (always `+mod` before final `%mod`).
- Forgetting to verify after a hash match (collisions!).
- In reverse integer: not checking overflow, returning wrong on the edge case.
- In multiply strings: indexing the result array incorrectly (i and j contribute to index i+j+1).

## Checklist Before Moving On
- [ ] Implemented rolling hash substring search
- [ ] Solved Pow(x, n) with binary exponentiation
- [ ] Solved Multiply Strings
- [ ] Know how to detect 32-bit overflow in Go
