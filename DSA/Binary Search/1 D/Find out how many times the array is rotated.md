**Problem**: Given an increasing sorted rotated array **arr[]** of distinct integers. The array is right-rotated **k** times. Find the value of **k**.  
Let's suppose we have an array `arr[] = [2, 4, 6, 9]`, if we rotate it by 2 times it will look like this:  
After 1st Rotation : `[9, 2, 4, 6]`
After 2nd Rotation : `[6, 9, 2, 4]`

**Example**:

**Input**: `arr[] = [5, 1, 2, 3, 4]`
**Output**: 1
**Explanation**: The given array is `[5, 1, 2, 3, 4]`. The original sorted array is `[1, 2, 3, 4, 5]`. We can see that the array was rotated 1 times to the right.

[Visit GFG](https://www.geeksforgeeks.org/problems/rotation4723/1)

---

### Brute Force
By doing Linear Search - [[Linear Search]]

**Time Complexity**: O(n) 
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
    int findKRotation(vector<int> &arr) {
	    int ans = INT_MAX, n = arr.size();
	    int index = -1;
	    
	    for (int i = 0; i < n; i++) {
		    if (arr[i] < ans) {
			    ans = arr[i];
			    index = i;
		    }
	    }
	    
	    return index;
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
	int findMin(vector<int>& nums) {
		int n = nums.size();
		int low = 0, high = n - 1;
		
		while (low <= high) {
			int mid = low + ((high - low) / 2);
			
			if (nums[mid] > nums[high]) {
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