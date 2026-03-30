A **vector** in the [[STL (Standard Template Library)]] is a **dynamic array** that can automatically **resize itself** when elements are inserted or removed.

- Elements are stored in **contiguous memory locations**.
- Supports **random access** (can access any element using index).
- Size increases automatically when new elements are inserted.
- Insertion at the **end is efficient**.

### Time Complexity

- Access: **O(1)**
- Insertion at end: **O(1) amortized**
- Insertion in middle: **O(n)** (due to shifting elements)

---

### Syntax

```cpp
vector<data_type> variable_name;
```

### Example

```cpp
vector<int> v;
vector<int> v(5); // Vector with size 5
vector<int> v(5, 100); // Vector with 5 elements, each initialized to 100
vector<pair<int,int>> v;
```

---

### Functions in Vector

### 1. push_back()

Adds an element **at the end** of the vector.

#### Syntax

```cpp
v.push_back(value);
```

#### Example

```cpp
vector<int> v;
v.push_back(10);
v.push_back(20);
```

---

### 2. emplace_back()

Inserts an element **at the end** (more efficient than push_back).

#### Syntax

```cpp
v.emplace_back(value);
```

#### Example

```cpp
vector<int> v;
v.emplace_back(30);
```

---

### 3. begin()

Returns an iterator pointing to the **first element**.

#### Syntax

```cpp
v.begin();
```

#### Example

```cpp
auto it = v.begin();
cout << *it;
```

---

### 4. end()

Returns an iterator pointing **just after the last element**.

#### Syntax

```cpp
v.end();
```

#### Example

```cpp
for(auto it = v.begin(); it != v.end(); it++) {
    cout << *it;
}
```

---

### 5. size()

Returns the **number of elements** in the vector.

#### Syntax

```cpp
v.size();
```

---

### 6. empty()

Checks whether the vector is empty.

#### Syntax

```cpp
v.empty();
```

#### Example

```cpp
if(v.empty()) {
    cout << "Vector is empty";
}
```

---

### 7. clear()

Removes **all elements** from the vector.

#### Syntax

```cpp
v.clear();
```

---

### 8. pop_back()

Removes the **last element**.

#### Syntax

```cpp
v.pop_back();
```

---

### 9. front()

Returns the **first element**.

#### Syntax

```cpp
v.front();
```

---

### 10. back()

Returns the **last element**.

#### Syntax

```cpp
v.back();
```

---

### 11. insert()

Inserts an element at a **specific position**.

#### Syntax

```cpp
v.insert(iterator, value);
```

#### Example

```cpp
vector<int> v = {1,2,3};

v.insert(v.begin()+1, 10);
// vector becomes: 1 10 2 3
```

---

### 12. erase()

Removes elements from a vector.

#### Syntax

```cpp
v.erase(iterator);
v.erase(start_iterator, end_iterator);
```

#### Example

```cpp
v.erase(v.begin()+1);
```

---

### 13. sort()

Sorts the elements of the vector in **ascending order** by default.

#### Syntax

```cpp
sort(start_iterator, end_iterator);
```

#### Example

```cpp
int main() {
	vector<int> v = {5, 2, 8, 1, 3};
	sort(v.begin(), v.end());

	for(int x : v) {
		cout << x << " ";
    }
    // Output: 1 2 3 5 8

    // To sort in descending order
    sort(v.begin(), v.end(), greater<int>());

    for(int x : v) {
		cout << x << " ";
    }
}
```

---

### 14. lower_bound()

Returns an iterator pointing to the **first element that is greater than or equal to (`>=`) the given value**.
⚠️ The vector **must be sorted**.

#### Syntax

```cpp
lower_bound(start_iterator, end_iterator, value);
```

#### Example

```cpp
int main() {
	vector<int> v = {1, 3, 5, 7, 9};

	auto it = lower_bound(v.begin(), v.end(), 5);
	cout << *it;   // Output: 5

	// Another example:
	auto it = lower_bound(v.begin(), v.end(), 6);
	cout << *it;   // Output: 7
}
```

---

### 15. upper_bound()

Returns an iterator pointing to the **first element strictly greater than (`>`) the given value**.
⚠️ The vector **must be sorted**.

#### Syntax

```cpp
upper_bound(start_iterator, end_iterator, value);
```

#### Example

```cpp
int main() {
	vector<int> v = {1, 3, 5, 7, 9};
	auto it = upper_bound(v.begin(), v.end(), 5);
	cout << *it;   // Output: 7
}
```

---

### Less Common (But Useful) Functions

- `cbegin()` / `cend()` – constant iterators
- `rbegin()` / `rend()` – reverse iterators
- `resize()` – changes vector size
- `capacity()` – returns allocated memory size
- `swap()` – swaps two vectors

