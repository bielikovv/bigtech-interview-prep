# Graph Patterns — Full Map

# Part I: True Algorithmic Engines

Master these first. These are the core traversal and shortest-path mathematics everything else is built from.

## 1. Pure Traversal (The Reachability Engine)

**Usage:** Determining what is reachable, counting connected components, or exploring every node/edge exactly once.

**Giveaway:** The question is binary reachability, counting islands/components, or "visit everything" with no weights involved.

**Structural Variations:**

- **A. DFS (Depth-first, recursive/stack-based):** Good for path existence, cycle detection, exhaustive exploration. (Number of Islands — LC 200)
- **B. BFS (Level-order, queue-based):** Required whenever "shortest number of steps/edges" matters on an unweighted graph, since BFS explores in strict distance layers. (Rotting Oranges — LC 994)
- **C. Multi-source BFS:** Seed the queue with all starting points simultaneously instead of one, so distances propagate correctly from many origins at once. (Rotting Oranges — LC 994, 01 Matrix — LC 542)

## 2. Union-Find / Disjoint Set (The Grouping Engine)

**Usage:** Dynamically merging elements into groups and answering "are these connected?" or "how many groups remain?" as edges are added over time.

**Giveaway:** Edges/connections arrive incrementally, or you need to detect a cycle in an undirected graph, or you need connectivity queries interleaved with unions.

**Structural Variations:**

- **A. Basic Union-Find with Path Compression + Union by Rank/Size:** Flattens tree depth so each query is near O(1) amortized. (Number of Provinces — LC 547)
- **B. Union-Find for Cycle Detection:** If two nodes are unioned but already share a root, adding that edge creates a cycle. (Redundant Connection — LC 684)
- **C. Weighted/Union-Find with Relationships:** Track a relative value (ratio, parity, offset) between a node and its parent during union, not just group membership. (Evaluate Division — LC 399)

## 3. Dijkstra's Algorithm (The Non-Negative Weighted Shortest Path Engine)

**Usage:** Finding shortest paths from a source when edges have non-negative weights.

**Giveaway:** Weighted graph, all weights ≥ 0, asks for shortest/cheapest/fastest path or cost.

**Structural Variations:**

- **A. Standard Single-Source Dijkstra (Min-Heap):** Greedily pop the lowest-cost frontier node, relax neighbors. (Network Delay Time — LC 743)
- **B. Dijkstra with Modified State (State-Expanded):** The "distance" isn't just node-based — the priority queue state also carries extra info (fuel remaining, stops used, path probability) so the same node can be visited multiple times under different states. (Cheapest Flights Within K Stops — LC 787, Path with Maximum Probability — LC 1514)

## 4. Bellman-Ford (The Negative-Weight-Tolerant Engine)

**Usage:** Shortest paths when negative edge weights are allowed, or you explicitly need to detect negative cycles.

**Giveaway:** Weights can be negative, or the problem explicitly caps "at most K edges/stops" (Bellman-Ford naturally bounds by relaxation rounds).

**Structural Variations:**

- **A. Standard Bellman-Ford (V-1 relaxation rounds):** Relax every edge V-1 times; if anything still improves on round V, a negative cycle exists.
- **B. Bounded-Hop Bellman-Ford:** Run only K rounds of relaxation instead of V-1, directly modeling "shortest path using at most K edges." (Cheapest Flights Within K Stops — LC 787)

## 5. Floyd-Warshall (The All-Pairs Engine)

**Usage:** You need shortest paths between every pair of nodes, not just from one source, and the graph is small enough for O(V³).

**Giveaway:** Graph size is small (V ≤ ~400-500), and the question asks about all-pairs distance, or "which node reaches the most others within a threshold."

**Structural Variations:**

- **A. Standard All-Pairs Matrix Fill:** For every intermediate node k, check if routing through k improves dist[i][j]. (Find the City With the Smallest Number of Neighbors — LC 1334)

## 6. Topological Sort (The Dependency Ordering Engine)

**Usage:** Ordering nodes so every directed dependency (prerequisite → dependent) is respected, or detecting whether that ordering is even possible (i.e., detecting a cycle in a directed graph).

**Giveaway:** Language like "prerequisite," "must happen before," "build order," or "dependency," on a directed graph.

**Structural Variations:**

- **A. Kahn's Algorithm (BFS, in-degree based):** Repeatedly peel off nodes with in-degree 0; if not all nodes get processed, a cycle exists. (Course Schedule — LC 207)
- **B. DFS-Based Topo Sort (Postorder + Reverse, 3-color marking):** Use white/gray/black node states to detect back-edges (cycles) while building the order via postorder recursion. (Course Schedule II — LC 210)

# Part II: Structural Topography Patterns

Master these second. Same engines above, applied to specific non-obvious shapes.

## 7. Minimum Spanning Tree (The Cheapest Full-Connection Engine)

**Usage:** Connecting all nodes with the minimum total edge weight, using no cycles.

**Giveaway:** "Connect all cities/computers/points with minimum cost," undirected weighted graph, no path-length requirement — just total connection cost.

**Structural Variations:**

- **A. Kruskal's (Edge-sorted + Union-Find):** Sort all edges by weight, greedily add if it doesn't create a cycle (checked via Union-Find). (Min Cost to Connect All Points — LC 1584)
- **B. Prim's (Node-expansion + Min-Heap):** Grow a single tree outward, always adding the cheapest edge that connects a new node. Better when the graph is dense.

## 8. Bipartite Graphs (Two-Coloring Topography)

**Usage:** Determining if a graph's nodes can be split into two groups with no edges inside either group, or matching problems between two distinct sets.

**Giveaway:** "Can this be divided into two groups," conflict/enemy relationships, or explicit two-sided matching (jobs↔workers, students↔schools).

**Structural Variations:**

- **A. Bipartite Check (BFS/DFS 2-Coloring):** Color each node opposite to its neighbor; a same-color conflict on an edge means it's not bipartite. (Is Graph Bipartite? — LC 785)
- **B. Bipartite Matching (Augmenting Paths / Hungarian-style):** Match nodes from set A to set B maximizing pairs, using augmenting paths to "steal back" a match when a better one is found.

## 9. Graph Cycle & Structure Detection (Specialized Topology)

**Usage:** Detecting cycles specifically, or finding critical structural elements (edges/nodes whose removal disconnects the graph).

**Giveaway:** "Find the edge that creates a cycle," "find critical connections," "find articulation points."

**Structural Variations:**

- **A. Directed Cycle Detection (3-color DFS):** Same coloring scheme as topo sort — gray-to-gray edge = cycle.
- **B. Undirected Cycle Detection (Union-Find or DFS-parent-tracking):** An edge to an already-visited non-parent node = cycle.
- **C. Bridges & Articulation Points (Tarjan's low-link algorithm):** Track discovery time and the lowest reachable ancestor via back-edges to identify edges/nodes that are single points of failure. (Critical Connections in a Network — LC 1192)

## 10. Grid-as-Graph (Implicit Adjacency Topography)

**Usage:** A 2D matrix is secretly a graph where each cell is a node and adjacency is implicit (up/down/left/right, sometimes diagonals).

**Giveaway:** Input is a grid/matrix, but the actual ask is a graph question (shortest path, connected regions, flood fill) — this is not a new engine, it's Part I applied with implicit edges instead of an edge list.

**Structural Variations:**

- **A. Flood Fill / Connected Components on Grid (DFS/BFS):** Same as pure traversal, just with 4-directional/8-directional neighbor generation instead of an adjacency list. (Number of Islands — LC 200)
- **B. Shortest Path on Grid (BFS or Dijkstra depending on weights):** If cells have uniform cost, BFS; if cells have variable cost/effort, Dijkstra. (Path With Minimum Effort — LC 1631)

# Part III: State Storage & Performance Optimizations

Master these last. Advanced state-tracking wrappers layered on top of the engines above.

## 11. Union-Find with Rollback / Offline Processing

**Usage:** Queries about connectivity need to be answered out of order (offline), often processed from a different direction than they arrived (e.g., process deletions in reverse as additions).

**Giveaway:** "Queries arrive over time" combined with "answer as of query i," especially with edge removals (which basic Union-Find can't do directly — so you reverse the process).

## 12. State-Expanded Shortest Path (Layered Graph)

**Usage:** Shortest path where the "state" is more than just your current node — e.g., number of keys collected, obstacles removed used, or parity of some condition.

**Giveaway:** BFS/Dijkstra problem where the answer depends on more than location alone — you need visited[node][extra_state] instead of just visited[node]. (Shortest Path to Get All Keys — LC 864, Shortest Path in a Grid with Obstacles Elimination — LC 1293)

## 13. Bitmask + Graph (Hamiltonian-Style Compression)

**Usage:** Finding optimal orderings/visits across graph nodes where you must track exactly which nodes have been visited — this is the graph-flavored version of Bitmask DP from your DP list.

**Giveaway:** Small node count (N ≤ ~15-20) plus "visit all nodes" phrasing on a graph rather than a plain array. (Shortest Path Visiting All Nodes — LC 847)

## 14. Euler Path/Circuit (Edge-Traversal-Once Engine)

**Usage:** Traversing every edge exactly once (not every node), reconstructing a valid sequence (e.g., flight itineraries, domino chains).

**Giveaway:** "Use every ticket/domino/edge exactly once" — this is edge-centric rather than node-centric, which is the key tell that separates it from every pattern above.

**Structural Variations:**

- **A. Hierholzer's Algorithm:** DFS that only backtracks and records a node once it has no more unused outgoing edges, then reverses the recorded order. (Reconstruct Itinerary — LC 332)
