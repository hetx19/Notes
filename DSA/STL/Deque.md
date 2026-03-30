A **deque** (`double-ended queue`) in the [[STL (Standard Template Library)]] is a sequence container that allows **insertion and deletion from both ends efficiently**.

- Elements are stored in **non-contiguous memory (like list, but in blocks)**
- Supports **random access (like vector)**
- Faster than vector for **front operations**

---

## 🔹 Key Features

- Dynamic size
- Efficient insertion/removal at **front and back**
- Supports **random access (`[]`, `at()`)**
- Implemented as **multiple fixed-size arrays (blocks) managed dynamically**

---

## ⏱️ Time Complexity

Access → O(1)
push_front/back → O(1)
pop_front/back → O(1)
insertion middle → O(n)
Deletion middle → O(n)

---

## 🔹 Syntax

```cpp
deque<data_type> variable_name;
```

---

## 🔹 Examples

```cpp
deque<int> dq;
deque<int> dq(5);            // size 5
deque<int> dq(5, 100);       // 5 elements, all 100
deque<pair<int,int>> dq;
```

---

## 🔹 Functions in Deque

### 1. push_back()

Adds an element **at the end**.

#### Syntax

```cpp
dq.push_back(value);
```

#### Example

```cpp
deque<int> dq;
dq.push_back(10);
dq.push_back(20);
```

---

### 2. push_front()

Adds an element **at the beginning**.

#### Syntax

```cpp
dq.push_front(value);
```

#### Example

```cpp
deque<int> dq;
dq.push_front(10);
dq.push_front(20);
```

---

### 3. emplace_back()

Constructs and inserts element **at the end**.

#### Syntax

```cpp
dq.emplace_back(value);
```

#### Example

```cpp
deque<int> dq;
dq.emplace_back(30);
```

---

### 4. emplace_front()

Constructs and inserts element **at the beginning**.

#### Syntax

```cpp
dq.emplace_front(value);
```

#### Example

```cpp
deque<int> dq;
dq.emplace_front(30);
```

---

### 5. pop_back()

Removes the **last element**.

#### Syntax

```cpp
dq.pop_back();
```

---

### 6. pop_front()

Removes the **first element**.

#### Syntax

```cpp
dq.pop_front();
```

---

### 7. front()

Access first element.

#### Syntax

```cpp
dq.front();
```

---

### 8. back()

Access last element

#### Syntax

```cpp
dq.back();
```

---

### 9. begin() / end()

Iterators to traverse

#### Syntax

```cpp
auto it1 = dq.begin();
auto it2 = dq.end();
```

---

### 10. size()

Returns number of elements

#### Syntax

```cpp
dq.size();
```

---

### 11. empty()

Checks whether the deque is empty.

#### Syntax

```cpp
dq.empty();
```

#### Example

```cpp
if(dq.empty()) {
    cout << "deque is empty";
}
```

---

### 12. clear()

Removes **all elements** from the deque.

#### Syntax

```cpp
dq.clear();
```

---

### 13. insert()

Inserts element at specific position.

#### Syntax

```cpp
dq.insert(iterator, value);
```

---

### 14. erase()

Removes elements from deque.

#### Syntax

```cpp
dq.erase(iterator);
dq.erase(start_iterator, end_iterator);
```

#### Example

```cpp
auto it = dq.begin();
advance(it, 1);
dq.erase(it);
```

---

### 15. at()

Access element with bounds checking

#### Syntax

```cpp
dq.at(index);
```

---

## 🔹 Less Common (Useful)

- `resize()` – change size
- `swap()` – swap two deques
- `assign()` – assign new values
