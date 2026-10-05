## Graph Data Structure Overview

A **Graph** is a non-linear data structure consisting of a finite set of **Vertices** (or Nodes) and **Edges** that connect pairs of vertices.

* **Mathematical Representation:** $G = (V, E)$
* $V$: Set of vertices $V = \{v_1, v_2, \dots, v_n\}$
* $E$: Set of edges $E = \{(u, v) \mid u, v \in V\}$



---

## Core Terminology

* **Vertex (Node):** An individual data point or entity.
* **Edge (Arc):** A link or connection between two vertices.
* **Degree of a Vertex:** The degree, written as $(deg(v))$, is the number of edges connected to the vertex.
    * **Undirected Graph:** The total number of edges connected to the vertex.
    * **Directed Graph:**
        * **In-degree:** Number of incoming edges directed toward the vertex.
        * **Out-degree:** Number of outgoing edges leaving the vertex.
* **Path:** A sequence of vertices where consecutive pairs are connected by an edge.
* **Cycle:** A path that starts and ends at the same vertex without repeating edges.
* **Self-Loop:** An edge that connects a vertex to itself.
* **Parallel Edges:** Two or more edges connecting the same pair of vertices.

---

## Graph Classification

```
                             ┌─────────────────┐
                             │   Graph Types            │
                             └────────┬────────┘
                                                  │
         ┌─────────────────┴─────────────────┐
         ▼                                                                                 ▼
  ┌──────────────┐                                    ┌──────────────┐
  │  Direction          │                                    │    Weights             │
  └──────┬───────┘                                    └──────┬───────┘
         ├──────────────┐                             ├──────────────┐
         ▼                                ▼                             ▼                                 ▼
   Undirected        Directed           Unweighted              Weighted

```

### 1. By Directionality

* **Undirected Graph:** Edges have no direction; connections are bidirectional ($u \to v$ implies $v \to u$).
* **Directed Graph (Digraph):** Edges have a specific direction ($u \to v$ does not imply $v \to u$).

### 2. By Edge Attributes

* **Unweighted Graph:** All edges are equal; no cost or distance value assigned.
* **Weighted Graph:** Every edge carries an associated numerical value (weight, cost, distance, or capacity).

### 3. By Topology & Structural Properties

* **Cyclic vs. Acyclic:** Cyclic graphs contain at least one cycle. A **DAG** (Directed Acyclic Graph) contains directed edges and no cycles.
* **Connected vs. Disconnected:** A graph is connected if a path exists between every pair of vertices.
* **Complete Graph ($K_n$):** Every vertex is directly connected to every other vertex. Total edges in undirected $K_n = \frac{V(V - 1)}{2}$.
* **Bipartite Graph:** Vertices can be partitioned into two disjoint sets such that no two vertices within the same set share an edge.

---

Refer to [Representations](representation.md)
