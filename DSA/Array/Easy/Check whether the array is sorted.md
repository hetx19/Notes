**Problem**: Given an array `arr`. Check if it is sorted or not

### Optimal Approach:

Idea: Traverse the array and check if every element is greater than or equal to the previous element. If any element is smaller than the previous one, the array is not sorted.

**Time Complexity:** O(n)
**Space Complexity** O(1)

```cpp
class Solution {
  public:
	bool isSorted(vector<int> &arr) {
		int n = arr.size();

		for (int i = 1; i < n; i++) {
			if (arr[i] < arr[i - 1]) {
				return false;
			}
		}

		return true;
	}
};
```

---

