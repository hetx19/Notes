**Problem**: Given a `m x n` `grid` filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path.

**Note:** You can only move either down or right at any point in time.

**Example**:
![[Minimum Path Sum.png]]
**Input**: `grid = [[1,3,1],[1,5,1],[4,2,1]]`
**Output**: 7
**Explanation**: Because the path 1 → 3 → 1 → 1 → 1 minimizes the sum.

[Visit Leetcode](https://leetcode.com/problems/minimum-path-sum/)
[Visit GFG](https://www.geeksforgeeks.org/problems/minimum-cost-path3833/1)

---
### Brute Force Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(2<sup>(m + n)</sup>)
**Space Complexity**: O(m + n)

```cpp
class Solution {
  private:
	int solve(vector<vector<int>>& grid, int row, int col) {
		if (row == 0 && col == 0) {
			return grid[0][0];
		}
		
		if (row < 0 || col < 0) {
			return INT_MAX;
		}
		
		int up = solve(grid, row - 1, col);
		int left = solve(grid, row, col - 1);
		
		return grid[row][col] + min(up, left);
	}
	
  public:
	int minPathSum(vector<vector<int>>& grid) {
		int m = grid.size(), n = grid[0].size();
		
		return solve(grid, m - 1, n - 1);
	}
};
```

---
### Memoization

**Time Complexity**: O(m + n)
**Space Complexity**: O(mn) + O(m + n)

```cpp
class Solution {
  private:
	int solve(vector<vector<int>>& grid, int row, int col, vector<vector<int>>& dp) {
		if (row == 0 && col == 0) {
			return grid[0][0];
		}
		
		if (row < 0 || col < 0) {
			return INT_MAX;
		}
		
		if (dp[row][col] != -1) {
			return dp[row][col];
		}
		
		int up = solve(grid, row - 1, col, dp);
		int left = solve(grid, row, col - 1, dp);
		
		dp[row][col] = grid[row][col] + min(up, left);
		
		return dp[row][col];
	}
	
  public:
	int minPathSum(vector<vector<int>>& grid) {
		int m = grid.size(), n = grid[0].size();
		vector<vector<int>> dp(m, vector<int> (n, -1));
		
		return solve(grid, m - 1, n - 1, dp);
	}
};
```

---
### Tabulation

**Time Complexity**: O(m + n)
**Space Complexity**: O(mn)

```cpp
class Solution {
  public:
	int minPathSum(vector<vector<int>>& grid) {
		int m = grid.size(), n = grid[0].size();
		vector<vector<int>> dp(m, vector<int> (n, 0));
		
		for (int i = 0; i < m; i++) {
			for (int j = 0; j < n; j++) {
				if (i == 0 && j == 0) {
					dp[i][j] = grid[i][j];
				} else {
					int up = (i > 0) ? dp[i - 1][j] : INT_MAX;
					int left = (j > 0) ? dp[i][j - 1] : INT_MAX;
					
					dp[i][j] = grid[i][j] + min(up, left);
				}
			}
		}
		
		return dp[m - 1][n - 1];
	}
};
```

---
### Space Optimization

**Time Complexity**: O(m + n)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
	int minPathSum(vector<vector<int>>& grid) {
		int m = grid.size(), n = grid[0].size();
		vector<int> prev(n, 0);
		
		for (int i = 0; i < m; i++) {
			vector<int> current(n, 0);
			for (int j = 0; j < n; j++) {
				if (i == 0 && j == 0) {
					current[j] = grid[i][j];
				} else {
					int up = (i > 0) ? prev[j] : INT_MAX;
					int left = (j > 0) ? current[j - 1] : INT_MAX;
					
					current[j] = grid[i][j] + min(up, left);
				}
			}
			prev = current;
		}
		
		return prev[n - 1];
	}
};
```

---