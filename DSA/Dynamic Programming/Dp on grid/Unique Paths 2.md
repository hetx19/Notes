You are given an `m x n` integer array `grid`. There is a robot initially located at the **top-left corner** (i.e., `grid[0][0]`). The robot tries to move to the **bottom-right corner** (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.

An obstacle and space are marked as `1` or `0` respectively in `grid`. A path that the robot takes cannot include **any** square that is an obstacle.

Return _the number of possible unique paths that the robot can take to reach the bottom-right corner_.

The testcases are generated so that the answer will be less than or equal to 2 * 10<sup>9</sup>.

**Example**:
![[Unique Paths 2.png]]
**Input**: `obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]`
**Output**: 2
**Explanation**: There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:
1. Right -> Right -> Down -> Down
2. Down -> Down -> Right -> Right

[Visit Leetcode](https://leetcode.com/problems/unique-paths-ii/)
[Visit GFG](https://www.geeksforgeeks.org/problems/unique-paths-in-a-grid--170647/1)

---
### Brute Force Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(2<sup>(m + n)</sup>)
**Space Complexity**: O(m + n)

```cpp
class Solution {
  private:
	int solve(vector<vector<int>>& obstacleGrid, int row, int col) {
		if (row >= 0 && col >= 0 && obstacleGrid[row][col]) {
			return 0;
		}
		
		if (row == 0 && col == 0) {
			return 1;
		}
		
		if (row < 0 || col < 0) {
			return 0;
		}
		
		int up = solve(obstacleGrid, row - 1, col);
		int down = solve(obstacleGrid, row, col - 1);
		
		return up + down;
	}
	
  public:
	int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
		int m = obstacleGrid.size(), n = obstacleGrid[0].size();
		
        return solve(obstacleGrid, m - 1, n - 1);
	}
};
```

---
### Memoization

**Time Complexity**: O(m + n)
**Space Complexity**: O(m + n) + O(mn)

```cpp
class Solution {
  private:
	int solve(vector<vector<int>>& obstacleGrid, int row, int col, vector<vector<int>>& dp) {
		if (row >= 0 && col >= 0 && obstacleGrid[row][col]) {
			return dp[row][col] = 0;
		}
		
		if (row == 0 && col == 0) {
			return dp[row][col] = 1;
		}
		
		if (row < 0 || col < 0) {
			return 0;
		}
		
		if (dp[row][col] != -1) {
			return dp[row][col];
		}
		
		int up = solve(obstacleGrid, row - 1, col, dp);
		int down = solve(obstacleGrid, row, col - 1, dp);
		
		return dp[row][col] = up + down;
	}
	
  public:
	int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
		int m = obstacleGrid.size(), n = obstacleGrid[0].size();
		vector<vector<int>> dp(m, vector<int> (n, -1));
		
        return solve(obstacleGrid, m - 1, n - 1, dp);
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
	int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
		int m = obstacleGrid.size(), n = obstacleGrid[0].size();
		vector<vector<int>> dp(m, vector<int>(n, 0));
		
		if (obstacleGrid[0][0] || obstacleGrid[m - 1][n - 1]) {
			return 0;
		}
		
		for (int i = 0; i < m; i++) {
			for (int j = 0; j < n; j++) {
				if (i == 0 && j == 0) {
					dp[i][j] = 1;
				} else if (obstacleGrid[i][j] == 1) {
					dp[i][j] = 0;
				} else {
					if (i > 0 && j > 0) {
						dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
					} else if (i > 0) {
						dp[i][j] = dp[i - 1][j];
					} else if (j > 0) {
						dp[i][j] = dp[i][j - 1];
					}
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
	int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
		int m = obstacleGrid.size(), n = obstacleGrid[0].size();
		vector<int> prev(n, 0);
		
		if (obstacleGrid[0][0] || obstacleGrid[m - 1][n - 1]) {
			return 0;
		}
		
		for (int i = 0; i < m; i++) {
			vector<int> current(n, 0);
			for (int j = 0; j < n; j++) {
				if (i == 0 && j == 0) {
					current[j] = 1;
				} else if (obstacleGrid[i][j] == 1) {
					current[j] = 0;
				} else {
					if (i > 0 && j > 0) {
						current[j] = prev[j] + current[j - 1];
					} else if (i > 0) {
						current[j] = prev[j];
					} else if (j > 0) {
						current[j] = current[j - 1];
					}
				}
			}
			prev = current;
		}
		
		return prev[n - 1];
	}
};
```

---