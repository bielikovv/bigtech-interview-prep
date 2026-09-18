# Linked List Patterns — Full Map

# Part I: True Algorithmic Patterns

Master these first. These form the foundational pointer mathematics everything else is built from.

## 1. Fast & Slow Pointers (The Cycle Detection Engine)

**Usage:** Detecting cycles, finding midpoints, or determining structural properties using two pointers moving at different speeds.

**Giveaway:** The problem asks about a cycle, a middle node, or "does this list loop back on itself."

**Structural Variations:**

- **A. Cycle Detection (Floyd's Algorithm):** Slow moves 1 step, fast moves 2 steps; if they meet, a cycle exists. (Linked List Cycle — LC 141)
- **B. Cycle Start Detection (Two-Phase Meeting):** After the initial meeting point, reset one pointer to head and move both at 1 step; where they meet again is the cycle's entry node. (Linked List Cycle II — LC 142)
- **C. Middle-Finding (Half-Speed Convergence):** When fast reaches the end, slow sits exactly at the midpoint — no length count needed beforehand. (Middle of the Linked List — LC 876)
- **D. Happy Number Style Cycle Detection (Non-List Application):** The same engine applied to a sequence of computed values instead of explicit next pointers, proving this is a math pattern, not just a list-shape pattern. (Happy Number — LC 202)

## 2. Reversal (The Pointer Inversion Engine)

**Usage:** Flipping the direction of next pointers across all or part of a list.

**Giveaway:** The problem explicitly asks to reverse, or requires processing a list "backwards" without extra memory.

**Structural Variations:**

- **A. Full Iterative Reversal (Three-Pointer Walk):** Track prev, curr, and next explicitly, re-pointing curr.next = prev at every step. (Reverse Linked List — LC 206)
- **B. Recursive Reversal (Trust-the-Recursion):** Recurse to the end first, then rewire pointers on the way back up the call stack.
- **C. Sublist Reversal (Bounded Window Inversion):** Reverse only nodes between position left and right, requiring careful reconnection of the untouched head and tail segments. (Reverse Linked List II — LC 92)
- **D. Group Reversal (Fixed-K Chunking):** Reverse nodes in groups of K, recursively or iteratively chaining each reversed group to the next group's head. (Reverse Nodes in k-Group — LC 25)

## 3. Merge & Combine (The Two-Pointer Weave Engine)

**Usage:** Combining two or more sorted (or unsorted) lists into a single coherent list.

**Giveaway:** The input explicitly gives multiple lists, and the ask is to interleave, merge, or combine them into one.

**Structural Variations:**

- **A. Two-List Merge (Direct Comparison Weave):** Compare heads of both lists at each step, attaching the smaller node and advancing only that pointer. (Merge Two Sorted Lists — LC 21)
- **B. K-List Merge (Heap-Accelerated Weave):** Push all list heads into a min-heap, repeatedly pop the smallest and push its successor — an extension of the two-list engine using a priority queue to scale beyond two inputs. (Merge k Sorted Lists — LC 23)
- **C. Divide-and-Conquer Merge (Pairwise Reduction):** Recursively pair up and merge lists two at a time (like merge sort's combine step) instead of using a heap, trading heap overhead for recursive halving.

## 4. Dummy Head / Sentinel Node (The Edge-Case Elimination Engine)

**Usage:** Simplifying insertion, deletion, or construction logic when the head node itself might change or be removed.

**Giveaway:** The problem requires removing or inserting at the head, or building a new list from scratch node-by-node.

**Structural Variations:**

- **A. Deletion Simplification (Sentinel Before Head):** Point a dummy node's next at the real head so removing the actual head requires no special-case branch. (Remove Linked List Elements — LC 203, Remove Nth Node From End of List — LC 19)
- **B. Construction Simplification (Tail-Pointer Building):** Maintain a dummy.next as the eventual answer while walking a separate tail pointer forward to append nodes one at a time, avoiding null-checks on an empty result list. (Add Two Numbers — LC 2, Merge Two Sorted Lists — LC 21)

## 5. N-th Node Positioning (The Offset Gap Engine)

**Usage:** Finding or removing a node at a specific offset from the end, or splitting a list at a specific fractional point, without knowing the total length upfront.

**Giveaway:** The problem references a position "from the end," or asks to find something in a single pass without pre-counting length.

**Structural Variations:**

- **A. Two-Pointer Gap Maintenance (N-Ahead Start):** Advance one pointer N steps ahead first, then move both together — when the lead pointer hits the end, the trailing pointer is exactly N nodes from the end. (Remove Nth Node From End of List — LC 19)
- **B. Two-Pass Length Counting (Explicit Count-Then-Walk):** Traverse once to count total length, then traverse again to the computed target index — the naive baseline the two-pointer trick is optimizing away.

# Part II: Structural Topography Patterns

Master these second. These are not new mathematical engines; they are the patterns from Part I applied to non-standard list shapes and layered structures.

## 6. Doubly Linked List Manipulation (Bidirectional Topography)

**Usage:** Maintaining both next and prev pointers so traversal and deletion can happen in either direction in O(1).

**Giveaway:** The problem requires O(1) deletion of an arbitrary given node, or a structure that must support moving both forward and backward (like a cache's recency order).

**Structural Variations:**

- **A. O(1) Arbitrary Node Removal (Prev/Next Rewiring):** Removing a node only requires touching its immediate neighbors' pointers, no traversal from head needed. (Design Linked List — LC 707)
- **B. LRU/LFU Cache Backing Structure (Doubly Linked List + HashMap):** Pair a doubly linked list (maintaining recency order) with a hashmap (O(1) node lookup by key) so both "access" and "evict" operations run in O(1). (LRU Cache — LC 146)

## 7. Multi-Level / Nested Pointer Structures (Layered Topography)

**Usage:** Lists where nodes carry an extra pointer beyond next (a random pointer, a child pointer, or a sibling level pointer) that must be preserved or flattened.

**Giveaway:** The node definition explicitly includes an extra field beyond next/prev — a random, child, or bottom pointer.

**Structural Variations:**

- **A. Deep Copy with Auxiliary Pointer (Interleaving or HashMap Mapping):** Either weave cloned nodes directly between originals to piggyback lookups, or use a hashmap from original→clone to resolve the extra pointer in a second pass. (Copy List with Random Pointer — LC 138)
- **B. Flattening Nested Levels (Recursive Child Splicing):** When a node has a child pointer to its own sublist, recursively flatten the child first, then splice it inline between the current node and its original next. (Flatten a Multilevel Doubly Linked List — LC 430)

## 8. Circular Linked List Handling (Closed-Loop Topography)

**Usage:** Lists where the tail explicitly points back to the head (by design, not by bug), requiring termination logic other than "until null."

**Giveaway:** The problem states the list is circular, or asks to insert/traverse in a structure with no natural null endpoint.

**Structural Variations:**

- **A. Circular Traversal (Start-Node Sentinel Check):** Since there's no null terminator, save the starting node and stop when you return to it rather than checking for null. (Insert into a Sorted Circular Linked List — LC 708)
- **B. List-to-Circle / Circle-to-List Conversion (Break/Join Point Rewiring):** Convert a linear list into a circular one (or vice versa) by explicitly connecting or cutting the tail-to-head link — often paired with the fast/slow midpoint engine. (Palindrome Linked List's rearrangement step)

## 9. List Reordering & Partitioning (Rearrangement Topography)

**Usage:** Rearranging existing nodes into a new relative order without allocating new nodes, often combining reversal and merge sub-engines.

**Giveaway:** The ask describes a specific target shape ("zigzag," "first half then reversed second half interleaved," "all evens before odds") rather than a simple sort.

**Structural Variations:**

- **A. Split-Reverse-Merge Combo (Composite Engine Chaining):** Find the middle (fast/slow), reverse the second half (reversal engine), then weave the two halves together alternately (merge engine) — showing how Part I engines compose into Part II shapes. (Reorder List — LC 143)
- **B. Value-Based Partitioning (Two-Bucket Stable Split):** Build two separate sub-lists (e.g., odd-indexed and even-indexed, or less-than-x and greater-or-equal-to-x) while preserving original relative order, then join them at the end. (Odd Even Linked List — LC 328, Partition List — LC 86)

## 10. Linked List as Number Representation (Arithmetic Topography)

**Usage:** Treating a linked list as a sequence of digits to perform arithmetic (addition, incrementing) without converting the whole thing to an integer.

**Giveaway:** Each node holds a single digit, and the problem asks for a mathematical operation (add, increment) performed digit-by-digit down the list.

**Structural Variations:**

- **A. Same-Direction Addition with Carry (Direct Walk, LSB-Last Ordering):** When digits are stored most-significant-first, reverse both lists (or use stacks) first so addition can proceed from the least-significant digit with carry propagation. (Add Two Numbers II — LC 445)
- **B. Reverse-Stored Direct Addition (No Pre-Reversal Needed):** When digits are stored least-significant-first already, walk both lists directly summing with carry — no reversal step needed, contrasting directly with the variation above. (Add Two Numbers — LC 2)

# Part III: State Storage & Performance Optimizations

Master these last. These are advanced techniques for verifying properties or achieving stricter complexity bounds.

## 11. Palindrome Verification (Symmetric Comparison Optimization)

**Usage:** Checking whether a list reads the same forwards and backwards, ideally in O(1) extra space instead of O(N) via array conversion.

**Giveaway:** The problem asks about palindrome structure specifically on a linked list, and often (as a follow-up) demands O(1) space — ruling out the naive "copy to array and use two pointers" approach.

**Structural Variations:**

- **A. Array-Copy Baseline (O(N) Space):** Copy all values into an array, then apply standard two-pointer palindrome comparison — the naive baseline.
- **B. In-Place Reversal Comparison (O(1) Space Composite):** Find the midpoint (fast/slow engine), reverse the second half in place (reversal engine), compare the two halves node-by-node, then optionally re-reverse to restore the original list. (Palindrome Linked List — LC 234)

## 12. Intersection Detection (Convergence Point Optimization)

**Usage:** Determining whether two separate lists eventually merge into the same tail, and finding exactly where, without extra memory proportional to length.

**Giveaway:** Two distinct list heads are given, and the question asks where (or whether) they intersect — the naive answer is a hashset of visited nodes, which the optimized pattern avoids.

**Structural Variations:**

- **A. HashSet Visited-Node Baseline (O(N) Space):** Walk one list storing every node's identity in a set, then walk the other checking for membership — the naive baseline.
- **B. Length-Difference Alignment (O(1) Space, Two-Pass):** Compute both lengths, advance the longer list's pointer by the difference, then walk both in lockstep until pointers match. (Intersection of Two Linked Lists — LC 160)
- **C. Pointer-Swap Equalization (O(1) Space, One-Pass Elegance):** Walk both pointers simultaneously; when one hits null, redirect it to the other list's head — this naturally equalizes total distance traveled so they meet exactly at the intersection (or both reach null together if none exists), without ever computing lengths explicitly.

## 13. In-Place Sorting (Constant-Space Reordering Optimization)

**Usage:** Sorting a linked list's node values without converting to an array, ideally hitting O(N log N) time and O(1) extra space.

**Giveaway:** The problem explicitly asks for sorting a linked list, often with a follow-up constraint of O(N log N) time and O(1) space — ruling out array-copy sort.

**Structural Variations:**

- **A. Merge Sort (Split via Fast/Slow + Merge Engine Composite):** Recursively split the list at its midpoint (fast/slow engine) down to single nodes, then recursively recombine using the merge engine — the standard optimal approach since linked lists can't be efficiently random-accessed for quicksort-style partitioning. (Sort List — LC 148)
- **B. Insertion Sort (Pointer-Rewiring Into Growing Prefix):** Walk the unsorted portion node-by-node, splicing each into its correct position within an already-sorted prefix by rewiring pointers rather than shifting array elements. (Insertion Sort List — LC 147)
