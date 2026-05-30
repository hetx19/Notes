**Problem**: You are given an `m x n` matrix `board` containing **letters** `'X'` and `'O'`, **capture regions** that are **surrounded**:

- **Connect**: A cell is connected to adjacent cells horizontally or vertically.
- **Region**: To form a region **connect every** `'O'` cell.
- **Surround**: A region is surrounded if none of the `'O'` cells in that region are on the edge of the board. Such regions are **completely enclosed** by `'X'` cells.

To capture a **surrounded region**, replace all `'O'`s with `'X'`s **in-place** within the original board. You do not need to return anything.

**Example**:
**Input:** board = `[["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]`

**Output:** `[["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]`

[Visit Leetcode](https://leetcode.com/problems/surrounded-regions/description/)
[Visit GFG](https://www.geeksforgeeks.org/problems/replace-os-with-xs0052/1)

---

### By DFS Traversal

**Time Complexity**: O(N + N) + (mn) + O(4mn) O(mn) → O(mn)
**Space Complexity**: O(mn) + O(mn) → O(mn)

```cpp
class Solution {
  private:
	void dfs(vector<vector<char>> &board, vector<vector<bool>> &visited, int row, int col, int deltaRow[], int deltaCol[], int m, int n) {
		visited[row][col] = true;
		
		for (int i = 0; i < 4; i++) {
			int nrow = row + deltaRow[i];
			int ncol = col + deltaCol[i];
			
			if (nrow >= 0 && nrow < m && ncol >= 0 && ncol < n && !visited[nrow][ncol] && board[nrow][ncol] == 'O') {
				dfs(board, visited, nrow, ncol, deltaRow, deltaCol, m, n);
			}
		}
	}
	
  public:
	void solve(vector<vector<char>> &board) {
		int m = board.size(), n = board[0].size();
		vector<vector<bool>> visited(m, vector<bool>(n, false));
		
		int deltaRow[] = {-1, 0, +1, 0};
		int deltaCol[] = {0, -1, 0, +1};
		
		for (int i = 0; i < m; i++) {
			for (int j = 0; j < n; j++) {
				if (i == 0 || i == m - 1 || j == 0 || j == n - 1) {
					if (!visited[i][j] && board[i][j] == 'O') {
						dfs(board, visited, i, j, deltaRow, deltaCol, m, n);
					}
				}
			}
		}
		
		for (int i = 0; i < m; i++) {
			for (int j = 0; j < n; j++) {
				if (!visited[i][j] && board[i][j] == 'O') {
					board[i][j] = 'X';
				}
			}
		}
	}
};
```

---

### By BFS Traversal

**Time Complexity**: O(N + N) + (mn) + O(4mn) O(mn) → O(mn)
**Space Complexity**: O(mn) + O(mn) → O(mn)

```cpp
class Solution {
  public:
	void solve(vector<vector<char>> &board) {
		int m = board.size(), n = board[0].size();
		vector<vector<bool>> visited(m, vector<bool>(n, false));
		queue<pair<int, int>> q;
		
		int deltaRow[] = {-1, 0, +1, 0};
		int deltaCol[] = {0, -1, 0, +1};
		
		for (int i = 0; i < m; i++) {
			for (int j = 0; j < n; j++) {
				if (i == 0 || i == m - 1 || j == 0 || j == n - 1) {
					if (!visited[i][j] && board[i][j] == 'O') {
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
				
				if (nrow >= 0 && nrow < m && ncol >= 0 && ncol < n && !visited[nrow][ncol] && board[nrow][ncol] == 'O') {
					visited[nrow][ncol] = true;
					q.push({nrow, ncol});
				}
			}
		}
		
		for (int i = 0; i < m; i++) {
			for (int j = 0; j < n; j++) {
				if (!visited[i][j] && board[i][j] == 'O') {
					board[i][j] = 'X';
				}
			}
		}
	}
};
