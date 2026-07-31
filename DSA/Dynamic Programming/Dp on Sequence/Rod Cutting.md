**Problem**: Given a rod of length **n** inches and an array **price[]**, where price[i] denotes the value of a piece of length i **(1-based Index)**. Determine the maximum value obtainable by cutting up the rod and selling the pieces.

**Note:** The value of n is equal to the size of price array.

**Example**:
**Input**: `price[] = [1, 5, 8, 9, 10, 17, 17, 20]`
**Output**: 22  
**Explanation**: The maximum obtainable value is 22 by cutting in two pieces of lengths 2 and 6, i.e., 5 + 17 = 22

[Visit GFG](https://www.geeksforgeeks.org/problems/rod-cutting0840/1)

---
### Brute Force Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(2<sup>n</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	int solve(vector<int>& price, int index, int length) {
		if (index == 0) {
			return length * price[index];
		}
		
		int notTake = solve(price, index - 1, length);
		int take = INT_MIN;
		
		if (index + 1 <= length) {
			take = price[index] + solve(price, index, length - (index + 1));
		}
		
		return max(take, notTake);
	}
	
  public:
    int cutRod(vector<int> &price) {
        int n = price.size();
        
        return solve(price, n - 1, n);
    }
};
```

---
### Memoization

**Time Complexity**: O(n * (n + 1)) → O(n<sup>2</sup>)
**Space Complexity**: O(n<sup>2</sup>) + O(n)

```cpp
class Solution {
  private:
	int solve(vector<int>& price, int index, int length, vector<vector<int>>& dp) {
		if (index == 0) {
			return dp[index][length] = length * price[index];
		}
		
		if (dp[index][length] != -1) {
			return dp[index][length];
		}
		
		int notTake = solve(price, index - 1, length, dp);
		int take = INT_MIN;
		
		if (index + 1 <= length) {
			take = price[index] + solve(price, index, length - (index + 1), dp);
		}
		
		return dp[index][length] = max(take, notTake);
	}
	
  public:
    int cutRod(vector<int> &price) {
        int n = price.size();
        vector<vector<int>> dp(n, vector<int>(n + 1, -1));
        
        return solve(price, n - 1, n, dp);
    }
};
```

---

### Tabulation

**Time Complexity**: O(n * (n + 1)) → O(n<sup>2</sup>)
**Space Complexity**: O(n<sup>2</sup>)

```cpp
class Solution {
  public:
	int cutRod(vector<int> &price) {
		int n = price.size();
        vector<vector<int>> dp(n, vector<int>(n + 1, 0));
        
        for (int length = 0; length <= n; length++) {
	        dp[0][length] = length * price[0];
        }
        
        for (int index = 1; index < n; index++) {
	        for (int length = 0; length <= n; length++) {
		        int notTake = dp[index - 1][length];
		        int take = INT_MIN;
		        
		        if ((index + 1) <= length) {
			        take = price[index] + dp[index][length - (index + 1)];
		        }
		        
		        dp[index][length] = max(take, notTake);
	        }
        }
        
        return dp[n - 1][n];
	}
};
```

---
### Space Optimization - I

**Time Complexity**: O(n * (n + 1)) → O(n<sup>2</sup>)
**Space Complexity**: O(2n)

```cpp
class Solution {
  public:
	int cutRod(vector<int> &price) {
		int n = price.size();
        vector<int> prev(n + 1, 0), current(n + 1, 0);
        
        for (int length = 0; length <= n; length++) {
	        prev[length] = length * price[0];
        }
        
        for (int index = 1; index < n; index++) {
	        for (int length = 0; length <= n; length++) {
		        int notTake = prev[length];
		        int take = INT_MIN;
		        
		        if ((index + 1) <= length) {
			        take = price[index] + current[length - (index + 1)];
		        }
		        
		        current[length] = max(take, notTake);
	        }
	        prev = current;
        }
        
        return prev[n];
	}
};
```

---
### Space Optimization - II

**Time Complexity**: O(n * (n + 1)) → O(n<sup>2</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
	int cutRod(vector<int> &price) {
		int n = price.size();
        vector<int> current(n + 1, 0);
        
        for (int length = 0; length <= n; length++) {
	        current[length] = length * price[0];
        }
        
        for (int index = 1; index < n; index++) {
	        for (int length = 0; length <= n; length++) {
		        int notTake = current[length];
		        int take = INT_MIN;
		        
		        if ((index + 1) <= length) {
			        take = price[index] + current[length - (index + 1)];
		        }
		        
		        current[length] = max(take, notTake);
	        }
        }
        
        return current[n];
	}
};
```

---

>[!tip]
>Big-O notation gives only the asymptotic growth. Actual runtime and memory usage depend on the recursion tree, constant factors, compiler optimizations, and input values. In interviews, it's usually sufficient to state the nature of the complexity (e.g., exponential, quadratic, linear) and justify it briefly.