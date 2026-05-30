**Approach**: Maximum at the right by adjacent swap.

Time Complexity:

- Worst-case / Average-case: O(n<sup>2</sup>)
- Best-case: O(n)
  Space complexity: O(1) // Not using any extra space

1. Iterative Method

```cpp
class Solution {
  public:
	void bubbleSort(vector<int>& arr) {
	    int n = arr.size();

	    for (int i = n - 1; i >= 0; i--) {
	        bool didSwap = false;
	        for (int j = 0; j < i; j++) {
	            if (arr[j] > arr[j + 1]) {
	                swap(arr[j], arr[j + 1]);
	                didSwap = true;
	            }
	        }

	        if (didSwap == false) {
		        break;
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
	void bubbleSort(vector<int>& arr, int n) {
		if (n == 1) {
			return;
		}

		bool didSwap = true;
		for (int i = 0; i < n - 1; i++) {
			if (arr[i] > arr[i + 1]) {
				swap(arr[i], arr[i + 1]);
				didSwap = true;
			}
		}

		if (didSwap == false) {
			return;
		}

		bubbleSort(arr, n - 1);
	}
};
```
