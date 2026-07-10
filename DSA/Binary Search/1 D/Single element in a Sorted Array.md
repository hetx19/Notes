**Problem**: You are given a sorted array consisting of only integers where every element appears exactly twice, except for one element which appears exactly once.

Return _the single element that appears only once_.

Your solution must run in `O(log n)` time and `O(1)` space.

**Example**:
**Input**: `nums = [1,1,2,3,3,4,4,8,8]`
**Output:** 2

[Visit Leetcode](https://leetcode.com/problems/single-element-in-a-sorted-array/)
[Visit GFG](https://www.geeksforgeeks.org/problems/find-the-element-that-appears-once-in-sorted-array0624/1)

---

### Brute Force

**Time Complexity**: O(n)
**Space Complexity**: O(1)

```cpp
class Solution { 
  public:
	int singleNonDuplicate(vector<int>& nums) {
		int n = nums.size();
		
		if (n == 1) {
			return nums[0];
		}
		
		for (int i = 0; i < n; i++) {
			if (i == 0) {
				if (nums[i] != nums[i + 1]) {
					return nums[i];
				}
			} else if (i == n - 1) {
				if (nums[i] != nums[i - 1]) {
					return nums[i];
				}
			} else {
				if (nums[i] != nums[i - 1] && nums[i] != nums[i + 1]) {
					return nums[i];
				}
			}
		}
		
		return -1;
	}
};
```

---
### Better Approach
By using XOR operation

**Time Complexity**: O(n)
**Space Complexity**: O(1)

```cpp
class Solution { 
  public:
	int singleNonDuplicate(vector<int>& nums) {
		int sigleElement = 0;
		
		for (int &num : nums) {
			sigleElement ^= num;
		}
		
		return sigleElement;
	}
};
```

---
### Optimal Solution
By using Binary Search Function - [[Binary Search]]

**Time Complexity**: O(log n)
**Space Complexity**: O(1)

```cpp
class Solution { 
  public:
	int singleNonDuplicate(vector<int>& nums) {
		int n = nums.size();
		
		if (n == 1) {
			return nums[0];
		}
		
		if (nums[0] != nums[1]) {
			return nums[0];
		}
		
		if (nums[n - 1] != nums[n - 2]) {
			return nums[n - 1];
		}
		
		if (n == 1) {
			return nums[0];
		}
		
		int low = 1, high = n - 2;
		
		while (low <= high) {
			int mid = low + ((high - low) / 2);
			
			if (nums[mid] != nums[mid - 1] && nums[mid] != nums[mid + 1]) {
				return nums[mid];
			}
			
			if ((mid & 1) && nums[mid] == nums[mid - 1]) {
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
