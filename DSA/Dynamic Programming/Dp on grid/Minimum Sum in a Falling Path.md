**Problem**: Given a matrix `mat[][]` of size n*n, where each cell contains an integer value. A falling path starts from any cell in the first row and moves downward to the last row.

 From a cell `(i, j)`, you may move to the cell directly below `(i+1, j)`, the cell diagonally down-left `(i+1, j-1)`, or the cell diagonally down-right `(i+1, j+1)` if they exist. Your task is to compute the **minimum sum of values** along any valid falling path from the top row to the bottom row.

**Example**:
**Input** `mat[][] = [[5, 10], [25, 15]]
**Output**: 20
**Explanation**: The minimum falling path is `5 → 15` with sum `20`.

[Visit GFG](https://www.geeksforgeeks.org/problems/minimum-sum-in-a-falling-path/1)

---
### Brute Force Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(3<sup>n</sup>) + O(n)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	int solve(vector<vector<int>>& mat, int row, int col) {
		if (col < 0 || row < 0 || col >= mat.size()) {
			return INT_MAX;
		}
		
		if (row == 0) {
			return mat[row][col];
		}
		
		int up = solve(mat, row - 1, col);
		int upLeft = (col > 0) ? solve(mat, row - 1, col - 1) : INT_MAX;
		int upRight = (col < mat[0].size() - 1) ? solve(mat, row - 1, col + 1) : INT_MAX;
		
		return mat[row][col] + min({up, upLeft, upRight});
	}
	
  public:
    int minFallingPathSum(vector<vector<int>>& mat) {
	    int n = mat.size();
	    
	    int ans = INT_MAX;
	    for (int col = 0; col < n; col++) {
		    ans = min(ans, solve(mat, n - 1, col));
	    }
	    
	    return ans;
    }
};
```

---
### Memoization

**Time Complexity**: O(n<sup>2</sup>) + O(n)
**Space Complexity**: O(n<sup>2</sup>) + O(n)

```cpp
class Solution {
  private:
	int solve(vector<vector<int>>& mat, int row, int col, vector<vector<int>>& dp) {
		if (row < 0 || col < 0 || col >= mat[0].size()) {
		    return INT_MAX;
		}
		
		if (row == 0) {
			return dp[row][col] = mat[row][col];
		}
		
		if (dp[row][col] != -1) {
		    return dp[row][col];
		}
		
		int up = solve(mat, row - 1, col, dp);
		int upLeft = (col > 0) ? solve(mat, row - 1, col - 1, dp) : INT_MAX;
		int upRight = (col < mat[0].size() - 1) ? solve(mat, row - 1, col + 1, dp) : INT_MAX;
		
		return dp[row][col] = mat[row][col] + min({up, upLeft, upRight});
	}
	
  public:
    int minFallingPathSum(vector<vector<int>>& mat) {
	    int n = mat.size();
	    vector<vector<int>> dp(n, vector<int> (n, -1));
	    
	    int ans = INT_MAX;
	    for (int col = 0; col < n; col++) {
		    ans = min(ans, solve(mat, n - 1, col, dp));
	    }
	    
	    return ans;
    }
};
```

---
### Tabulation

**Time Complexity**: O(n<sup>2</sup>) + O(n)
**Space Complexity**: O(n<sup>2</sup>)

```cpp
class Solution {
  public:
    int minFallingPathSum(vector<vector<int>>& mat) {
	    int n = mat.size();
	    vector<vector<int>> dp(n, vector<int> (n, 0));
	    
	    for (int col = 0; col < n; col++) {
	        dp[0][col] = mat[0][col];
	    }
	    
	    for (int i = 1; i < n; i++) {
	        for (int j = 0; j < n; j++) {
	            int up = dp[i - 1][j];
	            int upLeft = ((j > 0) ? dp[i - 1][j - 1] : INT_MAX);
	            int upRight = ((j < n - 1) ? dp[i - 1][j + 1] : INT_MAX);
	            
	            dp[i][j] = mat[i][j] + min({up, upLeft, upRight});
	        }
	    }
	    
	    int ans = INT_MAX;
	    for (int col = 0; col < n; col++) {
		    ans = min(ans, dp[n - 1][col]);
	    }
	    
	    return ans;
    }
};
```

---
### Space Optimization

**Time Complexity**: O(n<sup>2</sup>) + O(n)
**Space Complexity**: O(m)

```cpp
class Solution {
  public:
    int minFallingPathSum(vector<vector<int>>& mat) {
	    int n = mat.size();
	    vector<int> prev(n, 0);
	    
	    for (int col = 0; col < n; col++) {
	        prev[col] = mat[0][col];
	    }
	    
	    for (int i = 1; i < n; i++) {
	        vector<int> current(n, 0);
	        for (int j = 0; j < n; j++) {
	            int up = prev[j];
	            int upLeft = ((j > 0) ? prev[j - 1] : INT_MAX);
	            int upRight = ((j < n - 1) ? prev[j + 1] : INT_MAX);
	            
	            current[j] = mat[i][j] + min({up, upLeft, upRight});
	        }
	        prev = current;
	    }
	    
	    int ans = INT_MAX;
	    for (int col = 0; col < n; col++) {
		    ans = min(ans, prev[col]);
	    }
	    
	    return ans;
    }
};
```

---