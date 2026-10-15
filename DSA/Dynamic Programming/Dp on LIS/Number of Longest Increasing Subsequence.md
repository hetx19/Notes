**Problem**: Given an integer array `nums`, return _the number of longest increasing subsequences._
**Notice** that the sequence has to be **strictly** increasing.

**Example**:
**Input**: `nums = [1,3,5,4,7]`
**Output**: 2
**Explanation**: `The two longest increasing subsequences are [1, 3, 4, 7] and [1, 3, 5, 7].`

[Visit Leetcode]()
[Visit GFG](https://www.geeksforgeeks.org/problems/number-of-longest-increasing-subsequence/1)

---
### Optimal Solution

**Time Complexity**: O(n<sup>2</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
	int findNumberOfLIS(vector<int>& nums) {
		int n = nums.size();
		vector<int> dp(n, 1), count(n, 1);
		int maxLength = 1;

		for (int index = 0; index < n; index++) {
			for (int prev = 0; prev < index; prev++) {
				if (nums[index] > nums[prev] && dp[index] < 1 + dp[prev]) {
					dp[index] = 1 + dp[prev];
					count[index] = count[prev];
				} else if (nums[index] > nums[prev] && dp[index] == 1 + dp[prev]) {
					count[index] += count[prev];
				}
			}
			
			maxLength = max(maxLength, dp[index]);
		}
		
		int cnt = 0;
		for (int index = 0; index < n; index++) {
			if (dp[index] == maxLength) {
				cnt += count[index];
			}
		}

		return cnt;
	}
};
```