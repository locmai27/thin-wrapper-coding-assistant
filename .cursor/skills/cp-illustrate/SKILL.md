---
name: cp-illustrate
description: >-
  Makes algorithm explanations visual and intuitive. Draws ASCII diagrams,
  trees, arrays, graphs, and state transitions to illustrate how algorithms
  work. Uses analogies, figurative language, and concrete visual traces to
  make abstract concepts tangible. Use when explaining algorithms, data
  structures, recursion, DP states, graph traversals, or any concept that
  benefits from being seen rather than described.
---

# Visual Algorithm Explainer

When explaining algorithms, data structures, or problem solutions, **show
don't tell**. Default to drawing diagrams, tracing state visually, and
using analogies — only fall back to pure prose when visuals genuinely
don't help.

## When to draw

Draw a diagram whenever the explanation involves any of:

- Arrays, strings, or sequences (show indices and values)
- Trees (show nodes, edges, subtree boundaries)
- Graphs (show vertices, edges, weights, traversal order)
- DP tables or recurrences (show the table filling order)
- Pointers or windows moving across data (two pointers, sliding window)
- Stack/queue contents changing over time
- Recursion trees or call stacks
- State machines or transitions
- Before/after comparisons (sorting, rotations, balancing)
- Interval or sweep-line events on a number line

If unsure whether a diagram helps, draw one. Err on the side of visual.

## Diagram style guide

### Arrays and strings

```
Index:   0   1   2   3   4   5
Value: [ 3 | 1 | 4 | 1 | 5 | 9 ]
         ^               ^
         L               R        ← two pointers
```

Show pointer positions, window boundaries, and highlights with `^`, `*`,
or bracket annotations beneath the array.

### Trees

```
        5
       / \
      3    8
     / \    \
    1   4    9
```

For binary trees, use `/` and `\`. For general trees, use indented
notation or box-drawing characters:

```
1
├── 2
│   ├── 4
│   └── 5
├── 3
│   └── 6
└── 7
```

Mark relevant nodes with annotations: `(root)`, `(LCA)`, `← centroid`,
`[visited]`, etc.

### Graphs

```
A ──3──> B ──1──> D
│                 ^
2                 4
│                 │
v                 │
C ───────5────────┘
```

Use `──>` for directed, `───` for undirected. Label weights on the edge.
For traversal, number the visit order:

```
A[1] ──> B[2] ──> D[4]
│
v
C[3]
```

### DP tables

```
     j:  0    1    2    3    4   (capacity)
i=0:  [  0 |  0 |  0 |  0 |  0 ]
i=1:  [  0 |  0 |  3 |  3 |  3 ]  ← item 1 (w=2, v=3)
i=2:  [  0 |  0 |  3 |  4 |  4 ]  ← item 2 (w=3, v=4)
i=3:  [  0 |  0 |  3 |  4 |  5 ]  ← item 3 (w=2, v=2)
                             ^
                          answer
```

Show the fill direction with arrows if it matters. Highlight the cell
being computed and which earlier cells it depends on:

```
dp[2][4] = max(dp[1][4], dp[1][1] + 4)
              = max(3,     0 + 4)
              = 4
```

### State transitions / recursion

```
digitDP(pos=0, tight=T, sum=0)
├── digit=0 → (pos=1, tight=T, sum=0)
│   ├── digit=0 → (pos=2, tight=F, sum=0) → ...
│   └── digit=1 → (pos=2, tight=F, sum=1) → ...
├── digit=1 → (pos=1, tight=T, sum=1)
│   └── ...
└── digit=2 → (pos=1, tight=F, sum=2)
    └── ...
```

### Sweep line / timeline

```
Events on number line:
   0    2    4    6    8   10
   |----|----|----|----|----|--->
   [====A=====]
        [=====B==========]
             [==C==]
   
Active intervals at x=5: {A, B, C}
```

### Before / after

```
Before rotation:          After right-rotation:
      5                        3
     / \                      / \
    3    7        →          1    5
   / \                          / \
  1   4                        4    7
```

## Figurative language and analogies

Use analogies to make abstract concepts stick:

- **Stack**: "Like a stack of plates — you can only take from the top.
  Last plate placed is the first one removed."
- **BFS vs DFS**: "BFS explores like ripples in a pond — everything at
  distance 1, then distance 2, then 3. DFS explores like a maze runner
  who always goes as deep as possible before backtracking."
- **Binary search**: "Like the number-guessing game — each guess
  eliminates half the remaining possibilities."
- **DP memoization**: "Like writing answers on sticky notes so you never
  solve the same subproblem twice."
- **Greedy**: "Like always picking the biggest coin first when making
  change — works for US coins, fails for arbitrary denominations."
- **Segment tree**: "Like a tournament bracket — each internal node
  summarizes its two children, so you can query any range by combining
  O(log N) bracket nodes."
- **DSU**: "Like merging friend groups — once two groups merge, everyone
  in both groups is connected. The 'leader' is whoever you reach by
  following the chain of introductions."

Use analogies to **introduce** a concept, then switch to precise technical
language. Never let the analogy replace the real explanation.

## Tracing algorithms visually

When explaining how an algorithm processes input, **trace it step by step
visually**. Show the state after each step, not just the final result.

Example — two pointers finding a pair summing to 10:

```
Sorted: [ 1 | 3 | 5 | 6 | 8 | 9 ]

Step 1:  [ 1 | 3 | 5 | 6 | 8 | 9 ]
           ^                   ^
           L=0              R=5    sum = 1+9 = 10 ✓ Found!
```

Example — BFS layer by layer:

```
Start: A          Queue: [A]

Layer 0:  A*       Queue after: [B, C]
Layer 1:  B* C*    Queue after: [D, E]
Layer 2:  D* E*    Queue after: []

Visit order: A → B → C → D → E
```

## ML-specific diagram templates

### Matrix multiplication

```
[1 2]     [5 6]     [1·5+2·7  1·6+2·8]     [19 22]
[3 4]  ×  [7 8]  =  [3·5+4·7  3·6+4·8]  =  [43 50]

  A    ×    B    =        row·col          =    AB
```

Show the dot product of each row-column pair explicitly for small matrices.

### Vector projection

```
        b
       /|
      / |
     /  |  r = b - p  (residual, perpendicular)
    /   |
   /    |
  O─────p──────→ subspace (column space of A)
        ^
     projection of b
```

Label: original vector b, its shadow p on the subspace, and the residual r
pointing straight up from p to b.

### Scatter plot with principal components

```
     ·  ·                     PC1
  ·  ·  · ·              ╱
 ·  ·  ·  · ·          ╱
  · ·  ·  · ·        ╱
   ·  ·  ·  ·      ╱
    · · · ·       ╱
     · ·        ╱
              ╱
    ──────────────── PC2
```

Draw the data cloud, then overlay the principal component directions as
arrows through the centroid, with PC1 along the longest axis.

### Decision boundary

```
    Class +          │          Class -
                     │
   +    +    +       │       -    -
     +     +    +    │    -     -
   +    +       +    │  -    -     -
      +    +         │     -    -
                     │
              decision boundary (hyperplane)
```

### Confusion matrix

```
                   Predicted
              +              -
         ┌──────────┬──────────┐
    +    │  TP = 45  │  FN = 5   │  Recall = 45/50 = 90%
Actual   ├──────────┼──────────┤
    -    │  FP = 10  │  TN = 40  │
         └──────────┴──────────┘
           Prec = 45/55 = 82%
```

### Eigenvalue / eigenvector visualization

```
Before A applied:        After A applied:
     ↑ v₂                     ↑ λ₂·v₂ (stretched by λ₂)
     │                        │
     │                        │
 ────┼────→ v₁           ─────┼──────→ λ₁·v₁ (stretched by λ₁)
     │                        │
```

Show eigenvectors as arrows before and after the matrix is applied —
they stay in the same direction but change length.

### Gradient descent path

```
   Loss
    │  ·
    │   ·  ← start
    │    ·
    │     ·  step 1
    │      ·
    │       · step 2
    │        ·
    │         · step 3
    │          ·_____· ← converged (minimum)
    └──────────────────── weight
```

### Concept dependency map

```
Eigenvalues ──→ Spectral Theorem ──→ PCA
                      │                │
                      ▼                ▼
              Covariance Matrix    Kernel PCA
```

Use `──→` for prerequisites, `···>` for "related to."

## When NOT to draw

- Pure math derivations (use equations instead)
- One-line answers ("the complexity is O(N log N)")
- When the user explicitly asks for a concise text answer

## Interaction with other skills

This skill governs **presentation style**, not problem-solving strategy.
Combine it with:
- `cp-brainstorm` — draw diagrams while exploring approaches
- `cp-coach` — illustrate the algorithm being mapped to
- `ml-intuition` — visualize math/ML concepts with analogies
- `ml-connect` — draw concept dependency maps
- `ml-solve` — show step-by-step computations with matrix layouts

The visual style applies on top of whatever reasoning framework the other
skill provides.
