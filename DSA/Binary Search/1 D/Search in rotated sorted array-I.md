**Problem**: There is an integer array `nums` sorted in ascending order (with **distinct** values).

Prior to being passed to your function, `nums` is **possibly left rotated** at an unknown index `k` (`1 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,5,6,7]` might be left rotated by `3` indices and become `[4,5,6,7,0,1,2]`.

Given the array `nums` **after** the possible rotation and an integer `target`, return _the index of_ `target` _if it is in_ `nums`_, or_ `-1` _if it is not in_ `nums`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example**:

**Input**: `nums = [4,5,6,7,0,1,2], target = 0`
**Output**: 4

[Visit Leetcode](https://leetcode.com/problems/search-in-rotated-sorted-array/)
[Visit GFG](https://www.geeksforgeeks.org/problems/search-in-a-rotated-array4618/1)

---

### Brute Force
By doing Linear Search - [[Linear Search]]

**Time Complexity**: O(n) 
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int search(vector<int>& nums, int target) {
		int n = nums.size();
		
		for (int i = 0; i < n; i++) {
			if (nums[i] == target) {
				return i;
			}
		}
		
		return -1;
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
	int search(vector<int>& nums, int target) {
		int n = nums.size();
		int low = 0, high = n - 1;
		
		while (low <= high) {
			int mid = low + ((high - low) / 2);
			
			if (nums[mid] == target) {
				return mid;
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
		
		return -1;
	}
};
```