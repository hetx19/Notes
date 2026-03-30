# What is STL?

STL (Standard Template Library) is a built-in library in C++ that provides ready-to-use data structures and algorithms.

It is implemented using **templates**, which makes it generic and reusable for different data types.

The STL helps programmers write efficient and clean code without implementing common data structures and algorithms from scratch.

---

# Components of STL

STL mainly consists of:

1. Containers
2. Algorithms
3. Iterators
4. Functors
5. Utility

---

# 1️⃣ Containers

Containers are data structures used to store and organize data.
Each container is implemented as a **template class** and provides member functions for insertion, deletion, searching, and traversal.
Containers are classified into four types:

---

## 1. Sequence Containers

Store elements in a linear sequence.

- Vector - [[Vector]]
- Deque - [[Deque]]
- List - [[List]]
- String - [[String]]

---

## 2. Container Adaptors

Built on top of sequence containers with restricted functionality.

- Stack - [[Stack]]
- Queue - [[Queue]]
- Priority Queue - [[Priority Queue]]

---

## 3. Associative Containers

Store elements in **sorted order** (implemented using balanced trees).

- Set - [[Set]]
- Map - [[Map]]
- Multiset - [[Multiset]]
- Multimap - [[Multimap]]

---

## 4. Unordered Associative Containers

Store elements using **hash tables** (not sorted).

- Unordered Set - [[Unordered Set]]
- Unordered Map - [[Unordered Map]]
- Unordered Multiset - [[Unordered Multiset]]
- Unordered Multimap - [[Unordered Multimap]]

---

# 2️⃣ Algorithms

Most algorithms are defined in:

```cpp
#include <algorithm>
---
## 1. sort
Arranges elements in ascending order (default).

**Syntax:**
sort(first, last);
sort(first, last, comp);

**Time Complexity:** O(n log n)

---
## 2. binary_search
Searches an element in a sorted range.

**Syntax:**
binary_search(first, last, value);
binary_search(first, last, value, comp);

**Time Complexity:** O(log n)

---
## 3. find
Searches for the first occurrence of a value.
- Returns iterator to the element
- Returns `end()` if not found

**Syntax:**
auto it = find(arr.begin(), arr.end(), value);

**Time Complexity:** O(n)

---
## 4. count
Counts occurrences of a value in a range.

**Syntax:**
int cnt = count(arr.begin(), arr.end(), value);

**Time Complexity:** O(n)

---
## 5. reverse
Reverses elements in a range.
**Syntax:**
reverse(arr.begin(), arr.end());

**Time Complexity:** O(n)

---
## 6. accumulate
Computes sum of elements in a range.
Defined in:
#include <numeric>

**Syntax:**
int sum = accumulate(arr.begin(), arr.end(), 0);

**Time Complexity:** O(n)

---
## 7. unique
Removes consecutive duplicate elements.

auto it = unique(arr.begin(), arr.end());
arr.erase(it, arr.end());

**Time Complexity:** O(n)

---
## 8. lower_bound
Returns iterator to first element ≥ value (sorted range).

auto it = lower_bound(arr.begin(), arr.end(), value);

**Time Complexity:** O(log n)

---

## 9. upper_bound
Returns iterator to first element > value (sorted range).

auto it = upper_bound(arr.begin(), arr.end(), value);

**Time Complexity:** O(log n)

---

## 10. replace
Replaces all occurrences of a value.

replace(arr.begin(), arr.end(), old_value, new_value);

**Time Complexity:** O(n)

---
```

# 3️⃣ Iterators

Iterators are used to traverse containers.
They behave like pointers.

Example:

```cpp
vector<int>::iterator it;

## Types of Iterators
- Input Iterator
- Output Iterator
- Forward Iterator
- Bidirectional Iterator
- Random Access Iterator
```

---

# 4️⃣ Functors (Function Objects)

A functor is an object that behaves like a function (overloads `()` operator).

```cpp
struct Compare {
    bool operator()(int a, int b) {
        return a > b;
    }
};
```

Usage:

```cpp
sort(arr.begin(), arr.end(), Compare());
```

---

# 5️⃣ Utility

Utility components provide helper classes and functions that support STL containers and algorithms.

Most are defined in:

```cpp
#include <utility>
```

---

## 1️⃣ Pair – [[Pair]]

`pair` is a simple container that stores **two values** (possibly of different types).

It is defined as:
template <class T1, class T2>  
struct pair;

---

### 🔹 Declaration

```cpp
pair<int, int> p;
```

### 🔹 Initialization

```cpp
pair<int, int> p = {1, 2};
pair<int, int> p(1, 2);
auto p = make_pair(1, 2);
```

---

### 🔹 Accessing Elements

```cpp
cout << p.first;   // First element
cout << p.second;  // Second element

```

---

### 🔹 Example

```cpp
#include <iostream>
#include <utility>
using namespace std;

int main() {
    pair<int, string> p = {1, "STL"};
    cout << p.first << " " << p.second;
}
```

---

### 🔹 Where `pair` is Used

- In `map` and `unordered_map` (they store elements as `pair<const Key, Value>`)
- Returning multiple values from a function
- Competitive programming
- Custom sorting using `pair`

---

### 🔹 Comparing Pairs

Pairs are compared lexicographically:

```cpp
pair<int, int> p1 = {1, 2};
pair<int, int> p2 = {1, 3};

cout << (p1 < p2);   // true
```

Comparison order:

1. Compare `first`
2. If equal, compare `second`

