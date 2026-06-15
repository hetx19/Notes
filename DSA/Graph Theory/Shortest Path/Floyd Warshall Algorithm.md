**Problem**: You are given a weighted **directed** graph, represented by an adjacency matrix, `dist[][]` of size **n x n**, where `dist[i][j]` represents the weight of the edge from **node i to node j**. If there is no direct edge, `dist[i][j]` is set to a large value (i.e., **108**) to represent infinity.  
The graph may contain **negative edge weights**, but it does not contain any **negative weight cycles**.

Your task is to find the **shortest distance** between every pair of nodes **i** and **j** in the graph.

Note: Modify the distances for every pair **in place**.

**Example**:
**Input**: `dist[][] = [[0, 4, 108, 5, 108], [108, 0, 1, 108, 6], [2, 108, 0, 3, 108], [108, 108, 1, 0, 2], [1, 108, 108, 4, 0]]`
![[Floyd Warshall Algorithm.png|219]]
**Output**: `[[0, 4, 5, 5, 7], [3, 0, 1, 4, 6], [2, 6, 0, 3, 5], [3, 7, 1, 0, 2], [1, 5, 5, 4, 0]]`

[Visit GFG](https://www.geeksforgeeks.org/problems/implementing-floyd-warshall2042/1)

---

### Algorithm - [[Dynamic Programming]]
**Time Complexity**: O(n<sup>3</sup>)
**Space Complexity**: O(n<sup>2</sup>)

```cpp
class Solution {
  public:
    void floydWarshall(vector<vector<int>> &dist) {
        int n = dist.size();
        
        for (int i = 0; i < n; i++) {
            dist[i][i] = 0;
        }
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                for (int k = 0; k < n; k++) {
                    if (dist[j][i] == 100000000 || dist[i][k] == 100000000) {
                        continue;
                    }
                    
                    dist[j][k] = min(dist[j][k], dist[j][i] + dist[i][k]);
                }
            }
        }
    }
};
```

> [!tip]
> We are changing the given distance array. Hence Space Complexity will be O(n<sup>2</sup>)

---