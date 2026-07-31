**Problem**: Given a set of items, each with a weight and a value, represented by the array **wt[]** and **val[]** respectively. Also, a knapsack with a weight limit **capacity**.  
Your task is to fill the knapsack in such a way that we can get the maximum profit. Return the **maximum profit**.

**Note:** Each item can be taken any number of times.

**Example**:
**Input**: `val[] = [1, 1], wt[] = [2, 1], capacity = 3`
**Output**: 3
**Explanation**: The optimal choice is to pick the 2<sup>nd</sup> element 3 times.

[Visit GFG](https://www.geeksforgeeks.org/problems/knapsack-with-duplicate-items4201/1)

---
### Brute Force Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(2<sup>n</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	int solve(vector<int>& val, vector<int>& wt, int index, int capacity) {
		if (index == 0) {
			return (capacity / wt[index]) * val[index];
		}
		
		int notTake = solve(val, wt, index - 1, capacity);
		int take = INT_MIN;
	    if (wt[index] <= capacity) {
	        take = val[index] + solve(val, wt, index, capacity - wt[index]);
	    }
		
		return max(take, notTake);
	}
	
  public:
    int knapSack(vector<int>& val, vector<int>& wt, int capacity) {
	    int n = wt.size();
	    
	    return solve(val, wt, n - 1,  capacity);
    }
};
```

---
### Memoization

**Time Complexity**: O(n x capacity)
**Space Complexity**: O(n x capacity) + O(n)

```cpp
class Solution {
  private:
	int solve(vector<int>& val, vector<int>& wt, int index, int capacity, vector<vector<int>>& dp) {
		if (index == 0) {
			return dp[index][capacity] = (capacity / wt[index]) * val[index];
		}
		
		if (dp[index][capacity] != -1) {
			return dp[index][capacity];
		}
		
		int notTake = solve(val, wt, index - 1, capacity, dp);
		
		int take = INT_MIN;
	    if (wt[index] <= capacity) {
	        take = val[index] + solve(val, wt, index, capacity - wt[index], dp);
	    }
		
		return dp[index][capacity] = max(take, notTake);
	}
	
  public:
    int knapSack(vector<int>& val, vector<int>& wt, int capacity) {
	    int n = wt.size();
	    vector<vector<int>> dp(n, vector<int>(capacity + 1, -1));
	    
	    return solve(val, wt, n - 1,  capacity, dp);
    }
};
```

---
### Tabulation

**Time Complexity**: O(n x capacity)
**Space Complexity**: O(n x capacity)

```cpp
class Solution {
  public:
    int knapSack(vector<int>& val, vector<int>& wt, int capacity) {
	    int n = wt.size();
	    vector<vector<int>> dp(n, vector<int>(capacity + 1, 0));
	    
	    for (int weight = 0; weight <= capacity; weight++) {
		    dp[0][weight] = (weight / wt[0]) * val[0];
	    }
	    
	    for (int index = 1; index < n; index++) {
		    for (int weight = 0; weight <= capacity; weight++) {
			    int notTake = dp[index - 1][weight];
			    int take = INT_MIN;
			    if (wt[index] <= weight) {
				    take = val[index] + dp[index][weight - wt[index]];
			    }
			    
			    dp[index][weight] = max(take, notTake);
		    }
	    }
	    
	    return dp[n - 1][capacity];
    }
};
```

---
### Space Optimization - I

**Time Complexity**: O(n x capacity)
**Space Complexity**: O(2 x capacity)

```cpp
class Solution {
  public:
    int knapSack(vector<int>& val, vector<int>& wt, int capacity) {
	    int n = wt.size();
	    vector<int> prev(capacity + 1, 0);
	    
	    for (int weight = 0; weight <= capacity; weight++) {
		    prev[weight] = (weight / wt[0]) * val[0];
	    }
	    
	    for (int index = 1; index < n; index++) {
		    for (int weight = 0; weight <= capacity; weight++) {
			    int notTake = prev[weight];
			    int take = INT_MIN;
			    if (wt[index] <= weight) {
				    take = val[index] + prev[weight - wt[index]];
			    }
			    
			    prev[weight] = max(take, notTake);
		    }
	    }
	    
	    return prev[capacity];
    }
};
```

---
### Space Optimization - II

**Time Complexity**: O(n x capacity)
**Space Complexity**: O(capacity)

```cpp
class Solution {
  public:
    int knapSack(vector<int>& val, vector<int>& wt, int capacity) {
	    int n = wt.size();
	    vector<int> prev(capacity + 1, 0), current(capacity + 1, 0);
	    
	    for (int weight = 0; weight <= capacity; weight++) {
		    prev[weight] = (weight / wt[0]) * val[0];
	    }
	    
	    for (int index = 1; index < n; index++) {
		    for (int weight = 0; weight <= capacity; weight++) {
			    int notTake = prev[weight];
			    int take = INT_MIN;
			    if (wt[index] <= weight) {
				    take = val[index] + current[weight - wt[index]];
			    }
			    
			    current[weight] = max(take, notTake);
		    }
		    prev = current;
	    }
	    
	    return prev[capacity];
    }
};
```

---
>[!tip]
>Big-O notation gives only the asymptotic growth. Actual runtime and memory usage depend on the recursion tree, constant factors, compiler optimizations, and input values. In interviews, it's usually sufficient to state the nature of the complexity (e.g., exponential, quadratic, linear) and justify it briefly.