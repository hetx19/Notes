**Problem**: Given an `n x n` binary matrix `grid`, return _the length of the shortest **clear path** in the matrix_. If there is no clear path, return `-1`.

A **clear path** in a binary matrix is a path from the **top-left** cell (i.e., `(0, 0)`) to the **bottom-right** cell (i.e., `(n - 1, n - 1)`) such that:

- All the visited cells of the path are `0`.
- All the adjacent cells of the path are **8-directionally** connected (i.e., they are different and they share an edge or a corner).

The **length of a clear path** is the number of visited cells of this path.

**Example**:

![263](https://assets.leetcode.com/uploads/2021/02/18/example1_1.png)

**Input**: `grid = [[0,1],[1,0]]`
**Output:** 2

---

### By BFS Traversal

**Time complexity**: O()
**Space complexity**: O()

```cpp
class Solution {
  public:
	int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
		int n = grid.size();
		
		if (grid[0][0] == 1 || grid[n-1][n-1] == 1) return -1;
		if (n == 1) return 1;
		
		int deltaRow[] = {-1, -1, -1, 0, +1, +1, +1, 0};
		int deltaCol[] = {-1, 0, +1, +1, +1, 0, -1, -1};
		
		vector<vector<int>> distance(n, vector<int>(n, INT_MAX));		
		queue<pair<int, pair<int, int>>> q;
		
		distance[0][0] = 1;
		q.push({1, {0, 0}});
		
		while (!q.empty()) {
			auto it = q.front();
			int dis = it.first;
			int row = it.second.first;
			int col = it.second.second;
			q.pop();

			for (int i = 0; i < 8; i++) {
				int nrow = row + deltaRow[i];
				int ncol = col + deltaCol[i];
				
				if (nrow >= 0 && nrow < n && ncol >= 0 && ncol < n && grid[nrow][ncol] == 0 && dis + 1 < distance[nrow][ncol]) {
					if (nrow == n - 1 && ncol == n - 1) {
						return dis + 1;
					}
					distance[nrow][ncol] = dis + 1;
					q.push({dis + 1, {nrow, ncol}});
				}
			}
		}
		
		return -1;
	}
};
```