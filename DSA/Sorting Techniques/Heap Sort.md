**Approach**: Build a max heap from the array, then repeatedly swap the root with the last element and heapify the reduced heap until sorted.

Time Complexity: O(n log n)

Space Complexity: O(1) + Recursion Stack Space
Recursion Stack Space -> O(log n)

```cpp
class Solution {
  private:
	void heapify(vector<int>& arr, int n, int i) {
		int largest = i;
		int left = 2 * i + 1, right = 2 * i + 2;

		if (left < n && arr[left] > arr[largest]) {
			largest = left;
		}

		if (right < n && arr[right] > arr[largest]) {
			largest = right;
		}

		if (largest != i) {
			swap(arr[largest], arr[i]);
			heapify(arr, n, largest);
		}
	}
	
  public:
	void heapSort(vector<int>& arr) {
		int n = arr.size();

		for (int i = (n >> 1) - 1; i >= 0; i--) {
			heapify(arr, n, i);
		}

		for (int i = n - 1; i > 0; i--) {
			swap(arr[0], arr[i]);
			heapify(arr, i, 0);
		}
	}
};
```

