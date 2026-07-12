**Problem**: There is a robot on an `m x n` grid. The robot is initially located at the **top-left corner** (i.e., `grid[0][0]`). The robot tries to move to the **bottom-right corner** (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.

Given the two integers `m` and `n`, return _the number of possible unique paths that the robot can take to reach the bottom-right corner_.

The test cases are generated so that the answer will be less than or equal to 2 * 10<sup>9</sup>.

**Example**:
![[Unique Paths.png|400]]
**Input**: `m = 3, n = 7`
**Output**: 28

[Visit Leetcode](https://leetcode.com/problems/unique-paths/)
[Visit GFG](https://www.geeksforgeeks.org/problems/number-of-paths0926/1)

---
### Brute Force Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(2<sup>(m + n)</sup>)
**Space Complexity**: O(m + n)

```cpp
class Solution {
  private:
    int solve(int row, int col) {
        if (row == 0 && col == 0) {
            return 1;
        }

        if (row < 0 || col < 0) {
            return 0;
        }

        int up = solve(row - 1, col);
        int left = solve(row, col - 1);

        return up + left;
    }

  public:
    int uniquePaths(int m, int n) {
        return solve(m - 1, n - 1);
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
    int solve(int row, int col, vector<vector<int>>& dp) {
        if (row == 0 && col == 0) {
            return dp[0][0] = 1;
        }

        if (row < 0 || col < 0) {
            return 0;
        }
        
        if (dp[row][col] != -1) {
	        return dp[row][col];
        }

        int up = solve(row - 1, col, dp);
        int left = solve(row, col - 1, dp);

        return dp[row][col] = up + left;
    }

  public:
    int uniquePaths(int m, int n) {
	    vector<vector<int>> dp(m, vector<int> (n, -1));
	    
        return solve(m - 1, n - 1, dp);
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
	int uniquePaths(int m, int n) {
		vector<vector<int>> dp(m, vector<int>(n, 0));
		
		for (int i = 0; i < m; i++) {
			for (int j = 0; j < n; j++) {
				if (i == 0 && j == 0) {
					dp[i][j] = 1;
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
	int uniquePaths(int m, int n) {
		vector<int> prev(n, 0);
		
		for (int i = 0; i < m; i++) {
			vector<int> current(n, 0);
			for (int j = 0; j < n; j++) {
				if (i == 0 && j == 0) {
					current[j] = 1;
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
### Combinatorics

**Time Complexity**: O(min(m, n))
**Space Complexity**: O(1)

```cpp
using ll = long long;

class Solution {
  private:
	int nCr(int n, int r) {
		ll ans = 1;
		
		for (int i = 0; i < r; i++) {
			ans = ans * (n - i);
			ans = ans / (i + 1);
		}
		
		return (int)ans;
	}
	
  public:
	int uniquePaths(int m, int n) {
		// By using simple mathematics
		int length = max(m - 1, n - 1);
		int breath = min(m - 1, n - 1);
		
		int ans = nCr(length + breath, breath);
		
		return ans;
	}
};
```

---