**Problem**: You are given an integer array `prices` where `prices[i]` is the price of a given stock on the i<sup>th</sup> day, and an integer `k`.

Find the maximum profit you can achieve. You may complete at most `k` transactions: i.e. you may buy at most `k` times and sell at most `k` times.

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

**Example**:
**Input**: `k = 2, prices = [2,4,1]`
**Output**: 2
**Explanation**: Buy on day 1 (price = 2) and sell on day 2 (price = 4), profit = 4-2 = 2.

[Visit Leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/)

---
### Brute Force Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(2<sup>n</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	int solve(vector<int>& prices, int n, int index, int transactionNo, int maxTransaction) {
		if (index == n || transactionNo == maxTransaction) {
			return 0;
		}
		
		int profit = 0;
		
		if (transactionNo & 1) {
			profit = max(prices[index] + solve(prices, n, index + 1, transactionNo + 1, maxTransaction), solve(prices, n, index + 1, transactionNo, maxTransaction));
		} else {
			profit = max(-prices[index] + solve(prices, n, index + 1, transactionNo + 1, maxTransaction), solve(prices, n, index + 1, transactionNo, maxTransaction));
		}
		
		return profit;
	}
	
  public:
	int maxProfit(int k, vector<int>& prices) {
		int n = prices.size(); int maxTransaction = 2 * k;
		
		return solve(prices, n, 0, 0, maxTransaction);
	}
};
```

---
### Memoization

**Time Complexity**: O(nk)
**Space Complexity**: O(nk) + O(n)

```cpp
class Solution {
  private:
	int solve(vector<int>& prices, int n, int index, int transactionNo, int maxTransaction, vector<vector<int>>& dp) {
		if (index == n || transactionNo == maxTransaction) {
			return 0;
		}
		
		if (dp[index][transactionNo] != -1) {
			return dp[index][transactionNo];
		}
		
		int profit = 0;
		
		if (transactionNo & 1) {
			profit = max(prices[index] + solve(prices, n, index + 1, transactionNo + 1, maxTransaction, dp), solve(prices, n, index + 1, transactionNo, maxTransaction, dp));
		} else {
			profit = max(-prices[index] + solve(prices, n, index + 1, transactionNo + 1, maxTransaction, dp), solve(prices, n, index + 1, transactionNo, maxTransaction, dp));
		}
		
		return dp[index][transactionNo] = profit;
	}
	
  public:
	int maxProfit(int k, vector<int>& prices) {
		int n = prices.size(); int maxTransaction = 2 * k;
		vector<vector<int>> dp(n, vector<int>(maxTransaction, -1));
		
		return solve(prices, n, 0, 0, maxTransaction, dp);
	}
};
```

---
### Tabulation

**Time Complexity**: O(nk)
**Space Complexity**: O(nk)

```cpp
class Solution {
  public:
	int maxProfit(int k, vector<int>& prices) {
		int n = prices.size(); int maxTransaction = 2 * k;
		vector<vector<int>> dp(n + 1, vector<int>(maxTransaction + 1, 0));
		
		for (int index = n - 1; index >= 0; index--) {
			for (int transactionNo = 0; transactionNo < maxTransaction; transactionNo++) {
				int profit = 0;
				
				if (transactionNo & 1) {
					profit = max(prices[index] + dp[index + 1][transactionNo + 1], dp[index + 1][transactionNo]);
				} else {
					profit = max(-prices[index] + dp[index + 1][transactionNo + 1], dp[index + 1][transactionNo]);
				}
				
				dp[index][transactionNo] = profit;
			}
		}
		
		return dp[0][0];
	}
};
```

---
### Space Optimization

**Time Complexity**: O(nk)
**Space Complexity**: O(k)

```cpp
class Solution {
  public:
	int maxProfit(int k, vector<int>& prices) {
		int n = prices.size(); int maxTransaction = 2 * k;
		vector<int> current(maxTransaction + 1, 0), next(maxTransaction + 1, 0);
		
		for (int index = n - 1; index >= 0; index--) {
			for (int transactionNo = 0; transactionNo < maxTransaction; transactionNo++) {
				int profit = 0;
				
				if (transactionNo & 1) {
					profit = max(prices[index] + next[transactionNo + 1], next[transactionNo]);
				} else {
					profit = max(-prices[index] + next[transactionNo + 1], next[transactionNo]);
				}
				
				current[transactionNo] = profit;
			}
			
			next = current;
		}
		
		return current[0];
	}
};
```

---
