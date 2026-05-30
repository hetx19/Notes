**Problem**: Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return _the number of islands_.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example:**

**Input:** grid = `[["1","1","1","1","0"],["1","1","0","1","0"],["1","1","0","0","0"],["0","0","0","0","0"]]`
**Output:** 1

[Visit Leetcode](https://leetcode.com/problems/number-of-islands/)
[Visit GFG](https://www.geeksforgeeks.org/problems/find-the-number-of-islands/1)

---

### By DFS Traversal

**Time Complexity**: O(mn) + (4mn) → O(mn)
**Space Complexity**: O(mn) + O(mn) → O(mn)

```cpp
class Solution {
  private:
	void dfs(vector<vector<char>> &grid, vector<vector<bool>> &visited, int row, int col, int deltaRow[], int deltaCol[], int m, int n) {
		visited[row][col] = true;
		
		for (int i = 0; i < 4; i++) {
			int nrow = row + deltaRow[i];
			int ncol = col + deltaCol[i];
			
			if (nrow >= 0 && nrow < m && ncol >= 0 && ncol < n && !visited[nrow][ncol] && grid[nrow][ncol] == '1') {
				dfs(grid, visited, nrow, ncol, deltaRow, deltaCol, m, n);
			}
		}
	}
	
  public:
	int numIslands(vector<vector<char>> &grid) {
		int m = grid.size(), n = grid[0].size();
		int count = 0;
		vector<vector<bool>> visited(m, vector<bool> (n, false));
		
		int deltaRow[] = {-1, 0, +1, 0};
		int deltaCol[] = {0, -1, 0, +1};
		
		for (int row = 0; row < m; row++) {
			for (int col = 0; col < n; col++) {
				if (!visited[row][col] && grid[row][col] == '1') {
					count++;
					dfs(grid, visited, row, col, deltaRow, deltaCol, m, n);
				}
			}
		}
		
		return count;
	}
};
```

---

### By BFS Traversal

**Time Complexity**: O(mn) + (4mn) → O(mn)
**Space Complexity**: O(mn) + O(mn) → O(mn)

```cpp
class Solution {
  private:
	void bfs(vector<vector<char>> &grid, vector<vector<bool>> &visited, int row, int col, int deltaRow[], int deltaCol[], int m, int n) {
		queue<pair<int, int>> q;
		visited[row][col] = true;
		q.push({row, col});
		
		while (!q.empty()) {
			int curRow = q.front().first, curCol = q.front().second;
			q.pop();
			
			for (int i = 0; i < 4; i++) {
				int nrow = curRow + deltaRow[i];
				int ncol = curCol + deltaCol[i];
				
				if (nrow >= 0 && nrow < m && ncol >= 0 && ncol < n && !visited[nrow][ncol] && grid[nrow][ncol] == '1') {
					visited[nrow][ncol] = true;
					q.push({nrow, ncol});
				}
			}
		}
	}
	
  public:
	int numIslands(vector<vector<char>> &grid) {
		int m = grid.size(), n = grid[0].size();
		int count = 0;
		vector<vector<bool>> visited(m, vector<bool> (n, false));
		
		int deltaRow[] = {-1, 0, +1, 0};
		int deltaCol[] = {0, -1, 0, +1};
		
		for (int row = 0; row < m; row++) {
			for (int col = 0; col < n; col++) {
				if (!visited[row][col] && grid[row][col] == '1') {
					count++;
					bfs(grid, visited, row, col, deltaRow, deltaCol, m, n);
				}
			}
		}
		
		return count;
	}
};
```
