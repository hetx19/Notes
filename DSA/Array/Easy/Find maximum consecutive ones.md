**Problem**: Given a binary array `nums`, return _the maximum number of consecutive_ `1`_'s in the array_.

**Example**:
**Input:** `nums = [1,1,0,1,1,1]`
**Output:** 3
**Explanation:** The first two digits or the last three digits are consecutive 1s. The maximum number of consecutive 1s is 3.

[Visit Leetcode](https://leetcode.com/problems/max-consecutive-ones/)
[Visit GFG](https://www.geeksforgeeks.org/problems/max-consecutive-one/1)

---

### Optimal Approach

**Time Complexity**: O(n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int findMaxConsecutiveOnes(vector<int> &nums) {
		int count = 0;
		int maxCount = 0;
		
		for (int num : nums) {
			if (num == 1) {
				count++;
				maxCount = max(maxCount, count);
			} else {
				count = 0;
			}
		}
		
		return maxCount;
	}
};
```