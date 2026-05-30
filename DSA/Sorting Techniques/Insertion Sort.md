**Approach**: Takes an element and place it at its correct position.

Time Complexity:

- Worst-case / Average-case: O(n<sup>2</sup>)
- Best-case: O(n)
  Space complexity: O(1) // Not using any extra space

1. Iterative Method

```cpp
class Solution {
  public:
	void insertionSort(vector<int>& arr) {
	    int n = arr.size();

	    for (int i = 0; i < n; i++) {
		    int j = i;
	        while (j > 0 && arr[j - 1] > arr[j]) {
	            swap(arr[j - 1], arr[j]);
	            j--;
	        }
	    }
	}
};
```

2. Recursive Method
   Time Complexity:

- Worst-case / Average-case: O(n<sup>2</sup>)
- Best-case: O(n)
  Space complexity: O(N) // Recursion Stack Space

```cpp
class Solution {
  public:
	void insertionSort(vector<int>& arr, int i, int n) {
		if (i == n) {
			return;
		}

		int j = i;
		while (j > 0 && arr[j - 1] > arr[j]) {
	        swap(arr[j - 1], arr[j]);
	        j--;
	    }

		insertionSort(arr, i + 1, n);
	}
};
```

