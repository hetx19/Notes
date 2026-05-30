**Problem**: Given an **undirected graph** with **V** vertices and **E** edges, represented as a 2D vector **`edges[][]`**, where each entry `edges[i] = [u, v]` denotes an edge between vertices **u** and **v**, determine whether the graph contains a **cycle** or not.

**Note:** The graph can have multiple component.

**Example**:
![[Cycle Detection in undirected graph.png|185]]
**Input:** V = 4, E = 4, `edges[][] = [[0, 1], [0, 2], [1, 2], [2, 3]]`
**Output:** true

---
#### Method 1 (By using BFS Traversal)

```cpp
class Solution {
  private:
	bool checkComponents(vector<vector<int>>& adj, vector<bool>& visited, int start) {
		queue<pair<int, int>> q;
		q.push({start, -1});

	    visited[start] = true;

	    while(!q.empty()) {
		    int node = q.front().first;
		    int parent = q.front().second;

	        q.pop();

	        for (int it : adj[node]) {
	            if (!visited[it]) {
	                visited[it] = true;
	                q.push({it, node});
	            } else if (parent != it) {
	                return true;
	            }
	        }
	    }

	    return false;
	}
};

  public:
	bool isCycle(vector<vector<int>>& edges, int V) {
	    vector<vector<int>> adj(V);

	    for(auto &e : edges) {
	        adj[e[0]].push_back(e[1]);
	        adj[e[1]].push_back(e[0]);
	    }

	    vector<bool> visited(V, false);

	    for (int i = 0; i < V; i++) {
	        if (!visited[i]) {
	            if (checkComponents(adj,visited,i)) {
	                return true;
	            }
	        }
	    }

	    return false;
	}
};
```

**Time Complexity:** O(V + 2E) + O(V)
**Space Complexity:** O(V) + O(V) → O(V)

---

#### Method 2 (By using DFS Traversal)

```cpp
class Solution {
  private:
	bool checkComponents(vector<vector<int>>& adj, vector<bool>& visited, int node, int parent) {
	    visited[node] = true;

	    for(int it : adj[node]) {
	        if(!visited[it]) {
	            if(checkComponents(adj, visited, it, node)) {
	                return true;
	            }
	        } else if(parent != it) {
	            return true;
	        }
	    }

	    return false;
	}

  public:
	bool isCycle(vector<vector<int>>& edges, int V) {
	    vector<vector<int>> adj(V);

	    for(auto &e : edges) {
	        adj[e[0]].push_back(e[1]);
	        adj[e[1]].push_back(e[0]);
	    }

	    vector<bool> visited(V, false);

	    for(int i = 0; i < V; i++) {
	        if (!visited[i]) {
	            if(checkComponents(adj, visited, i, -1)) {
	                return true;
	            }
	        }
	    }

	    return false;
	}
};
```

**Time Complexity:** O(V + 2E) + O(V)
**Space Complexity:** O(V) + O(V)

