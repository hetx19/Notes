**Problem**: Given a sorted array **arr[]** and an integer **x**, find the index (0-based) of the smallest element in arr[] that is greater than or equal to x. This element is called the **ceil** of x. If such an element does not exist, return -1.

**Note**: In case of multiple occurrences of ceil of x, return the index of the first occurrence.

**Example**:

**Input**: `arr[] = [1, 2, 8, 10, 11, 12, 19], x = 5`
**Output**: 2
**Explanation**: Smallest number greater than 5 is 8, whose index is 2.

[Visit GFG](https://www.geeksforgeeks.org/problems/ceil-in-a-sorted-array/1)

---

### Brute Force
By doing Linear Search - [[Linear Search]]

**Time Complexity**: O(n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int findCeil(vector<int>& arr, int x) {
		int n = arr.size();
		int ans = -1;
		
		for (int i = 0; i < n; i++) {
			if (arr[i] >= x) {
				ans = i;
			}
		}
		
		return ans;
	}
};
```

---

### Optimal Solution
By using lower bound function - [[Lower Bound]]

**Time Complexity**: O(log n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int findCeil(vector<int>& arr, int x) {
		int n = arr.size();
		int low = 0, high = n - 1;
		int ans = -1;
		
		while (low <= high) {
			int mid = low + ((high - low) / 2);
			
			if (arr[mid] >= x) {
				ans = mid;
				high = mid - 1
			} else {
				low = mid + 1;
			}
		}
		
		return ans;
	}
};
```