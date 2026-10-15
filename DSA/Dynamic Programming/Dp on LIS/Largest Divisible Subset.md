**Problem**: Given a set of **distinct** positive integers `nums`, return the largest subset `answer` such that every pair `(answer[i], answer[j])` of elements in this subset satisfies:

- `answer[i] % answer[j] == 0`, or
- `answer[j] % answer[i] == 0`

If there are multiple solutions, return any of them.

**Example**:
**Input:** `nums = [1,2,3]`
**Output**: `[1,2]`
**Explanation**: `[1,3]` is also accepted.

[Visit Leetcode](https://leetcode.com/problems/largest-divisible-subset/)
[Visit GFG](https://www.geeksforgeeks.org/problems/largest-divisible-subset--170643/1)

---
### Tabulation + Greedy Backtracking

**Time Complexity**: O(n logn) + O(n<sup>2</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
	vector<int> largestDivisibleSubset(vector<int>& nums) {
		sort(nums.begin(), nums.end());
		int n = nums.size();
		
		vector<int> dp(n , 1), hash(n);
		int maxi = 1, lastIndex = 0;
		
		for (int index = 0; index < n; index++) {
			hash[index] = index;
			
			for (int prev = 0; prev < index; prev++) {
				if (nums[index] % nums[prev] == 0 && 1 + dp[prev] > dp[index]) {
					dp[index] = 1 + dp[prev];
					hash[index] = prev;
				}
			}
			
			if (dp[index] > maxi) {
				maxi = dp[index];
				lastIndex = index;
			}
		}
		
		vector<int> LDS;
		LDS.push_back(nums[lastIndex]);
		
		while (hash[lastIndex] != lastIndex) {
			lastIndex = hash[lastIndex];
			LDS.push_back(nums[lastIndex]);
		}
		
		return LDS;
	}
};
```