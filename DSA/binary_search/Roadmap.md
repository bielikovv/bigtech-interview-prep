# Binary Search Patterns — Full Map

# Part I: True Algorithmic Patterns

Master these first. These form the foundational mathematics of how a search space is proven monotonic and repeatedly halved.

## 1. Classic Search on Sorted Array (The Exact Match Engine)

**Usage:** Finding a specific target value's index within a sorted array.

**Giveaway:** The array is explicitly sorted and you need an exact index or "does it exist" answer.

**Structural Variations:**

- **A. Standard Binary Search (Left/Right Pointer Halving):** while lo <= hi, compare mid to target, discard the half that can't contain it. (Binary Search — LC 704)
- **B. Rotated Sorted Array Search:** One half of the array around mid is always still linearly sorted even after rotation; identify which half is sorted, then check if target lies in that half's range. (Search in Rotated Sorted Array — LC 33)
- **C. Search with Duplicates (Ambiguous Sorted Half):** Duplicates can make both halves look "equal" at the boundaries, forcing a linear shrink (lo++, hi--) as a fallback when nums[lo] == nums[mid] == nums[hi]. (Search in Rotated Sorted Array II — LC 81)

## 2. Boundary / First-and-Last Position Search (The Bisect Engine)

**Usage:** Finding the leftmost or rightmost index satisfying a condition, rather than any single match.

**Giveaway:** Language like "first," "last," "leftmost," "count of occurrences," or "insert position" on a sorted array.

**Structural Variations:**

- **A. Lower Bound (First True):** Custom comparator biases toward the left when nums[mid] == target, continuing to search left (hi = mid) instead of stopping. (Find First and Last Position of Element in Sorted Array — LC 34)
- **B. Upper Bound (Last True / Insert Position):** Mirror of lower bound, biasing right (lo = mid + 1) on ties to find the boundary just past the last match. (Search Insert Position — LC 35)

## 3. Binary Search on the Answer (The Monotonic Predicate Engine)

**Usage:** The array/input itself isn't what you search — instead you search a numeric range of possible answers, checking each candidate with a feasibility function.

**Giveaway:** The problem asks to "minimize the maximum" or "maximize the minimum" of something, and a brute-force check of one candidate answer is easy (usually O(N)), even though the input array may be unsorted.

**Structural Variations:**

- **A. Minimize-the-Max (Feasibility Check Trends "Easier" as Answer Grows):** Binary search over [low, high] candidate answers; a canAchieve(mid) helper returns true/false, and true results push the search left to find the smallest working value. (Split Array Largest Sum — LC 410, Capacity To Ship Packages Within D Days — LC 1011)
- **B. Maximize-the-Min (Feasibility Check Trends "Easier" as Answer Shrinks):** Same skeleton, but true results push the search right to find the largest working value. (Koko Eating Bananas — LC 875, Magnetic Force Between Two Balls — LC 1552)
- **C. Real-Valued / Floating-Point Search:** Same predicate structure, but lo/hi are doubles and the loop runs a fixed number of iterations (or until precision epsilon) instead of lo < hi, since there's no true "middle index." (Nth Magical Number-style precision search)

## 4. Search in Implicit / Unbounded Structures (The Boundless Search Engine)

**Usage:** The "array" has no defined size, or is only accessible through an API rather than direct indexing.

**Giveaway:** You're told the array size is unknown, or you're given a callback/API (isBadVersion, ArrayReader.get) instead of a literal array.

**Structural Variations:**

- **A. Exponential/Galloping Bound-Finding:** Repeatedly double a bound (1, 2, 4, 8...) until it overshoots the target or array end, then run standard binary search inside that bracket. (Search in a Sorted Array of Unknown Size)
- **B. API-Gated Monotonic Search:** The predicate itself is a black-box call (e.g., isBadVersion(mid)) that's guaranteed monotonic (false...false...true...true), letting you binary search purely on that guarantee. (First Bad Version — LC 278)

# Part II: Structural Topography Patterns

Master these second. These are not new mathematical engines; they are the patterns from Part I applied to non-linear or multi-dimensional shapes.

## 5. 2D Matrix Search (Grid-Flattening Topography)

**Usage:** Searching within a matrix that has row-wise and/or column-wise sorted order.

**Giveaway:** A 2D grid is given with an explicit sorted-order guarantee (either fully sorted as if flattened, or sorted per-row and per-column independently).

**Structural Variations:**

- **A. Fully Sorted Matrix (Flattened 1D Treatment):** If each row's last element is smaller than the next row's first, treat the matrix as one long virtual sorted array using mid // cols, mid % cols to convert a single binary search index into 2D coordinates. (Search a 2D Matrix — LC 74)
- **B. Row/Column Independently Sorted (Staircase Elimination):** Rows and columns are sorted independently but not globally, so no single flattening works — instead start at a corner (top-right) and eliminate one full row or column per comparison, an O(M+N) staircase walk rather than true O(log) binary search. (Search a 2D Matrix II — LC 240)

## 6. Peak Finding (Local-Optimum Topography)

**Usage:** Finding a local maximum/minimum in an array where global sortedness doesn't hold, but a directional "slope" always points toward a peak.

**Giveaway:** The array is explicitly not sorted, but is described as "bitonic" (rises then falls) or guaranteed to have a peak due to boundary conditions (nums[-1] = nums[n] = -infinity).

**Structural Variations:**

- **A. Single Peak in Bitonic Array:** Compare mid to mid+1; if ascending, the peak is to the right, else it's to the left or at mid — same halving logic as classic search but driven by slope instead of a target value. (Find Peak Element — LC 162)
- **B. Peak in Mountain Array via API (Bitonic + Black-Box Access):** Same slope-comparison logic, but wrapped behind an API with limited calls, forcing you to minimize redundant lookups. (Find in Mountain Array — LC 1095)

## 7. Median / Order-Statistic Search Across Multiple Arrays (Partition Topography) (ADVANCED)

**Usage:** Finding the k-th smallest element or median across two (or more) separately sorted arrays without merging them.

**Giveaway:** You're given two (or more) sorted arrays and asked for a combined median or k-th element, with an implicit or explicit O(log(min(M,N))) time requirement.

**Structural Variations:**

- **A. Binary Search on Partition Point (Two Arrays):** Binary search over the smaller array's partition index; the corresponding partition in the other array is derived algebraically, and you check that maxLeft <= minRight on both sides simultaneously. (Median of Two Sorted Arrays — LC 4)
- **B. K-th Smallest via Elimination (Generalized Partition):** Generalizes the above to find the k-th element (not just the median) by discarding k/2 elements from whichever array's k/2-th element is smaller, each round — a repeated halving of the combined search space rather than a single array.

# Part III: State Storage & Performance Optimizations

Master these last. These are advanced wrappers that combine binary search with auxiliary data structures or precomputation to accelerate queries.

## 8. Binary Search + Prefix Sum / Precomputation (Transformed-Space Search)

**Usage:** Answering range or threshold queries by first transforming the array (via prefix sums, sorting, or coordinate compression) into a monotonic space, then binary searching that transformed space.

**Giveaway:** The raw array isn't sorted or monotonic, but a derived array (running sum, sorted copy, frequency count) is — and the question asks "how many," "at least," or "smallest window achieving X."

**Structural Variations:**

- **A. Prefix Sum + Binary Search (Monotonic Derived Array):** Build a prefix sum array (valid only when all values are non-negative, guaranteeing monotonicity), then binary search it for the smallest window/index hitting a sum threshold. (Minimum Size Subarray Sum — LC 209, when using the binary search approach instead of sliding window)
- **B. Coordinate Compression + Binary Search:** Sort and deduplicate values into a rank array, then use binary search purely to map arbitrary values to compressed indices in O(log N), enabling other structures (Fenwick/segment trees) to operate on small dense ranges instead of raw large values. (Count of Smaller Numbers After Self — LC 315)

## 9. Patience Sorting / LIS via Binary Search (Tails-Array Acceleration)

**Usage:** Maintaining a dynamically updated "best tails so far" array and using binary search to find where a new element should overwrite or extend it, turning an O(N²) DP into O(N log N).

**Giveaway:** This is the binary-search-flavored twin of DP pattern 9 (LIS) — any time you'd normally loop back through all previous DP states, a sorted "tails" array plus binary search replaces that inner loop.

**Structural Variations:**

- **A. Tails-Array Overwrite (LIS Acceleration):** Binary search the tails array for the first element >= current, overwrite it (or append if none found); final tails length is the LIS length, though the array itself isn't the actual subsequence. (Longest Increasing Subsequence — LC 300, O(N log N) approach)
- **B. Patience Sorting for Envelopes/Piles (2D Nesting + Tails Array):** After the same sort-ascending/sort-descending-tiebreak trick used in DP's Russian Doll pattern, run the exact same tails-array binary search on the second dimension only. (Russian Doll Envelopes — LC 354, O(N log N) approach)

## 10. Binary Search on Trees / BST-Adjacent Search (Structural Halving)

**Usage:** Exploiting an ordering invariant embedded in a tree or heap-like structure to prune half the remaining space at each step, rather than an array.

**Giveaway:** The structure is a BST (or BST-like), and the question is a "closest value," "k-th smallest," or "count less than X" query that could be brute-forced via full traversal but has an O(log N)-average shortcut via the ordering property.

**Structural Variations:**

- **A. Closest Value via Directed Descent:** At each node, compare target to node value, keep the closer candidate, and descend only left or right — never both, unlike a full inorder traversal. (Closest Binary Search Tree Value — LC 270)
- **B. Augmented BST with Subtree-Size Counts (Order-Statistics Tree):** Store subtree sizes at each node so "find k-th smallest" or "count elements less than X" can binary-search down the tree in O(log N) instead of an O(N) inorder walk — the tree equivalent of a Fenwick tree's rank query. (Kth Smallest Element in a BST — LC 230, optimized/augmented version)
