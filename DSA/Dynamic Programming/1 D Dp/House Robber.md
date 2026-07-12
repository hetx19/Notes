**Problem**: You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return _the maximum amount of money you can rob tonight **without alerting the police**_.

**Example**:
**Input**: `nums = [1,2,3,1]`
**Output**: 4
**Explanation**: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.

[Visit Leetcode](https://leetcode.com/problems/house-robber/)
[Visit GFG](https://www.geeksforgeeks.org/problems/stickler-theif-1587115621/1)

---
### Brute Force Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(2<sup>n</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	int solve(vector<int>& nums, int index) {
		if (index < 0) {
			return 0;
		}
		
		if (index == 0) {
			return nums[0];
		}
		
		int pick = nums[index] + solve(nums, index - 2);
		int notPick = solve(nums, index - 1);
		
		return max(pick, notPick);
	}
	
  public:
	int rob(vector<int>& nums) {
		int n = nums.size();
		
		return solve(nums, n - 1);
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
	int solve(vector<int>& nums, int index, vector<int>& dp) {
		if (index < 0) {
			return 0;
		}
		
		if (index == 0) {
			return nums[0];
		}
		
		if (dp[index] != -1) {
			return dp[index];
		}
		
		int pick = nums[index] + solve(nums, index - 2, dp);
		int notPick = solve(nums, index - 1, dp);
		
		return dp[index] = max(pick, notPick);
	}
	
  public:
	int rob(vector<int>& nums) {
		int n = nums.size();
		vector<int> dp(n, -1);
		return solve(nums, n - 1, dp);
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
	int rob(vector<int>& nums) {
		int n = nums.size();
		vector<int> dp(n, 0);
		dp[0] = nums[0];
		
		
		for (int i = 1; i < n; i++) {
			int pick = nums[i] + ((i > 1) ? dp[i - 2] : 0);
			int notPick = dp[i - 1];
			
			dp[i] = max(pick, notPick);
		}
		
		return dp[n - 1];
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
	int rob(vector<int>& nums) {
		int n = nums.size();
		int prev1 = nums[0], prev2 = 0;
		
		for (int i = 1; i < n; i++) {
			int pick = nums[i] + ((i > 1) ? prev2 : 0);
			int notPick = prev1;
			
			int current = max(pick, notPick);
			prev2 = prev1;
			prev1 = current;
		}
		
		return prev1;
	}
};
```
---