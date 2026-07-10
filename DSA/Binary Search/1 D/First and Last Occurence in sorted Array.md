**Problem**: Given an array of integers `nums` sorted in non-decreasing order, find the starting and ending position of a given `target` value.

If `target` is not found in the array, return `[-1, -1]`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example**:

**Input**: `nums = [5,7,7,8,8,10], target = 8`
**Output**: `[3,4]`

[Visit Leetcode](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)
[Visit GFG](https://www.geeksforgeeks.org/problems/first-and-last-occurrences-of-x3116/1)

---

### Brute Force
By doing Linear Search - [[Linear Search]]

**Time Complexity**: O(2n)
**Space Complexity**: O(1)

```cpp
class Solution {
  private:
	int firstOccurence(vector<int>& nums, int target) {
		int n = nums.size();
		
		for (int i = 0; i < n; i++) {
			if (nums[i] == target) {
				return i;
			}
		}
		
		return -1;
	}
	
	int lastOccurence(vector<int>& nums, int target) {
		int n = nums.size();
		
		for (int i = n - 1; i >= 0; i--) {
			if (nums[i] == target) {
				return i;
			}
		}
		
		return -1;
	}
	
  public:
	vector<int> searchRange(vector<int>& nums, int target) {
		vector<int> range(2, -1);
		range[0] = firstOccurence(nums, target);
		range[1] = lastOccurence(nums, target);
		
		return range;
	}
};
```

---

### Optimal Solution
By using lower bound function - [[Lower Bound]]

**Time Complexity**: O(2 log n)
**Space Complexity**: O(1)

```cpp
class Solution {
  private:
	int firstOccurence(vector<int>& nums, int target) {
		int n = nums.size();
		int low = 0, high = n - 1;
		int ans = -1;
		
		while (low <= high) {
			int mid = low + ((high - low) / 2);
			
			if (nums[mid] == target) {
				ans = mid;
				high = mid - 1;
			} else if (nums[mid] > target) {
				high = mid - 1;
			} else {
				low = mid + 1;
			}
		}
		
		return ans;
	}
	
	int lastOccurence(vector<int>& nums, int target) {
		int n = nums.size();
		int low = 0, high = n - 1;
		int ans = -1;
		
		while (low <= high) {
			int mid = low + ((high - low) / 2);
			
			if (nums[mid] == target) {
				ans = mid;
				low = mid + 1;
			} else if (nums[mid] < target) {
				low = mid + 1;
			} else {
				high = mid - 1;
			}
		}
		
		return ans;
	}
	
  public:
	vector<int> searchRange(vector<int>& nums, int target) {
		vector<int> range(2, -1);
		range[0] = firstOccurence(nums, target);
		range[1] = lastOccurence(nums, target);
		
		return range;
	}
};
```