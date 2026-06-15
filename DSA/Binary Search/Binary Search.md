### What is Binary Search?
Binary Search is an efficient searching algorithm used on **sorted data structures**.

Instead of checking every element, it repeatedly divides the search space into half.

#### Key Idea
```text
Target < Mid → Search Left Half
Target > Mid → Search Right Half
Target == Mid → Found
```

---

### Prerequisites

> The array must be sorted.

❌ Unsorted Array

```cpp
{5, 1, 9, 3}
```

✅ Sorted Array

```cpp
{1, 3, 5, 7, 9}
```

---

### Time Complexity

| Operation | Complexity |
|------------|------------|
| Best Case | O(1) |
| Average Case | O(log n) |
| Worst Case | O(log n) |
| Space (Iterative) | O(1) |
| Space (Recursive) | O(log n) |

---

###  Intuition

Given:
```cpp
arr = {1, 3, 5, 7, 9, 11, 13}
target = 9
```

#### Step 1

```text
1 3 5 7 9 11 13
      ↑
     mid
```

```cpp
arr[mid] = 7
```

Target > 7

Discard left half.

---

#### Step 2

```text
9 11 13
 ↑
mid
```

Found target.

---

### Standard Binary Search

### Iterative Approach

```cpp
int binarySearch(vector<int>& arr, int target) {
    int low = 0;
    int high = arr.size() - 1;

    while(low <= high) {
        int mid = low + (high - low) / 2;

        if(arr[mid] == target)
            return mid;

        if(arr[mid] < target)
            low = mid + 1;
        else
            high = mid - 1;
    }

    return -1;
}
```

---

### Recursive Approach

```cpp
int binarySearch(vector<int>& arr, int low, int high, int target) {
    if(low > high)
        return -1;

    int mid = low + (high - low) / 2;

    if(arr[mid] == target)
        return mid;

    if(arr[mid] < target)
        return binarySearch(arr, mid + 1, high, target);

    return binarySearch(arr, low, mid - 1, target);
}
```

---

## 🚨 Avoid Overflow

❌

```cpp
int mid = (low + high) / 2;
```

May overflow when indices are large.

✅

```cpp
int mid = low + (high - low) / 2;
```

---

### STL Functions

#### Binary Search

```cpp
binary_search(begin(arr), end(arr), target);
```

Returns:

```cpp
true / false
```

---

#### Lower Bound

```cpp
lower_bound(begin(arr), end(arr), target)
```

Returns iterator to first element:

```cpp
>= target
```

---

#### Upper Bound

```cpp
upper_bound(begin(arr), end(arr), target)
```

Returns iterator to first element:

```cpp
> target
```

---

### Common Mistakes

#### Forgetting Sorted Condition
Binary Search requires:

```text
Sorted Data
```

---

#### Infinite Loop

❌

```cpp
low = mid;
high = mid;
```

Always move beyond mid:

✅

```cpp
low = mid + 1;
high = mid - 1;
```

---

#### Overflow in Mid Calculation

❌

```cpp
(low + high) / 2
```

✅

```cpp
low + (high - low) / 2
```

---

#### Wrong Search Space
Before coding, define:

```text
1. Low
2. High
3. Answer
4. Monotonic Condition
```

Especially in Binary Search on Answer.

---

### Binary Search Template

```cpp
int low = ...;
int high = ...;

while(low <= high) {
    int mid = low + (high - low) / 2;

    if(condition(mid)) {
        high = mid - 1;
    }
    else {
        low = mid + 1;
    }
}
```

---

### Key Takeaways
- Works only on sorted or monotonic data.
- Reduces search space by half every iteration.
- Time Complexity = O(log n).
- Lower Bound → First >= target.
- Upper Bound → First > target.
- Binary Search on Answer is one of the most important interview patterns.
- Always think in terms of a monotonic condition.

---
Problems on Binary Search - [[Problems on Binary Search]]