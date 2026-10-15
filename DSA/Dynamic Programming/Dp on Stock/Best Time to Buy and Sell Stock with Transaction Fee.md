**Problem**: You are given an array `prices` where `prices[i]` is the price of a given stock on the i<sup>th</sup> day, and an integer `fee` representing a transaction fee.

Find the maximum profit you can achieve. You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction.

**Note**:
- You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).
- The transaction fee is only charged once for each stock purchase and sale.

**Example**:
**Input:** `prices = [1,3,2,8,4,9], fee = 2`
**Output**: 8
**Explanation**: The maximum profit can be achieved by:
- Buying at `prices[0] = 1`
- Selling at `prices[3] = 8`
- Buying at `prices[4] = 4`
- Selling at `prices[5] = 9`
The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.

[Visit Leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)

---
### Brute Force Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(2<sup>n</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	int solve(vector<int>& prices, int n, int fee, int index, bool canBuy) {
		if (index == n) {
			return 0;
		}
		
		int profit = 0;
		
		if (canBuy) {
			profit = max(-prices[index] + solve(prices, n, fee, index + 1, false), solve(prices, n, fee, index + 1, true));
		} else {
			profit = max(prices[index] - fee + solve(prices, n, fee, index + 1, true), solve(prices, n, fee, index + 1, false));
		}
		
		return profit;
	}
	
  public:
	int maxProfit(vector<int>& prices, int fee) {
		int n = prices.size();
		
		return solve(prices, n, fee, 0, true);
	}
};
```

---
### Memoization

**Time Complexity**: O(n)
**Space Complexity**: O(n) + O(n)

```cpp
class Solution {
  private:
	int solve(vector<int>& prices, int n, int fee, int index, bool canBuy, vector<vector<int>>& dp) {
		if (index == n) {
			return 0;
		}
		
		if (dp[index][canBuy] != -1) {
			return dp[index][canBuy];
		}
		
		int profit = 0;
		
		if (canBuy) {
			profit = max(-prices[index] + solve(prices, n, fee, index + 1, false, dp), solve(prices, n, fee, index + 1, true, dp));
		} else {
			profit = max(prices[index] - fee + solve(prices, n, fee, index + 1, true, dp), solve(prices, n, fee, index + 1, false, dp));
		}
		
		return dp[index][canBuy] = profit;
	}
	
  public:
	int maxProfit(vector<int>& prices, int fee) {
		int n = prices.size();
		vector<vector<int>> dp(n, vector<int>(2, -1));
		
		return solve(prices, n, fee, 0, true, dp);
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
	int maxProfit(vector<int>& prices, int fee) {
		int n = prices.size();
		vector<vector<int>> dp(n + 1, vector<int>(2, 0));
		
		for (int index = n - 1; index >= 0; index--) {
			for (int canBuy = 0; canBuy < 2; canBuy++) {
				int profit = 0;
				
				if (canBuy) {
					profit = max(-prices[index] + dp[index + 1][0], dp[index + 1][1]);
				} else {
					profit = max(prices[index] - fee + dp[index + 1][1], dp[index + 1][0]);
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
	int maxProfit(vector<int>& prices, int fee) {
		int n = prices.size();
		vector<int> current(2, 0), next(2, 0);
		
		for (int index = n - 1; index >= 0; index--) {
			for (int canBuy = 0; canBuy < 2; canBuy++) {
				int profit = 0;
				
				if (canBuy) {
					profit = max(-prices[index] + next[0], next[1]);
				} else {
					profit = max(prices[index] - fee + next[1], next[0]);
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