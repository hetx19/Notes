**Problem**: You are given an `m x n` binary matrix `grid`, where `0` represents a sea cell and `1` represents a land cell.

A **move** consists of walking from one land cell to another adjacent (**4-directionally**) land cell or walking off the boundary of the `grid`.

Return _the number of land cells in_ `grid` _for which we cannot walk off the boundary of the grid in any number of **moves**_.

**Example**: 
![[Number of Enclaves.png]]
**Input:** grid = `[[0,0,0,0],[1,0,1,0],[0,1,1,0],[0,0,0,0]]`
**Output:** 3
**Explanation:** There are three 1s that are enclosed by 0s, and one 1 that is not enclosed because its on the boundary.

[Visit Leetcode](https://leetcode.com/problems/number-of-enclaves/description/)
[Visit GFG](https://www.geeksforgeeks.org/problems/number-of-enclaves/1)

---

### By DFS Traversal

**Time Complexity**: O(mn) + (4mn) → O(mn)
**Space Complexity**: O(mn) + (mn) → O(mn)

```cpp
class Solution {
  private:
	void dfs(vector<vector<int>> &grid, vector<vector<bool>> &visited, int row, int col, int deltaRow[], int deltaCol[], int m, int n) {
		visited[row][col] = true;
		
		for (int i = 0; i < 4; i++) {
			int nrow = row + deltaRow[i];
			int ncol = col + deltaCol[i];
			
			if (nrow >= 0 && nrow < m && ncol >= 0 && ncol < n && !visited[nrow][ncol] && grid[nrow][ncol] == 1) {
				dfs(grid, visited, nrow, ncol, deltaRow, deltaCol, m, n);
			}
		}
	}
	
  public:
	int numEnclaves(vector<vector<int>>& grid) {
		int m = grid.size(), n = grid[0].size();
		vector<vector<bool> visited(m, vector<bool>(n, false));
		
		int deltaRow[] = {-1, 0, +1, 0};
		int deltaCol[] = {0, -1, 0, +1};
		
		for (int i = 0; i < m; i++) {
			for (int j = 0; j < n; j++) {
				if (i == 0 || i == m - 1 || j == 0 || j == n - 1) {
					if (grid[i][j] == 1 && !visited[i][j]) {
						dfs(grid, visited, i, j, deltaRow, deltaCol, m, n);
					}
				}
			}
		}
		
		int ans = 0;
		for (int i = 0; i < m; i++) {
			for (int j = 0; j < n; j++) {
				if (!visited[i][j] && grid[i][j] == 1) {
					ans++;
				}
			}
		}
		
		return ans;
	}
};
```

---

### By BFS Traversal

**Time Complexity**: O(mn) + (4mn) → O(mn)
**Space Complexity**: O(mn) + (mn) → O(mn)

```cpp
class Solution {	
  public:
	int numEnclaves(vector<vector<int>>& grid) {
		int m = grid.size(), n = grid[0].size();
		vector<vector<bool> visited(m, vector<bool>(n, false));
		queue<pair<int, int>> q;
		
		int deltaRow[] = {-1, 0, +1, 0};
		int deltaCol[] = {0, -1, 0, +1};
		
		for (int i = 0; i < m; i++) {
			for (int j = 0; j < n; j++) {
				if (i == 0 || i == m - 1 || j == 0 || j == n - 1) {
					if (grid[i][j] == 1) {
						visited[i][j] = true;
						q.push({i, j});
					}
				}
			}
		}
		
		while (!q.empty()) {
			int row = q.front().first;
			int col = q.front().second;
			q.pop();
			
			for (int i = 0; i < 4; i++) {
				int nrow = row + deltaRow[i];
				int ncol = col + deltaCol[i];
				
				if (nrow >= 0 && nrow < m && ncol >= 0 && ncol < n && !visited[nrow][ncol] && grid[nrow][ncol] == 1) {
					visited[nrow][ncol] = true;
					q.push({nrow, ncol});
				}
			}
		}
		
		int ans = 0;
		for (int i = 0; i < m; i++) {
			for (int j = 0; j < n; j++) {
				if (!visited[i][j] && grid[i][j] == 1) {
					ans++;
				}
			}
		}
		
		return ans;
	}
};
```