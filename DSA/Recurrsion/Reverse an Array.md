**Problem:** Given an array of size `n`, reverse it using recursion.

Time Complexity: O(n)  
Space Complexity: O(n) → recursive stack

### Parameterized / With Indices

```cpp
class Solution {
  public:
	void reverseArray(vector<int> &arr, int i, int j) {
		if (i > j) {
			return;
		}

		swap(arr[i], arr[j]);
		reverseArray(arr, i + 1, j - 1);
	}
};
```

**Call:**

```cpp
reverseArray(arr, 0, n - 1);
```

---

### Functional / Backtracking Style

```cpp
class Solution {
  public:
	void reverseArray(vector<int> &arr, int i = 0) {
	    int n = arr.size();
	    if (i >= n / 2) {
		    return;
	    }

	    swap(arr[i], arr[n - i - 1]);
	    reverseArray(arr, i + 1);
	}
};
```

Call:

```cpp
reverseArray(arr);
```

### Iterative / Optimal

Time Complexity: O(n)
Space Complexity: O(n) → recursive stack

```cpp
class Solution {
  public:
	void reverseArray(vector<int> &arr) {
		int i = 0, j = arr.size() - 1;
		while (i < j) {
			swap(arr[i], arr[j]);
			i++;
			j--;
		}
	}
};
```

