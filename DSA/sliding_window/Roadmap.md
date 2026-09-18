# Part I: True Algorithmic Patterns

Master these first. They form the foundational mathematics of how a window expands and contracts across a linear sequence.

## 1. Fixed-Size Window (The Rigid Frame Engine)

**Usage:** The window size KK K is given explicitly and never changes. You slide it one step at a time across the array.

**Giveaway:** The problem states an exact subarray/substring length KK K upfront ("of size k").

**Structural Variations:**

- **A. Running Aggregate (Sum/Average):** Add the incoming element, subtract the outgoing element as the window shifts by exactly one — never recompute from scratch. (Maximum Average Subarray I — LC 643)
- **B. Running Frequency/State Map (Fixed Anagram Check):** Maintain a character/element frequency count of size KK K; compare against a target frequency map each shift instead of resorting. (Find All Anagrams in a String — LC 438, Permutation in String — LC 567)
- **C. Fixed-Window Extremum (Deque-Backed):** Track the max/min inside every fixed window using a monotonic deque instead of rescanning KK K elements each time. (Sliding Window Maximum — LC 239)

## 2. Variable-Size Window — Shrinkable (The Longest-Valid Engine)

**Usage:** The window grows by moving right, and shrinks from the left only when a constraint is violated, always trying to maximize length.

**Giveaway:** Language like "longest substring/subarray such that [condition]" — you want the biggest window that still satisfies a rule.

**Structural Variations:**

- **A. Uniqueness Constraint (No Repeats):** Shrink the left edge until a duplicate (tracked via a set/hashmap of last-seen index) is removed. (Longest Substring Without Repeating Characters — LC 3)
- **B. Budget/Threshold Constraint (Numeric Cap):** Shrink while a running numeric quantity (product, sum, distinct-count budget) exceeds an allowed limit. (Longest Substring with At Most K Distinct Characters — LC 340, Max Consecutive Ones III — LC 1004)
- **C. Replacement/Tolerance Constraint (Allowed Violations):** Shrink only when violations exceed an explicit tolerance count KK K, tracked via the gap between window length and the dominant/majority element's frequency. (Longest Repeating Character Replacement — LC 424)

## 3. Variable-Size Window — Growable-on-Deficit (The Shortest-Valid Engine)

**Usage:** The window grows by moving right until a condition is first satisfied, then shrinks from the left as far as possible while the condition still holds, always trying to minimize length.

**Giveaway:** Language like "smallest/minimum window/substring/subarray such that [condition] is met" — you want the smallest window that still satisfies a rule.

**Structural Variations:**

- **A. Coverage Constraint (Must-Contain-All):** Track a "required" frequency map and a "satisfied count"; shrink left greedily the instant full coverage is hit, recording the minimum each time. (Minimum Window Substring — LC 76)
- **B. Numeric-Threshold Constraint (Sum ≥ Target):** Shrink left the instant the running sum meets or exceeds a target, no frequency map needed — pure numeric comparison. (Minimum Size Subarray Sum — LC 209)

# Part II: Structural Topography Patterns

Master these second. These are not new mathematical engines; they are the Part I patterns applied to non-obvious shapes or combined with an auxiliary structure.

## 4. Two-Pointer Partition Window (Same-Direction Collapse)

**Usage:** Both pointers move strictly forward (never backward), partitioning the array in place rather than tracking length/sum — the "window" is really a boundary between processed and unprocessed regions.

**Giveaway:** "Remove duplicates in place," "move zeroes," "partition array around a pivot" — an in-place rearrangement rather than a reported window size.

**Structural Variations:**

- **A. Write-Pointer Compaction (Slow/Fast Pointers):** Fast pointer scans every element; slow pointer marks where the next "kept" element should be written. (Remove Duplicates from Sorted Array — LC 26, Move Zeroes — LC 283)
- **B. Dutch National Flag (Three-Way Partition):** Extend to three pointers (low, mid, high) to partition into three buckets in a single pass. (Sort Colors — LC 75)

## 5. Opposite-Direction Two Pointers (Converging Window)

**Usage:** One pointer starts at each end of a sorted or symmetric structure and moves inward — technically a "window" spanning the whole remaining range that shrinks from both sides based on a comparison.

**Giveaway:** The input is sorted (or can be treated as such), and the problem asks for a pair/triplet meeting a target, or a container/area maximization.

**Structural Variations:**

- **A. Target-Sum Convergence (Sorted Pair Sum):** Move the left pointer right if the sum is too small, right pointer left if too large. (Two Sum II - Input Array Is Sorted — LC 167, 3Sum — LC 15)
- **B. Area/Capacity Maximization (Greedy Discard):** At each step, discard whichever boundary is the limiting (shorter) factor, since keeping it can never produce a better answer. (Container With Most Water — LC 11, Trapping Rain Water — LC 42)

## 6. Multi-Window / Comparative Window (Parallel Frame Topography)

**Usage:** Two or more windows (often over two different arrays/strings) are compared or synchronized simultaneously rather than one window sliding over one sequence.

**Giveaway:** Two separate strings/arrays are given, and you're matching a moving window in one against a fixed or moving pattern in the other.

**Structural Variations:**

- **A. Pattern-Matching Window (Fixed Target, Sliding Source):** Same mechanics as Fixed-Size Frequency Matching (1B), but framed explicitly as matching one string's window against a second string's full frequency profile. (Permutation in String — LC 567)
- **B. Merge-Interval-Style Dual Window:** Two independent pointers/windows advance across two separate sorted arrays simultaneously, intersecting or merging ranges as they go — window logic applied across two sequences instead of within one. (Interval List Intersections — LC 986)

# Part III: State Storage & Performance Optimizations

Master these last. Advanced data-layout wrappers used to accelerate window extremum/state tracking beyond brute force.

## 7. Monotonic Deque Window (Amortized Extremum Compression)

**Usage:** Repeatedly querying the max/min of a sliding window in better than O(K)O(K) O(K) per step.

**Giveaway:** The problem needs the max or min inside every window as it slides, not just a single global max/min.

**Structural Variations:**

- **A. Monotonic Decreasing Deque (Max Tracking):** Pop smaller trailing elements before pushing; front of deque is always the current max. (Sliding Window Maximum — LC 239)
- **B. Monotonic Deque for Bounded-Difference Windows:** Maintain both a max-deque and a min-deque simultaneously; shrink the window from the left whenever max−min exceeds a limit. (Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit — LC 1438)

## 8. Prefix-Sum + Hashmap Window (Non-Contiguous-Boundary Compression)

**Usage:** Finding subarrays matching a sum/property where the classic shrink-from-left two-pointer breaks down (e.g., negative numbers present), so you compress "window boundaries" into prefix-sum lookups instead.

**Giveaway:** The array contains negative numbers or the target involves modular/XOR equality — anything where "shrinking reduces the sum" is no longer guaranteed true, invalidating pure two-pointer logic.

**Structural Variations:**

- **A. Exact-Sum Subarray Count (Prefix Sum Frequency Map):** Store counts of prefix sums seen so far; for each new prefix sum PP P, look up P−targetP - target P−target in the map. (Subarray Sum Equals K — LC 560)
- **B. Modular/Parity Window (Remainder Bucketing):** Bucket prefix sums by remainder (mod K) or parity instead of raw value, turning a sliding-window-shaped problem into a hashmap lookup problem. (Continuous Subarray Sum — LC 523, Contiguous Array — LC 525)

## 9. Sliding Window + Ordered Structure (Order-Statistics Window)

**Usage:** Needing the median, or the k-th smallest/largest element, or bounded rank queries inside a moving window — beyond what a simple deque or hashmap can answer.

**Giveaway:** "Sliding window median," or a window constraint involving relative rank/order rather than sum or count.

**Structural Variations:**

- **A. Dual-Heap Window (Lazy Deletion):** Maintain a max-heap for the lower half and min-heap for the upper half, using lazy deletion (marking stale entries) since heaps can't remove arbitrary elements efficiently. (Sliding Window Median — LC 480)
- **B. Balanced BST / Ordered-Multiset Window:** Use a self-balancing tree structure (e.g., TreeMap/SortedList) to support O(log⁡K)O(\log K) O(logK) insert, delete, and rank queries as the window slides. (Sliding Window Maximum — LC 239 alternate solution, Sliding Window Median — LC 480 alternate solution)
