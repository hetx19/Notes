**Problem**: Given an integer array `nums`, **left rotate** the array by `k` steps.

### Brute force Approach:

**Idea**:

- Store first `k` elements in a temporary array
- Shift remaining elements to the left
- Place stored elements at the end

**Time Complexity:** O(n + k)
**Space Complexity** O(k)

```cpp
class Solution {
  public:
	void leftRotate(vector<int> &nums, int k) {
	    if (nums.empty()) return;

	    int n = nums.size();
	    k = k % n;

	    vector<int> temp;

	    for (int i = 0; i < k; i++) {
	        temp.push_back(nums[i]);
	    }

	    for (int i = k; i < n; i++) {
	        nums[i - k] = nums[i];
	    }

	    for (int i = 0; i < k; i++) {
	        nums[n - k + i] = temp[i];
	    }
	}
};
```

### Optimal Approach (Reversal Algorithm)

**Idea**:

- Reverse first `k` elements
- Reverse remaining `n-k` elements
- Reverse entire array

**Time Complexity:** O(2n) → O(n)
**Space Complexity** O(1)

```cpp
class Solution {
  public:
	void leftRotate(vector<int> &nums, int k) {
	    if (nums.empty()) return;

	    int n = nums.size();
	    k = k % n;

	    reverse(nums.begin(), nums.begin() + k);
	    reverse(nums.begin() + k, nums.end());
	    reverse(nums.begin(), nums.end());
	}
};
```

---

**Problem**: Given an integer array `nums`, **right rotate** the array by `k` steps.

### Brute force Approach:

**Idea**:

- Store last `k` elements in a temporary array
- Shift remaining elements to the right
- Place stored elements at the beginning

**Time Complexity:** O(n + k)
**Space Complexity** O(k)

```cpp
class Solution {
  public:
	void rightRotate(vector<int> &nums, int k) {
	    if (nums.empty()) return;

	    int n = nums.size();
	    k = k % n;

	    vector<int> temp;

	    for (int i = n - k; i < n; i++) {
	        temp.push_back(nums[i]);
	    }

	    for (int i = n - k - 1; i >= 0; i--) {
	        nums[i + k] = nums[i];
	    }

	    for (int i = 0; i < k; i++) {
	        nums[i] = temp[i];
	    }
	}
};
```

### Optimal Approach (Reversal Algorithm):

**Idea**:

1. Reverse entire array
2. Reverse first `k` elements
3. Reverse remaining `n-k` elements

**Time Complexity:** O(2n) → O(n)
**Space Complexity** O(1)

```cpp
class Solution {
  public:
	void rightRotate(vector<int> &nums, int k) {
	    if (nums.empty()) return;

	    int n = nums.size();
	    k = k % n;

	    reverse(nums.begin(), nums.end());
	    reverse(nums.begin(), nums.begin() + k);
	    reverse(nums.begin() + k, nums.end());
	}
};
```

---

#### If reverse function is not given

```cpp
void reverse(vector<int> &arr, int start, int end) {
    while (start <= end) {
        int temp = arr[start];      // Or we can use swap function
        arr[start] = arr[end];
        arr[end] = temp;
        start++;
        end--;
    }
}
```

