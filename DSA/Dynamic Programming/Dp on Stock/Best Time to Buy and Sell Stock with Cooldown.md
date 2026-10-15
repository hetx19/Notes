**Problem**: You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:

- After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

**Example**:
**Input**: `prices = [1,2,3,0,2]`
**Output**: 3
**Explanation**: `transactions = [buy, sell, cooldown, buy, sell]`

[Visit Leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

---
### Brute Force Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(2<sup>n</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	int solve(vector<int>& prices, int n, int index, bool canBuy) {
		if (index >= n) {
			return 0;
		}
		
		int profit = 0;
		
		if (canBuy) {
			profit = max(-prices[index] + solve(prices, n, index + 1, false), solve(prices, n, index + 1, true));
		} else {
			profit = max(prices[index] + solve(prices, n, index + 2, true), solve(prices, n, index + 1, false));
		}
		
		return profit;
	}
	
  public:
	int maxProfit(vector<int>& prices) {
		int n = prices.size();
		
		return solve(prices, n, 0, true);
	}
};
```

---
### Memoization

**Time Complexity**: O(n)
Space Complexity: O(n) + O(n)

```cpp
class Solution {
  private:
	int solve(vector<int>& prices, int n, int index, bool canBuy, vector<vector<int>>& dp) {
		if (index >= n) {
			return 0;
		}
		
		if (dp[index][canBuy] != -1) {
			return dp[index][canBuy];
		}
		
		int profit = 0;
		
		if (canBuy) {
			profit = max(-prices[index] + solve(prices, n, index + 1, false, dp), solve(prices, n, index + 1, true, dp));
		} else {
			profit = max(prices[index] + solve(prices, n, index + 2, true, dp), solve(prices, n, index + 1, false, dp));
		}
		
		return dp[index][canBuy] = profit;
	}
	
  public:
	int maxProfit(vector<int>& prices) {
		int n = prices.size();
		vector<vector<int>> dp(n, vector<int>(2, -1));
		
		return solve(prices, n, 0, true, dp);
	}
};
```

---
### Tabulation

**Time Complexity**: O(n)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
	int maxProfit(vector<int>& prices) {
		int n = prices.size();
		vector<vector<int>> dp(n + 2, vector<int>(2, 0));
		
		for (int index = n - 1; index >= 0; index--) {
			for (int canBuy = 0; canBuy < 2; canBuy++) {
				int profit = 0;
			
				if (canBuy) {
					profit = max(-prices[index] + dp[index + 1][0], dp[index + 1][1]);
				} else {
					profit = max(prices[index] + dp[index + 2][1], dp[index + 1][0]);
				}
				
				dp[index][canBuy] = profit;
			}
		}
		
		return dp[0][1];
	}
};
```

---
### Space Optimization

**Time Complexity**: O(n)
**Space Complexity**: O(1)

```cpp
class Solution { 
  public:
	int maxProfit(vector<int>& prices) {
		int n = prices.size();
		vector<int> current(2, 0), next(2, 0), ahead(2, 0);
		
		for (int index = n - 1; index >= 0; index--) {
			for (int canBuy = 0; canBuy < 2; canBuy++) {
				int profit = 0;
				
				if (canBuy) {
					profit = max(-prices[index] + next[0], next[1]);
				} else {
					profit = max(prices[index] + ahead[1], next[0]);
				}
				
				current[canBuy] = profit;
			}
			
			ahead = next;
			next = current;	
		}
		
		return current[1];
	}
};
```

---
