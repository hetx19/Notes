**Problem**: Given a weighted, undirected, and connected graph with V vertices and E edges, your task is to find the sum of the weights of the edges in the Minimum Spanning Tree (MST) of the graph. The graph is provided as a list of edges, where each edge is represented as `[u, v, w]`, indicating an edge between vertex u and vertex v with edge weight w.

**Example**:
![[Minimum spanning tree.png|195]]
**Input**: V = 3, E = 3, `Edges = [[0, 1, 5], [1, 2, 3], [0, 2, 1]]`

[Vist_GFG](https://www.geeksforgeeks.org/problems/minimum-spanning-tree/1)

---

### By Prim's Algorithm (Greedy Approach) - [[Greedy]]

**Time Complexity**: O(E log E)
**Space Complexity**: O(E + V) + O(adjList)

```cpp
class Solution {
  public:
	int spanningTree(int V, vector<vector<int>>& edges) {
		vector<vector<pair<int, int>>> adj(V);
		
		for (auto &e : edges) {
			adj[e[0]].push_back({e[1], e[2]});
			adj[e[1]].push_back({e[0], e[2]});
		}
		
		priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
		
		vector<bool> visited(V, false);
		int sum = 0;
		pq.push({0, 0});
		
		while (!pq.empty()) {
			auto it = pq.top();
			pq.pop();
			
			int node = it.second;
			int weight = it.first;
			
			if (visited[node]) {
				continue;
			}
			
			visited[node] = true;
			
			sum += weight;
			
			for (auto &it : adj[node]) {
				int adjNode = it.first ;
				int edgeWeight = it.second;
				
				if (!visited[adjNode]) {
					pq.push({edgeWeight, adjNode});
				}
			}
		}
		
		return sum;
	}
};
```


