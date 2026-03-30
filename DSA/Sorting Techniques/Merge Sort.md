**Approach**: Divide the array into smaller subarrays, sort them recursively, and then merge the sorted subarrays to form the final sorted array.

Time Complexity: O(n log n)
Space Complexity: O(n) (due to temporary array in `merge`)

```cpp
void merge(vector<int>& arr, int low, int mid, int high) {
	vector<int> temp;
	temp.reserve(high - low + 1);
	int left = low, right = mid + 1;

	while (left <= mid && right <= high) {
		if (arr[left] <= arr[right]) {
			temp.push_back(arr[left++]);
		} else {
			temp.push_back(arr[right++]);
		}
	}

	while (left <= mid) {
		temp.push_back(arr[left++]);
	}

	while (right <= high) {
		temp.push_back(arr[right++]);
	}

	for (int i = low; i <= high; i++) {
		arr[i] = temp[i - low];
	}
}

void mergeSort(vector<int>& arr, int low, int high) {
	if (low >= high) {
		return;
	}

	int mid = low + ((high - low) >> 1);
	mergeSort(arr, low, mid);
	mergeSort(arr, mid + 1, high);
	merge(arr, low, mid, high);
}
```

