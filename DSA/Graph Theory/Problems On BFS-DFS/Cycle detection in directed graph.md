**Problem**: Given a Directed Graph with **V** vertices (Numbered from **0** to **V-1**) and **E** edges, check whether it contains any **cycle** or not. The graph is represented as a 2D vector **`edges[][]`**, where each entry **`edges[i] = [u, v]`** denotes an edge from vertex **u** to **v.**

**Note:** The graph can have multiple component.

#### Method 1 (By using DFS Traversal)

```cpp
bool checkDFS(vector<vector<int>>& adj, vector<bool> &visited, vector<bool> &pathVisited, int node) {
    visited[node] = true;
    pathVisited[node] = true;
    
    for (int it : adj[node]) {
        if (!visited[it]) {
            if (checkDFS(adj, visited, pathVisited, it)) {
                return true;
            }
        } else if (pathVisited[it]) {
            return true;
        }
    }
    
    pathVisited[node] = false;
    return false;
}

bool isCyclic(int V, vector<vector<int>> &edges) {
    vector<vector<int>> adj(V);
    
    for (auto &e : edges) {
        adj[e[0]].push_back(e[1]);
    }
    
    vector<bool> visited(V, false);
    vector<bool> pathVisited(V, false);
    
    for (int i = 0; i < V; i++) {
        if (!visited[i]) {
            if (checkDFS(adj, visited, pathVisited, i)) {
                return true;
            }
        }
    }
    
    return false;
}
```

**Time Complexity:** O(V + E)
**Space Complexity:** O(V) + O(V) → O(V)

---

#### Method 2 (By using BFS Traversal (Topological Sort)) / Khan's Algorithm

```cpp
bool isCyclic(int V, vector<vector<int>> &edges) {
    vector<vector<int>> adj(V);
    
    for (auto &e : edges) {
        adj[e[0]].push_back(e[1]);
    }
    
    vector<int> indegree(V, 0);
    
    for (int i = 0; i < V; i++) {
        for (int it : adj[i]) {
            indegree[it]++;
        }
    }
    
    queue<int> q;
    for (int i = 0; i < V; i++) {
        if (indegree[i] == 0) {
            q.push(i);
        }
    }
    
    int cnt = 0;
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        
        cnt++;
        
        for (int it : adj[node]) {
            indegree[it]--;
            if (indegree[it] == 0) {
                q.push(it);
            }
        }
    }
    
    return cnt != V;
}
```

**Time Complexity:** O(V + E)
**Space Complexity:** O(V) + O(V) → O(V)

