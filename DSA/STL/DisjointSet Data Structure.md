The **Disjoint Set Union (DSU)**, also called **Union-Find**, is a data structure used to efficiently manage a collection of disjoint (non-overlapping) sets.

It supports:
- Finding which set an element belongs to
- Merging two sets

It is widely used in:
- Graph algorithms
- Kruskal’s Minimum Spanning Tree - [[Minimum Spanning Tree (MST)]]
- Connected components - [[Number of Provinces]]
- Network connectivity
- Dynamic grouping problems

---

### Core Operations

#### 1. `find(x)`
Returns the representative (parent/root) of the set containing `x`.

Two elements belong to the same set if:

```cpp
find(a) == find(b)
```

---

#### 2. `union(a, b)`

Merges the sets containing `a` and `b`.

---

### Basic Representation

Each set is represented as a tree.

```text
1 → 1
2 → 1
3 → 1
```

Here:
- `1` is the representative/root
- `{1,2,3}` belong to the same set

---

### Initialization

Initially every element is its own parent.

```cpp
parent[i] = i;
```

Example:

```text
1  2  3  4
↑  ↑  ↑  ↑
1  2  3  4
```

---

### Naive DSU Implementation

```cpp
class DisjointSet {
  private:
	vector<int> parent, rank;
	
  public:
    DisjointSet(int V) {
	    parent.resize(V);
	    rank.resize(V, 0);
	    
	    for (int i = 0; i < V; i++) {
		    parent[i] = i;
	    }
    }
    
    int findParent(int node) {
	    if (node == parent[node]) {
		    return node;
	    }
	    
	    return findParent(parent[node]);
    }
    
    void unionByRank(int u, int v) {
	    int pu = findParent(u), pv = findParent(v);
	    
	    if (pu == pv) return;
	    
	    if (rank[pu] < rank[pv]) {
		    parent[pu] = pv;
	    } else if (rank[pu] > rank[pv]) {
		    parent[pv] = pu;
	    } else {
		    parent[pv] = pu;
		    rank[pu]++;
	    }
    }
};
```

---

### Optimizations
Without optimizations, DSU can become slow in worst cases.

Two important optimizations make it nearly constant time.

---

### 1. Path Compression
During `find(x)`, make every visited node directly point to the root.

#### Before Compression

```text
1 ← 2 ← 3 ← 4
```

Finding `4` traverses all nodes.

#### After Compression

```text
1 ← 2
↑
3
↑
4
```

All nodes directly connect to root.

---

#### Implementation

```cpp
int find(int x) {
    if(parent[x] == x)
        return x;

    return parent[x] = find(parent[x]);
}
```

---

### 2. Union by Rank / Size
Always attach the smaller tree under the larger tree.

This prevents tall trees.

---

#### Union by Size

```cpp
vector<int> size;

void unionBySize(int u, int v) {
	int pu = findParent(u), pv = findParent(v);
	    
	if (pu == pv) return;
	
	if (rank[pu] < rank[pv]) {
		parent[pu] = pv;
		size[pv] += size[pu]
	} else {
		parent[pv] = pu;
		size[pu] += size[pv];
	}
}
```

---

### Fully Optimized DSU

```cpp
class DisjointSet {
    vector<int> parent, size;

public:
    DisjointSet(int V) {
        parent.resize(V + 1);
        size.resize(V + 1, 1);

        for(int i = 0; i <= V; i++) {
            parent[i] = i;
        }
    }

    int findParent(int node) {
        if(parent[node] == node) {
            return node;
        }

        return parent[node] = findParent(parent[node]);
    }

    void unionBySize(int u, int v) {
	    int pu = findParent(u), pv = findParent(v);

        if (pu == pv) {
            return;
        }

        if (size[pu] > size[pv]) {
	        parent[pv] = pu;
	        size[pu] += size[pv];
        } else {
	        parent[pu] = pv;
	        size[pv] += size[pu];
        }
    }
};
```

---

### Time Complexity

With:
- Path Compression
- Union by Size/Rank

Complexity becomes:

```text
O(α(n)) → O(4α)
```

Where:
- `α(n)` = Inverse Ackermann Function
- Practically constant time

---

### Applications

#### 1. Connected Components
Check whether nodes belong to the same component.

---

#### 2. Kruskal’s Algorithm
Used to detect cycles while building MST. - [[Minimum Spanning Tree (MST)]]

---

#### 3. Dynamic Connectivity
Efficiently process:

- Add edge
- Query connectivity

---

#### 4. Number of Provinces
Common graph interview problem. - [[Number of Provinces]]

---

### Example

#### Input

```text
union(1,2)
union(2,3)
find(1)
find(3)
```

#### Result

```text
1 and 3 belong to same set
```

---

### Important Notes
- DSU works only for **merge operations**
- Cannot efficiently split sets 
- Extremely useful in graph problems

---

### Interview Tips

#### Common Variants
- Union by Rank
- Union by Size
- DSU with rollback
- DSU on Trees

---

### Summary

| Operation | Complexity |
| --------- | ---------- |
| Find      | ~O(1)      |
| Union     | ~O(1)      |
| Space     | O(n)       |

DSU is one of the most important data structures for graph and connectivity problems.