# Part I: True Algorithmic Patterns

Master these first. These are the foundational traversal and recursion mathematics everything else is built from.

## 1. Depth-First Traversal (The Recursive Descent Engine)

**Usage:** Visiting every node via recursion (or an explicit stack), processing parent/child relationships in a specific order.

**Giveaway:** The problem talks about "visit all nodes," ordering output, or needs a value computed bottom-up/top-down through the hierarchy.

**Structural Variations:**

- **A. Preorder (Root → Left → Right):** Process the node before its children. Useful for cloning trees, serializing structure-first. (Serialize and Deserialize Binary Tree — LC 297)
- **B. Inorder (Left → Root → Right):** For a BST, this yields nodes in sorted order — the defining property of the whole BST family. (Validate Binary Search Tree — LC 98, Kth Smallest Element in a BST — LC 230)
- **C. Postorder (Left → Right → Root):** Process children before the parent. Required whenever the parent's answer depends on aggregated child results. (Diameter of Binary Tree — LC 543)

## 2. Breadth-First Traversal (The Level Engine)

**Usage:** Processing nodes level-by-level using a queue, where "depth" or "layer" is semantically meaningful.

**Giveaway:** Language like "level order," "per-level average/max," "zigzag," "right side view," or "minimum depth" (BFS finds this in shortest steps, unlike DFS).

**Structural Variations:**

- **A. Standard Level Order (Queue Snapshot per Level):** Capture len(queue) at the start of each level to process exactly that layer before moving on. (Binary Tree Level Order Traversal — LC 102)
- **B. Directional/Positional Level Order:** Track extra per-level metadata (last node seen = right view, alternate append direction = zigzag). (Binary Tree Zigzag Level Order Traversal — LC 103, Binary Tree Right Side View — LC 199)

## 3. Binary Search Tree Property Exploitation (The Ordered Pruning Engine)

**Usage:** Using the BST invariant (left < node < right) to eliminate half the tree at every step instead of visiting all nodes.

**Giveaway:** The tree is explicitly a BST, and the problem asks for search, insert, delete, closest value, or range-based queries — anything where sorted order lets you prune.

**Structural Variations:**

- **A. Directed Search/Insert/Delete (O(log N) average):** At each node, compare and go left or right — never both. (Search in a Binary Search Tree — LC 700, Delete Node in a BST — LC 450)
- **B. Successor/Predecessor via Structure:** Finding the next larger/smaller value uses parent pointers or leftmost/rightmost descent instead of a full inorder scan. (Inorder Successor in BST — LC 285)

## 4. Lowest Common Ancestor (The Divergence Point Engine)

**Usage:** Finding the deepest node that is an ancestor of two (or more) given nodes.

**Giveaway:** The problem explicitly asks for "the lowest common ancestor" of two nodes, or an equivalent framed as "the point where paths split."

**Structural Variations:**

- **A. General Binary Tree LCA (Postorder Bubble-Up):** Recurse into both children; if both return non-null, the current node is the LCA — otherwise bubble up whichever side found something. (Lowest Common Ancestor of a Binary Tree — LC 236)
- **B. BST LCA (Directed via Property):** No need to search both sides — compare the target values against the current node's value to decide a single direction. (Lowest Common Ancestor of a BST — LC 235)

## 5. Path Sum / Root-to-Node Accumulation (The Running Total Engine)

**Usage:** Tracking a cumulative value (sum, XOR, max path) as you descend or traverse across the tree, often requiring backtracking to "undo" state.

**Giveaway:** "Path from root to leaf," "path sum equals target," or "count paths" language, especially when paths don't have to include the root.

**Structural Variations:**

- **A. Root-to-Leaf Fixed Path (Direct Accumulation):** Pass a running sum downward through recursion; check the condition only at leaf nodes. (Path Sum — LC 112)
- **B. Any-Node-to-Any-Node Path (Prefix Sum + HashMap):** Track prefix sums seen along the current root-to-node path in a hashmap, backtracking (removing) the entry when leaving that branch, to count subpaths in O(N). (Path Sum III — LC 437)
- **C. Global Max Path Tracking:** Same crossroads pattern as Tree DP in graphs — each call returns the best single downward branch, but a global variable captures left + right + node at every step. (Binary Tree Maximum Path Sum — LC 124)

# Part II: Structural Topography Patterns

Master these second. Same engines from Part I, applied to specific non-obvious tree shapes and construction problems.

## 6. Tree Construction & Serialization (Reassembly Topography)

**Usage:** Rebuilding a unique tree structure from traversal arrays, or converting a tree to/from a transportable format.

**Giveaway:** You're given two traversal arrays (e.g., preorder + inorder) and asked to rebuild the tree, or asked to serialize/deserialize.

**Structural Variations:**

- **A. Build from Two Traversals (Index Partitioning):** Use one traversal to find the root, then use that root's position in the other traversal to partition left/right subtree index ranges recursively. (Construct Binary Tree from Preorder and Inorder Traversal — LC 105)
- **B. Serialize/Deserialize (Preorder + Null Markers):** Encode structure explicitly using null sentinels so the tree can be perfectly reconstructed without needing a second traversal array. (Serialize and Deserialize Binary Tree — LC 297)

## 7. Tree Comparison & Transformation (Structural Equivalence Topography)

**Usage:** Comparing two trees for equality/symmetry, or transforming one tree's shape into another.

**Giveaway:** "Same tree," "mirror," "symmetric," "subtree of another tree," "invert/flip."

**Structural Variations:**

- **A. Mirrored Recursion (Symmetry Check):** Recurse on (left.left vs right.right) and (left.right vs right.left) simultaneously instead of a normal single-tree descent. (Symmetric Tree — LC 101)
- **B. Subtree Matching (Nested Traversal):** For every node in the main tree, run a full equality check against the candidate subtree — an O(N*M) nested-traversal pattern distinct from single-pass comparison. (Subtree of Another Tree — LC 572)

## 8. Balanced Tree Maintenance (Self-Correcting Topography)

**Usage:** Verifying or maintaining a height-balance invariant so operations stay O(log N) instead of degrading to O(N) on skewed input.

**Giveaway:** "Balanced binary tree," or a BST insert/delete problem that explicitly warns about worst-case skewed input.

**Structural Variations:**

- **A. Balance Verification (Height Bubble-Up with Early Exit):** Postorder recursion returns height, but returns a sentinel (e.g., -1) the instant imbalance is detected to short-circuit further work. (Balanced Binary Tree — LC 110)
- **B. Self-Balancing Rotations (AVL/Red-Black Conceptual):** After insert/delete, rotate subtrees (left-rotate/right-rotate) to restore the height invariant — the mechanism real-world TreeMap/TreeSet implementations rely on.

## 9. Trie / Prefix Tree (Shared-Prefix Topography)

**Usage:** Storing a set of strings so that shared prefixes are stored only once, enabling fast prefix search, autocomplete, and word validation.

**Giveaway:** The problem involves a dictionary of words and asks about prefixes, autocomplete, or "does this exact word/prefix exist" — this is Tree traversal applied to a 26(or N)-ary branching structure keyed by characters instead of just two children.

**Structural Variations:**

- **A. Standard Trie (Insert/Search/StartsWith):** Each node holds an array/map of children keyed by character plus an isEndOfWord flag; descending character-by-character is just DFS on a wide tree. (Implement Trie (Prefix Tree) — LC 208)
- **B. Wildcard-Augmented Trie (DFS with Branching on '.'):** Search recursively branches into all children when a wildcard character is hit instead of following one path — turning a normal O(L) trie descent into a bounded backtracking search. (Design Add and Search Words Data Structure — LC 211)
- **C. Trie + DFS on Grid (Combined Traversal):** Layer a trie on top of a grid-traversal DFS so multiple word searches share pruning — the moment a grid path no longer matches any trie branch, that whole subtree of exploration is cut. (Word Search II — LC 212)
- **D. Bitwise Trie (XOR Maximization):** Instead of characters, each node branches on a single bit (0/1) of a number's binary representation, letting you greedily choose the opposite bit at each level to maximize XOR. (Maximum XOR of Two Numbers in an Array — LC 421)

## 10. N-ary / Generic Tree Traversal (Multi-Child Topography)

**Usage:** The same DFS/BFS engines from Part I, but nodes can have more than two children.

**Giveaway:** The input explicitly gives a children: [] list instead of just left/right, or it's a filesystem/org-chart style hierarchy.

**Structural Variations:**

- **A. N-ary DFS/BFS (Loop Over Children Instead of Left/Right):** Structurally identical to binary traversal, just replacing the fixed two-child recursion with a loop. (N-ary Tree Preorder Traversal — LC 589, N-ary Tree Level Order Traversal — LC 429)
- **B. Encoding N-ary as Binary (Left-Child Right-Sibling):** A classic transformation representing "first child" as left and "next sibling" as right, collapsing an N-ary tree into a binary one so binary-tree algorithms apply directly. (Encode N-ary Tree to Binary Tree — LC 431)

# Part III: State Storage & Performance Optimizations

Master these last. Advanced data-layout wrappers used to accelerate or compress tree operations.

## 11. Segment Tree (Range Query Compression)

**Usage:** Answering range queries (sum/min/max) over an array while also supporting point or range updates, faster than brute-force recomputation.

**Giveaway:** The problem interleaves "update a value" with "query a range" repeatedly — a static prefix-sum array can't handle updates efficiently, but a segment tree does both in O(log N).

**Structural Variations:**

- **A. Sum/Min/Max Segment Tree (Recursive Build + Query + Update):** Each node stores an aggregate over a range; leaves are individual elements, and internal nodes combine their two children's aggregates. (Range Sum Query - Mutable — LC 307)
- **B. Lazy Propagation (Deferred Range Updates):** When updating an entire range at once instead of a single point, defer pushing the update down to children until they're actually queried, avoiding an O(N) cascade per update.

## 12. Binary Indexed Tree / Fenwick Tree (Bitwise Prefix Compression)

**Usage:** A lighter-weight alternative to Segment Trees specifically for prefix sums with point updates, using the binary representation of indices to jump between "responsible" ranges.

**Giveaway:** You only need prefix sum + point update (not arbitrary range aggregates like min/max), and want less code/memory overhead than a full segment tree.

**Structural Variations:**

- **A. Standard Fenwick Update/Query (Lowbit Jumping):** Use i & (-i) to walk to the next responsible index on update, and the reverse direction on query. (Range Sum Query - Mutable — LC 307, Count of Smaller Numbers After Self — LC 315)

## 13. Tree DP / Rerooting (Whole-Tree Aggregate Optimization)

**Usage:** Computing an answer that depends on choosing every node as a hypothetical root, without recomputing the whole tree from scratch each time.

**Giveaway:** "For each node, if it were the root..." phrasing, or "find the node that minimizes/maximizes the sum of distances to all others" — brute-force recomputation per node is O(N²), rerooting brings it to O(N).

**Structural Variations:**

- **A. Two-Pass Rerooting (Down-Pass + Up-Pass):** First pass computes subtree aggregates rooted arbitrarily; second pass "re-derives" each node's answer using its parent's already-adjusted answer plus/minus its own subtree's contribution. (Sum of Distances in Tree — LC 834)

## 14. Morris Traversal (O(1) Space Threading)

**Usage:** Traversing a tree in-order (or preorder) without recursion or an explicit stack, achieving O(1) auxiliary space.

**Giveaway:** The problem explicitly demands O(1) space traversal — the giveaway is purely a constraint statement, not a shape of the input.

**Structural Variations:**

- **A. Threaded Inorder (Temporary Right-Pointer Rewiring):** Temporarily link a node's inorder predecessor's right pointer back to itself to enable a "return path," then undo the link once traversed through — simulating a stack using the tree's own unused pointers. (Binary Tree Inorder Traversal — LC 94, follow-up)
