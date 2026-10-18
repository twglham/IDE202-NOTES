# Data Structures & Algorithms

---

## Module 1 — Algorithm Analysis

**Idea:** measure how an algorithm's cost grows as input size `n` grows, not raw seconds.

- **Time complexity** = number of basic operations; **space complexity** = extra memory used.
- **Big-O (O)** = upper bound (worst case). **Omega (Ω)** = lower bound. **Theta (Θ)** = tight bound.
- Common growth order (small → big):
  `O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(2ⁿ) < O(n!)`
- Rules: drop constants (`3n → n`), drop lower terms (`n² + n → n²`).
- Loop cost: one loop = `n`, nested loop = `n²`, loop that halves each time = `log n`.
- Cases: best / average / worst. Usually we quote **worst**.

---

## Module 2 — Arrays & Linked Lists

**Array:** contiguous memory, index access `O(1)`, insert/delete in the middle `O(n)` (shifting).

**Linked list:** nodes with `data + next`. No shifting, but no random access.

| Op | Array | Singly LL |
|---|---|---|
| Access by index | O(1) | O(n) |
| Insert at head | O(n) | O(1) |
| Insert at tail | O(1)* | O(n) (O(1) with tail ptr) |
| Search | O(n) | O(n) |

\*amortized for dynamic arrays.

**Types:** singly, doubly (`prev` + `next`), circular.

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def insert_head(head, data):
    n = Node(data)
    n.next = head
    return n
```

**Classic tricks:** reverse a list (3 pointers: prev, curr, next), find middle (slow/fast pointer), detect cycle (Floyd's).

---

## Module 3 — Stacks & Queues

**Stack (LIFO):** `push`, `pop`, `peek` — all `O(1)`.
- Uses: undo, function call stack, balanced brackets, infix → postfix, DFS.

**Queue (FIFO):** `enqueue`, `dequeue` — `O(1)`.
- Uses: scheduling, BFS, buffering.
- Variants: **circular queue** (reuses space), **deque** (both ends), **priority queue** (highest priority out first).

```python
# balanced brackets
def balanced(s):
    pairs = {')':'(', ']':'[', '}':'{'}
    st = []
    for c in s:
        if c in '([{': st.append(c)
        elif c in pairs:
            if not st or st.pop() != pairs[c]: return False
    return not st
```

---

## Module 4 — Recursion & Searching

**Recursion:** function calls itself. Needs a **base case** + **recursive case**.
- Each call sits on the call stack → too deep = stack overflow.
- Recurrence examples: `T(n) = T(n-1) + 1 → O(n)`, `T(n) = T(n/2) + 1 → O(log n)`.

**Searching**
- **Linear search:** `O(n)`, works on anything.
- **Binary search:** `O(log n)`, needs **sorted** data.

```python
def binary_search(a, x):
    lo, hi = 0, len(a) - 1
    while lo <= hi:
        mid = (lo + hi) // 2
        if a[mid] == x: return mid
        if a[mid] < x: lo = mid + 1
        else: hi = mid - 1
    return -1
```

---

## Module 5 — Sorting

| Algorithm | Best | Average | Worst | Stable? | Notes |
|---|---|---|---|---|---|
| Bubble | O(n) | O(n²) | O(n²) | Yes | simple, slow |
| Selection | O(n²) | O(n²) | O(n²) | No | fewest swaps |
| Insertion | O(n) | O(n²) | O(n²) | Yes | great for small / nearly sorted |
| Merge | O(n log n) | O(n log n) | O(n log n) | Yes | divide & conquer, O(n) extra space |
| Quick | O(n log n) | O(n log n) | O(n²) | No | in-place, pivot choice matters |

- **Merge sort:** split in half → sort each → merge.
- **Quick sort:** pick pivot → partition (smaller left, bigger right) → recurse. Bad pivot (already sorted + first element) → `O(n²)`.
- **Stable** = equal elements keep their original order.
- Comparison-based sorting can't beat `O(n log n)`.

---

## Module 6 — Trees & Binary Search Trees

**Tree terms:** root, parent, child, leaf, height, depth, level.

**Binary tree:** each node has ≤ 2 children.

**Traversals**
- Inorder: Left → Root → Right (gives **sorted** order in a BST)
- Preorder: Root → Left → Right
- Postorder: Left → Right → Root
- Level order: BFS using a queue

**BST rule:** left subtree < node < right subtree.
- Search / insert / delete: `O(h)` → `O(log n)` if balanced, `O(n)` if skewed.
- **Delete cases:** leaf (just remove), one child (replace with child), two children (replace with inorder successor).

```python
def inorder(node):
    if node:
        inorder(node.left)
        print(node.val)
        inorder(node.right)
```

---

## Module 7 — Balanced Trees (AVL) & Heaps

**AVL tree:** BST that keeps itself balanced.
- **Balance factor** = height(left) − height(right) → must be −1, 0, or 1.
- Fix imbalance with **rotations**:
  - LL case → right rotation
  - RR case → left rotation
  - LR case → left then right
  - RL case → right then left
- Guarantees `O(log n)` for search/insert/delete.

**Heap:** complete binary tree stored in an array.
- **Max-heap:** parent ≥ children. **Min-heap:** parent ≤ children.
- Index formulas: parent `(i-1)//2`, left `2i+1`, right `2i+2`.
- Insert / extract-max: `O(log n)`. Peek: `O(1)`. Build heap: `O(n)`.
- Uses: priority queue, **heap sort** (`O(n log n)`).

---

## Module 8 — Hashing

**Hash table:** key → hash function → index → value. Average `O(1)` for search/insert/delete.

- **Good hash function:** fast, spreads keys evenly. e.g. `h(k) = k mod m`.
- **Collision** = two keys map to the same index.
  - **Chaining:** each slot holds a linked list.
  - **Open addressing:** find another slot — linear probing, quadratic probing, double hashing.
- **Load factor** `α = n / m`. Higher α → more collisions → resize (rehash) when it gets too high.
- Worst case (everything collides): `O(n)`.

---

## Module 9 — Graphs & Traversals

**Graph** `G = (V, E)`. Directed / undirected, weighted / unweighted.

**Representations**
- **Adjacency matrix:** `O(V²)` space, edge check `O(1)`. Good for dense graphs.
- **Adjacency list:** `O(V + E)` space. Good for sparse graphs (most common).

**BFS** (queue): level by level. Finds **shortest path in unweighted** graphs. `O(V + E)`.

**DFS** (stack / recursion): go deep, then backtrack. Used for cycle detection, topological sort, connected components. `O(V + E)`.

```python
from collections import deque
def bfs(graph, start):
    seen, q = {start}, deque([start])
    while q:
        u = q.popleft()
        for v in graph[u]:
            if v not in seen:
                seen.add(v); q.append(v)
```

---

## Module 10 — Shortest Paths & Spanning Trees

**Dijkstra:** single-source shortest path, **non-negative weights only**. Uses a min-heap. `O((V + E) log V)`.
- Idea: always pick the unvisited node with the smallest distance, relax its edges.

**Bellman-Ford:** handles **negative weights**, detects negative cycles. `O(V·E)`.

**Minimum Spanning Tree (MST):** connects all vertices with min total weight, no cycles (`V − 1` edges).
- **Prim:** grow the tree from one node, always add the cheapest edge out.
- **Kruskal:** sort all edges, add smallest that doesn't form a cycle (uses Union-Find).

**Topological sort:** linear ordering of a DAG where every edge `u → v` has `u` before `v`.


