**Problem**: You are given a matrix `mat[][]` of size **n** x **m** where each element is a positive integer. Starting from any cell in the first row, you are allowed to move to the next row, but with specific movement constraints. From any cell `(r, c)` in the current row, you can move to any of the three possible positions :

1. `(r+1, c-1)` — move diagonally to the left.
2. `(r+1, c)` — move directly down.
3. `(r+1, c+1)` — move diagonally to the right.

Find the maximum sum of any path starting from any column in the first row and ending at any column in the last row, following the above movement constraints.

**Example**:

**Input**: `mat[][] = [[3, 6, 1], [2, 3, 4], [5, 5, 1]]`
**Output**: 15
**Explaination**: The best path is (0, 1) -> (1, 2) -> (2, 1). It gives the maximum sum as 15.

[Visit GFG](https://www.geeksforgeeks.org/problems/path-in-matrix3805/1)

---
### Brute Force Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(3<sup>n</sup>) + O(m)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	int solve(vector<vector<int>>& mat, int row, int col) {
		if (col < 0 || row < 0 || com >= mat[0].size()) {
			return INT_MIN;
		}
		
		if (row == 0) {
			return mat[row][col];
		}
		
		int up = solve(mat, row - 1, col);
		int upLeft = (col > 0) ? solve(mat, row - 1, col - 1) : INT_MIN;
		int upRight = (col < mat[0].size() - 1) ? solve(mat, row - 1, col + 1) : INT_MIN;
		
		return mat[row][col] + max({up, upLeft, upRight});
	}
	
  public:
    int maximumPath(vector<vector<int>>& mat) {
	    int n = mat.size(), m = mat[0].size();
	    
	    int ans = INT_MIN;
	    for (int col = 0; col < m; col++) {
		    ans = max(ans, solve(mat, n - 1, col));
	    }
	    
	    return ans;
    }
};
```

---
### Memoization

**Time Complexity**: O(mn) + O(m)
**Space Complexity**: O(mn) + O(n)

```cpp
class Solution {
  private:
	int solve(vector<vector<int>>& mat, int row, int col, vector<vector<int>>& dp) {
		if (row < 0 || col < 0 || col >= mat[0].size()) {
		    return INT_MIN;
		}
		
		if (row == 0) {
			return dp[row][col] = mat[row][col];
		}
		
		if (dp[row][col] != -1) {
		    return dp[row][col];
		}
		
		int up = solve(mat, row - 1, col, dp);
		int upLeft = (col > 0) ? solve(mat, row - 1, col - 1, dp) : INT_MIN;
		int upRight = (col < mat[0].size() - 1) ? solve(mat, row - 1, col + 1, dp) : INT_MIN;
		
		return dp[row][col] = mat[row][col] + max({up, upLeft, upRight});
	}
	
  public:
    int maximumPath(vector<vector<int>>& mat) {
	    int n = mat.size(), m = mat[0].size();
	    vector<vector<int>> dp(n, vector<int> (m, -1));
	    
	    int ans = INT_MIN;
	    for (int col = 0; col < m; col++) {
		    ans = max(ans, solve(mat, n - 1, col, dp));
	    }
	    
	    return ans;
    }
};
```

---
### Tabulation

**Time Complexity**: O(mn) + O(m)
**Space Complexity**: O(mn)

```cpp
class Solution {
  public:
    int maximumPath(vector<vector<int>>& mat) {
	    int n = mat.size(), m = mat[0].size();
	    vector<vector<int>> dp(n, vector<int> (m, 0));
	    
	    for (int col = 0; col < m; col++) {
	        dp[0][col] = mat[0][col];
	    }
	    
	    for (int i = 1; i < n; i++) {
	        for (int j = 0; j < m; j++) {
	            int up = dp[i - 1][j];
	            int upLeft = ((j > 0) ? dp[i - 1][j - 1] : INT_MIN);
	            int upRight = ((j < m - 1) ? dp[i - 1][j + 1] : INT_MIN);
	            
	            dp[i][j] = mat[i][j] + max({up, upLeft, upRight});
	        }
	    }
	    
	    int ans = INT_MIN;
	    for (int col = 0; col < m; col++) {
		    ans = max(ans, dp[n - 1][col]);
	    }
	    
	    return ans;
    }
};
```

---
### Space Optimization

**Time Complexity**: O(mn) + O(m)
**Space Complexity**: O(m)

```cpp
class Solution {
  public:
    int maximumPath(vector<vector<int>>& mat) {
	    int n = mat.size(), m = mat[0].size();
	    vector<int> prev(m, 0);
	    
	    for (int col = 0; col < m; col++) {
	        prev[col] = mat[0][col];
	    }
	    
	    for (int i = 1; i < n; i++) {
	        vector<int> current(m, 0);
	        for (int j = 0; j < m; j++) {
	            int up = prev[j];
	            int upLeft = ((j > 0) ? prev[j - 1] : INT_MIN);
	            int upRight = ((j < m - 1) ? prev[j + 1] : INT_MIN);
	            
	            current[j] = mat[i][j] + max({up, upLeft, upRight});
	        }
	        prev = current;
	    }
	    
	    int ans = INT_MIN;
	    for (int col = 0; col < m; col++) {
		    ans = max(ans, prev[col]);
	    }
	    
	    return ans;
    }
};
```

---