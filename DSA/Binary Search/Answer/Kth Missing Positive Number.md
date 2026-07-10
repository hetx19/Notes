**Problem**: Given an array `arr` of positive integers sorted in a **strictly increasing order**, and an integer `k`.

Return _the_ `kth` _**positive** integer that is **missing** from this array._

**Example**:
**Input**: `arr = [2,3,4,7,11], k = 5`
**Output**: 9
**Explanation**: The missing positive integers are `[1,5,6,8,9,10,12,13,...]`. The 5th missing positive integer is 9.

[Visit Leetcode](https://leetcode.com/problems/kth-missing-positive-number/)
[Visit GFG](https://www.geeksforgeeks.org/problems/kth-missing-positive-number-in-a-sorted-array/1)

---

### Brute Force Solution
By using Linear Search function - [[Linear Search]]


**Time Complexity**: O(n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int findKthPositive(vector<int>& arr, int k) {
		for (int &num : arr) {
			if (num <= k) {
				k++;
			} else {
				break;
			}
		}
		
		return k;
	}
};
```

---
### Optimal Solution
By using Binary Search function - [[Binary Search]]


**Time Complexity**: O(log n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int findKthPositive(vector<int>& arr, int k) {
		int low = 0, high = arr.size() - 1;
		
		while (low <= high) {
			int mid = low + ((high - low) / 2);
			int missing = arr[mid] - (mid + 1);
			
			if (missing < k) {
				low = mid + 1;
			} else {
				high = mid - 1;
			}
		}
		
		return k + high + 1;
	}
};
```

---
