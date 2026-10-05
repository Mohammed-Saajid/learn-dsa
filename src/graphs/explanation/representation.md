# Graph Representations

A graph $G = (V, E)$ consists of a set of vertices $V$ and a set of edges $E$. How a graph is represented in memory determines the space complexity and the time complexity of fundamental graph operations (e.g., searching for neighbors, inserting edges, checking for edge existence).

The two primary memory representations are **Adjacency Matrix** and **Adjacency List**, alongside alternative representations such as **Edge List** and **Compressed Sparse Row (CSR)**.

---

## 1. Adjacency Matrix

An **Adjacency Matrix** is a 2D grid/array of size $V \times V$, where the cell at row $i$ and column $j$ indicates whether an edge exists between vertex $i$ and vertex $j$.

### Key Properties

* **Unweighted Graph:** $A[i][j] = 1$ if $(i, j) \in E$, else $0$.
* **Weighted Graph:** $A[i][j] = w$ if edge $(i, j)$ has weight $w$, else $\infty$ (or $0$).
* **Undirected Graph:** The matrix is symmetric across the main diagonal ($A[i][j] = A[j][i]$).
* **Directed Graph:** The matrix is asymmetric in general.

### C++ Implementation

```cpp
#include <iostream>
#include <vector>

class GraphMatrix {
private:
    int numVertices;
    bool isDirected;
    std::vector<std::vector<int>> matrix;

public:
    GraphMatrix(int vertices, bool directed = false)
        : numVertices(vertices), isDirected(directed), 
          matrix(vertices, std::vector<int>(vertices, 0)) {}

    void addEdge(int u, int v, int weight = 1) {
        matrix[u][v] = weight;
        if (!isDirected) {
            matrix[v][u] = weight;
        }
    }

    bool hasEdge(int u, int v) const {
        return matrix[u][v] != 0;
    }

    void print() const {
        for (int i = 0; i < numVertices; ++i) {
            for (int j = 0; j < numVertices; ++j) {
                std::cout << matrix[i][j] << " ";
            }
            std::cout << "\n";
        }
    }
};

```

---

## 2. Adjacency List

An **Adjacency List** uses an array or `std::vector` of length $V$, where each element is a collection (such as `std::vector` or `std::list`) containing all adjacent target vertices.

### Key Properties

* **Space-Efficient for Sparse Graphs:** Stores only actual edges rather than potential pairs.
* **Cache Behavior:** Using dynamic arrays (`std::vector<std::vector<T>>`) offers better cache locality than linked lists (`std::list`).

### C++ Implementation

```cpp
#include <iostream>
#include <vector>
#include <utility>

class GraphList {
private:
    int numVertices;
    bool isDirected;
    // Stores pair<neighbor, weight>
    std::vector<std::vector<std::pair<int, int>>> adj;

public:
    GraphList(int vertices, bool directed = false)
        : numVertices(vertices), isDirected(directed), adj(vertices) {}

    void addEdge(int u, int v, int weight = 1) {
        adj[u].push_back({v, weight});
        if (!isDirected) {
            adj[v].push_back({u, weight});
        }
    }

    const std::vector<std::pair<int, int>>& getNeighbors(int u) const {
        return adj[u];
    }

    void print() const {
        for (int i = 0; i < numVertices; ++i) {
            std::cout << "Vertex " << i << ":";
            for (const auto& edge : adj[i]) {
                std::cout << " -> (" << edge.first << ", w:" << edge.second << ")";
            }
            std::cout << "\n";
        }
    }
};

```

---

## 3. Alternative Representations

### A. Edge List

An unordered array/vector of all edges represented as tuples `(u, v)` or `(u, v, weight)`.

* **Space Complexity:** $O(E)$
* **Primary Use Cases:** Kruskal's Minimum Spanning Tree algorithm, Bellman-Ford algorithm, edge sorting.

```cpp
struct Edge {
    int src;
    int dest;
    int weight;
};

using EdgeList = std::vector<Edge>;

```

### B. Compressed Sparse Row (CSR)

A low-level, high-performance representation that flattens an adjacency list into two static array buffers:

1. **`row_ptr` (size $V + 1$):** Index offsets indicating where the outgoing edges for each vertex start in `col_ind`.
2. **`col_ind` (size $E$):** Stores target destination vertices consecutively.

* **Space Complexity:** $O(V + E)$ in contiguous memory.
* **Primary Use Cases:** GPU Graph Analytics (CUDA), High-Performance Computing (HPC), static graph processing.

---

## Structural & Algorithmic Comparison

| Operations & Characteristics | Adjacency Matrix | Adjacency List | Edge List | CSR (Compressed Sparse Row) |
| --- | --- | --- | --- | --- |
| **Space Complexity** | $O(V^2)$ | $O(V + E)$ | $O(E)$ | $O(V + E)$ (contiguous) |
| **Check Edge Existence $(u, v)$** | $O(1)$ | $O(\text{deg}(u))$ | $O(E)$ | $O(\log(\text{deg}(u)))$ or $O(\text{deg}(u))$ |
| **Find All Neighbors of $u$** | $O(V)$ | $O(\text{deg}(u))$ | $O(E)$ | $O(\text{deg}(u))$ |
| **Add Edge** | $O(1)$ | $O(1)$ | $O(1)$ | $O(V + E)$ (requires rebuild) |
| **Remove Edge** | $O(1)$ | $O(\text{deg}(u))$ | $O(E)$ | $O(V + E)$ |
| **Cache Locality** | Fair | Moderate | Excellent | Optimal |
| **Optimal Graph Density** | Dense ($E \approx V^2$) | Sparse ($E \ll V^2$) | Any | Static / Read-Heavy |

---

## Selection Criteria Guidelines

1. **Use Adjacency List when:**
* Graph is sparse ($E \ll V^2$).
* Frequent iteration over vertex neighbors is required (BFS, DFS, Dijkstra).
* Graph structure undergoes dynamic edge insertions.


2. **Use Adjacency Matrix when:**
* Graph is dense ($E \approx V^2$).
* Fast edge existence lookups ($O(1)$) are essential.
* Vertex count $V$ is small, fitting easily within cache bounds.


3. **Use Edge List when:**
* Processing edges globally without needing adjacency lookups (e.g., Kruskal's algorithm).


4. **Use CSR when:**
* Graph topology is static and read-heavy.
* Target hardware requires high memory bandwidth efficiency or continuous memory access patterns (e.g., vector units, GPUs).