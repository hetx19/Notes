**Problem**: You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return _the fewest number of coins that you need to make up that amount_. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

**Example**:
**Input**: `coins = [1,2,5], amount = 11`
**Output**: 3
**Explanation**: 11 = 5 + 5 + 1

[Visit Leetcode](https://leetcode.com/problems/coin-change/)
[Visit GFG](https://www.geeksforgeeks.org/problems/number-of-coins1824/1)

---
### Brute Force Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(2<sup>n</sup>) → Exponential
**Space Complexity**: O(n) → Linear

```cpp
class Solution {
  private:
	int solve(vector<int> &coins, int amount, int index) {
		if (index == 0) {
			if (amount % coins[index] == 0) {
				return amount / coins[index];
			}
			return INT_MAX;
		}
		
		int notTake = solve(coins, amount, index - 1);
		int take = INT_MAX;
		
		if (coins[index] <= amount) {
			take = solve(coins, amount - coins[index], index);
			if (take != INT_MAX) {
				take++;
			}
		}
		
		return min(take, notTake);
	}
	
  public:
	int coinChange(vector<int>& coins, int amount) {
		int n = coins.size();
		int ans = solve(coins, amount, n - 1);
		
		return (ans == INT_MAX) ? -1 : ans;
	}
};
```

---
### Memoization

**Time Complexity**: O(n x amount)
**Space Complexity**: O(n x amount) + O(n)

```cpp
class Solution {
  private:
	int solve(vector<int> &coins, int amount, int index, vector<vector<int>>& dp) {
		if (index == 0) {
			if (amount % coins[index] == 0) {
				return dp[index][amount] = amount / coins[index];
			}
			return dp[index][amount] = INT_MAX;
		}
		
		if (dp[index][amount] != -1) {
			return dp[index][amount];
		}
		
		int notTake = solve(coins, amount, index - 1, dp);
		int take = INT_MAX;
		
		if (coins[index] <= amount) {
			take = solve(coins, amount - coins[index], index, dp);
			if (take != INT_MAX) {
				take++;
			}
		}
		
		return dp[index][amount] = min(take, notTake);
	}
	
  public:
	int coinChange(vector<int>& coins, int amount) {
		int n = coins.size();
		vector<vector<int>> dp(n, vector<int> (amount + 1, -1));
		int ans = solve(coins, amount, n - 1, dp);
		
		return (ans == INT_MAX) ? -1 : ans;
	}
};
```

---
### Tabulation

**Time Complexity**: O(n x amount)
**Space Complexity**: O(n x amount)

```cpp
class Solution {
  public:
	int coinChange(vector<int>& coins, int amount) {
		int n = coins.size();
		vector<vector<int>> dp(n, vector<int> (amount + 1, 0));
		
		for (int t = 0; t <= amount; t++) {
			if (t % coins[0] == 0) {
				dp[0][t] = t / coins[0];
			} else {
				dp[0][t] = INT_MAX;
			}
		}
		
		for (int index = 1; index < n; index++) {
			for (int target = 0; target <= amount; target++) {
				int notTake = dp[index - 1][target];
				
				int take = INT_MAX;
				if (coins[index] <= target) {
					take = dp[index][target - coins[index]];
					if (take != INT_MAX) {
						take++;
					}
				}
				
				dp[index][target] = min(take, notTake);
			}
		}
		
		return (dp[n - 1][amount] == INT_MAX) ? -1 : dp[n - 1][amount];
	}
};
```

---
### Space Optimization

**Time Complexity**: O(n x amount)  
**Space Complexity**: O(amount)

```cpp
class Solution {
  public:
	int coinChange(vector<int>& coins, int amount) {
		int n = coins.size();
		vector<int> prev(amount + 1, 0), current(amount + 1, 0);
		
		for (int t = 0; t <= amount; t++) {
			if (t % coins[0] == 0) {
				prev[t] = t / coins[0];
			} else {
				prev[t] = INT_MAX;
			}
		}
		
		for (int index = 1; index < n; index++) {
			for (int target = 0; target <= amount; target++) {
				int notTake = prev[target];
				
				int take = INT_MAX;
				if (coins[index] <= target) {
					take = current[target - coins[index]];
					if (take != INT_MAX) {
						take++;
					}
				}
				
				current[target] = min(take, notTake);
			}
			prev = current;
		}
		
		return (prev[amount] == INT_MAX) ? -1 : prev[amount];
	}
};
```

---
>[!tip]
>Big-O notation gives only the asymptotic growth. Actual runtime and memory usage depend on the recursion tree, constant factors, compiler optimizations, and input values. In interviews, it's usually sufficient to state the nature of the complexity (e.g., exponential, quadratic, linear) and justify it briefly.
