**Problem**: You are given an integer array `prices` where `prices[i]` is the price of a given stock on the i<sup>th</sup> day.

On each day, you may decide to buy and/or sell the stock. You can only hold **at most one** share of the stock at any time. However, you can sell and buy the stock multiple times on the **same day**, ensuring you never hold more than one share of the stock.

Find and return _the **maximum** profit you can achieve_.

**Example**:
**Input:** `prices = [7,1,5,3,6,4]`
**Output:** 7
**Explanation:** Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
Total profit is 4 + 3 = 7.

[Visit Leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

---
### Brute Force Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(2<sup>n</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	int solve(vector<int>& prices, int index, int n, bool canBuy) {
		if (index == n) {
			return 0;
		}
		
		int profit = 0;
		
		if (canBuy) {
			profit = max(-prices[index] + solve(prices, index + 1, n, false), solve(prices, index + 1, n, true));
		} else {
			profit = max(prices[index] + solve(prices, index + 1, n, true), solve(prices, index + 1, n, false));
		}
		
		return profit;
	}
	 
  public:
	int maxProfit(vector<int>& prices) {
		int n = prices.size();
		
		return solve(prices, 0, n, true);	
	}
};
```

---
### Memoization

**Time Complexity**: O(2n)
**Space Complexity**: O(2n) + O(n)

```cpp
class Solution {
  private:
	int solve(vector<int>& prices, int index, int n, bool canBuy, vector<vector<int>>& dp) {
		if (index == n) {
			return 0;
		}
		
		if (dp[index][canBuy] != -1) {
			return dp[index][canBuy];
		}
		
		int profit = 0;
		
		if (canBuy) {
			profit = max(-prices[index] + solve(prices, index + 1, n, false, dp), solve(prices, index + 1, n, true, dp));
		} else {
			profit = max(prices[index] + solve(prices, index + 1, n, true, dp), solve(prices, index + 1, n, false, dp));
		}
		
		return dp[index][canBuy] = profit;
	}
	 
  public:
	int maxProfit(vector<int>& prices) {
		int n = prices.size();
		vector<vector<int>> dp(n, vector<int>(2, -1));
		
		return solve(prices, 0, n, true, dp);
	}
};
```

---
### Tabulation

**Time Complexity**: O(2n)
**Space Complexity**: O(2n)

```cpp
class Solution {
  public:
	int maxProfit(vector<int>& prices) {
		int n = prices.size();
		vector<vector<int>> dp(n + 1, vector<int>(2, 0));
		
		for (int index = n - 1; index >= 0; index--) {
			for (int canBuy = 0; canBuy < 2; canBuy++) {
				int profit = 0;
		
				if (canBuy) {
					profit = max(-prices[index] + dp[index + 1][0], dp[index + 1][1]);
				} else {
					profit = max(prices[index] + dp[index + 1][1], dp[index + 1][0]);
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

**Time Complexity**: O(2n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int maxProfit(vector<int>& prices) {
		int n = prices.size();
		vector<int> next(2, 0), current(2, 0);
		
		for (int index = n - 1; index >= 0; index--) {
			for (int canBuy = 0; canBuy < 2; canBuy++) {
				int profit = 0;
		
				if (canBuy) {
					profit = max(-prices[index] + next[0], next[1]);
				} else {
					profit = max(prices[index] + next[1], next[0]);
				}
				
				current[canBuy] = profit;
			}
			next = current;
		}
		
		return current[1];
	}
};
```

---