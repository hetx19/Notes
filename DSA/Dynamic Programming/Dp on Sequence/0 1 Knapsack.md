**Problem**: Given two arrays, **val[]** and **wt[]**, where each element represents the value and weight of an item respectively, and an integer **W** representing the maximum capacity of the knapsack (the total weight it can hold).

The task is to put the items into the knapsack such that the total value obtained is **maximum** without exceeding the capacity W.

**Note:** You can either include an item completely or exclude it entirely — fractional selection of items is not allowed. Each item is available only once.

**Example**:
**Input**: `W = 4`, `val[] = [1, 2, 3]`, `wt[] = [4, 5, 1]`
**Output**: 3  
**Explanation**: Choose the last item, which weighs 1 unit and has a value of 3.

[Visit GFG](https://www.geeksforgeeks.org/problems/0-1-knapsack-problem0945/1)

---
### Brute Force Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(2<sup>n</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	int solve(int W, vector<int> &val, vector<int> &wt, int index) {
		if (index == 0) {
			if (wt[index] <= W) {
				return val[index];
			}
			return 0;
		}
		
		int notTake = solve(W, val, wt, index - 1);
		int take = INT_MIN;
		
		if (wt[index] <= W) {
			take = val[index] + solve(W - wt[index], val, wt, index - 1);
		}
		
		return max(take, notTake);
	}
	
  public:
    int knapsack(int W, vector<int> &val, vector<int> &wt) {
	    int n = wt.size();
	    return solve(W, val, wt, n - 1);
    }
};
```

---
### Memoization

**Time Complexity**: O(nw)
**Space Complexity**: O(nw) + O(n)

```cpp
class Solution {
  private:
	int solve(int W, vector<int> &val, vector<int> &wt, int index, vector<vector<int>>& dp) {
		if (index == 0) {
			if (wt[index] <= W) {
				return dp[index][W] = val[index];
			}
			return dp[index][W] = 0;
		}
		
		if (dp[index][W] != -1) {
			return dp[index][W];
		}
		
		int notTake = solve(W, val, wt, index - 1, dp);
		int take = INT_MIN;
		
		if (wt[index] <= W) {
			take = val[index] + solve(W - wt[index], val, wt, index - 1, dp);
		}
		
		return dp[index][W] = max(take, notTake);
	}
	
  public:
    int knapsack(int W, vector<int> &val, vector<int> &wt) {
	    int n = wt.size();
	    vector<vector<int>> dp(n, vector<int>(W + 1, -1));
	    return solve(W, val, wt, n - 1, dp);
    }
};
```

---
### Tabulation

**Time Complexity**: O(nw)
**Space Complexity**: O(nw)

```cpp
class Solution {
  public:
    int knapsack(int W, vector<int> &val, vector<int> &wt) {
	    int n = wt.size();
	    vector<vector<int>> dp(n, vector<int>(W + 1, 0));
	    
	    for (int i = wt[0]; i <= W; i++) {
		    dp[0][i] = val[0];
	    }
	    
	    for (int index = 1; index < n; index++) {
		    for (int w = 0; w <= W; w++) {
			    int take = INT_MIN;
			    int notTake = dp[index - 1][w];
			    
			    if (wt[index] <= w) {
				    take = val[index] + dp[index - 1][w - wt[index]];
			    }
			    
			    dp[index][w] = max(take, notTake);
		    }
	    }
	    
	    return dp[n - 1][W];
    }
};
```

---
### Space Optimization - I

**Time Complexity**: O(nw)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
    int knapsack(int W, vector<int> &val, vector<int> &wt) {
	    int n = wt.size();
	    vector<int> prev(W + 1, 0), current(W + 1, 0);
	    
	    for (int i = wt[0]; i <= W; i++) {
		    prev[i] = val[0];
	    }
	    
	    for (int index = 1; index < n; index++) {
		    for (int w = 0; w <= W; w++) {
			    int take = INT_MIN;
			    int notTake = prev[w];
			    
			    if (wt[index] <= w) {
				    take = val[index] + prev[w - wt[index]];
			    }
			    
			    current[w] = max(take, notTake);
		    }
		    prev = current;
	    }
	    
	    return prev[W];
    }
};
```

---
### Space Optimization - II

**Time Complexity**: O(nw)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
    int knapsack(int W, vector<int> &val, vector<int> &wt) {
	    int n = wt.size();
	    vector<int> prev(W + 1, 0);
	    
	    for (int i = wt[0]; i <= W; i++) {
		    prev[i] = val[0];
	    }
	    
	    for (int index = 1; index < n; index++) {
		    for (int w = W; w >= 0; w--) {
			    int take = INT_MIN;
			    int notTake = prev[w];
			    
			    if (wt[index] <= w) {
				    take = val[index] + prev[w - wt[index]];
			    }
			    
			    prev[w] = max(take, notTake);
		    }
	    }
	    
	    return prev[W];
    }
};
```

---