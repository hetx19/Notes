**Problem**: Given an array of integers `nums` and an integer `threshold`, we will choose a positive integer `divisor`, divide all the array by it, and sum the division's result. Find the **smallest** `divisor` such that the result mentioned above is less than or equal to `threshold`.

Each result of the division is rounded to the nearest integer greater than or equal to that element. (For example: `7/3 = 3` and `10/2 = 5`).

The test cases are generated so that there will be an answer.

**Example**:
**Input**: `nums = [1,2,5,9], threshold = 6`
**Output**: 5
**Explanation**: We can get a sum to 17 (1+2+5+9) if the divisor is 1. 
If the divisor is 4 we can get a sum of 7 (1+1+2+3) and if the divisor is 5 the sum will be 5 (1+1+1+2).

[Visit Leetcode](https://leetcode.com/problems/find-the-smallest-divisor-given-a-threshold/)
[Visit GFG](https://www.geeksforgeeks.org/problems/smallest-divisor/1)

---
### Brute Force
By using linear Search - [[Linear Search]]

**Time Complexity**: O(n max(nums)) → O(n<sup>2</sup>)
**Space Complexity**: O(1)

```cpp
class Solution {
  private:
	int sumByD(vector<int> &nums, int d) {
		int sum = 0;
		
		for (int &num : nums) {
				sum += (ceil)((double)(num) / (double)(d));
			}
		}
		
		return sum;
	}
	
  public:
	int smallestDivisor(vector<int>& nums, int threshold) {
		int maxi = *max_element(nums.begin(), nums.end());
		
		for (int d = 1; d <= maxi; d++) {
			int sum = sumByD(nums, d);
			
			if (sum <= threshold) return d;
		}
		
		return -1;
	}
};
```

---
### Optimal Solution
By using Binary Search Function - [[Binary Search]]

**Time Complexity**: O(n * log (max(num))) → O(n log n)
**Space Complexity**: O(1)

```cpp
class Solution {
  private:
	int sumByD(vector<int> &nums, int d) {
		int sum = 0;
		
		for (int &num : nums) {
			sum += (ceil)((double)(num) / (double)(d));
		}
		
		return sum;
	}
	
  public:
	int smallestDivisor(vector<int>& nums, int threshold) {
		int n = nums.size();
		if (n > threshold) return -1;
		
		int low = 1;
		int high = *max_element(nums.begin(), nums.end());
		
		while (low <= high) {
			int mid = low + ((high - low) / 2);
			
			int sum = sumByD(nums, mid);
			
			if (sum > threshold) {
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