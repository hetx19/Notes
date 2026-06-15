**Problem**: Given a sorted array **arr[]** (following **0-based indexing**) and a number **target**, find the **lower bound** of the **target** in this given array. The **lower bound** of a number is defined as the smallest **index** in the sorted array where the element is **greater than or equal to** the given number.

**Note:** If all the elements in the given array are smaller than the **target**, the lower bound will be the length of the array.

**Example**:

**Input**: `arr[] = [2, 3, 7, 10, 11, 11, 25], target = 9`
**Output**: 3
**Explanation**: 3 is the smallest index in arr[] where element (`arr[3] = 10`) is greater than or equal to 9.

[Visit GFG](https://www.geeksforgeeks.org/problems/implement-lower-bound/1)

---
### Brute Force
By using Linear Search - [[Linear Search]]

**Time Complexity**: O(n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
    int lowerBound(vector<int>& arr, int target) {
	    int n = arr.size();
	    
	    for (int i = 0; i < n; i++) {
		    if (arr[i] >= target) {
			    return i;
			}
	    }
	    
	    return n;
    }
};
```

---

### Optimal Solution
By using Binary Search - [[Binary Search]]

**Time Complexity**: O(log n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
    int lowerBound(vector<int>& arr, int target) {
	    int n = arr.size();
	    int low = 0, high = n - 1;
	    int ans = n;
	    
	    while (low <= high) {
		    int mid = low + ((high - low) / 2);
		    
			if (arr[mid] >= target) {
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