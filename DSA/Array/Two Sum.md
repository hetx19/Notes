**Problem Statement**: Given an array of integers `nums` and an integer `target`, return _indices of the two numbers such that they add up to `target`_.

You may assume that each input would have **_exactly_ one solution**, and you may not use the _same_ element twice.

You can return the answer in any order.

**Example**:
**Input**: `nums = [2,7,11,15], target = 9`
**Output**: `[0,1]`
**Explanation**: Because `nums[0] + nums[1] == 9`, we return `[0, 1]`.

[Visit Leetcode](https://leetcode.com/problems/two-sum/)
[Visit GFG](https://www.geeksforgeeks.org/problems/key-pair5616/1)

---

### Brute Force Approach
By simple nested for loop

**Time Complexity**: O(n<sup>2</sup>)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	vector<int> twoSum(vector<int>& nums, int target) {
		int n = nums.size();
		
		for (int i = 0; i < n; i++) {
			for (int j = i + 1; j < n; j++) {
				if (nums[i] + nums[j] == target) {
					return {i, j};
				}
			}
		}
		
		return {-1, -1};
	}
};
```

---

### Optimal Solution (if array is unsorted)
By using concept of hashing - [[Hashing]]

**Time Complexity**: O(n)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
	vector<int> twoSum(vector<int>& nums, int target) {
		int n = nums.size();
		unordered_map<int, int> mpp;
		
		for (int i = 0; i < n; i++) {
			int rem = target - nums[i];
			
			if (mpp.find(rem) != mpp.end()) {
				return {mpp[rem], i};
			} else {
				mpp[nums[i]] = i;
			}
		}
		
		return {-1, -1};
	}
};
```

---

### Optimal Solution (if array is sorted)
By using **_Two Pointer Approach_** 

**Time Complexity**: O(n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	vector<int> twoSum(vector<int>& nums, int target) {
		int left = 0, right = nums.size() - 1;
		
		while (left < right) {
			int sum = nums[left] + nums[right];
			
			if (sum == target) {
				return {left, right};
			} else if (sum < target) {
				left++;
			} else {
				right--;
			}
		}
		
		return {-1, -1};
	}
};
```