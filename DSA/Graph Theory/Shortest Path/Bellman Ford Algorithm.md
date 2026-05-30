**Problem**: Given an weighted graph with **V** vertices numbered from 0 to V-1 and **E** edges, represented by a 2d array `edges[][]`, where `edges[i] = [u, v, w]` represents a direct edge from node **u** to **v** having **w** edge weight. You are also given a source vertex **src**.

Your task is to compute the **shortest distances** from the **source** to all other vertices. If a vertex is unreachable from the source, its distance should be marked as **108**. Additionally, if the graph contains a **negative weight cycle**, return `[-1]` to indicate that shortest paths cannot be reliably computed.

**Example**:
**Input**: V = 5, `edges[][] = [[1, 3, 2], [4, 3, -1], [2, 4, 1], [1, 2, 1], [0, 1, 5]]`, src = 0
![[Bellman Ford Algorithm.png|193]]
**Output**: `[0, 5, 6, 6, 7]`

[Visit GFG](https://www.geeksforgeeks.org/problems/distance-from-the-source-bellman-ford-algorithm/1)

---

### Algorithm
**Time Complexity**: O(VE)
**Space Complexity**: O(V)

```cpp
class Solution {
  public:
	vector<int> bellmanFord(int V, vector<vector<int>>& edges, int src) {
		vector<int> distance(V, 1e8);
		distance[src] = 0;
		
		for (int i = 0; i < V - 1; i++) {
			for (auto &e : edges) {
				int u = e[0];
				int v = e[1];
				int weight = e[2];
				
				if (distance[u] != 1e8 && distance[u] + weight < distance[v]) {
					distance[v] = distance[u] + weight;
				}
			}
		}
		
		// Nth relaxation to check for negative cycle
		for (auto &e : edges) {
			int u = e[0];
			int v = e[1];
			int weight = e[2];
				
			if (distance[u] != 1e8 && distance[u] + weight < distance[v]) {
				return {-1};
			}
		}
		
		return distance;
    }
};
```
