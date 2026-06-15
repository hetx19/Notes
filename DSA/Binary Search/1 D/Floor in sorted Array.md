**Problem**: Given a sorted array **arr[]** and an integer **x**, find the index (0-based) of the largest element in arr[] that is less than or equal to x. This element is called the **floor** of x. If such an element does not exist, return -1.

**Note**: In case of multiple occurrences of floor of x, return the index of the last occurrence.

**Example**

**Input**: `arr[] = [1, 2, 8, 10, 10, 12, 19], x = 5`
**Output**: 1
**Explanation**: Largest number less than or equal to 5 is 2, whose index is 1.

[Visit GFG](https://www.geeksforgeeks.org/problems/floor-in-a-sorted-array-1587115620/1)

---

### Brute Force
By doing Linear Search - [[Linear Search]]

**Time Complexity**: O(n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int findFloor(vector<int>& arr, int k) {
		int n = arr.size();
		int ans = -1;
		
		for (int i = 0; i < n; i++) {
			if (arr[i] <= k) {
				ans = i;
			}
		}
		
		return ans;
	}
};
```

---

### Optimal Solution
By using Binary Search (Similar to lower bound) - [[Lower Bound]]

**Time Complexity**: O(log n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int findFloor(vector<int>& arr, int k) {
		int n = arr.size();
		int low = 0, high = n - 1;
		int ans = -1;
		
		while (low <= high) {
			int mid = low + ((high - low) / 2);
			
			if (arr[mid] <= k) {
				ans = mid;
				low = mid + 1;
			} else {
				high = mid - 1;
			}
		}
		
		return ans;
	}
};
```