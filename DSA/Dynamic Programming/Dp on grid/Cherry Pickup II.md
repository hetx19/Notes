**Problem**: You are given a `rows x cols` matrix `grid` representing a field of cherries where `grid[i][j]` represents the number of cherries that you can collect from the `(i, j)` cell.

You have two robots that can collect cherries for you:

- **Robot #1** is located at the **top-left corner** `(0, 0)`, and
- **Robot #2** is located at the **top-right corner** `(0, cols - 1)`.

Return _the maximum number of cherries collection using both robots by following the rules below_:

- From a cell `(i, j)`, robots can move to cell `(i + 1, j - 1)`, `(i + 1, j)`, or `(i + 1, j + 1)`.
- When any robot passes through a cell, It picks up all cherries, and the cell becomes an empty cell.
- When both robots stay in the same cell, only one takes the cherries.
- Both robots cannot move outside of the grid at any moment.
- Both robots should reach the bottom row in `grid`.

**Example**:
![[Cherry Pickup II.png|269]]
**Input**: `grid = [[3,1,1],[2,5,1],[1,5,5],[2,1,1]]`
**Output**: 24
**Explanation**: Path of robot #1 and #2 are described in color green and blue respectively.
Cherries taken by Robot #1, (3 + 2 + 5 + 2) = 12.
Cherries taken by Robot #2, (1 + 5 + 5 + 1) = 12.
Total of cherries: 12 + 12 = 24.

[Visit Leetcode](https://leetcode.com/problems/cherry-pickup-ii/)
[Visit GFG](https://www.geeksforgeeks.org/problems/chocolates-pickup/1)

---
### Brute Force Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(3<sup>n</sup> * 3<sup>n</sup>)
**Space Complexity**: O()

```cpp
class Solution {
  private:
	int solve(vector<vector<int>>& grid, int row, int col1, int col2, int n, int m) {
		if (col1 < 0 || col1 >= m || col2 < 0 || col2 >= m) {
			return INT_MIN;
		}
		
		if (row == n - 1) {
			if (col1 == col2) {
				return grid[row][col1];
			}
			
			return grid[row][col1] + grid[row][col2];
		}
		
		int maxi = INT_MIN;
		vector<int> deltaCol = {-1, 0, +1};
		
		for (int i = 0; i < 3; i++) {
			for (int j = 0; j < 3; j++) {
				int value = 0;
				if (col1 == col2) {
					value = grid[row][col1];
				} else {
					value = grid[row][col1] + grid[row][col2];
				}
				
				int ncol1 = col1 + deltaCol[i];
				int ncol2 = col2 + deltaCol[j];
				value += solve(grid, row + 1, ncol1, ncol2, n, m);
				
				maxi = max(maxi, value);
			}
		}
		
		return maxi;
	}
	
  public:
    int cherryPickup(vector<vector<int>>& grid) {
        int n = grid.size(), m = grid[0].size();
        
        return solve(grid, 0, 0, m - 1, n, m);
    }
};
```

---
### Memoization

**Time Complexity**: O()
**Space Complexity**: O()

```cpp
class Solution {
  private:
	int solve(vector<vector<int>>& grid, int row, int col1, int col2, int n, int m, vector<vector<vector<int>>>& dp) {
		if (col1 < 0 || col1 >= m || col2 < 0 || col2 >= m) {
			return INT_MIN;
		}
		
		if (row == n - 1) {
			if (col1 == col2) {
				return dp[row][col1][col2] = grid[row][col1];
			}
			
			return dp[row][col1][col2] = grid[row][col1] + grid[row][col2];
		}
		
		if (dp[row][col1][col2] != -1) {
		    return dp[row][col1][col2];
		}
		
		int maxi = INT_MIN;
		vector<int> deltaCol = {-1, 0, +1};
		
		for (int i = 0; i < 3; i++) {
			for (int j = 0; j < 3; j++) {
				int value = 0;
				if (col1 == col2) {
					value = grid[row][col1];
				} else {
					value = grid[row][col1] + grid[row][col2];
				}
				
				int ncol1 = col1 + deltaCol[i];
				int ncol2 = col2 + deltaCol[j];
				value += solve(grid, row + 1, ncol1, ncol2, n, m, dp);
				
				maxi = max(maxi, value);
			}
		}
		
		return dp[row][col1][col2] = maxi;
	}
	
  public:
    int cherryPickup(vector<vector<int>>& grid) {
        int n = grid.size(), m = grid[0].size();
        vector<vector<vector<int>>> dp(n, vector<vector<int>>(m, vector<int> (m, -1)));
        
        return solve(grid, 0, 0, m - 1, n, m, dp);
    }
};
```

---
### Tabulation

**Time Complexity**: O()
**Space Complexity**: O()

```cpp
class Solution {
  public:
    int cherryPickup(vector<vector<int>>& grid) {
        int n = grid.size(), m = grid[0].size();
        vector<vector<vector<int>>> dp(n, vector<vector<int>>(m, vector<int> (m, 0)));
        
        for (int col1 = 0; col1 < m; col1++) {
	        for (int col2 = 0; col2 < m; col2++) {
		        if (col1 == col2) {
			        dp[n - 1][col1][col2] = grid[n - 1][col1];
		        } else {
			        dp[n - 1][col1][col2] = grid[n - 1][col1] + grid[n - 1][col2];
		        }
	        }
        }
        
        for (int i = n - 2; i >= 0; i--) {
	        for (int col1 = 0; col1 < m; col1++) {
		        for (int col2 = 0; col2 < m; col2++) {
			        int maxi = INT_MIN;
			        vector<int> deltaCol = {-1, 0, +1};
			        
					for (int x = 0; x < 3; x++) {
						for (int y = 0; y < 3; y++) {
							int value = 0;
							if (col1 == col2) {
								value = grid[i][col1];
							} else {
								value = grid[i][col1] + grid[i][col2];
							}
				
							int ncol1 = col1 + deltaCol[x];
							int ncol2 = col2 + deltaCol[y];
							
							if (ncol1 >= 0 && ncol1 < m && ncol2 >= 0 && ncol2 < m) {
								value += dp[i + 1][ncol1][ncol2];
							} else {
								value += INT_MIN;
							}
							
							maxi = max(maxi, value);
						}
					}
					
					dp[i][col1][col2] = maxi;
		        }
	        }
        }
        
        return dp[0][0][m - 1];
    }
};
```

---
### Space Optimization

**Time Complexity**: O()
**Space Complexity**: O()

```cpp
class Solution {
  public:
    int cherryPickup(vector<vector<int>>& grid) {
        int n = grid.size(), m = grid[0].size();
        vector<vector<int>> front(m, vector<int> (m, 0));
        vector<vector<int>> current(m, vector<int>(m, 0));
        
        for (int col1 = 0; col1 < m; col1++) {
	        for (int col2 = 0; col2 < m; col2++) {
		        if (col1 == col2) {
			        front[col1][col2] = grid[n - 1][col1];
		        } else {
			        front[col1][col2] = grid[n - 1][col1] + grid[n - 1][col2];
		        }
	        }
        }
        
        for (int i = n - 2; i >= 0; i--) {
	        for (int col1 = 0; col1 < m; col1++) {
		        for (int col2 = 0; col2 < m; col2++) {
			        int maxi = INT_MIN;
			        vector<int> deltaCol = {-1, 0, +1};
			        
					for (int x = 0; x < 3; x++) {
						for (int y = 0; y < 3; y++) {
							int value = 0;
							if (col1 == col2) {
								value = grid[i][col1];
							} else {
								value = grid[i][col1] + grid[i][col2];
							}
				
							int ncol1 = col1 + deltaCol[x];
							int ncol2 = col2 + deltaCol[y];
							
							if (ncol1 >= 0 && ncol1 < m && ncol2 >= 0 && ncol2 < m) {
								value += front[ncol1][ncol2];
							} else {
								value += INT_MIN;
							}
							
							maxi = max(maxi, value);
						}
					}
					
					current[col1][col2] = maxi;
		        }
	        }
	        
	        front = current;
        }
        
        return front[0][m - 1];
    }
};
```

---