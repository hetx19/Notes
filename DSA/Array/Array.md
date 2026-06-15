### What is an Array?
An **Array** is a linear data structure that stores elements of the **same data type** in **contiguous memory locations**.

```cpp
int arr[5] = {1, 2, 3, 4, 5};
```

#### Characteristics
- Fixed size
- Contiguous memory allocation
- Constant-time random access
- Same data type for all elements

---

### Memory Representation

```cpp
int arr[5] = {10, 20, 30, 40, 50};
```

| Index | Value |
| ----- | ----- |
| 0     | 10    |
| 1     | 20    |
| 2     | 30    |
| 3     | 40    |
| 4     | 50    |

Memory:

```text
1000  1004  1008  1012  1016
+----+----+----+----+----+
| 10 | 20 | 30 | 40 | 50 |
+----+----+----+----+----+
```

Address Formula:

```cpp
Address(arr[i]) = Base_Address + i * sizeof(data_type)
```

---

### Declaration & Initialization

#### Declaration

```cpp
int arr[5];
```

#### Initialization

```cpp
int arr[5] = {1, 2, 3, 4, 5};
```

#### Partial Initialization

```cpp
int arr[5] = {1, 2};
```

Result:

```cpp
{1, 2, 0, 0, 0}
```

#### Size Inference

```cpp
int arr[] = {1, 2, 3, 4};
```

---

### Accessing Elements

```cpp
cout << arr[0];
cout << arr[3];
```

#### Update

```cpp
arr[2] = 100;
```

---

###  Time Complexity

| Operation | Complexity |
|------------|------------|
| Access | O(1) |
| Update | O(1) |
| Search (Unsorted) | O(n) |
| Search (Sorted) | O(log n) |
| Insert at End | O(1) |
| Insert at Beginning | O(n) |
| Delete | O(n) |

---

### Traversal
#### Using Loop

```cpp
for(int i = 0; i < n; i++) {
    cout << arr[i] << " ";
}
```

#### Range-Based Loop

```cpp
for(int num : arr) {
    cout << num << " ";
}
```

---

### Searching
#### Linear Search

```cpp
int target = 5;

for(int i = 0; i < n; i++) {
    if(arr[i] == target) {
        return i;
    }
}

return -1;
```

**Time Complexity:** O(n)

---

#### Binary Search
Prerequisite:
> Array must be sorted.

```cpp
int low = 0;
int high = n - 1;

while(low <= high) {
    int mid = low + (high - low) / 2;

    if(arr[mid] == target)
        return mid;

    if(arr[mid] < target)
        low = mid + 1;
    else
        high = mid - 1;
}
```

**Time Complexity:** O(log n)

---

### Insertion

#### Insert at End

```cpp
arr[n] = value;
n++;
```

Time: O(1)

---

#### Insert at Position

```cpp
for(int i = n; i > pos; i--) {
    arr[i] = arr[i - 1];
}

arr[pos] = value;
n++;
```

Time: O(n)

---

### Deletion

#### Delete by Index

```cpp
for(int i = pos; i < n - 1; i++) {
    arr[i] = arr[i + 1];
}

n--;
```

Time: O(n)

---

### 📚 STL Array

Header:

```cpp
#include <array>
```

Declaration:

```cpp
array<int, 5> arr = {1, 2, 3, 4, 5};
```

Access:

```cpp
arr[0]
arr.at(0)
```

Useful Functions:

```cpp
arr.size();
arr.front();
arr.back();
arr.fill(10);
```

---

### Common DSA Patterns on Arrays

#### 1. Prefix Sum

```cpp
prefix[0] = arr[0];

for(int i = 1; i < n; i++) {
    prefix[i] = prefix[i - 1] + arr[i];
}
```

Range Sum:

```cpp
sum(l, r) = prefix[r] - prefix[l - 1];
```

Complexity:
- Build: O(n)
- Query: O(1)

---

#### 2. Sliding Window

Used for:
- Maximum Sum Subarray of Size K
- Longest Substring Problems

```cpp
int sum = 0;

for(int i = 0; i < k; i++) {
    sum += arr[i];
}

int maxSum = sum;

for(int i = k; i < n; i++) {
    sum += arr[i];
    sum -= arr[i - k];

    maxSum = max(maxSum, sum);
}
```

Complexity: O(n)

---

#### 3. Two Pointers

Used in:
- Sorted Arrays
- Pair Sum Problems

```cpp
int left = 0;
int right = n - 1;

while(left < right) {
    int sum = arr[left] + arr[right];

    if(sum == target)
        break;

    if(sum < target)
        left++;
    else
        right--;
}
```

Complexity: O(n)

---

#### 4. Kadane's Algorithm
Maximum Subarray Sum

```cpp
int currentSum = 0;
int maxSum = INT_MIN;

for(int num : arr) {
    currentSum += num;

    maxSum = max(maxSum, currentSum);

    if(currentSum < 0)
        currentSum = 0;
}
```

Complexity: O(n)

---

### Common Mistakes

#### Out of Bounds Access
❌

```cpp
arr[n]
```

Valid indices:

```cpp
0 to n - 1
```

---

#### Integer Overflow

Use:
```cpp
long long
```

instead of:
```cpp
int
```

when constraints are large.

---

#### Wrong Mid Calculation

❌

```cpp
int mid = (low + high) / 2;
```

✅

```cpp
int mid = low + (high - low) / 2;
```

---

### Cheat Sheet

| Technique | Complexity |
|------------|------------|
| Traversal | O(n) |
| Linear Search | O(n) |
| Binary Search | O(log n) |
| Prefix Sum Query | O(1) |
| Sliding Window | O(n) |
| Two Pointers | O(n) |
| Kadane's Algorithm | O(n) |

---

### Key Takeaways
- Arrays provide **O(1) random access**.
- Binary Search requires a **sorted array**.
- Prefix Sum helps in **range queries**.
- Sliding Window optimizes fixed/variable length subarray problems.
- Two Pointers are powerful for sorted arrays.
- Kadane's Algorithm solves Maximum Subarray Sum in **O(n)**.

---

### Problems on Array - [[Problems on Array]]
---