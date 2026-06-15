**Problem**: Given an undirected graph of `V` vertices numbered from `0` to `V-1` and a 2D array `edges[][]`, where each edge `[u, v]` represents an undirected edge between vertices `u` and `v` with unit weight (distance = 1)

Find the **shortest** path from **src(0)** vertex to all the vertices and if it is impossible to reach any vertex, then return **-1** for that vertex.

**Example**:
**Input:** V = 3, E = 2, `edges = [[0,1], [0,2]]`, start = 0
**Output:** `[0, 1, 1]`

---

### By BFS Traversal

**Time Complexity**: O(V + 2E)
**Space Complexity**: O(V) + O(adjList)

```cpp
class Solution {
  private:
	void bfs(vector<vector<int>>& adj, vector<int>& distance, int start) {
	    queue<int> q;
	    distance[start] = 0;
	    q.push(start);
	    
	    while (!q.empty()) {
	        int node = q.front();
	        q.pop();
	        
	        for (int it : adj[node]) {
	            if (distance[it] == 1e9) {
	                distance[it] = distance[node] + 1;
	                q.push(it);
	            }
	        }
	    }
	}
	
  public:
	vector<int> shortestDistance(vector<vector<int>>& edges, int start, int V) {
		vector<vector<int>> adj(V);
		
		for (auto &e : edges) {
			adj[e[0]].push_back(e[1]);
			adj[e[1]].push_back(e[0]);
		}
		
	    vector<int> distance(V, 1e9);
	    
	    bfs(adj, distance, start);
	    
	    for (int i = 0; i < V; i++) {
		    if (distance[i] == 1e9) {
			    distance[i] = -1;
		    }
		}
	    
	    return distance;
	}
};
```

---