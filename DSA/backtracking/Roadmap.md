# Backtracking Patterns — Full Map

# Part I: True Algorithmic Patterns

Master these first. They form the foundational recursion mathematics of how the search space is built and pruned.

## 1. Subset / Combination Generation (The Include-Exclude Engine)

**Usage:** Choosing which elements from a set to keep, where order doesn't matter and each element is a binary in/out decision.

**Giveaway:** The problem asks for "all subsets," "all combinations," or "all subsets summing to target," and duplicates in the choice order don't matter.

**Structural Variations:**

- **A. Pure Subsets (Binary Include/Exclude):** At each index, branch into "take it" and "skip it." (Subsets — LC 78)
- **B. Subsets with Duplicate Elements (Sorted Skip):** Sort first, then skip over adjacent duplicates at the same recursion depth to avoid generating the same subset twice. (Subsets II — LC 90)
- **C. Combination Sum (Bounded Target, Unbounded Reuse):** Pick from a start index onward; allow reusing the same index again (unbounded) or moving strictly forward (bounded), pruning once running sum exceeds target. (Combination Sum — LC 39, Combination Sum II — LC 40)
- **D. Fixed-Size Combinations (Choose K of N):** Same include/exclude skeleton, but pruned early when remaining elements can't fill the remaining slots needed. (Combinations — LC 77)

## 2. Permutation Generation (The Ordered Arrangement Engine)

**Usage:** Arranging all elements where order matters — every distinct ordering counts as a separate answer.

**Giveaway:** The problem asks for "all permutations," "all arrangements," or "all orderings," explicitly caring about sequence, not just membership.

**Structural Variations:**

- **A. Standard Permutations (Visited Array/Set):** Track a boolean "used" array; at each level, try every unused element as the next slot. (Permutations — LC 46)
- **B. Permutations with Duplicates (Sorted Skip + Used Guard):** Sort first, then skip a duplicate value at the same recursion depth unless its identical predecessor was already used in this branch. (Permutations II — LC 47)
- **C. In-Place Swap Permutations (Index Swapping):** Instead of a used array, swap the current index with each candidate to its right, recurse, then swap back — avoids extra memory for tracking usage.

## 3. Constraint Satisfaction / Grid Placement (The Board Validity Engine)

**Usage:** Placing pieces or values onto a grid/board one cell at a time, where each placement must satisfy row/column/region constraints before continuing.

**Giveaway:** The problem describes a board, grid, or fixed layout (chessboard, sudoku grid) with explicit placement rules that must never be violated.

**Structural Variations:**

- **A. Row-by-Row Placement (N-Queens Style):** Process one row (or column) at a time, trying every valid column position and recursing to the next row only if the placement is currently safe. (N-Queens — LC 51)
- **B. Cell-by-Cell Fill with Multi-Constraint Check (Sudoku Style):** Scan for the next empty cell, try every candidate value that satisfies row+column+box constraints simultaneously, and backtrack on total failure. (Sudoku Solver — LC 37)

## 4. Partitioning (The Segment Division Engine)

**Usage:** Splitting a sequence into contiguous pieces where every piece must individually satisfy some property.

**Giveaway:** The problem asks to "partition," "split," or "divide" a string/array into pieces, and each valid full split is a separate answer.

**Structural Variations:**

- **A. Property-Gated Partitioning (Palindrome Cut):** At each start index, try every possible end index for the next piece; only recurse into the remainder if the current piece satisfies the property (e.g., is a palindrome). (Palindrome Partitioning — LC 131)
- **B. Structural-Constraint Partitioning (Fixed Segment Count/Format):** Partition under an explicit structural rule (e.g., exactly 4 segments, each 0-255) rather than a content property. (Restore IP Addresses — LC 93)

## 5. Path Search on Implicit Grid (The Directional Exploration Engine)

**Usage:** Searching for a path through a grid where you move step-by-step in allowed directions, marking cells as visited during the current path and unmarking on backtrack.

**Giveaway:** The problem gives a 2D grid/board and asks if a target word/path/sequence can be traced through adjacent cells without reusing a cell in the same path.

**Structural Variations:**

- **A. Word/Path Existence (Mark-Recurse-Unmark):** Temporarily mark the current cell as visited (often by mutating it in place), recurse into 4 directions, then restore the cell before returning — the canonical backtracking "undo" step. (Word Search — LC 79)
- **B. Multi-Directional Exhaustive Path Count (Count All Paths):** Same mark/unmark mechanism, but instead of returning on first success, accumulate a count across every valid complete path. (Unique Paths III — LC 980)

# Part II: Structural Topography Patterns

Master these second. These are not new mathematical engines; they are the Part I patterns applied to non-obvious shapes or layered with extra bookkeeping.

## 6. Graph Coloring / Assignment (Mutual Exclusion Topography)

**Usage:** Assigning labels/colors/groups to nodes such that connected or conflicting nodes never share the same assignment.

**Giveaway:** "Color the graph with K colors," "assign groups such that no two connected items match" — this is subset/permutation logic applied to a conflict graph instead of a flat array.

**Structural Variations:**

- **A. K-Coloring Feasibility:** Try each color for the current node, check against all already-colored neighbors, recurse, backtrack on dead end.

## 7. Expression / Formula Construction (Operator Insertion Topography)

**Usage:** Inserting operators, parentheses, or symbols between fixed elements to reach a target result, exploring every combination of insertion points.

**Giveaway:** The problem gives a fixed string/array of digits and asks "insert operators to reach target" or "how many ways to add parentheses."

**Structural Variations:**

- **A. Operator Slot Filling (Digit String + Target):** At each position, try every operator choice, recursing forward with an updated running value and a tracked "last operand" for correct precedence handling on backtrack. (Expression Add Operators — LC 282)
- **B. Parenthesization Enumeration (Divide at Every Operator):** Recursively split the expression at every operator, combine left and right results, and let the split points themselves define the structural branching instead of operator choice. (Different Ways to Add Parentheses — LC 241)

## 8. Combinatorial Game / Move Simulation (State Mutation Topography)

**Usage:** Simulating a sequence of moves in a puzzle or game where each move mutates shared state, requiring exact reversal to explore other branches.

**Giveaway:** The problem describes a game, puzzle, or physical board (like tic-tac-toe move validation, matchstick arrangement) where a "move" changes state that must be fully undone before trying the next move.

**Structural Variations:**

- **A. Full State Mutation + Manual Undo (Matchsticks/Board Games):** Apply a move directly to shared arrays/counters, recurse, then manually reverse every field that was changed — distinct from grid path search because multiple pieces of state (not just one cell) must be restored. (Matchsticks to Square — LC 473)

## 9. Subsequence / String Building with Global Constraints (Character-Level Topography)

**Usage:** Building strings character-by-character under structural rules (balance, matching counts) rather than a simple property check per full piece.

**Giveaway:** "Generate all valid parentheses combinations," or any problem building a string where legality must be checked incrementally at every character addition, not just at the end.

**Structural Variations:**

- **A. Balanced Construction (Running Counter Pruning):** Track open/close counts as running parameters; only recurse into "add close" if it wouldn't exceed "add open" count so far — prunes invalid branches before they're ever fully built. (Generate Parentheses — LC 22)

# Part III: State Storage & Performance Optimizations

Master these last. These are advanced pruning and acceleration wrappers layered on top of the engines above — they don't change the shape of the recursion, they make it survive larger inputs.

## 10. Pruning via Sorting + Bounds (Early Termination Optimization)

**Usage:** Cutting off entire branches early by exploiting sorted order or cumulative bounds, rather than exploring every leaf.

**Giveaway:** Same backtracking skeleton as Part I, but input size constraints are noticeably higher, signaling brute-force-only recursion will TLE without aggressive cuts.

**Structural Variations:**

- **A. Sorted-Array Sum Pruning:** Sort input first; if the current partial sum already exceeds target (ascending order) or the smallest remaining element can't possibly help, break out of the loop immediately instead of just skipping. (Combination Sum family)
- **B. Duplicate-Branch Elimination via Sorting:** Sorting plus an "only skip if same value as previous sibling at this depth" check, turning an exponential duplicate blowup into a clean unique-result generator (shared mechanism across Subsets II, Permutations II, Combination Sum II).

## 11. Memoized Backtracking (Overlapping Subproblem Bridge)

**Usage:** When a backtracking search revisits identical states repeatedly (not just structurally similar ones), caching results turns exponential search into polynomial — the direct bridge between Backtracking and DP.

**Giveaway:** The recursive state (e.g., remaining string, remaining index, remaining mask) repeats across different call paths, and the problem only needs a count/boolean/optimal value rather than every explicit path enumerated.

**Structural Variations:**

- **A. Boolean/Count Memoization (Collapse Enumeration into DP):** Same recursive structure as Partitioning or Combination Sum, but cache on the recursive state to answer "is it possible" or "how many ways" without regenerating every explicit result. (Word Break — LC 139, Partition to K Equal Sum Subsets — LC 698)
- **B. Bitmask-Backed Backtracking (State Compression Bridge):** Represent "which elements used so far" as a bitmask instead of a visited array/set, enabling both faster state checks and direct memoization on the mask — the explicit link to Bitmask DP. (Partition to K Equal Sum Subsets — LC 698, Shortest Superstring-style problems)

## 12. Branch and Bound (Best-Value Pruning Optimization)

**Usage:** When the goal is an optimal value (min/max) rather than all valid results, maintain a running best-so-far and prune any branch that provably cannot beat it.

**Giveaway:** The problem asks for the "minimum/maximum" achievable value via exhaustive choice-making, and input size is too large for plain unpruned backtracking but too irregular for standard DP.

**Structural Variations:**

- **A. Running Bound Comparison:** Before recursing deeper, compare the current partial cost/value against the best complete answer found so far; abandon the branch immediately if it already can't win, even before reaching a full leaf.
