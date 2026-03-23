# Algorithm Taxonomy

Organized by family. Numbers in parentheses are approximate problem frequency
from curated competitive programming problem sets. Use frequency to prioritize
which techniques to suggest first when multiple could apply.

When mapping a problem, start from the **family**, then drill into the
**specific technique** that matches the problem's structure.

---

## Dynamic Programming (185+)

The largest family. Key question: "Can I define a state such that the answer
to the full problem is built from answers to smaller subproblems?"

| Technique | When to use |
|---|---|
| **Linear DP** | Process elements left-to-right, state depends on position and a few parameters |
| **Knapsack (26)** | Select items with weight/value constraints; bounded or unbounded variants |
| **Range DP (12)** | Optimal merging/splitting of intervals; state is dp[l][r] |
| **Bitmask DP (37)** | N ≤ 20, need to track subset membership; state includes a bitmask |
| **Digit DP** | Count/sum numbers in [0, N] satisfying a digit property; state tracks position, tight constraint, and accumulated info |
| **Tree DP** | DP on tree structure; root subtrees, re-rooting technique |
| **DP on DAG** | Topological order gives evaluation order |
| **SOS DP (8)** | Sum over subsets; iterate over bitmask dimensions |
| **Slope Trick (13)** | Maintain a convex cost function using priority queues; problems involving abs-value costs |
| **Broken Profile (4)** | Tile a grid row-by-row; state is the boundary between filled and unfilled |
| **Profile DP** | Similar to broken profile; state encodes the boundary configuration |
| **Convex Hull Trick / Li Chao** | Optimize DP transitions of form dp[i] = min(a[j]·x[i] + dp[j]); maintain convex hull of lines |
| **Divide and Conquer DP** | Optimization when opt[i] ≤ opt[i+1]; reduces O(N²) to O(N log N) |
| **Connection to other families** | DP on graphs (shortest paths), DP + SegTree (optimize transitions), DP + Matrix Expo (linear recurrence with large N) |

### Recognizing DP sub-type — signal → technique → why

**Linear DP**
- Signal: Process a sequence left-to-right; answer at position i depends on answers at earlier positions.
- Why: Each element extends or resets a running computation. Greedy fails because future choices depend on accumulated state.
- Next-time rule: "Array/string processed in order, answer depends on prefix state → Linear DP."

**Knapsack (26)**
- Signal: Choose a subset of items, each with a cost and a value, subject to a capacity constraint.
- Why: Greedy by ratio fails for integer items (0-1 knapsack). You need to track remaining capacity as state.
- Next-time rule: "Select items under a budget to maximize/minimize value → Knapsack DP."

**Range DP (12)**
- Signal: Merge or split contiguous segments optimally. The cost of combining [l, r] depends on how you split it.
- Why: There are O(N²) subproblems [l, r] and O(N) split points per subproblem. No greedy shortcut exists because the optimal split depends on the costs of both halves.
- Next-time rule: "Optimal way to merge/split a contiguous range, cost depends on partition → Range DP dp[l][r]."

**Bitmask DP (37)**
- Signal: N ≤ 20, and you need to track which elements have been used/visited/assigned.
- Why: There are 2^N subsets, each is a state. Iterating all subsets is feasible when N ≤ 20. Without the bitmask you'd need N! enumeration.
- Next-time rule: "Small N, track subset membership → Bitmask DP. State includes a bitmask of chosen elements."

**Digit DP**
- Signal: "Count/sum over all integers in [A, B] satisfying some property about their digits." The range is too large to iterate (up to 10¹⁸).
- Why: Instead of iterating every number, process digit-by-digit from most significant to least. Track whether you're still "tight" against the upper bound's digits. Decompose [A, B] into f(B) − f(A−1).
- Next-time rule: "How many numbers in [A, B] have [digit property]? → Digit DP. State: dp[position][tight][accumulated_info]."

**Tree DP**
- Signal: Compute something for each subtree; the answer for a node depends on the answers for its children.
- Why: Trees have no cycles, so subtrees are independent subproblems. Process leaves-first (post-order DFS).
- Next-time rule: "Optimize/count over subtrees of a rooted tree → Tree DP. Consider re-rooting if you need answers for all roots."

**SOS DP (8)**
- Signal: For each bitmask, compute the sum/min/max over all its submasks (or supermasks).
- Why: Naively iterating submasks of all masks is O(3^N). SOS DP does it in O(N · 2^N) by iterating one bit dimension at a time.
- Next-time rule: "Sum over all subsets/supersets of each mask → SOS DP."

**Slope Trick (13)**
- Signal: Minimize a sum of absolute value or piecewise-linear costs; the cost function stays convex after each step.
- Why: The optimal cost function can be represented by its slope-change points, maintained with a priority queue. Adding |x − a| shifts the breakpoints.
- Next-time rule: "Minimize total absolute-value cost where each step adds a V-shape → Slope Trick."

**Convex Hull Trick / Li Chao**
- Signal: DP transition looks like dp[i] = min(dp[j] + a[j]·x[i] + ...) — the transition is a linear function of x[i] parameterized by j.
- Why: Each previous state j defines a line; you need the minimum over all lines at query point x[i]. Maintaining a convex hull of lines answers each query in O(log N).
- Next-time rule: "DP transition is min/max over linear functions of the current index → Convex Hull Trick."

**Divide and Conquer DP**
- Signal: O(N²) DP where the optimal split point opt[i] is monotone (opt[i] ≤ opt[i+1]).
- Why: Monotonicity means you can binary-search the split point range, reducing total work to O(N log N).
- Next-time rule: "Quadratic DP with monotone optimal split → D&C DP optimization."

---

## Binary Search (93)

Key question: "Is the answer monotonic — if X works, does X+1 also work (or vice versa)?"

| Technique | When to use |
|---|---|
| **Binary search on answer** | "What is the minimum/maximum value such that [condition]?" — binary search the answer, check feasibility |
| **Binary search on sorted array** | Find insertion point, lower/upper bound |
| **Binary search + greedy check** | Combine BS on answer with a greedy feasibility check |
| **Ternary Search (8)** | Unimodal function; find the peak or valley |
| **Parallel Binary Search** | Multiple queries, each needs its own binary search; batch them |
| **Fractional Binary Search** | Binary search on real-valued answer (use eps or fixed iterations) |

### Recognition patterns

**Binary search on answer**
- Signal: "What is the minimum/maximum X such that we can achieve [goal]?" and there is a clear yes/no feasibility check for a given X.
- Why: The answer space is monotonic — if X is feasible, all larger (or smaller) values are too. This lets you halve the search space each step instead of trying every value.
- Why not brute force: The answer range can be up to 10⁹ or 10¹⁸; iterating is infeasible, but O(log N) binary search with an O(N) or O(N log N) check is fast.
- Next-time rule: "Minimize/maximize a value where feasibility is monotonic → binary search on answer + feasibility check."

**Ternary Search (8)**
- Signal: A function that first increases then decreases (or vice versa) — unimodal. You need the peak/valley.
- Why: Binary search needs monotonicity; ternary search works on unimodal functions by comparing two interior points to eliminate one-third of the range each step.
- Next-time rule: "Find the extremum of a unimodal function → Ternary Search."

---

## Sorting & Order (77)

| Technique | When to use |
|---|---|
| **Sort + Greedy** | Greedy exchange argument works after sorting by some key |
| **Sort + Two Pointers** | Find pairs/triples satisfying a condition |
| **Sort + Binary Search** | After sorting, binary search for complements or thresholds |
| **Inversions (6)** | Count pairs (i,j) with i<j, a[i]>a[j]; merge sort or BIT |
| **Coordinate Compression (3)** | Map large value range to [0, N); prerequisite for BIT/SegTree on values |
| **Custom comparators** | Sort by complex criteria (e.g., angles, ratios) |

---

## Greedy (73)

Key question: "Can I prove that making the locally optimal choice never hurts the global solution?"

| Technique | When to use |
|---|---|
| **Exchange argument** | Show swapping any two elements in a non-greedy solution doesn't improve it |
| **Sort by deadline/end time** | Interval scheduling, job scheduling |
| **Sort by ratio** | Fractional knapsack, weighted job scheduling |
| **Huffman-style merging** | Merge smallest elements first |
| **Greedy + Priority Queue** | Process events in order, greedily pick best available |

### Recognition patterns

**Sort by end time**
- Signal: Select the maximum number of non-overlapping intervals.
- Why: Picking the interval that ends earliest always leaves the most room for future intervals. You can prove this by exchange argument — swapping in an interval that ends later never increases the total count.
- Why not DP: DP works too, but greedy is O(N log N) with simpler code. Use DP instead if intervals have weights.
- Next-time rule: "Maximize count of non-overlapping intervals → sort by end time, greedily pick."

**Exchange argument (general)**
- Signal: You suspect greedy works but need to prove it. If you can show that swapping any element in an optimal solution with the greedy choice does not worsen the result, greedy is correct.
- Next-time rule: "If you can't construct a counterexample and the exchange argument holds → Greedy. If you find a counterexample → fall back to DP."

---

## Prefix Sums (66)

| Technique | When to use |
|---|---|
| **1D Prefix Sums** | Range sum queries on static array; O(1) per query after O(N) build |
| **2D Prefix Sums (9)** | Submatrix sum queries; O(1) per query after O(NM) build |
| **Difference Array (4)** | Range updates, point queries; inverse of prefix sums |
| **Prefix XOR / Prefix GCD** | Generalize to other associative operations |
| **Prefix + Binary Search** | Find shortest/longest subarray with sum ≥ K (if values non-negative) |

### Recognition patterns

**1D Prefix Sums**
- Signal: Many queries asking "what is the sum of elements from index l to r?" on a static (unchanging) array.
- Why: Building prefix[i] = a[0]+...+a[i-1] once lets you answer sum(l,r) = prefix[r+1] − prefix[l] in O(1). Without this, each query costs O(N).
- Why not SegTree: If the array never changes, prefix sums are simpler and faster. Use SegTree/BIT only when you also have updates.
- Next-time rule: "Static array, many range sum queries → Prefix Sums."

**Difference Array (4)**
- Signal: Many range updates "add X to all elements in [l, r]", then read the final array.
- Why: The inverse of prefix sums. Record +X at position l and −X at position r+1, then take prefix sums at the end to recover the final array. Each update is O(1) instead of O(N).
- Next-time rule: "Batch range additions, read final values → Difference Array."

---

## Trees (62)

| Technique | When to use |
|---|---|
| **DFS on tree** | Subtree queries, tree traversal |
| **Euler Tour (16)** | Flatten tree to array → use SegTree/BIT for subtree queries |
| **LCA (17)** | Lowest common ancestor; binary lifting O(log N) or Sparse Table O(1) |
| **Centroid Decomposition (17)** | Divide-and-conquer on trees; path queries, counting pairs |
| **HLD (10)** | Heavy-light decomposition; path queries + updates with SegTree |
| **LCT (17)** | Link-Cut Trees; dynamic connectivity, path queries on dynamic forest |
| **Binary Jumping / Lifting (9)** | k-th ancestor, LCA, functional graph cycle detection |
| **Small to Large (8)** | Merge smaller sets into larger (DSU on tree / Euler tour merge) |
| **Virtual Tree (5)** | Build compressed tree on subset of key nodes |
| **Re-rooting** | Compute answer for all roots from answer at one root in O(N) |
| **Tree Diameter** | Two BFS/DFS or DP to find the longest path |

### Recognition patterns

**Euler Tour (16)**
- Signal: Subtree queries on a tree (sum, min, count in a subtree), possibly with updates.
- Why: The Euler tour assigns each node an entry and exit time. All nodes in a subtree form a contiguous range [entry, exit] in the tour array. This turns subtree queries into range queries on an array, solvable with SegTree or BIT.
- Why not HLD: HLD handles path queries; Euler tour handles subtree queries. If you need both, you can combine them.
- Next-time rule: "Subtree aggregate queries → Euler Tour + SegTree/BIT."

**Centroid Decomposition (17)**
- Signal: Count or aggregate over all paths in a tree, or answer path-distance queries efficiently.
- Why: The centroid splits the tree into subtrees of at most N/2 size. Recursively decomposing gives O(log N) layers. Every path passes through the centroid of some level, so you process paths through each centroid and combine.
- Next-time rule: "Count pairs of nodes by path property, or answer distance queries on tree → Centroid Decomposition."

**HLD (10)**
- Signal: Path queries with updates on a tree (e.g., "update edge weights on path u→v, query sum on path").
- Why: HLD decomposes the tree into O(log N) heavy chains. Each chain is a contiguous segment in an array, so path queries become O(log² N) SegTree queries.
- Next-time rule: "Path queries + updates on a tree → HLD + SegTree."

**Re-rooting**
- Signal: "Compute the answer for every node as root" — e.g., sum of distances from each node to all others.
- Why: Computing from scratch for each root is O(N²). Instead, compute for one root, then "re-root" in O(1) per edge by adjusting the contribution of the parent and child subtrees.
- Next-time rule: "Answer for all roots, answer changes predictably when moving root to neighbor → Re-rooting DP."

---

## Graph Algorithms

### Traversal & Connectivity
| Technique | When to use |
|---|---|
| **BFS (30)** | Shortest path in unweighted graph, level-order traversal |
| **DFS (29)** | Cycle detection, topological sort, connected components, bridges/articulation points |
| **Flood Fill (16)** | Connected regions in a grid |
| **Connected Components (17)** | Find components via DFS/BFS or DSU |
| **Bipartite (10)** | 2-coloring check via BFS/DFS |
| **SCC (12)** | Strongly connected components; Tarjan's or Kosaraju's |
| **BCC (10)** | Biconnected components; block-cut tree |
| **2SAT (7)** | Boolean satisfiability with clauses of size 2; implication graph + SCC |
| **TopoSort (12)** | Ordering of DAG; Kahn's (BFS) or DFS-based |
| **Functional Graph (13)** | Each node has exactly one outgoing edge; cycle detection + binary lifting |

**Key graph recognition patterns:**
- "Can I reach X from Y?" → BFS/DFS or DSU
- "Are there cycles?" → DFS (back edge detection) or Functional Graph (each node has out-degree 1)
- "Partition into two groups with constraints" → Bipartite check or 2SAT
- "Collapse cycles into super-nodes, then process DAG" → SCC + TopoSort
- "Grid with connected regions" → Flood Fill (BFS/DFS on grid)

### Shortest Paths
| Technique | When to use |
|---|---|
| **BFS** | Unweighted or 0/1 weights (0-1 BFS with deque) |
| **Dijkstra (SP 24)** | Non-negative weights; O((V+E) log V) with priority queue |
| **Bellman-Ford** | Negative weights; detect negative cycles |
| **SPFA** | Bellman-Ford with queue optimization (avoid in worst case) |
| **Floyd-Warshall (APSP 3)** | All-pairs shortest paths; O(V³), V ≤ 400 |

**How to pick the right shortest-path algorithm:**
- All edges weight 1 → **BFS**. Why not Dijkstra? BFS is simpler and O(V+E) vs O((V+E) log V).
- Edge weights 0 or 1 only → **0-1 BFS** with deque. Push weight-0 edges to front, weight-1 to back.
- Non-negative weights → **Dijkstra**. Why not Bellman-Ford? Dijkstra is faster: O((V+E) log V) vs O(VE).
- Negative weights → **Bellman-Ford**. Dijkstra fails because it assumes processed nodes are final; negative edges can improve them later.
- All pairs, V ≤ 400 → **Floyd-Warshall**. O(V³) with tiny constant; simpler than running Dijkstra V times.

### Minimum Spanning Tree (17)
| Technique | When to use |
|---|---|
| **Kruskal's** | Sort edges + DSU; works well for sparse graphs |
| **Prim's** | Priority queue; better for dense graphs |
| **Borůvka's** | Parallelizable; useful for Manhattan MST (2) |

### Flow & Matching
| Technique | When to use |
|---|---|
| **Max Flow (4)** | Maximum flow / minimum cut; Dinic's or push-relabel |
| **MCF (3)** | Minimum cost maximum flow; SPFA-based or potential method |
| **Bipartite Matching** | Hungarian algorithm, Hopcroft-Karp, or flow reduction |

---

## Data Structures

### Union-Find / DSU (34)
| Technique | When to use |
|---|---|
| **DSU with path compression + rank** | Online connectivity, Kruskal's MST |
| **DSU with rollback (5)** | Offline divide-and-conquer on edges; undo union operations |
| **Weighted DSU** | Track relative weights between elements (e.g., potential) |

### Segment Trees & Variants
| Technique | When to use |
|---|---|
| **Point Update Range Query — PURQ (19)** | Single element updates, range aggregate queries |
| **Point Update Range Sum — PURS (32)** | Specific case: range sums with point updates |
| **Lazy SegTree (15)** | Range updates + range queries; lazy propagation |
| **Persistent SegTree (13)** | Version history; query past states; functional updates |
| **SegTree Beats (10)** | Range chmin/chmax + range sum queries |
| **Sparse SegTree (1)** | Large coordinate range, few actual updates |
| **Merge Sort Tree** | SegTree where each node stores sorted list; count elements in range within value range |
| **2DRQ (12)** | 2D range queries; 2D SegTree, 2D BIT, or merge sort tree |

**How to pick the right range query structure:**
- Point updates + range queries → **BIT** (simpler) or **SegTree PURQ** (more flexible)
- Range updates + range queries → **Lazy SegTree**. Why not plain SegTree? Without lazy propagation, range updates cost O(N) instead of O(log N).
- Need to query past versions → **Persistent SegTree**. Each update creates a new root; old roots remain queryable.
- Range chmin/chmax combined with range sum → **SegTree Beats**. Standard lazy can't handle "set all values > X to X" efficiently.
- Static array, range queries only → **Prefix Sums** or **Sparse Table**. Don't use SegTree for problems that don't need updates.
- Next-time rule: "Updates + queries on ranges → SegTree family. No updates → Prefix Sums or Sparse Table."

### Binary Indexed Tree / Fenwick (9)
| Technique | When to use |
|---|---|
| **BIT** | Point update + prefix sum; simpler and faster constant than SegTree for this case |
| **2D BIT** | 2D point update + 2D prefix sum |
| **BIT with coordinate compression** | When value range is large but count of elements is small |

### Other Structures
| Technique | When to use |
|---|---|
| **Stack (25)** | Next greater/smaller element, monotonic stack, expression parsing |
| **Priority Queue (17)** | Greedy selection of min/max, Dijkstra, event simulation |
| **Sorted Set / Multiset (23)** | Ordered access, predecessor/successor queries |
| **Set (14)** | Membership, distinct elements |
| **Deque (4)** | Sliding window min/max, 0-1 BFS |
| **Trie (13)** | Prefix queries on strings; XOR maximum with binary trie |
| **Treap (9)** | Balanced BST with split/merge; implicit treap for array operations |
| **Wavelet Tree (6)** | k-th smallest in range, count of values in range; offline alternative to merge sort tree |
| **Sqrt Decomposition (21)** | Block-based queries; Mo's Algorithm (4) for offline range queries |

---

## Two Pointers & Sliding Window (48)

| Technique | When to use |
|---|---|
| **Two Pointers (25)** | Sorted array pair search, merging sorted lists, shrinking intervals |
| **Sliding Window (23)** | Fixed or variable-length window; maintain window aggregate |
| **Sliding Window + Deque** | Track min/max in sliding window in O(1) amortized |
| **Sliding Window + HashMap** | Track frequency / distinct count in window |

---

## Strings (17)

| Technique | When to use |
|---|---|
| **KMP (8)** | Pattern matching; failure function; period of a string |
| **Z-algorithm (3)** | Z-array: longest match starting at each position with prefix |
| **Hashing (18)** | Polynomial rolling hash; substring comparison in O(1) |
| **Suffix Array (5)** | All suffixes sorted; LCP array for substring queries |
| **Suffix Structures (10)** | Suffix tree / automaton for complex substring operations |
| **Palindromic Tree (2)** | Count distinct palindromic substrings; eertree |
| **Aho-Corasick** | Multiple pattern matching; trie + failure links |
| **Manacher's** | All palindromic substrings in O(N) |

---

## Math & Number Theory

| Technique | When to use |
|---|---|
| **Combinatorics (24)** | Counting; nCr with modular inverse; Stars and Bars |
| **Modular Arithmetic (7)** | Modular inverse, exponentiation; Fermat's little theorem |
| **Exponentiation (12)** | Fast power; matrix exponentiation for linear recurrences |
| **PIE (12)** | Principle of Inclusion-Exclusion; count union of sets |
| **Prime Factorization (8)** | Sieve of Eratosthenes, smallest prime factor sieve |
| **Divisibility (8)** | GCD/LCM, divisor enumeration, Euler's totient |
| **Catalan (4)** | Catalan numbers; parenthesizations, BST counts, grid paths |
| **FFT / NTT (5)** | Polynomial multiplication; convolution; large number multiplication |
| **Nimbers / Sprague-Grundy (11)** | Impartial games; XOR of Grundy values |
| **Game Theory (17)** | Win/lose states; minimax; alpha-beta |
| **Matrix (13)** | Matrix exponentiation, Gaussian elimination, determinant |

---

## Geometry (16)

| Technique | When to use |
|---|---|
| **Convex Hull (29)** | Graham scan, Andrew's monotone chain |
| **Sweep Line (9)** | Events sorted by x/y; line segment intersection, closest pair |
| **Radial Sweep (4)** | Sort by angle; problems involving angular ordering |
| **Point-in-polygon** | Ray casting, winding number |
| **Line intersection** | Cross product, parametric intersection |
| **Manhattan distance** | Rotate 45°; reduce to Chebyshev |

---

## Bitwise & Bitmasks

| Technique | When to use |
|---|---|
| **Bitmask enumeration (37)** | Iterate subsets; N ≤ 20 |
| **Bitwise operations (18)** | XOR properties, AND/OR prefix/suffix, contribution technique |
| **Bitset (13)** | Speed up boolean DP/reachability by factor of 64 |
| **SOS DP (8)** | Sum over subsets / supsets of a bitmask |
| **XOR Convolution (1)** | Walsh-Hadamard transform |
| **Binary Trie** | Maximum XOR pair, k-th XOR |

---

## Misc Techniques

| Technique | When to use |
|---|---|
| **Complete Search (38)** | N small enough for brute force; generate and test |
| **Simulation (29)** | Follow the process described; no shortcut needed |
| **Meet in the Middle (7)** | Split problem in two halves; combine results; N ≤ 40 |
| **Constructive (2)** | Build a valid solution directly; often involves invariants |
| **Ad Hoc (3)** | No standard algorithm; problem-specific insight |
| **Coordinate Compression (3)** | Map sparse large values to dense indices |
| **Offline processing (2)** | Answer queries in a different order than given (Mo's, offline + sweep) |
| **Random (6)** | Randomized hashing, random pivot, probabilistic checks |
| **LIS (6)** | Longest increasing subsequence; patience sorting / BIT in O(N log N) |

---

## Quick-reference: keyword → technique

| Problem keyword | First technique to consider |
|---|---|
| "subsequence" | DP, LIS, Bitmask DP |
| "subarray sum" | Prefix Sums, Sliding Window, Two Pointers |
| "minimum operations" | BFS (state-space), Greedy, DP |
| "number of ways" | DP, Combinatorics, Matrix Expo |
| "lexicographically smallest" | Greedy, Stack |
| "connected" | DSU, DFS/BFS |
| "diameter" | Tree: two BFS. Graph: BFS from furthest |
| "cycle" | DFS, Functional Graph, DSU |
| "palindrome" | Manacher, Palindromic Tree, Hashing |
| "common" (LCS/LCA) | DP (LCS), Binary Lifting (LCA) |
| "independent set" | Bitmask DP (small N), bipartite matching |
| "expected value" | Linearity of expectation, DP |
| "probability" | DP with probabilities, linearity of expectation |
| "permutation" | Inversions, Cycle decomposition, DP |
| "grid" | BFS/DFS, DP on grid, Flood Fill |
| "matrix power" | Matrix Exponentiation |
| "modulo" | Modular arithmetic, Fermat's theorem |
