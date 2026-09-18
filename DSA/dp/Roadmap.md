# Part I: True Algorithmic Patterns

Master these first. They form the foundational mathematics of how problems are broken down into independent subproblems.

## 1. Knapsack DP (The Bounded Choice Engine)

**Usage:** You are given a pool of distinct items. Each item has a cost (weight) and a reward (value). You have a strict limit (capacity).

**Giveaway:** You are making choices against a hard capacity constraint you cannot cross.

**Structural Variations:**

- **A. 0/1 Knapsack:** Exactly one copy of each item type available. Loop capacities backwards in 1D array optimization to protect yesterday's data. (Partition Equal Subset Sum - LC 416)
- **B. Unbounded Knapsack:** Infinite copies of each item type available. Loop capacities forwards in 1D optimization to allow immediate item reuse. (Coin Change - LC 322)
- **C. Bounded Knapsack:** Each item type has a strict maximum quantity limit $K$. Solved by either introducing a 3rd nested loop checking $0 \dots K$, or by binary grouping (splitting quantity $K$ into powers of 2 like $1, 2, 4, 8\dots$) to turn it back into a standard 0/1 Knapsack problem.
- **D. Multi-Dimensional Constraints:** Fitting selections into multiple independent capacity limits simultaneously (e.g., both weight limits and volume limits). (Ones and Zeros - LC 474)

## 2. State Machine / Decision DP (The Linear Transition Engine)

**Usage:** Moving linearly through a sequence, where your choices today depend on a specific "state" you locked yourself into yesterday.

**Giveaway:** The problem introduces operational rules like "you cannot rob adjacent houses" or "after you sell, you must wait 1 day (cooldown)."

**Structural Variations:**

- **A. History-Based Lookbacks (Alternating Track):** Binary decisions where your choice today simply looks back $K$ steps behind you to verify legality. (House Robber - LC 198)
- **B. Explicit State Diagrams (Parallel Tracks):** Multiple continuous operational states governed by strict transition laws. Requires maintaining multiple parallel tracking variables representing your max value at that exact state. (Best Time to Buy and Sell Stock with Cooldown - LC 309)

## 3. Two-String Linear DP (The Coordinate Alignment Engine)

**Usage:** Comparing two completely distinct strings or sequences to calculate a longest common subsequence, alignment score, or minimum transformation cost.

**Giveaway:** The input explicitly provides two distinct strings or sequences, and you are shifting indices through both simultaneously.

**Structural Variations:**

- **A. Common Element Tracking (LCS):** Finding characters that match without changing them. Mismatches just inherit the max of left or up. (Longest Common Subsequence - LC 1143)
- **B. Alignment & Transformation (Edit Distance):** Evaluating explicit modification penalties (Insert, Delete, Replace). Mismatches require evaluating a minimum of three lookback states: left, up, and diagonal. (Edit Distance - LC 72)

## 4. Interval DP (The Range Collapse Engine)

**Usage:** Subproblems are ranges [i...j] that shrink inward or collapse together when an element is destroyed, making the remaining elements new neighbors.

**Giveaway:** The problem asks for a min/max score where the reward depends on elements adjacent to your action, and $N \le 500$ (signaling an $O(N^3)$ matrix fill).

**Structural Variations:**

- **A. The "Last Action" Element (Standard Collapse):** You choose which element is handled last in the current interval, cleanly isolating the left and right subproblems. (Burst Balloons - LC 312)
- **B. Range Merging (Linear Consolidation):** Combining adjacent elements step-by-step into a single element, where the cost of the final merge depends on the entire interval sum. (Minimum Cost to Merge Stones - LC 1000)

# Part II: Structural Topography Patterns

Master these second. These are not new mathematical engines; they are simply the patterns from Part I applied to non-linear physical structures.

## 5. Palindrome DP (Symmetric Range Topography)

**Usage:** Problems asking you to identify, count, or optimally slice substrings based on whether they read the same backwards and forwards.

**Giveaway:** Subproblem boundaries natively expand from the center outwards or collapse from the outer edges inwards. (This is a highly specialized variation of Interval DP).

**Structural Variations:**

- **A. Matrix Property Verification (Boolean Grid):** Building a 2D boolean lookup table where dp[i][j] checks if s[i] == s[j] and looks down-left at dp[i+1][j-1]. (Longest Palindromic Substring - LC 5)
- **B. Partition Slicing (Hybrid Linear-Palindrome):** Using a pre-computed palindrome boolean matrix as a baseline lookup, then running a 1D linear decision loop over the string to calculate the minimum cuts needed to split it. (Palindrome Partitioning II - LC 132)

## 6. Tree & Graph DP (Hierarchical Topography)

**Usage:** Calculating an optimal path, node selection, or sum across a non-linear hierarchical structure (a tree or DAG).

**Giveaway:** The input gives you a tree root or an edge list, and choices at a parent node depend entirely on its children.

**Structural Variations:**

- **A. Vertex Selection (Include/Exclude):** Choosing a parent node locks or unlocks the availability of its children. Each node returns a strict state pair: (max_if_included, max_if_excluded). (House Robber III - LC 337)
- **B. Global Crossroads Maximization (Path Tracking):** Finding optimal continuous pathways that pass through nodes without breaking links. The recursive function returns the maximum single linear branch to its parent, but actively computes a global crossroads maximum (left_branch + right_branch + parent_value) to update a global tracking pointer. (Binary Tree Maximum Path Sum - LC 124)

# Part III: State Storage & Performance Optimizations

Master these last. These are advanced data layout wrappers used to compress tracking histories or accelerate slow transitions.

## 7. Bitmask DP (State Set Compression)

**Usage:** Finding an optimal permutation, grouping, or subset assignment where you must strictly track exactly which specific items have already been "visited" or "used."

**Giveaway:** The input size constraint is explicitly, suspiciously tiny—usually $N \le 15$ or $N \le 20$.

**Structural Variations:**

- **A. Hamilton Path / TSP (Permutation):** Finding the optimal order to visit nodes. State tracks the mask and your current position. (Shortest Path Visiting All Nodes - LC 847)
- **B. Subset Partitioning (Grouping):** Splitting a set into $K$ equal-sum groups. State tracks just the mask, and you match it against a target bucket capacity. (Matchsticks to Square - LC 473)

## 8. Digit DP (Astronomical Boundary Optimization)

**Usage:** Counting how many integers within a massive range $[L, R]$ satisfy a specific numerical property.

**Giveaway:** The upper limit $R$ is astronomical ($10^{18}$), meaning linear loops will instantly TLE.

**Structural Variations:**

- **A. Bounded Prefix Constraints (Standard):** Processing digits from left to right while tracking a tight flag to ensure you don't exceed the upper limit $R$. (Count of Integers - LC 2719)
- **B. Leading Zero Management:** A variation where leading zeros distort properties like digit frequency or balance (e.g., 005 shouldn't be counted as containing two zeros). Adds a mandatory is_leading_zero boolean flag. (Number of Digit One - LC 233)

## 9. Longest Increasing Subsequence (Binary Search Acceleration)

**Usage:** Finding the longest sequence of items that follow a strict sorting or nesting rule, where elements do not have to be continuous.

**Giveaway:** The input is either a 1D array or a list of 2D pairs [[w, h], [w, h]] where items cannot overlap or cross in space.

**Structural Variations:**

- **A. Pure 1D Sequence ($O(N \log N)$):** Using Binary Search (bisect) to actively overwrite a running tails array to maintain optimal lower thresholds. (Longest Increasing Subsequence - LC 300)
- **B. Coordinated Co-dependent Sorting (2D/3D Nesting):** Sorting the first dimension ascending, and the second dimension descending as a tie-breaker. This prevents items with identical widths from nesting inside each other, reducing a 2D constraint into a pure 1D LIS problem. (Russian Doll Envelopes - LC 354)

## 10. Matrix / 2D-to-1D Area DP (Geometric Monotonic Integration)

**Usage:** Searching for geometric shapes, optimal areas, or counts of submatrices within a rigid 2D grid layout.

**Giveaway:** The problem forces a rigid 2D geometric constraint across grid elements (e.g., "find the largest square or rectangle of all 1s").

**Structural Variations:**

- **A. Square-Based Evolution (Local Grid Transitions):** Checking symmetric square formations by looking directly at your immediate local neighbors (top, left, and top-left). (Maximal Square - LC 221)
- **B. Non-Symmetric Rectangles (Histogram Reduction):** Condensing the rows of a 2D grid into a 1D array of histogram heights, then running a Monotonic Stack optimization to find uneven rectangular bounds. (Maximal Rectangle - LC 85)
