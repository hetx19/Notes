**Problem**: You are given an array of integers **nums[]**, return the **Longest Increasing Subsequence (LIS)** of the given array.  
**LIS** is the longest subsequence where each element is **strictly** greater than the previous one.  
**Note:** If multiple **LIS** of the same maximum length exist, return the one that appears **first** based on the **lexicographical order** of their **indices** (i.e., the earliest combination of positions from the original sequence).

**Examples:**

**Input**: `nums[] = [10, 20, 3, 40]`
**Output**: `[10, 20, 40]`
**Explanation**: `[10, 20, 40]` is the longest subsequence where each number is greater than the previous one, maintaining the original order.

[Visit GFG](https://www.geeksforgeeks.org/problems/printing-longest-increasing-subsequence/1)

---
### Tabulation + Greedy BackTracking

**Time Complexity**: O(n<sup>2</sup>) + O(n)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
    vector<int> getLIS(vector<int>& nums) {
	    int n = nums.size();
	    
        if (n == 0) {
			return {};
		}

	    vector<int> dp(n, 1), hash(n);
	    int maxi = 1, lastIndex = 0;

	    for (int index = 0; index < n; index++) {
			hash[index] = index;
	        for (int prevIndex = 0; prevIndex < index; prevIndex++) {
	            if (nums[prevIndex] < nums[index] && 1 + dp[prevIndex] > dp[index]) {
					hash[index] = prevIndex;
	                dp[index] = 1 + dp[prevIndex];
	            }
	        }

	        if (dp[index] > maxi) {
				maxi = dp[index];
				lastIndex = index;
			}
	    }

		vector<int> LIS(maxi);
		LIS[0] = nums[lastIndex];
		int index = 1;
		while (hash[lastIndex] != lastIndex) {
			lastIndex = hash[lastIndex];
			LIS[index++] = nums[lastIndex]; 
		}

		reverse(LIS.begin(), LIS.end());

	    return LIS;
    }
};
```