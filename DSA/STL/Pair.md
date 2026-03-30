A **pair** in the [[STL (Standard Template Library)]] is a container that stores **two values together as a single unit**.

- The two values may be of **different data types**.
- The first value is accessed using **`first`** and the second using **`second`**.
- Pairs are commonly used in **maps, priority queues, graphs, and competitive programming**.

---

### Syntax

```cpp
pair <data_type1, data_type2> variable_name;
```

#### Example

```cpp
pair<int, int> p;
pair<int, string> p;
pair<int, pair<int, int>> p;
```

---

#### **Creating and Initializing a Pair**

### Method 1: Direct Assignment

```cpp
pair<int, int> p = {1, 2};
```

### Method 2: Using make_pair()

```cpp
pair<int, int> p = make_pair(1, 2);
```

---

## Accessing Elements of a Pair

- `first`  
  Accesses the **first element** of the pair.
- `second`  
  Accesses the **second element** of the pair.

### Example

```cpp
pair<int, int> p = {10, 20};

cout << p.first;   // Output: 10
cout << p.second;  // Output: 20
```

---

## Nested Pair

A pair can contain another pair.

### Example

```cpp
pair<int, pair<int, int>> p = {1, {2, 3}};

cout << p.first;            // 1
cout << p.second.first;     // 2
cout << p.second.second;    // 3

```

---

## Pair in Array

Pairs are often used in arrays for storing two related values.

#### Example

```cpp
pair<int, int> arr[3];

arr[0] = {1, 2};
arr[1] = {3, 4};
arr[2] = {5, 6};

cout << arr[1].second;   // Output: 4
```

---

## Swapping Pairs

Pairs can be swapped using `swap()`.

#### Syntax

```cpp
swap(p1, p2);
```

### Example

```cpp
pair<int, int> p1 = {1, 2};
pair<int, int> p2 = {3, 4};

swap(p1, p2);

// p1 = {3,4}
// p2 = {1,2}
```

---

## Comparing Pairs

Pairs are compared **lexicographically**:

1. Compare `first`
2. If `first` is equal, compare `second`

### Example

```cpp
pair<int, int> p1 = {1, 5};
pair<int, int> p2 = {2, 3};

cout << (p1 < p2);   // Output: 1 (true)
```

Because `1 < 2`;

---

## Useful Functions with Pair

#### 1. make_pair()

Creates a pair without specifying types explicitly.

```cpp
auto p = make_pair(10, 20);
```

---

#### 2. swap()

Swaps two pairs.

```cpp
swap(p1, p2);
```

---

## Example Program

```cpp
#include <iostream>
using namespace std;

int main() {
    pair<int, int> p = {5, 10};

    cout << p.first << endl;
    cout << p.second << endl;

    pair<int, pair<int, int>> q = {1, {2, 3}};

    cout << q.second.first;
}
```

---

