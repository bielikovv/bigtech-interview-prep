# Bit Manipulation — Full Map

# Part 0: Foundations — What Bits Actually Are

Master this before any pattern below. This is the physical/mathematical substrate everything else builds on.

## 0.1 What a Bit Is

A bit is a binary digit — the smallest unit of information a computer stores, physically represented as one of two voltage/charge states (on/off, high/low). Everything (numbers, characters, images, instructions) is ultimately an arrangement of bits.

- Byte = 8 bits. Word = platform-dependent (commonly 32 or 64 bits).
- Bit indices are typically numbered from the right, starting at 0 (the least significant bit, LSB), increasing leftward to the most significant bit (MSB).

## 0.2 Number Representation

- **Unsigned integers:** straightforward binary place-value (base 2), same as decimal but with powers of 2.
- **Signed integers — Two's Complement:** the dominant representation. To negate a number, invert all bits (one's complement) and add 1. The MSB acts as a sign indicator (1 = negative) without needing separate sign-magnitude logic. This is why -1 is all 1-bits, and why addition/subtraction circuits don't need special-casing for sign.
- **Overflow & wraparound:** fixed-width registers (int32, int64) wrap silently on overflow — critical for predicting bitmask behavior at boundary values.
- **Hex/Octal as bit shorthand:** hexadecimal maps 1 digit → 4 bits exactly (nibble), which is why hex is used to read bit patterns at a glance; octal maps 1 digit → 3 bits.

## 0.3 Core Bitwise Operators (The Physical Gates)

- `&` (AND) — masking, checking, clearing.
- `|` (OR) — setting, merging.
- `^` (XOR) — toggling, comparing-for-difference, self-inverse (a^a=0, a^0=a).
- `~` (NOT) — inversion, complementing.
- `<<` (left shift) — multiply by 2^k, shift bits toward MSB.
- `>>` (right shift) — divide by 2^k; arithmetic shift (sign-preserving, fills with sign bit) vs logical shift (fills with 0) — this distinction is a classic gotcha.

## 0.4 Emergent Properties (Why These Operators Matter)

- `x & (x-1)` clears the lowest set bit.
- `x & (-x)` isolates the lowest set bit (relies on two's complement).
- `x | (x+1)` sets the lowest unset bit.
- XOR is its own inverse — the algebraic backbone of swap-without-temp and single-number-finding problems.
- AND/OR are associative & commutative, enabling prefix-style aggregation (like prefix sums, but for bits).
- Bits as an implicit set: an integer of width N can represent a subset of an N-element universe, where bit i = "element i is in the set." This single idea underlies Bitmask DP, subset enumeration, and state compression.

## 0.5 Why Bit Tricks Exist At All

Bitwise ops map to single CPU instructions (O(1), extremely fast, no branching), so bit manipulation is used to replace loops, conditionals, or arithmetic with equivalent-but-faster (or more memory-compact) operations. This motivates every pattern in Parts I–III: they are all "do X without doing X the naive way."

# Part I: True Algorithmic Patterns

Master these first. These are the foundational bit-math engines everything else is built from.

## 1. Single-Bit Manipulation (The Direct Access Engine)

**Usage:** Reading, setting, clearing, or toggling one specific bit position in a number.

**Giveaway:** The problem talks about "bit at position i," flags, or permission-style boolean states packed into an integer.

**Structural Variations:**

- **A. Get Bit:** `(x >> i) & 1` — shift target bit to LSB, mask off the rest.
- **B. Set Bit:** `x | (1 << i)` — OR in a 1 at position i, leaves others untouched.
- **C. Clear Bit:** `x & ~(1 << i)` — AND with everything-but-position-i.
- **D. Toggle Bit:** `x ^ (1 << i)` — XOR flips exactly that bit. (Single Number — LC 136, conceptually)

## 2. XOR Property Exploitation (The Cancellation Engine)

**Usage:** Finding a unique/missing/duplicated element by exploiting that XOR cancels identical values and is order-independent.

**Giveaway:** "Every element appears twice except one," "find the missing number," or "swap without a temp variable."

**Structural Variations:**

- **A. Single Unique Element (Full Cancellation):** XOR the whole array — duplicates cancel to 0, the unique value survives. (Single Number — LC 136)
- **B. Two Unique Elements (Partition by Differing Bit):** XOR everything to get a^b, isolate any set bit (`diff & -diff`) to split the array into two groups, one containing a, the other b. (Single Number III — LC 260)
- **C. Missing Number via Index-Value XOR:** XOR all array values with all indices 0..n; every present pair cancels, leaving the missing number. (Missing Number — LC 268)

## 3. Bit Counting (The Population Count Engine)

**Usage:** Counting how many bits are set (1s) in a number, or across a range of numbers.

**Giveaway:** "Number of 1 bits," "Hamming weight/distance," or a DP recurrence over consecutive integers' bit counts.

**Structural Variations:**

- **A. Brian Kernighan's Algorithm:** Repeatedly apply `x = x & (x-1)` to strip the lowest set bit; loop count = popcount. Runs in O(number of set bits) rather than O(bit width). (Number of 1 Bits — LC 191)
- **B. DP Popcount Propagation:** `dp[i] = dp[i >> 1] + (i & 1)` — reuse the popcount of a smaller, already-computed prefix instead of recounting from scratch for every integer 0..n. (Counting Bits — LC 338)
- **C. Hamming Distance:** XOR two numbers, then popcount the result — differing bits become 1s that XOR conveniently isolates. (Hamming Distance — LC 461)

## 4. Bit Shifting Arithmetic (The Power-of-2 Substitution Engine)

**Usage:** Replacing multiplication, division, or parity checks with shifts when the factor is a power of 2.

**Giveaway:** The problem involves powers of 2 explicitly, or asks for a "fast" version of arithmetic that's normally O(log n) or worse.

**Structural Variations:**

- **A. Power-of-Two Detection:** `x > 0 && (x & (x-1)) == 0` — a true power of 2 has exactly one set bit; clearing the lowest set bit yields 0. (Power of Two — LC 231)
- **B. Fast Exponentiation (Binary Exponentiation):** Decompose the exponent into its binary representation; square the base each iteration and multiply into the result only when the current bit is 1 — turns O(n) multiplication into O(log n). (Pow(x, n) — LC 50)

## 5. Bitmasking for Subsets (The Implicit Set Engine)

**Usage:** Representing membership of elements in a subset using bit positions, and enumerating or comparing subsets directly as integers.

**Giveaway:** Small N (subset enumeration threshold), or a problem talks about "combinations of flags," "subset of characters used," or overlaps with Bitmask DP from Part III of your DP list.

**Structural Variations:**

- **A. Full Subset Enumeration:** Loop mask from 0 to 2^n - 1; each mask's bits directly encode one subset — replaces recursive subset generation with a flat loop. (Subsets — LC 78)
- **B. Bitmask as Visited/Used Tracker:** Pass a mask through recursion/DP to represent "which elements have been used so far" without an array or hashset. (Overlaps with Bitmask DP — LC 526, Beautiful Arrangement)
- **C. Bitmask Character/Word Comparison:** Encode a string's character set as a 26-bit mask (`mask |= 1 << (c - 'a')`); disjoint character sets become instant via `maskA & maskB == 0`. (Maximum Product of Word Lengths — LC 318)

# Part II: Structural Topography Patterns

Master these second. Same engines from Part I, applied to non-obvious shapes and combined with other data structures.

## 6. Bitmask DP (State Compression as DP State)

**Usage:** DP where the "state" itself is a bitmask representing progress/membership, not just an index.

**Giveaway:** N ≤ ~20, and the DP transition needs to know exactly which subset of elements has been processed — this is the DP-flavored version of Pattern 5. (Shortest Path Visiting All Nodes — LC 847, Partition to K Equal Sum Subsets — LC 698)

**Structural Variations:**

- **A. Mask-Only State:** `dp[mask]` where the mask alone fully describes progress. (Matchsticks to Square — LC 473)
- **B. Mask + Auxiliary State:** `dp[mask][i]` where you also need "current position" alongside "which nodes visited" (TSP-style). (Shortest Path Visiting All Nodes — LC 847)

## 7. XOR in Data Structures (Structural XOR Topography)

**Usage:** Using XOR's cancellation property inside a non-array structure — linked lists, tries, prefix arrays — to compress or encode information.

**Giveaway:** The problem needs O(1) auxiliary space for a structure that "should" require pointers/extra storage in both directions, or needs "maximum XOR pair" style queries.

**Structural Variations:**

- **A. XOR Linked List (Memory-Efficient Doubly Linked List):** Each node stores `prev XOR next` instead of two pointers; traversal direction reconstructs the other pointer on the fly using the previous node's address. (Conceptual / systems pattern)
- **B. Prefix XOR Array:** Like prefix sums, but with XOR — `prefixXOR[i] = prefixXOR[i-1] ^ arr[i]`, enabling O(1) range-XOR queries via `prefixXOR[r] ^ prefixXOR[l-1]`. (XOR Queries of a Subarray — LC 1310)
- **C. Bitwise Trie for Maximum XOR (from your Tree map, Pattern 9D):** Insert numbers bit-by-bit (MSB→LSB) into a binary trie; for each query, greedily descend into the opposite bit at each level to maximize the XOR result. (Maximum XOR of Two Numbers in an Array — LC 421, Maximum XOR With an Element From Array — LC 1707)

## 8. Gray Code (Adjacent-Difference Topography)

**Usage:** Generating sequences where consecutive elements differ by exactly one bit — used for combinatorial generation, error-correction-friendly counting, and Hamiltonian-cycle-style enumeration over the hypercube of bit patterns.

**Giveaway:** "Generate all n-bit sequences such that consecutive ones differ by only one bit."

**Structural Variations:**

- **A. Direct Formula Construction:** `gray(i) = i ^ (i >> 1)` — a closed-form transform, no recursion needed. (Gray Code — LC 89)
- **B. Reflect-and-Prefix Construction:** Recursively build the sequence for n-1 bits, mirror it, and prefix the original half with 0 and the mirrored half with 1 — a structural/recursive alternative to the formula.

## 9. Bit Manipulation on Matrices/Grids (Row-as-Bitmask Topography)

**Usage:** Encoding an entire row or column of a small grid as a single integer bitmask, letting row-vs-row comparisons or transitions become O(1) bitwise ops instead of O(width) loops.

**Giveaway:** Grid width is small (≤ ~20), and the problem needs union/intersection logic between rows (compatibility, no-adjacent-conflict, coverage). This is Pattern 5/6 applied to grid rows instead of flat arrays.

**Structural Variations:**

- **A. Row Compatibility Masking:** Each row of a broken profile (e.g., "no two adjacent houses selected") is validated as a bitmask against a "no two adjacent 1-bits" filter (`mask & (mask << 1) == 0`) before being used as a DP transition. (Broken Profile / Tiling-style problems)
- **B. Row-Pair Intersection Check:** Compare adjacent-row masks via `&` to enforce or forbid vertical adjacency conflicts in one operation.

# Part III: State Storage & Performance Optimizations

Master these last. Advanced bit-level layout tricks used to compress storage or accelerate operations beyond naive implementations.

## 10. Bitset / Bit Array (Dense Boolean Storage Compression)

**Usage:** Storing a large array of booleans far more compactly (1 bit each instead of 1 byte/word each), and performing set operations across the whole array in O(N/64) instead of O(N) using word-level parallelism.

**Giveaway:** "Track which of N items are present," N is large, and set operations (union, intersection, count) are needed in bulk — replacing boolean[] or HashSet<Integer> with a packed integer array.

**Structural Variations:**

- **A. Fixed-Width Bitset (Manual Packing):** Use an int[]/long[] array where element i lives at `arr[i / 64]`, bit `i % 64` — manually replicate get/set/clear from Pattern 1 at scale. (Sieve of Eratosthenes optimization; Bitwise AND of Numbers Range-adjacent problems)
- **B. Word-Parallel Set Operations:** Combine two bitsets via `&`/`|`/`^` array-wise to compute intersection/union/symmetric-difference across millions of elements in one pass per word instead of one pass per element.

## 11. Bit Tricks for Range Queries (Interval-as-Bits Optimization)

**Usage:** Answering questions about a range of consecutive integers' bit properties without iterating each one individually.

**Giveaway:** "Bitwise AND/OR/XOR of all numbers in range [L, R]" — a naive loop is O(R-L), but the answer has an O(log(max)) closed form.

**Structural Variations:**

- **A. Bitwise AND of a Range (Common Prefix Extraction):** The AND of all numbers from L to R equals their shared binary prefix — right-shift both L and R together until they're equal, then shift back left, since any differing bit gets zeroed out by at least one number in the range. (Bitwise AND of Numbers Range — LC 201)
- **B. Digit-DP-Style Bit Range Counting (overlaps with Digit DP, DP Pattern 8):** Counting integers in [L, R] satisfying a bit-based property (e.g., popcount parity) by processing bit-by-bit with a tight/free flag, rather than iterating the whole range.

## 12. Bit-Level Hashing & Compression (Fingerprint Optimization)

**Usage:** Using bit patterns as compact, fast-to-compare fingerprints/signatures of larger data, trading a small false-positive/collision risk for massive space and speed savings.

**Giveaway:** "Approximate membership," "fast duplicate/similarity check," or "compress a large state into a fixed-size signature for fast comparison."

**Structural Variations:**

- **A. Bloom Filter (Multi-Hash Bit Array):** Hash an element with k independent hash functions, set the corresponding k bits in a bitset; membership check is "are all k bits set?" — allows false positives, never false negatives.
- **B. Rolling/XOR Hash for State Deduplication:** Encode a complex state (visited cells, used items) as a single integer signature (often via Pattern 5's bitmask, or XOR-combining hashed features) to detect previously-seen states in O(1) instead of deep equality checks.
