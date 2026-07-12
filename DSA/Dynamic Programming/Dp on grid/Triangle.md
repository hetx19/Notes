**Problem**: Given a `triangle` array, return _the minimum path sum from top to bottom_.

For each step, you may move to an adjacent number of the row below. More formally, if you are on index `i` on the current row, you may move to either index `i` or index `i + 1` on the next row.

**Example**:
**Input**: `triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]`
**Output**: 11
**Explanation**: The triangle looks like:
```
   2
  3 4
 6 5 7
4 1 8 3
```
The minimum path sum from top to bottom is 2 + 3 + 5 + 1 = 11 (underlined above).

[Visit Leetcode](https://leetcode.com/problems/triangle/description/)
[Visit GFG](https://www.geeksforgeeks.org/problems/triangle-path-sum/1)

---
### Brute Force Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(2<sup>(n)</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	int solve(vector<vector<int>>& triangle, int row, int col, int n) {
		if (row == n - 1) {
			return triangle[row][col];
		}
		
		int down = solve(triangle, row + 1, col, n);
		int diag = solve(triangle, row + 1, col + 1, n);
		
		return triangle[row][col] + min(down, diag);
	}
	
  public:
	int minimumTotal(vector<vector<int>>& triangle) {
		int n = triangle.size();
		return solve(triangle, 0, 0, n, dp);
	}
};
```

---
### Memoization

**Time Complexity**: O(n<sup>2</sup>)
**Space Complexity**: O(n<sup>2</sup>) + O(n)

```cpp
class Solution {
  private:
	int solve(vector<vector<int>>& triangle, int row, int col, int n, vector<vector<int>>& dp) {
		if (row == n - 1) {
			return dp[row][col] = triangle[row][col];
		}
		
		if (dp[row][col] != -1) {
			return dp[row][col];
		}
		
		int down = solve(triangle, row + 1, col, n, dp);
		int diag = solve(triangle, row + 1, col + 1, n, dp);
		
		dp[row][col] = triangle[row][col] + min(down, diag);
		
		return dp[row][col];
	}
	
  public:
	int minimumTotal(vector<vector<int>>& triangle) {
		int n = triangle.size();
		vector<vector<int>> dp(n);
		
		for (int i = 0; i < n; i++) {
			dp[i].resize(i + 1, -1);
		}
		
		return solve(triangle, 0, 0, n, dp);
	}
};
```

---
### Tabulation

**Time Complexity**: O(n<sup>2</sup>)
**Space Complexity**: O(n<sup>2</sup>)

```cpp
class Solution {
  public:
    int minimumTotal(vector<vector<int>>& triangle) {
	    int n = triangle.size();
	    vector<vector<int>> dp(n);
	    
	    for (int i = 0; i < n; i++) {
		    dp[i].resize(i + 1, 0);
		}
		
		for (int j = 0; j < n; j++) {
			dp[n - 1][j] = triangle[n - 1][j];
		}
		
		for (int i = n - 2; i >= 0; i--) {
			for (int j = i; j >= 0; j--) {
				int down = dp[i + 1][j];
				int diag = dp[i + 1][j + 1];
				
				dp[i][j] = triangle[i][j] + min(down, diag);
			}
		}
		
		return dp[0][0];
	}
};
```

---
### Space Optimization

**Time Complexity**: O(n<sup>2</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
	int minimumTotal(vector<vector<int>>& triangle) {
		int n = triangle.size();
		vector<int> front(n, 0), current(n, 0);

		for (int j = 0; j < n; j++) {
			front[j] = triangle[n - 1][j];
		}

  

		for (int i = n - 2; i >= 0; i--) {
			for (int j = i; j >= 0; j--) {
				int down = front[j];
				int diag = front[j + 1];
				
				current[j] = triangle[i][j] + min(down, diag);
			}
			
			front = current;
		}
		
		return front[0];
	}
};
```

---