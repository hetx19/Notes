**Problem**: Given an integer array `nums` and an integer `k`, split `nums` into `k` non-empty subarrays such that the largest sum of any subarray is **minimized**.
Return _the minimized largest sum of the split_.
A **subarray** is a contiguous part of the array.

**Example**:
**Input**: `nums = [7,2,5,10,8], k = 2`
**Output**: 18
**Explanation**: There are four ways to split nums into two subarrays.
`The best way is to split it into [7,2,5] and [10,8], where the largest sum among the two subarrays is only 18.`

[Visit Leetcode](https://leetcode.com/problems/split-array-largest-sum/)
[Visit GFG](https://www.geeksforgeeks.org/problems/split-array-largest-sum--141634/1)

---
### Brute Force Solution
By using Linear Search function - [[Linear Search]]

**Time Complexity**: **Time Complexity**: O(n * (sum - max + 1)) → O(n<sup>2</sup>)
**Space Complexity**: O(1)

```cpp
class Solution { 
  private:
	int countPartition(vector<int>& nums, int maxSum) {
		int partitions = 1, subArraySum = 0;
		
		for (int &num : nums) {
			if (num + subArraySum <= maxSum) {
				subArraySum += num;
			} else {
				partitions++;
				subArraySum = num;
			}
		}
		
		return partitions;
	}
	
  public:
	int splitArray(vector<int>& nums, int k) {
		int low = *max_element(nums.begin(), nums.end());
		int high = accumulate(nums.begin(), nums.end(), 0);
		
		for (int i = low; i <= high; i++) {
			if (countPartition(nums, i) <= k) {
				return i;
			}
		}
		
		return low;
	}
};
```

---
### Optimal Solution
By using Binary Search Function - [[Binary Search]]

**Time Complexity**: O(n * log(sum - max + 1)) → O(n logn)
**Space Complexity**: O(1)

```cpp
class Solution { 
  private:
	int countPartition(vector<int>& nums, int maxSum) {
		int partitions = 1, subArraySum = 0;
		
		for (int &num : nums) {
			if (num + subArraySum <= maxSum) {
				subArraySum += num;
			} else {
				partitions++;
				subArraySum = num;
			}
		}
		
		return partitions;
	}
	
  public:
	int splitArray(vector<int>& nums, int k) {
		int low = *max_element(nums.begin(), nums.end());
		int high = accumulate(nums.begin(), nums.end(), 0);
		
		while (low <= high) {
			int mid = low + ((high - low) / 2);
			int partitions = countPartition(nums, mid);
			
			if (partitions > k) {
				low = mid + 1;
			} else {
				high = mid - 1;
			}
		}
		
		return low;
	}
};
```

---
