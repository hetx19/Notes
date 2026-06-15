### Definition
**Tarjan's Algorithm** is a graph algorithm used to find all **Strongly Connected Components (SCCs)** in a **directed graph** using a **single DFS traversal**.

Unlike Kosaraju's algorithm, Tarjan's algorithm does **not require reversing the graph** and finds SCCs in one pass.

A **Strongly Connected Component** is a maximal group of vertices where every vertex is reachable from every other vertex in the group.

---

### Core Idea
The algorithm assigns each node:
- **Discovery Time (`disc`)**: The order in which the node is visited.
- **Low-Link Value (`low`)**: The smallest discovery time reachable from that node, including itself.

During DFS:
1. Push visited nodes onto a stack.
2. Update low-link values using DFS tree edges and back edges.
3. When a node satisfies:

```text
disc[node] == low[node]
```

it becomes the root of an SCC.
4. Pop nodes from the stack until the root is reached.

---

### Why Does It Work?
The low-link value of a node represents the earliest ancestor reachable from its DFS subtree.
- If a node can reach an ancestor, its low-link value becomes smaller.
- Nodes belonging to the same SCC eventually obtain low-link values that point to the same SCC root.
- When:

```text
disc[node] == low[node]
```

there is no back edge connecting this SCC to any earlier node in the DFS tree.
Therefore, the node is the root of an SCC, and all nodes above it on the stack belong to the same component.

---

### Important Concepts
#### Discovery Time
Each node receives a unique timestamp when first visited.

Example:
```text
disc[0] = 0
disc[1] = 1
disc[2] = 2
...
```

#### Low-Link Value
For a node `u`:

```text
low[u] = min(disc[u], low[v] for DFS children, disc[v] for back edges)
```

Low-link values help determine whether a node can reach an earlier ancestor.

#### Stack
The stack stores nodes that belong to the current DFS path and whose SCC has not yet been identified.
Only nodes currently in the stack can contribute to low-link updates through back edges.

---

### Steps
#### Step 1: Initialize Arrays
```cpp
disc[V] = -1
low[V]
inStack[V] = false
```

Maintain:
- DFS timer
- Stack of active nodes

---

#### Step 2: Perform DFS
For each unvisited node:
- Assign discovery time.
- Push it onto the stack.
- Explore all neighbors.

---

#### Step 3: Update Low-Link Values
For every neighbor:
#### Tree Edge
```text
u → v
```

If `v` is unvisited:
```cpp
dfs(v);
low[u] = min(low[u], low[v]);
```

##### Back Edge
If `v` is currently in the stack:
```cpp
low[u] = min(low[u], disc[v]);
```

---

#### Step 4: Identify SCC Root

If:
```text
disc[u] == low[u]
```
then `u` is the root of an SCC.

Pop nodes until `u` is removed from the stack.
All popped nodes form one SCC.

---

### Complexity Analysis

| Operation        | Complexity   |
| ---------------- | ------------ |
| DFS Traversal    | O(V + E)     |
| Stack Operations | O(V)         |
| **Total**        | **O(V + E)** |

#### Space Complexity

```text
O(V)
```

for:
- Discovery array
- Low-link array
- Stack
- In-stack array

Excluding graph storage.
Including adjacency list storage:

```text
O(V + E)
```

---

### C++ Implementation

```cpp
class Tarjan {
  private:
    int timer = 0;

    void dfs(int node, vector<vector<int>>& adj, vector<int>& disc, vector<int>& low, vector<bool>& inStack, stack<int>& st, vector<vector<int>>& sccs) {
        disc[node] = low[node] = timer++;

        st.push(node);
        inStack[node] = true;

        for (int it : adj[node]) {
            if (disc[it] == -1) {
                dfs(it, adj, disc, low, inStack, st, sccs);
                low[node] = min(low[node], low[it]);
            } else if (inStack[it]) {
                low[node] = min(low[node], disc[it]);
            }
        }

        if (disc[node] == low[node]) {
            vector<int> component;

            while (true) {
                int topNode = st.top();
                st.pop();
                inStack[topNode] = false;
                component.push_back(topNode);

                if (topNode == node) {
                    break;
                }
            }

            sccs.push_back(component);
        }
    }

public:
    vector<vector<int>> getSCCs(int V, vector<vector<int>>& adj) {
        vector<int> disc(V, -1);
        vector<int> low(V);
        vector<bool> inStack(V, false);
        stack<int> st;
        vector<vector<int>> sccs;

        for (int i = 0; i < V; i++) {
            if (disc[i] == -1) {
                dfs(i, adj, disc, low, inStack, st, sccs);
            }
        }

        return sccs;
    }
};
```

---

### Example

#### Graph

```text
0 → 2
↑   ↓
1 ←

0 → 3 → 4
```

#### Discovery Times

```text
disc[0] = 0
disc[2] = 1
disc[1] = 2
disc[3] = 3
disc[4] = 4
```

#### Low-Link Values

```text
low[0] = 0
low[1] = 0
low[2] = 0
low[3] = 3
low[4] = 4
```

#### SCCs

```text
Possible SCCs:

{0,1,2}
{3}
{4}
```

---

### Interview Notes

#### When to Use?
- Finding SCCs in directed graphs.
- Online SCC detection.
- Dependency resolution.
- Graph compression.
- Condensation DAG construction.

---

#### Tarjan vs Kosaraju

| Feature              | Tarjan       | Kosaraju              |
| -------------------- | ------------ | --------------------- |
| DFS Passes           | 1            | 2                     |
| Reverse Graph Needed | No           | Yes                   |
| Time Complexity      | O(V + E)     | O(V + E)              |
| Space Complexity     | O(V) + Graph | O(V) + Reversed Graph |
| Interview Popularity | High         | Very High             |

---

#### Key Observation
Nodes within the same SCC eventually share the same SCC root.
A node becomes the root of an SCC when:

```text
disc[node] == low[node]
```

This is the most important condition in the algorithm.

---

#### Common Pitfalls
1. Using `low[it]` instead of `disc[it]` for back edges.
2. Forgetting the `inStack` check.
3. Not removing nodes from the stack after forming an SCC.
4. Updating low-link values incorrectly.
5. Forgetting to run DFS from every unvisited node.

---

#### Memory Trick

```text
disc = when node is discovered
low  = earliest reachable ancestor

disc == low
⇒ SCC root found
```

```text
1. DFS
2. Maintain Stack
3. Update Low-Link Values
4. disc == low → Extract SCC
```

**Tarjan = DFS + Low-Link + Stack**

[Visit Leetcode](https://leetcode.com/problems/critical-connections-in-a-network/)