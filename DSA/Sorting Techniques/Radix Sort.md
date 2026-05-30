**Approach**: Sort numbers digit by digit from least significant digit (LSD) to most significant digit (MSD) using counting sort as a subroutine.

- Time Complexity:
  Best / Average / Worst: O(d \* (n + k))
  n = number of elements, k = range of digits (0–9), d = number of digits in the maximum number

- Space Complexity: O(n + k)

```cpp
class Solution {
  private:
	void countingSort(vector<int>& arr, int exp) {
		int n = arr.size();
		vector<int> output(n);
		int count[10] = {0};

		for (int i = 0; i < n; i++) {
			count[(arr[i] / exp) % 10]++;
		}

		for (int i = 1; i < 10; i++) {
			count[i] += count[i - 1];
		}

		for (int i = n - 1; i >= 0; i--) {
			int digit = (arr[i] / exp) % 10;
			output[count[digit] - 1] = arr[i];
			count[digit]--;
		}

		for (int i = 0; i < n; i++) {
			arr[i] = output[i];
		}
	}
	
  public:
	void radixSort(vector<int>& arr) {
		int maxVal = *max_element(arr.begin(), arr.end());

		for (int exp = 1; maxVal / exp > 0; exp *= 10) {
			countingSort(arr, exp);
		}
	}
};
```

