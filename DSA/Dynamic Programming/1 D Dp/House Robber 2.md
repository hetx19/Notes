**Problem**: You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle.** That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return _the maximum amount of money you can rob tonight **without alerting the police**_.

**Example**:
**Input**: `nums = [2,3,2]`
**Output**: 3
**Explanation**: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.

[Visit Leetcode](https://leetcode.com/problems/house-robber-ii/)
[Visit GFG](https://www.geeksforgeeks.org/problems/house-robber-ii/1)

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
		
		if (n == 1) {
			return nums[0];
		}
		
		vector<int> temp1, temp2;
		
		for (int i = 0; i < n; i++) {
			if (i > 0) {
				temp1.push_back(nums[i]);
			}
			
			if (i < n - 1) {
				temp2.push_back(nums[i]);
			}
		}
		
		return max(solve(temp1, n - 2), solve(temp2, n - 2));
	}
};
```

---
### Memoization

**Time Complexity**: O(n)
**Space Complexity**: O(2n) + O(2n)

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
		
		if (n == 1) {
			return nums[0];
		}
		
		vector<int> dp1(n - 1, -1), dp2(n - 1, -1);
		
		vector<int> temp1, temp2;
		
		for (int i = 0; i < n; i++) {
			if (i > 0) {
				temp1.push_back(nums[i]);
			}
			
			if (i < n - 1) {
				temp2.push_back(nums[i]);
			}
		}
		
		return max(solve(temp1, n - 2, dp1), solve(temp2, n - 2, dp2));
	}
};
```

---
### Tabulation

**Time Complexity**: O(n)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	int solve(vector<int>& nums) {
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
	
  public:
	int rob(vector<int>& nums) {
		int n = nums.size();
		
		if (n == 1) {
			return nums[0];
		}
		
		vector<int> temp1, temp2;
		
		for (int i = 0; i < n; i++) {
			if (i > 0) {
				temp1.push_back(nums[i]);
			}
			
			if (i < n - 1) {
				temp2.push_back(nums[i]);
			}
		}
		
		return max(solve(temp1), solve(temp2));
	}
};
```

---
### Space Optimization

**Time Complexity**: O(n)
**Space Complexity**: O(1)

```cpp
class Solution {
  private:
	int solve(vector<int>& nums) {
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
	
  public:
	int rob(vector<int>& nums) {
		int n = nums.size();
		
		if (n == 1) {
			return nums[0];
		}
		
		vector<int> temp1, temp2;
		
		for (int i = 0; i < n; i++) {
			if (i > 0) {
				temp1.push_back(nums[i]);
			}
			
			if (i < n - 1) {
				temp2.push_back(nums[i]);
			}
		}
		
		return max(solve(temp1), solve(temp2));
	}
};
```
---