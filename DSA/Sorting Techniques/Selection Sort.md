**Approach**: Minimim at the left

- Time Complexity:
  Best / Average / Worst: O(n<sup>2</sup>)
  n = number of elements

- Space Complexity: O(1)

```cpp
class Solution {
  public:
	void selectionSort(vector<int>& arr) {
	    int n = arr.size();

	    for (int i = 0; i < n - 1; i++) {
	        int mini = i;
	        for (int j = i + 1; j < n; j++) {
	            if (arr[j] < arr[mini]) {
	                mini = j;
	            }
	        }
	        swap(arr[i], arr[mini]);
	    }
	}
};
```
