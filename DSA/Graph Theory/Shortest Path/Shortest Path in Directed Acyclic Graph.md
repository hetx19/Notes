**Problem**: Given a Directed Acyclic Graph of V vertices from 0 to (n - 1) and a 2D Integer array(or vector) `edges[ ][ ]` of length E, where there is a directed edge from `edge[i][0]` to `edge[i][1]` with a distance of `edge[i][2]` for all i.

Find the **shortest** path from **src(0)** vertex to all the vertices and if it is impossible to reach any vertex, then return **-1** for that vertex.

**Example**:
**Input:** V = 4, E = 2, `edges = [[0,1,2], [0,2,1]]`
**Output:** `[0, 2, 1, -1]`
**Explanation:** Shortest path from 0 to 1 is 0->1 with edge weight 2. Shortest path from 0 to 2 is 0->2 with edge weight 1. There is no way we can reach 3, so it's -1 for 3.

[Vist GFG](https://www.geeksforgeeks.org/problems/shortest-path-in-undirected-graph/1)

---

### By DFS Traversal + Stack
**Time Complexity**: 
**Space Complexity**: 

```cpp
class Solution {
  private:
	void dfs(vector<vector<pair<int, int>>> &adj, vector<bool> &visited, stack<int> &st, int node) {
		visited[node] = true;
		
		for (auto it : adj[node]) {
			int v = it.first;
			if (!visited[v]) {
				dfs(adj, visited, st, v);
			}
		}
		
		st.push(node);
	}
	
  public:
	vector<int> shortestPath(int V, int E, vector<vector<int>> &edges) {
		vector<vector<pair<int, int>>> adj(V);
		
		for (int i = 0; i < E; i++) {
			int u = edges[i][0];
            int v = edges[i][1];
            int w = edges[i][2];
            
            adj[u].push_back({v, w});
		}
		
		vector<bool> visited(V, false);
        stack<int> st;
        
        for (int i = 0; i < V; i++) {
            if (!visited[i]) {
                dfs(adj, visited, st, i);
            }
        }
        
        vector<int> distance(V, INT_MAX);
        distance[0] = 0;
        
        while (!st.empty()) {
            int node = st.top();
            st.pop();
            
            if (distance[node] != INT_MAX) {
                for (auto it : adj[node]) {
                    int v = it.first;
                    int w = it.second;
                    
                    if (distance[node] + w < distance[v]) {
                        distance[v] = distance[node] + w;
                    }
                }
            }
        }
        
        for (int i = 0; i < V; i++) {
            if (distance[i] == INT_MAX) {
                distance[i] = -1;
            }
        }
        
        return distance;
	}
};
```

---

### By Topological Sort
**Time Complexity**:
**Space Complexity**:

```cpp
class Solution {
  public:
	vector<int> shortestPath(int V, int E, vector<vector<int>> &edges) {
		vector<vector<pair<int, int>>> adj(V);
		vector<int> indegree(V, 0);
		
		for (int i = 0; i < E; i++) {
			int u = edges[i][0];
            int v = edges[i][1];
            int w = edges[i][2];
            
            adj[u].push_back({v, w});
            indegree[v]++;
		}
		
        queue<int> q;
        
        for (int i = 0; i < V; i++) {
            if (indegree[i] == 0) {
	            q.push(i);
            }
        }
        
        vector<int> distance(V, INT_MAX);
        distance[0] = 0;
        
        while (!q.empty()) {
            int node = q.front();
            q.pop();

            for (auto it : adj[node]) {
                int v = it.first;
                int w = it.second;
                    
                if (distance[node] != INT_MAX && distance[node] + w < distance[v]) {
                    distance[v] = distance[node] + w;
                }
                
                indegree[v]--;
                if (indegree[v] == 0) {
	                q.push(v);
                }
            }
        }
        
        for (int i = 0; i < V; i++) {
            if (distance[i] == INT_MAX) {
                distance[i] = -1;
            }
        }
        
        return distance;
	}
};
```