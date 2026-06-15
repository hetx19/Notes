**Problem**: Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You must write an algorithm with `O(log n)` runtime complexity.

**Example**:

**Input**: `nums = [1,3,5,6], target = 5`
**Output**: 2

[Visit Leetcode](https://leetcode.com/problems/search-insert-position/)
[Visit GFG](https://www.geeksforgeeks.org/problems/search-insert-position-of-k-in-a-sorted-array/1)

---

### Brute Force
By doing Linear Search

**Time Complexity**: O(n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int searchInsert(vector<int>& nums, int target) {
		int n = nums.size();
	    
	    for (int i = 0; i < n; i++) {
		    if (nums[i] >= target) {
			    return i;
			}
	    }
	    
	    return n;
	}
};
```

---

### Optimal Solution
By using Lower Bound Function - [[Lower Bound]]

```cpp
class Solution {
  public:
    int searchInsert(vector<int>& nums, int target) {
	    int n = nums.size();
	    int low = 0, high = n - 1;
	    int ans = n;
	    
	    while (low <= high) {
		    int mid = low + ((high - low) / 2);
		    
			if (nums[mid] >= target) {
				ans = mid;
				high = mid - 1;
			} else {
				low = mid + 1;
			}
	    }
	   
	    return ans;
    }
};
```