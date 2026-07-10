**Problem**: There is an integer array `nums` sorted in non-decreasing order (not necessarily with **distinct** values).

Before being passed to your function, `nums` is **rotated** at an unknown pivot index `k` (`0 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,4,4,5,6,6,7]` might be rotated at pivot index `5` and become `[4,5,6,6,7,0,1,2,4,4]`.

Given the array `nums` **after** the rotation and an integer `target`, return `true` _if_ `target` _is in_ `nums`_, or_ `false` _if it is not in_ `nums`_._

You must decrease the overall operation steps as much as possible.

**Example**:

**Input**: `nums = [2,5,6,0,0,1,2], target = 0`
**Output**: true

[Visit Leetcode](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)
[Visit GFG](https://www.geeksforgeeks.org/problems/search-in-rotated-array-2/1)

---

### Brute Force
By doing Linear Search - [[Linear Search]]

**Time Complexity**: O(n) 
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	bool search(vector<int>& nums, int target) {
		int n = nums.size();
		
		for (int i = 0; i < n; i++) {
			if (nums[i] == target) {
				return true;
			}
		}
		
		return false;
	}
};
```

---

### Optimal Solution
By using binary search function - [[Binary Search]]

**Time Complexity**: O(log n)  
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	bool search(vector<int>& nums, int target) {
		int n = nums.size();
		int low = 0, high = n - 1;
		
		while (low <= high) {
			int mid = low + ((high - low) / 2);
			
			if (nums[mid] == target) {
				return true;
			}
			
			if (nums[low] == nums[mid] && nums[mid] == nums[high]) {
				low++;
				high--;
				continue;
			}
			
			if (nums[low] <= nums[mid]) {
				// Left Half is Sorted
				if (nums[low] <= target && nums[mid] >= target) {
					high = mid - 1;
				} else {
					low = mid + 1;
				}
			} else {
				// Right Half is Sorted
				if (nums[mid] <= target && nums[high] >= target) {
					low = mid + 1;
				} else {
					high = mid - 1;
				}
			}
		}
		
		return false;
	}
};
```