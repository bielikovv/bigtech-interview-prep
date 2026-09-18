# Greedy Patterns — Full Map

# Part I: True Algorithmic Patterns

Master these first. These are the foundational mathematics of how a locally optimal choice, made once and never revisited, produces a globally optimal result.

## 1. Interval Scheduling (The Non-Overlapping Selection Engine)

**Usage:** You have a set of intervals (start, end) and must select the maximum number that don't overlap, or merge/remove the minimum to eliminate overlap.

**Giveaway:** The problem gives you intervals and asks "maximum non-overlapping," "minimum to remove," or "can you attend all."

**Structural Variations:**

- **A. Maximum Non-Overlapping Count:** Sort by end time; greedily take an interval if its start is ≥ the last taken interval's end. Sorting by end (not start) is the critical proof point — it always leaves the most room for future intervals. (Non-overlapping Intervals — LC 435)
- **B. Merge Overlapping Intervals:** Sort by start time; walk through and merge whenever the current start ≤ the running end. (Merge Intervals — LC 56)
- **C. Minimum Arrows / Point Covering:** Sort by end time; count a new "arrow" only when the current interval's start exceeds the last arrow's position. (Minimum Number of Arrows to Burst Balloons — LC 452)

## 2. Activity/Resource Allocation (The Capacity-Matching Engine)

**Usage:** Matching a stream of requests against limited resources (rooms, platforms, people) where you must decide allocation on the fly or count simultaneous demand.

**Giveaway:** "Minimum meeting rooms," "minimum platforms," or "can a person attend every meeting" — same interval data as above, but now asking about concurrent capacity instead of selection count.

**Structural Variations:**

- **A. Two-Pointer Sweep on Sorted Starts/Ends:** Sort start times and end times separately; walk both pointers, incrementing a room counter when a meeting starts before the earliest one ends. (Meeting Rooms II — LC 253)
- **B. Chronological Event Sweep (Diff Array / Line Sweep):** Treat every start as +1 and every end as -1 on a timeline, sort all events, and track the running sum's peak to find max concurrency. (Car Pooling — LC 1094)

## 3. Exchange Argument Sorting (The Pairwise Swap Proof Engine)

**Usage:** You must order or pair up elements to minimize/maximize a sum or cost, where the optimal order can be proven by showing any adjacent swap of a "wrong" order never helps.

**Giveaway:** The problem hides a mathematical inequality — reordering two adjacent elements changes the total in a provable direction (e.g., "minimize sum of waiting times," "maximize sum of products").

**Structural Variations:**

- **A. Simple Ascending/Descending Sort:** Sort by a single derived key (e.g., ratio, difference, or raw value) so that the greedy pairing falls out directly. (Boats to Save People — LC 881)
- **B. Custom Comparator Sort (Concatenation/Ratio Trick):** Sort using a non-trivial pairwise comparator (e.g., compare a+b vs b+a as strings) because no single numeric key captures the right order. (Largest Number — LC 179)

## 4. Jump / Reachability Greedy (The Frontier Extension Engine)

**Usage:** Moving through an array where each position grants a "reach" and you must determine reachability, minimum jumps, or furthest extent.

**Giveaway:** "Can you reach the last index," "minimum jumps to reach the end," array of reach/power values at each position.

**Structural Variations:**

- **A. Reachability Check (Running Max Frontier):** Walk left to right, tracking the furthest index reachable so far; fail the instant your current index exceeds that frontier. (Jump Game — LC 55)
- **B. Minimum Jumps (Level-by-Level Frontier, BFS-flavored Greedy):** Track current frontier and next frontier; increment jump count each time you exhaust the current frontier, without explicit BFS queue overhead. (Jump Game II — LC 45)

## 5. Prefix Feasibility / Running Deficit (The Running Balance Engine)

**Usage:** Validating a sequence where a running total must never go negative (or must satisfy a threshold), and a single rotation point or reset determines feasibility.

**Giveaway:** "Gas station," "can you complete the circuit," "minimum starting value," or "candy distribution" — any problem where a running sum's sign or trend at each step drives the decision.

**Structural Variations:**

- **A. Total-Sum Feasibility Shortcut:** If total gain ≥ total cost, a valid single starting point is guaranteed to exist; find it by resetting the candidate start whenever the running sum goes negative. (Gas Station — LC 134)
- **B. Two-Pass Local Comparison (Neighbor-Constrained Greedy):** Enforce a local ordering constraint (e.g., "higher rating gets more candy than neighbor") with one forward pass and one backward pass, taking the max of both at each index. (Candy — LC 135)

# Part II: Structural Topography Patterns

Master these second. These are not new mathematical engines; they are the patterns from Part I applied to non-obvious data shapes, or combined with a supporting data structure to make the greedy choice efficient.

## 6. Heap-Assisted Greedy (The "Always Pick Extreme" Engine)

**Usage:** At every step you must pick the current minimum/maximum from a dynamic pool of candidates, where the pool changes as you make choices.

**Giveaway:** "Always combine the two smallest," "always take the task with the highest priority available right now," or "reorganize so no two adjacent are the same" — a heap is doing the sorting work incrementally instead of upfront.

**Structural Variations:**

- **A. Repeated Extreme Merge (Priority Queue Reduction):** Repeatedly pop the two smallest elements, combine them, and push the result back, until one remains. (Minimum Cost to Connect Sticks / similar to Huffman coding)
- **B. Cooldown-Constrained Scheduling (Max-Heap + Waiting Queue):** Pop the most frequent/urgent task, execute it, then place it in a cooldown holding queue before it's eligible to be pushed back into the heap. (Task Scheduler — LC 621, Rearrange String k Distance Apart — LC 358)
- **C. Streaming Median / Top-K Maintenance (Dual Heap):** Maintain a max-heap of the lower half and min-heap of the upper half so the greedy "boundary" element is always accessible in O(log N). (Find Median from Data Stream — LC 295)

## 7. Greedy on Graph Structures (Weighted Structural Topography)

**Usage:** The same "always pick the cheapest/safest next option" logic, but the underlying structure is a graph rather than a flat array.

**Giveaway:** Nodes/edges instead of array elements, but the decision rule is still "greedily pick the best available option right now, never reconsider."

**Structural Variations:**

- **A. Minimum Spanning Tree (Kruskal's / Prim's):** The graph-flavored version of exchange-argument sorting — greedily add the cheapest edge that doesn't create a cycle. (This is the same engine as MST in the Graph map — greedy is its proof foundation.)
- **B. Huffman-Style Encoding Tree:** Same as the heap-merge pattern above, but the "combination" builds an explicit binary tree used for optimal prefix-free encoding.

## 8. String/Array Reconstruction Greedy (Local Decision, Global Shape)

**Usage:** Building or removing characters/elements from a string or array one decision at a time, where each decision is locally justified (e.g., "remove this digit because a smaller one follows") but produces a globally optimal shape.

**Giveaway:** "Remove K digits to form the smallest number," "create the maximum number from two arrays," "smallest subsequence with distinct characters" — output shape/order matters, not just a count or sum.

**Structural Variations:**

- **A. Monotonic Stack Greedy (Maintain Increasing/Decreasing Shape):** Push elements onto a stack, popping whenever the current element invalidates the desired monotonic property and popping is still "affordable" (budget/count remaining). (Remove K Digits — LC 402, Remove Duplicate Letters — LC 316)
- **B. Last-Occurrence Boundary Greedy (Partition Labels style):** Track the last index each element appears at; greedily close a partition/window the moment the current index reaches the max last-occurrence seen so far within it. (Partition Labels — LC 763)

# Part III: State Storage & Performance Optimizations

Master these last. These are advanced supporting structures or proof techniques layered on top of the core greedy engines above to make them efficient or to formally justify correctness.

## 9. Matroid / Exchange Property Justification (The Formal Correctness Layer)

**Usage:** Not a coding pattern by itself, but the theoretical backbone explaining why a greedy choice works — recognizing when a problem's constraint structure (independence system) guarantees that greedy = optimal.

**Giveaway:** You're unsure whether greedy actually works and want to verify it, rather than defaulting to DP. If the constraint set satisfies the exchange property (any independent set can "trade up" toward a larger one by swapping in an element from a bigger independent set), greedy is provably optimal. MST (Kruskal's) is the canonical matroid example.

**Structural Variations:**

- **A. Proof by Contradiction / Exchange Argument:** Assume an optimal solution differs from the greedy one at the first point of divergence, show swapping in the greedy choice doesn't make things worse — the standard proof template for every pattern in Part I.
- **B. Proof by Staying Ahead:** Show that at every prefix/step, the greedy solution is at least as good as any other valid solution at that same point, so by induction it stays optimal to the end. (Used to justify Interval Scheduling and Jump Game greedily.)

## 10. Greedy + Binary Search Hybrid (Monotonic Feasibility Acceleration)

**Usage:** The greedy check itself ("is this threshold/capacity feasible?") is fast and monotonic, so instead of computing the answer directly, you binary search over the answer space and use a greedy feasibility check at each guess.

**Giveaway:** "Minimize the maximum" or "maximize the minimum" phrasing, combined with a greedy simulation that can cheaply validate one candidate answer.

**Structural Variations:**

- **A. Binary Search on Answer + Greedy Feasibility Simulation:** Binary search over possible answer values; for each candidate, run a greedy O(N) pass to check if it's achievable. (Split Array Largest Sum — LC 410, Capacity To Ship Packages Within D Days — LC 1011)
