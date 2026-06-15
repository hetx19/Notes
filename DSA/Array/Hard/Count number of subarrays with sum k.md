**Problem**: Given an array of integers `nums` and an integer `k`, return _the total number of subarrays whose sum equals to_ `k`.

A subarray is a contiguous **non-empty** sequence of elements within an array.

**Example**:
**Input**: `nums = [1,1,1], k = 2`
**Output**: 2

[Visit Leetcode](https://leetcode.com/problems/subarray-sum-equals-k/)
[Visit GFG](https://www.geeksforgeeks.org/problems/subarrays-with-sum-k/1)

---

### Brute Force
By Generating all subarrays

**Time Complexity**: O(n<sup>3</sup>)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int subarraySum(vector<int>& nums, int k) {
		int count = 0, n = nums.size();
		
		for (int i = 0; i < n; i++) {
			for (int j = i; j < n; j++) {
				int sum = 0;
				for (int k = i; k <= j; k++) {
					sum += nums[k];
				}
				
				if (sum == k) {
					count++;
				}
			}
		}
		
		return count;
	}
};
```

---

### Better Solution
Betterment in Brute

**Time Complexity**: O(n<sup>2</sup>)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int subarraySum(vector<int>& nums, int k) {
		int count = 0, n = nums.size();
		
		for (int i = 0; i < n; i++) {
			int sum = 0;
			for (int j = i; j < n; j++) {
				sum += nums[j];
				
				if (sum == k) {
					count++;
				}
			}
		}
		
		return count;
	}
};
```

---

### Optimal Solution
By using concept of hashing - [[Hashing]]

**Time Complexity**: O(n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int subarraySum(vector<int>& nums, int k) {
		int n = nums.size(), count = 0, preSum = 0;
		unordered_map<int, int> mpp;
		
		mpp[0] = 1;
		
		for (int i = 0; i < n; i++) {
			preSum += nums[i];
			int remaining = preSum - k;
			
			count += mpp[remaining];
			
			mpp[preSum] += 1;
		}
		
		return count;
	}
};
```

---