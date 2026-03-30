**Approach**: Divide the array around a pivot element, recursively sort the subarrays on either side of the pivot, and combine the results.

Time Complexity:

- Best-Case / Average-Case: O(n log n)
- Worst-case: O(n<sup>2</sup>)

Space Complexity: O(1) + Recursion Stack Space
Recursion Stack Space -> O(log n) on average, O(n) worst case

```cpp
int partition(vector<int>& arr, int low, int high) {
	int pivot = arr[low], i = low, j = high;

	while (i < j) {
		while (arr[j] > pivot && j >= low + 1) {
			j--;
		}

		while (arr[i] <= pivot && i <= high - 1) {
			i++;
		}

		if (i < j) {
			swap(arr[i], arr[j]);
		}
	}

	swap(arr[low], arr[j]);
	return j;
}

void quickSort(vector<int>& arr, int low, int high) {
	if (low >= high) {
		return;
	}

	int p_index = partition(arr, low, high);
	quickSort(arr, low, p_index - 1);
	quickSort(arr, p_index + 1, high);
}
```

