**Problem**: You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return _the number of combinations that make up that amount_. If that amount of money cannot be made up by any combination of the coins, return `0`.

You may assume that you have an infinite number of each kind of coin.

The **final** answer is **guaranteed** to fit into a signed **32-bit** integer.

**Example**:
**Input**: `amount = 5, coins = [1,2,5]`
**Output**: 4
**Explanation**: there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1

[Visit Leetcode](https://leetcode.com/problems/coin-change-ii/)
[Visit GFG](https://www.geeksforgeeks.org/problems/coin-change2448/1)

---
### Brute Force Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(2<sup>n</sup>) → Exponential  
**Space Complexity**: O(n) → Linear

```cpp
class Solution {
  private:
	int solve(vector<int>& coins, int index, int amount) {
		if (index == 0) {
			return ((amount % coins[index] == 0) ? 1 : 0);
		}
		
		int notTake = solve(coins, index - 1, amount);
		int take = (coins[index] <= amount) ? solve(coins, index, amount - coins[index]) : 0;
		
		return notTake + take;
	}
	
  public:
	int change(int amount, vector<int>& coins) {
		int n = coins.size();
		return solve(coins, n - 1, amount);
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
	int solve(vector<int>& coins, int index, int amount, vector<vector<int>>& dp) {
		if (index == 0) {
			return dp[index][amount] = ((amount % coins[index] == 0) ? 1 : 0);
		}
		
		if (dp[index][amount] != -1) {
			return dp[index][amount];
		}
		
		int notTake = solve(coins, index - 1, amount, dp);
		int take = (coins[index] <= amount) ? solve(coins, index, amount - coins[index], dp) : 0;
		
		return dp[index][amount] = notTake + take;
	}
	
  public:
	int change(int amount, vector<int>& coins) {
		int n = coins.size();
		vector<vector<int>> dp(n, vector<int>(amount + 1, -1));
		
		return solve(coins, n - 1, amount, dp);
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
	int change(int amount, vector<int>& coins) {
		int n = coins.size();
		vector<vector<int>> dp(n, vector<int>(amount + 1, 0));
		
		for (int target = 0; target <= amount; target++) {
			dp[0][target] = (target % coins[0] == 0);
		}
		
		for (int index = 1; index < n; index++) {
			for (int target = 0; target <= amount; target++) {
				int notTake = dp[index - 1][target];
				int take = (coins[index] <= target) ? dp[index][target - coins[index]] : 0;
				
				dp[index][target] = take + notTake;
			}
		}
		
		return dp[n - 1][amount];
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
	int change(int amount, vector<int>& coins) {
		int n = coins.size();
		vector<int> prev(amount + 1, 0), current(amount + 1, 0);
		
		for (int target = 0; target <= amount; target++) {
			prev[target] = (target % coins[0] == 0);
		}
		
		for (int index = 1; index < n; index++) {
			for (int target = 0; target <= amount; target++) {
				int notTake = prev[target];
				int take = (coins[index] <= target) ? current[target - coins[index]] : 0;
				
				current[target] = take + notTake;
			}
			prev = current;
		}
		
		return prev[amount];
	}
};
```

---
>[!tip]
>Big-O notation gives only the asymptotic growth. Actual runtime and memory usage depend on the recursion tree, constant factors, compiler optimizations, and input values. In interviews, it's usually sufficient to state the nature of the complexity (e.g., exponential, quadratic, linear) and justify it briefly.