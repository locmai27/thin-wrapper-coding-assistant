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

### Recognizing DP sub-type

- Bounds N ≤ 20 with subsets → **Bitmask DP**
- "How many numbers in [A, B] satisfy…" → **Digit DP**
- dp[l][r] with merging → **Range DP**
- Minimizing sum of absolute differences → **Slope Trick**
- Linear recurrence, N ≤ 10¹⁸ → **Matrix Exponentiation**
- Transition is min/max over linear functions → **Convex Hull Trick**

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

---

## Prefix Sums (66)

| Technique | When to use |
|---|---|
| **1D Prefix Sums** | Range sum queries on static array; O(1) per query after O(N) build |
| **2D Prefix Sums (9)** | Submatrix sum queries; O(1) per query after O(NM) build |
| **Difference Array (4)** | Range updates, point queries; inverse of prefix sums |
| **Prefix XOR / Prefix GCD** | Generalize to other associative operations |
| **Prefix + Binary Search** | Find shortest/longest subarray with sum ≥ K (if values non-negative) |

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

### Shortest Paths
| Technique | When to use |
|---|---|
| **BFS** | Unweighted or 0/1 weights (0-1 BFS with deque) |
| **Dijkstra (SP 24)** | Non-negative weights; O((V+E) log V) with priority queue |
| **Bellman-Ford** | Negative weights; detect negative cycles |
| **SPFA** | Bellman-Ford with queue optimization (avoid in worst case) |
| **Floyd-Warshall (APSP 3)** | All-pairs shortest paths; O(V³), V ≤ 400 |

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
