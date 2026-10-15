**Problem**: Given an array **arr[]** which represents **the** dimensions of a sequence of matrices where the i<sup>th</sup> matrix has the dimensions `(arr[i-1] x arr[i])` for i>=1, find the most efficient way to multiply these matrices together. The efficient way is the one that involves the least number of multiplications.

**Example**:
**Input**: `arr[] = [2, 1, 3, 4]`
**Output**: 20
**Explanation**: There are 3 matrices of dimensions 2 × 1, 1 × 3, and 3 × 4, Let this 3 input matrices be M1, M2, and M3. There are two ways to multiply: ((M1 x M2) x M3) and (M1 x (M2 x M3)), note that the result of (M1 x M2) is a 2 x 3 matrix and result of (M2 x M3) is a 1 x 4 matrix. ((M1 x M2) x M3)  requires (2 x 1 x 3) + (2 x 3 x 4) = 30 
(M1 x (M2 x M3))  requires (1 x 3 x 4) + (2 x 1 x 4) = 20. The minimum of these two is 20.

[Visit GFG](https://www.geeksforgeeks.org/problems/matrix-chain-multiplication0303/1)

---
### Brute Force Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(3<sup>n</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
    int solve(vector<int> &arr, int start, int end) {
        if (start == end) {
            return 0;
        }
        
        int minCost = INT_MAX;
        
        for (int k = start; k < end; k++) {
            int cost = arr[start - 1] * arr[k] * arr[end] + solve(arr, start, k) + solve(arr, k + 1, end);
            minCost = min(minCost, cost);
        }
        
        return minCost;
    }
    
  public:
    int matrixMultiplication(vector<int> &arr) {
        int n = arr.size();
        vector<vector<int>> dp(n + 1, vector<int> (n + 1, -1));
        
        return solve(arr, 1, n - 1, dp);
    }
};
```

---
### Memoization

**Time Complexity**: O(n<sup>3</sup>)
**Space Complexity**: O(n<sup>2</sup>)

```cpp
class Solution {
  private:
    int solve(vector<int> &arr, int start, int end, vector<vector<int>>& dp) {
        if (start == end) {
            return dp[start][end] = 0;
        }
        
        if (dp[start][end] != -1) {
            return dp[start][end];
        }
        
        int minCost = INT_MAX;
        
        for (int k = start; k < end; k++) {
            int cost = arr[start - 1] * arr[k] * arr[end] + solve(arr, start, k, dp) + solve(arr, k + 1, end, dp);
            minCost = min(minCost, cost);
        }
        
        return dp[start][end] = minCost;
    }
    
  public:
    int matrixMultiplication(vector<int> &arr) {
        int n = arr.size();
        vector<vector<int>> dp(n + 1, vector<int> (n + 1, -1));
        
        return solve(arr, 1, n - 1, dp);
    }
};
```

---
### Tabulation

**Time Complexity**: O(n<sup>3</sup>)
**Space Complexity**: O(n<sup>2</sup>)

```cpp
class Solution {    
  public:
    int matrixMultiplication(vector<int> &arr) {
        int n = arr.size();
        vector<vector<int>> dp(n + 1, vector<int> (n + 1, INT_MAX));
        
        for (int i = 0; i < n; i++) {
	        dp[i][i] = 0;
        }
        
        for (int length = 2; length < n; ++length) {
	        for (int start = 1; start <= n - length; start++) {
		        int end = start + length - 1;
		        for (int k = start; k < end; k++) {
			        int cost = dp[start][k] + dp[k + 1][end] + arr[start - 1] * arr[k] * arr[end];
				    if (cost < dp[start][end]) {
					    dp[start][end] = cost;
					}
				}
	        }
        }
        
        return dp[1][n - 1];
    }
};
```

---