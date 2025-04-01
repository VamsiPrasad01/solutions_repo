# **<span style="color:#E74C3C">Problem 1: Equivalent Resistance Using Graph Theory</span>**

---

## **<span style="color:#28B463">1. Introduction & Motivation</span>**

When dealing with complex resistor networks, traditional reduction methods (series-parallel rules) often become tedious and error-prone. Graph theory provides a robust framework to **automate and simplify** this process by treating the circuit as a **weighted undirected graph**:

- **Nodes** = Circuit junctions  
- **Edges** = Resistors (weights = resistance values)

By employing **graph traversal algorithms**, we can iteratively identify **series and parallel** connections and simplify them—making this approach ideal for simulation software and scalable to large systems.

---

## **<span style="color:#5DADE2">2. Algorithm Description (Pseudocode)</span>**

### **High-Level Steps:**

1. **Represent the Circuit as a Graph**
2. **Identify Simplifiable Subgraphs (Series or Parallel)**
3. **Iteratively Reduce** until a single edge remains between the source and target nodes.

---

### **Pseudocode: Equivalent Resistance Reduction**

```plaintext
Input: Graph G(V, E), where each edge has resistance R

function EquivalentResistance(G, source, target):
    while number_of_nodes(G) > 2:
        for each node n in G:
            if degree(n) == 2:
                reduce_series(n)
        for each pair of nodes (u, v):
            if multiple_edges(u, v):
                reduce_parallel(u, v)
    return resistance_between(source, target)

function reduce_series(node n):
    neighbors = [u, v]  # nodes connected to n
    R_total = R(n, u) + R(n, v)
    remove node n and its edges
    add edge(u, v) with resistance R_total

function reduce_parallel(u, v):
    R_eq = 1 / sum(1 / R for each edge(u, v))
    remove all edges(u, v)
    add edge(u, v) with resistance R_eq
```

---

## **<span style="color:#F1C40F">3. Python Implementation (Click to View Code)</span>**

<details>
<summary>Click to see the Python code</summary>

```python
import networkx as nx

def reduce_series(G):
    changed = True
    while changed:
        changed = False
        for node in list(G.nodes):
            if G.degree[node] == 2:
                neighbors = list(G.neighbors(node))
                if len(neighbors) == 2:
                    u, v = neighbors
                    R1 = G[node][u]['resistance']
                    R2 = G[node][v]['resistance']
                    G.add_edge(u, v, resistance=R1 + R2)
                    G.remove_node(node)
                    changed = True
                    break
    return G

def reduce_parallel(G):
    for u, v in list(G.edges()):
        parallel_edges = list(G.get_edge_data(u, v).values())
        if len(parallel_edges) > 1:
            R_eq = 1 / sum(1 / edge['resistance'] for edge in parallel_edges)
            G.remove_edges_from([(u, v)] * len(parallel_edges))
            G.add_edge(u, v, resistance=R_eq)
    return G

def equivalent_resistance(G, source, target):
    G = reduce_series(G)
    G = reduce_parallel(G)
    return G[source][target]['resistance']
```

</details>

---

## **<span style="color:#9B59B6">4. Example Circuit Configurations</span>**

### ✅ **Example 1: Simple Series**

- A --[2Ω]-- B --[3Ω]-- C  
- Equivalent: 5Ω

---

### ✅ **Example 2: Parallel Branches**

- A --[2Ω]-- B  
- A --[2Ω]-- B  
- Equivalent: 1Ω

---

### ✅ **Example 3: Nested Graph (Series + Parallel)**

- A --[2Ω]-- B --[3Ω]-- C  
- A --[5Ω]-- C  
- Series: A-B-C = 5Ω  
- Parallel with A-C = 5Ω  
- Total: \( \frac{1}{\frac{1}{5} + \frac{1}{5}} = 2.5Ω \)

---

## **<span style="color:#34495E">5. Algorithm Analysis</span>**

### ⏱️ **Time Complexity**

- **Series reduction**: O(V)
- **Parallel detection**: O(E²) in worst case (multiple edge check)
- For sparse circuits: ≈ linear to number of components.

### 🧠 **Advantages**

- Handles **complex and nested networks**
- Automatable using libraries like **NetworkX**
- Adaptable to **symbolic or variable resistors**

### 🚀 **Improvements**

- Incorporate **cycle detection (DFS/BFS)** for general loop analysis
- Use **Kirchhoff’s laws** for edge cases (non-reducible graphs)
- Allow symbolic algebra (SymPy) for unknown resistors

---
