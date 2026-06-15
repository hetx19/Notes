**Problem**: Given an array `arr`. Return the largest element in the array.

### Brute Force Approach:

idea: sort and return the last element.

**Time Complexity:** O(n log n)
**Space Complexity**:
O(1) → if the sorting algorithm is **in-place**
O(n) → if a **copy of the array** is created before sorting

```cpp
class Solution {
  public:
	int largest(vector<int> &arr) {
		vector<int> temp = arr;
		int n = temp.size();
		sort(temp.begin(), temp.end());

		return temp[n - 1];
	}
};
```

---

### Optimal Approach:

idea: Traverse the array and keep track of the maximum element seen so far.

**Time Complexity:** O(n)
**Space Complexity** O(1)

```cpp
class Solution {
	int largest(vector<int> &arr) {
		int largest = arr[0];
		int n = arr.size();

		for (int i = 1; i < n; i++) {
			if (arr[i] > largest) {
				largest = arr[i];
			}
		}

		return largest;
	}
};
```

---

##### Important point: It is a good practice not to alter the input unless the question told you to do it
