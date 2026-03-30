A **list** in the [[STL (Standard Template Library)]] is a **doubly linked list** container.

- Elements are **not stored in contiguous memory**.
- Each element stores **data + pointers to next and previous elements**.
- Efficient insertion and deletion **anywhere in the list**.
- Does **not support random access**.

### Time Complexity

- Insertion: **O(1)**
- Deletion: **O(1)**
- Access: **O(n)**

---

### Syntax

```cpp
list<data_type> variable_name;
```

### Example

```cpp
list<int> ls;
list<int> ls(5); // List with size 5
list<int> ls(5, 100); // List with 5 elements, each initialized to 100
list<pair<int,int>> ls;
```

---

### Functions in List

### 1. push_back()

Adds an element **at the end** of the list.

#### Syntax

```cpp
ls.push_back(value);
```

#### Example

```cpp
list<int> ls;
ls.push_back(10);
ls.push_back(20);
```

---

### 2. push_front()

Adds an element **at the beginning** of the list.

#### Syntax

```cpp
ls.push_front(value);
```

#### Example

```cpp
list<int> ls;
ls.push_front(10);
ls.push_front(20);
```

---

### 3. emplace_back()

Constructs and inserts element **at the end**.

#### Syntax

```cpp
ls.emplace_back(value);
```

#### Example

```cpp
list<int> ls;
ls.emplace_back(30);
```

---

### 4. emplace_front()

Constructs and inserts element **at the beginning**.

#### Syntax

```cpp
ls.emplace_front(value);
```

#### Example

```cpp
list<int> ls;
ls.emplace_front(30);
```

---

### 5. pop_back()

Removes the **last element**.

#### Syntax

```cpp
ls.pop_back();
```

---

### 6. pop_front()

Removes the **first element**.

#### Syntax

```cpp
ls.pop_front();
```

---

### 7. begin()

Returns iterator to **first element**.

#### Syntax

```cpp
ls.begin();
```

---

### 8. end()

Returns iterator **after last element**.

#### Syntax

```cpp
ls.end();
```

---

### 9. size()

Returns the **number of elements** in the list.

#### Syntax

```cpp
ls.size();
```

---

### 10. empty()

Checks whether the list is empty.

#### Syntax

```cpp
ls.empty();
```

#### Example

```cpp
if(ls.empty()) {
    cout << "list is empty";
}
```

---

### 11. clear()

Removes **all elements** from the list.

#### Syntax

```cpp
ls.clear();
```

---

### 12. insert()

Inserts element at specific position.

#### Syntax

```cpp
ls.insert(iterator, value);
```

---

### 13. erase()

Removes elements from a list.

#### Syntax

```cpp
ls.erase(iterator);
ls.erase(start_iterator, end_iterator);
```

#### Example

```cpp
auto it = ls.begin();
advance(it, 1);
ls.erase(it);
```

---

### 14. reverse()

Reverses the list.

#### Syntax

```cpp
ls.reverse();
```

---

### 15. sort()

Sorts the list.

#### Syntax

```cpp
ls.sort();
```

---

### Less Common (But Useful) Functions

- `splice()` – transfers elements from another list
- `merge()` – merges two sorted lists
- `remove()` – removes elements with a specific value
- `unique()` – removes consecutive duplicate elements

