**Problem**: Given an array `arr` and an integer `k`, **return the index** of `k` in `arr`.  
If `k` is **not present**, return `-1`.

**Example1**:

**Input:** arr = `[3,4,5,1,2]` k = 5
**Output:** 2

**Example2**:

**Input:** arr = `[3,4,5,1,2]` k = 6
**Output:** -1

### Optimal Approach:

Idea: Visit the next index until you find the element

**Time Complexity:** O(n)
**Space Complexity** O(1)

```cpp
class Solution {
  public:
	int linearSearch(vector<int> &arr, int k) {
		int n = arr.size();

		for (int i = 0; i < n; i++) {
			if (arr[i] == k) {
				return i;
			}
		}

		return -1;
	}
};
```

---
