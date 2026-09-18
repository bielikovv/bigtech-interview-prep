# Stack Patterns — Full Map

# Part I: True Algorithmic Patterns

Master these first. These form the foundational mathematics of how stacks track "unresolved" state until something arrives to resolve it.

## 1. Monotonic Stack (The Next-Greater/Smaller Engine)

**Usage:** For each element, finding the nearest element to the left or right that is strictly greater/smaller than it.

**Giveaway:** The problem asks "next greater element," "next warmer day," "distance until a bigger value appears," or anything framed as "how far until X is beaten."

**Structural Variations:**

- **A. Next Greater Element (Decreasing Stack):** Maintain a stack of indices/values in decreasing order. When a new element beats the stack top, pop and resolve those elements — the new element is their "answer." (Next Greater Element I — LC 496, Daily Temperatures — LC 739)
- **B. Next Smaller Element (Increasing Stack):** Mirror version — maintain an increasing stack, pop when the new element is smaller. (Next Smaller Element variants, Online Stock Span — LC 901)
- **C. Circular Array Variant (Double Pass):** Loop through the array twice (using i % n) without actually duplicating it, so "next greater" can wrap around the end. (Next Greater Element II — LC 503)
- **D. Previous Greater/Smaller (Reversed Direction):** Same engine, just scanning left-to-right while asking about what's behind you instead of ahead — used as a sub-step inside larger problems (e.g. histogram, trapping rain water).

## 2. Monotonic Stack for Area/Volume (The Boundary Collapse Engine)

**Usage:** Finding the largest rectangle, container, or trapped volume where the bottleneck is the nearest smaller boundary on each side.

**Giveaway:** The problem mentions bars/heights forming a skyline, and asks for max area/volume — this is Monotonic Stack applied to geometry, not a new mathematical engine.

**Structural Variations:**

- **A. Largest Rectangle in Histogram (Pop-and-Compute):** Maintain an increasing stack of bar indices; when a shorter bar arrives, pop the taller bar and compute its max rectangle using the current index as the right boundary and the new stack top as the left boundary. (Largest Rectangle in Histogram — LC 84)
- **B. Maximal Rectangle in Binary Matrix (Histogram Reduction Reuse):** Collapse each row of a 2D grid into histogram heights, then rerun the Histogram engine per row. (Maximal Rectangle — LC 85)
- **C. Trapping Rain Water (Stack-Based Two-Boundary Fill):** Maintain a decreasing stack of walls; when a taller wall arrives, pop the "floor" and compute trapped water using the new wall, the popped floor, and the new stack top as the other wall. (Trapping Rain Water — LC 42)

## 3. Matching / Bracket Validity (The Nested Closure Engine)

**Usage:** Verifying that opening and closing symbols nest correctly, or resolving/repairing improperly nested symbols.

**Giveaway:** The problem involves parentheses, brackets, tags, or any symbol pair where "closing" must match the most recent unmatched "opening."

**Structural Variations:**

- **A. Strict Validity Check (Push Open, Pop-and-Compare on Close):** Push opening symbols; on a closing symbol, pop and verify it matches the expected pair — mismatch or empty-pop means invalid. (Valid Parentheses — LC 20)
- **B. Minimum Insertions/Removals to Fix (Counting Unmatched):** Same push/pop mechanics, but instead of failing outright, count leftover unmatched opens/closes to compute a repair cost. (Minimum Add to Make Parentheses Valid — LC 921, Minimum Remove to Make Valid Parentheses — LC 1249)
- **C. Longest Valid Substring (Index-Based Stack):** Push indices instead of characters; popping and measuring the gap between indices tracks the length of the longest valid run. (Longest Valid Parentheses — LC 32)

## 4. Expression Evaluation (The Operator Precedence Engine)

**Usage:** Parsing and evaluating a string-based mathematical expression respecting operator precedence and/or parentheses.

**Giveaway:** The problem gives a string like "3+2*2" or "(1+(4+5+2)-3)" and asks for a computed integer result.

**Structural Variations:**

- **A. Basic Calculator (No Precedence, Just Sign Tracking + Parens):** Use a stack to save/restore the running result and sign whenever a ( is entered and restore on ). (Basic Calculator — LC 224)
- **B. Calculator With Precedence (Operand + Operator Stack):** Push numbers onto an operand stack; when a lower-precedence operator arrives, resolve all pending higher-precedence operations first. (Basic Calculator II — LC 227)
- **C. Reverse Polish / Postfix Evaluation (Pure Operand Stack):** No precedence logic needed at all — operators consume the top two stack operands as they're encountered, left to right. (Evaluate Reverse Polish Notation — LC 150)
- **D. Infix-to-Postfix Conversion (Shunting Yard):** Use an operator stack to reorder infix tokens into postfix form, popping operators of equal/higher precedence before pushing a new one.

# Part II: Structural Topography Patterns

Master these second. Same engines from Part I, applied to non-obvious carriers of the "stack" idea.

## 5. Call Stack Simulation (Recursion-to-Iteration Topography)

**Usage:** Manually replacing a recursive process with an explicit stack to avoid recursion depth limits or to control traversal order precisely.

**Giveaway:** The problem is naturally recursive (tree/graph DFS, nested structure parsing) but explicitly asks for or benefits from an iterative solution.

**Structural Variations:**

- **A. Iterative DFS (Explicit Stack Replacing Call Frames):** Push children in reverse order so popping produces the same order as recursive DFS. (Binary Tree Preorder Traversal — LC 144, iterative)
- **B. Iterative Inorder (State-Tracking Stack):** Push left children until null, pop-and-process, then move to the right — simulating the "return point" a recursive call would naturally have. (Binary Tree Inorder Traversal — LC 94, iterative)
- **C. Backtracking State Save/Restore (Explicit Frame Stack):** Push a full "frame" (current path, remaining choices, index) instead of just a value, to simulate backtracking without recursion.

## 6. Nested Structure Decoding (Layered Container Topography)

**Usage:** Parsing strings/data with nested layers (brackets, tags, directories) where each layer's result depends on fully resolving the layer inside it first.

**Giveaway:** The problem has bracket-delimited repetition or nesting, like "3[a2[c]]", and asks you to expand/decode it — Bracket Matching engine applied to string building instead of validity checking.

**Structural Variations:**

- **A. Decode String (Push Count + Partial String on Open, Pop-and-Repeat on Close):** Push the current multiplier and accumulated string whenever [ appears; on ], pop and repeat/append. (Decode String — LC 394)
- **B. Flatten Nested List (Push Iterators/Contexts):** Push an iterator or index pointer for each nested list level; popping resumes the outer list exactly where it left off. (Flatten Nested List Iterator — LC 341)
- **C. Directory Path Simplification (Push Valid Segments, Pop on ..):** Treat path segments like a bracket-matching problem where .. pops the last valid directory pushed. (Simplify Path — LC 71)

## 7. Stack-Based String Reduction (Adjacent Collapse Topography)

**Usage:** Repeatedly removing adjacent pairs/runs of elements that satisfy some cancellation rule, until no more removals are possible.

**Giveaway:** "Remove adjacent duplicates," "remove adjacent elements that sum to zero/cancel," or any "keep collapsing until stable" phrasing.

**Structural Variations:**

- **A. Adjacent Duplicate Removal (Push, Pop-on-Match):** Push each character; if it matches the current stack top, pop instead of pushing — the stack top is always the "surviving" reduced string. (Remove All Adjacent Duplicates in String — LC 1047)
- **B. Counted Duplicate Removal (Push with Count Pairs):** Push (char, count) pairs; increment count on match, pop entirely when count hits K. (Remove All Adjacent Duplicates in String II — LC 1209)
- **C. Asteroid/Entity Collision Collapse (Directional Cancellation):** Push entities; on collision (opposite directions), resolve by comparing magnitudes and popping the loser, possibly cascading further pops. (Asteroid Collision — LC 735)

# Part III: State Storage & Performance Optimizations

Master these last. Advanced data-layout wrappers used to extend what a plain stack can answer in O(1).

## 8. Min/Max Stack (Auxiliary Tracking Stack)

**Usage:** Supporting O(1) retrieval of the minimum or maximum element currently in the stack, alongside normal push/pop.

**Giveaway:** The problem explicitly asks for a stack that also supports getMin()/getMax() in constant time.

**Structural Variations:**

- **A. Parallel Auxiliary Stack (Twin Stack):** Maintain a second stack that pushes the current min/max alongside every main-stack push, so popping both stays perfectly synchronized. (Min Stack — LC 155)
- **B. Single-Stack Encoded Difference (Space-Optimized):** Store the difference between the new value and the current min instead of a second stack, reconstructing actual values and updating min during pops using that encoded delta.

## 9. Stack via Two Queues / Queue via Two Stacks (Structural Inversion)

**Usage:** Emulating one structure's interface (FIFO or LIFO) using only operations from the opposite structure.

**Giveaway:** The problem explicitly asks to "implement a queue using stacks" or vice versa — testing whether you understand why the order-reversal property of a stack is fundamental.

**Structural Variations:**

- **A. Queue Using Two Stacks (Input/Output Stack Pair):** Push new elements onto an "in" stack; when a dequeue is needed and the "out" stack is empty, dump all of "in" into "out" to reverse the order once. (Implement Queue using Stacks — LC 232)
- **B. Stack Using Two Queues (Rotate-to-Front):** Push the new element, then rotate the queue so the newly pushed element moves to the front — simulating LIFO order using FIFO primitives. (Implement Stack using Queues — LC 225)

## 10. Monotonic Deque (Sliding Window Extension)

**Usage:** Finding the max/min within every sliding window of size K, where a plain monotonic stack can't discard "too old" elements from the back.

**Giveaway:** "Sliding window maximum/minimum" — Monotonic Stack's engine, but needing eviction from both ends as the window slides forward.

**Structural Variations:**

- **A. Monotonic Decreasing Deque (Sliding Window Max):** Push indices, popping smaller elements from the back before adding a new one; pop from the front when the front index falls outside the window. (Sliding Window Maximum — LC 239)
- **B. Monotonic Increasing Deque (Sliding Window Min):** Mirror version for minimums, often paired with the Max deque to solve "longest subarray with max-min ≤ limit" problems. (Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit — LC 1438)
