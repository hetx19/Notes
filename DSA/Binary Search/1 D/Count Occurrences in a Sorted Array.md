**Problem**: Given a **sorted** array, **arr[]** and a number **target**, you need to find the number of occurrences of **target** in **arr[]**. 

**Example**:

**Input**: `arr[] = [1, 1, 2, 2, 2, 2, 3], target = 2`
**Output**: 4
**Explanation**: target = 2 occurs 4 times in the given array so the output is 4.

[Visit GFG](https://www.geeksforgeeks.org/problems/number-of-occurrence2259/1)

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
	int countFreq(vector<int>& arr, int target) {
		return lastOccurence(arr, target) - firstOccurence(arr, target) + 1;
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
	int countFreq(vector<int>& arr, int target) {
		return lastOccurence(arr, target) - firstOccurence(arr, target) + 1;
    }
};
```