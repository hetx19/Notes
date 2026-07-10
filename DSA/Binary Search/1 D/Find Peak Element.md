**Problem**: A peak element is an element that is strictly greater than its neighbors.

Given a **0-indexed** integer array `nums`, find a peak element, and return its index. If the array contains multiple peaks, return the index to **any of the peaks**.

You may imagine that `nums[-1] = nums[n] = -∞`. In other words, an element is always considered to be strictly greater than a neighbor that is outside the array.

You must write an algorithm that runs in `O(log n)` time.

**Example**:
**Input**: `nums = [1,2,3,1]`
**Output**: 2
**Explanation**: 3 is a peak element and your function should return the index number 2.

[Visit Leetcode](https://leetcode.com/problems/find-peak-element/)
[Visit GFG](https://www.geeksforgeeks.org/problems/peak-element/1)

---

### Brute Force
By using Linear Search - [[Linear Search]]

**Time Complexity**: O(n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int findPeakElement(vector<int>& nums) {
		int n = nums.size();
		
		for (int i = 0; i < n; i++) {
			bool left = (i == 0) || (nums[i] > nums[i - 1]);
			bool right = (i == n - 1) || (nums[i] > nums[i + 1]);
			
			if (left && right) {
				return i;
			}
		}
		
		return -1;
	}
};
```

---

### Optimal Solution
By using Binary Search Function - [[Binary Search]]

```cpp
class Solution {
  public:
	int findPeakElement(vector<int>& nums) {
		int n = nums.size();
		int low = 1, high = n - 2;
		
		// Single element Array
		if (n == 1) {
			return 0;
		}
		
		// Trimming down search space
		if (nums[0] > nums[1]) {
			return 0;
		}
		
		if (nums[high + 1] > nums[high]) {
			return high + 1;
		}
		
		while(low <= high) {
			int mid = low + ((high - low) / 2);
			
			if (nums[mid] > nums[mid - 1] && nums[mid] > nums[mid + 1]) {
				return mid;
			} else if (nums[mid] > nums[mid - 1]) {
				low = mid + 1;
			} else {
				high = mid - 1;
			}
		}
		
		return -1;
	}
};
```

---



