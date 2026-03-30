**Problem**: Given an integer array `nums`, left rotate the array to the right by `1` steps.

### Optimal Approach:

Idea: Store the first element in temp variable and left shift the array by 1,

**Time Complexity:** O(n)
**Space Complexity** O(1)

```cpp
void leftRotate(vector<int> &nums) {
	int n = nums.size();
	int temp = nums[0];

	for (int i = 1; i < n; i++) {
		nums[i - 1] = nums[i];
	}

	nums[n - 1] = temp;
}
```

---

Problem: Given an integer array `nums`, right rotate the array to the right by `1` steps.

### Optimal Approach:

Idea: Store the last element in temp variable and right shift the array by 1,

**Time Complexity:** O(n)
**Space Complexity** O(1)

```cpp
void rightRotate(vector<int> &nums) {
	int n = nums.size();
	int temp = nums[n - 1];

	for (int i = n - 1; i > 0; i--) {
		nums[i] = nums[i - 1];
	}

	nums[0] = temp;
}
```

