**Problem**: Given an array `arr`. Return the second largest element in the array.

### Brute Force Approach:

Idea: Sort the array and traverse from the end to find the largest element that is smaller than the maximum element.

**Time Complexity:** O(n log n) + O(n)
**Space Complexity**:
O(1) → if the sorting algorithm is **in-place**
O(n) → if a **copy of the array** is created before sorting

```cpp
int second_largest(vector<int> &arr) {
	vector<int> temp = arr;
	int n = temp.size();
	sort(temp.begin(), temp.end());

	for (int i = n - 2; i >= 0; i--) {
		if (temp[i] != temp[n - 1]) {
			return temp[i];
		}
	}

	return -1;
}
```

---

### Better Approach:

Idea: Traverse the array to find the maximum element. Then run another loop to find the largest element smaller than the maximum.

**Time Complexity:** O(n) + O(n)
**Space Complexity** O(1)

```cpp
int second_largest(vector<int> &arr) {
    int n = arr.size();
    int largest = arr[0];

    for (int i = 1; i < n; i++) {
        largest = max(largest, arr[i]);
    }

    int s_largest = -1;

    for (int i = 0; i < n; i++) {
        if (arr[i] != largest) {
            s_largest = max(s_largest, arr[i]);
        }
    }

    return s_largest;
}
```

---

### Optimal Approach:

Idea: Traverse the array once while maintaining two variables: largest and second_largest. Whenever the largest updates, the previous largest becomes second_largest.

**Time Complexity:** O(n)
**Space Complexity** O(1)

```cpp
int second_largest(vector<int> &arr) {
    int n = arr.size();
    int largest = arr[0];
    int s_largest = -1;

    for (int i = 0; i < n; i++) {
	    if (arr[i] > largest) {
		    s_largest = largest;
		    largest = arr[i];
	    } else if (arr[i] > s_largest && arr[i] < largest) {
		    s_largest = arr[i];
	    }
    }

    return s_largest;
}

```

##### Important point: It is a good practice not to alter the input unless the question told you to do it

##### Important point: If array contains negative elements the use INT_MIN instead of -1.
