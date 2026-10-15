**Problem**: You are given an array `prices` where `prices[i]` is the price of a given stock on the i<sup>th</sup> day.

Find the maximum profit you can achieve. You may complete **at most two transactions**.

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

**Example**:
**Input**: `prices = [3,3,5,0,0,3,1,4]`
**Output**: 6
**Explanation**: Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.

[Visit Leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)

---
### Brute Force Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(2<sup>n</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	int solve(vector<int>& prices, int index, bool canBuy, int cap) {
		if (index == prices.size() || cap == 0) {
			return 0;
		}
		
		int profit = 0;
		
		if (canBuy) {
			profit = max(-prices[index] + solve(prices, index + 1, false, cap), solve(prices, index + 1, true, cap));
		} else {
			profit = max(prices[index] + solve(prices, index + 1, true, cap - 1), solve(prices, index + 1, false, cap));
		}
		
		return profit;
	}
	
  public:
	int maxProfit(vector<int>& prices) {
		return solve(prices, 0, true, 2);
	}
};
```

### Alternative Solution

**Time Complexity**: O(2<sup>n</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	int solve(vector<int>& prices, int n, int index, int transactionId) {
		if (index == n || transactionId == 4) {
			return 0;
		}
		
		int profit = 0;
		
		if (transactionId & 1) {
			profit = max(prices[index] + solve(prices, n, index + 1, transactionId + 1), solve(prices, n, index + 1, transactionId));
		} else {
			profit = max(-prices[index] + solve(prices, n, index + 1, transactionId + 1), solve(prices, n, index + 1, transactionId));
		}
		
		return profit;
	}
	
  public:
	int maxProfit(vector<int>& prices) {
		int n = prices.size();
		return solve(prices, n, 0, 0);
	}
};
```

---
### Memoization

**Time Complexity**: O(n x 2 x 3)
**Space Complexity**: O(n x 2 x 3) + O(n)

```cpp
class Solution {
  private:
	int solve(vector<int>& prices, int n, int index, bool canBuy, int cap, vector<vector<vector<int>>>& dp) {
		if (index == n) {
			return 0;
		}
		
		if (cap == 0) {
			return dp[index][canBuy][cap] = 0;
		}
		
		if (dp[index][canBuy][cap] != -1) {
			return dp[index][canBuy][cap];
		}
		
		int profit = 0;
		
		if (canBuy) {
			profit = max(-prices[index] + solve(prices, n, index + 1, false, cap, dp), solve(prices, n, index + 1, true, cap, dp));
		} else {
			profit = max(prices[index] + solve(prices, n, index + 1, true, cap - 1, dp), solve(prices, n, index + 1, false, cap, dp));
		}
		
		return dp[index][canBuy][cap] = profit;
	}
	
  public:
	int maxProfit(vector<int>& prices) {
		int n = prices.size();
		vector<vector<vector<int>>> dp(n, vector<vector<int>>(2, vector<int>(3, -1)));
		
		return solve(prices, n, 0, true, 2, dp);
	}
};
```

### Alternative Solution

**Time Complexity**: O(n x 4)
**Space Complexity**: O(n x 4) + O(n)

```cpp
class Solution {
  private:
	int solve(vector<int>& prices, int n, int index, int transactionId, vector<vector<int>>& dp) {
		if (index == n || transactionId == 4) {
			return 0;
		}
		
		if (dp[index][transactionId] != -1) {
			return dp[index][transactionId];
		}
		
		int profit = 0;
		
		if (transactionId & 1) {
			profit = max(prices[index] + solve(prices, n, index + 1, transactionId + 1, dp), solve(prices, n, index + 1, transactionId, dp));
		} else {
			profit = max(-prices[index] + solve(prices, n, index + 1, transactionId + 1, dp), solve(prices, n, index + 1, transactionId, dp));
		}
		
		return dp[index][transactionId] = profit;
	}
	
  public:
	int maxProfit(vector<int>& prices) {
		int n = prices.size();
		vector<vector<int>> dp(n, vector<int>(4, -1));
		
		return solve(prices, n, 0, 0, dp);
	}
};
```


---
### Tabulation

**Time Complexity**: O(n x 2 x 3)
**Space Complexity**: O(n x 2 x 3)

```cpp
class Solution {
  public:
	int maxProfit(vector<int>& prices) {
		int n = prices.size();
		vector<vector<vector<int>>> dp(n + 1, vector<vector<int>>(2, vector<int> (3, 0)));
		
		for (int index = n - 1; index >= 0; index--) {
			for (int canBuy = 0; canBuy < 2; canBuy++) {
				for (int cap = 1; cap < 3; cap++) {
					int profit = 0;
				
					if (canBuy) {
						profit = max(-prices[index] + dp[index + 1][0][cap], dp[index + 1][1][cap]);
					} else {
						profit = max(prices[index] + dp[index + 1][1][cap - 1], dp[index + 1][0][cap]);
					}
				
					dp[index][canBuy][cap] = profit;
				}
			}
		}
		
		return dp[0][true][2];
	}
};
```

### Alternative Solution

**Time Complexity**: O(n x 4)
**Space Complexity**: O((n + 1) x 5)

```cpp
class Solution {
  public:
	int maxProfit(vector<int>& prices) {
		int n = prices.size();
		vector<vector<int>> dp(n + 1, vector<int>(5, 0));
		
		for (int index = n - 1; index >= 0; index--) {
			for (int transactionId = 0; transactionId < 4; transactionId++) {
				int profit = 0;
				
				if (transactionId & 1) {
					profit = max(prices[index] + dp[index + 1][transactionId + 1], dp[index + 1][transactionId]);
				} else {
					profit = max(-prices[index] + dp[index + 1][transactionId + 1], dp[index + 1][transactionId]);
				}
				
				dp[index][transactionId] = profit;
			}
		}
		
		return dp[0][0];
	}
};
```

---
### Space Optimization

**Time Complexity**: O(n x 2 x 3)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int maxProfit(vector<int>& prices) {
		int n = prices.size();
		vector<vector<int>> current(2, vector<int>(3, 0)), next(2, vector<int>(3, 0));
		
		for (int index = n - 1; index >= 0; index--) {
			for (int canBuy = 0; canBuy < 2; canBuy++) {
				for (int cap = 1; cap < 3; cap++) {
					int profit = 0;
				
					if (canBuy) {
						profit = max(-prices[index] + next[0][cap], next[1][cap]);
					} else {
						profit = max(prices[index] + next[1][cap - 1], next[0][cap]);
					}
				
					current[canBuy][cap] = profit;
				}
			}
			next = current;
		}
		
		return current[true][2];
	}
};
```

### Alternative Solution

**Time Complexity**: O(n x 4)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int maxProfit(vector<int>& prices) {
		int n = prices.size();
		vector<int> current(5, 0), next(5, 0);
		
		for (int index = n - 1; index >= 0; index--) {
			for (int transactionId = 0; transactionId < 4; transactionId++) {
				int profit = 0;
				
				if (transactionId & 1) {
					profit = max(prices[index] + next[transactionId + 1], next[transactionId]);
				} else {
					profit = max(-prices[index] + next[transactionId + 1], next[transactionId]);
				}
				
				current[transactionId] = profit;
			}
			
			next = current;
		}
		
		return current[0];
	}
};
```

---