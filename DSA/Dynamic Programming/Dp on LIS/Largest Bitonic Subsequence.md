**Problem**: Given an array of positive integers. Find the maximum length of Bitonic subsequence.  A subsequence of array is called Bitonic if it is first strictly increasing, then strictly decreasing. Return the maximum length of bitonic subsequence.  
   
**Note** : A strictly increasing or a **strictly** decreasing sequence should not be considered as a bitonic sequence

**Examples :**

**Input**: `n = 5, nums[] = [1, 2, 5, 3, 2]`
**Output**: 5
**Explanation**: `The sequence [1, 2, 5] is increasing and the sequence [3, 2] is decreasing so merging both we will get length 5.`

[Visit GFG](https://www.geeksforgeeks.org/problems/longest-bitonic-subsequence0824/1)

---
### Tabulation

**Time Complexity**: O(n<sup>2</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
    int longestBitonicSequence(int n, vector<int> &nums) {
	    vector<int> dp1(n, 1), dp2(n, 1);
	    
	    for (int index = 0; index < n; index++) {
		    for (int prevIndex = 0; prevIndex < index; prevIndex++) {
			    if (nums[prevIndex] < nums[index] && dp1[index] < 1 + dp1[prevIndex]) {
				    dp1[index] = 1 + dp1[prevIndex];
			    }
		    }
		}
		
		int maxLength = 0;
		
		for (int index = n - 1; index >= 0; index--) {
		    for (int prevIndex = n - 1; prevIndex > index; prevIndex--) {
			    if (nums[prevIndex] < nums[index] && dp2[index] < 1 + dp2[prevIndex]) {
				    dp2[index] = 1 + dp2[prevIndex];
			    }
		    }
		    
			maxLength = max(maxLength, dp1[index] + dp2[index] - 1);
		}
		
		return maxLength;
    }
};
```