A **`multiset`** in the **C++ [[STL (Standard Template Library)]]** is an associative container that stores elements in **sorted order**.
- Unlike `set`, a `multiset` allows **duplicate elements**.    
- Elements are automatically sorted in **ascending order** by default.
- It is typically implemented using a **balanced binary search tree (Red-Black Tree)**.
- Since elements are stored in sorted order:
    - insertion
    - deletion
    - search  
    operations take:

```text
O(log n)
```

- Duplicate values are stored adjacent to each other.    

---

### Syntax

```cpp
multiset<object_type> variable_name;
```

#### Example

```cpp
multiset<int> ms;
multiset<string> ms;
multiset<pair<int, int>> ms;
```

---

### Functions in multiset

#### 1. insert()
Inserts an element into the multiset.
Duplicate values are allowed.

##### Syntax

```cpp
ms.insert(value);
```

##### Example

```cpp
multiset<int> ms;

ms.insert(10);
ms.insert(10);
ms.insert(20);
```

Contents:

```text
10 10 20
```

---

#### 2. begin()
Returns an iterator pointing to the first (smallest) element.

##### Syntax

```cpp
ms.begin();
```

##### Example

```cpp
auto it = ms.begin();
cout << *it;
```

---

#### 3. end()
Returns an iterator pointing just after the last element.

##### Syntax

```cpp
ms.end();
```

##### Example

```cpp
for(auto it = ms.begin(); it != ms.end(); it++) {
    cout << *it << " ";
}
```

---

#### 4. clear()
Removes all elements from the multiset.

##### Syntax

```cpp
ms.clear();
```

---

#### 5. find()
Returns iterator to the first occurrence of the given value.
If value does not exist, returns `ms.end()`.

##### Syntax

```cpp
ms.find(value);
```

##### Example

```cpp
if(ms.find(10) != ms.end()) {
    cout << "Found";
}
```

---

#### 6. erase()
Removes elements from the multiset.

##### Syntax

```cpp
ms.erase(value);      // removes ALL occurrences
ms.erase(iterator);   // removes ONE occurrence
```

##### Example

```cpp
ms.erase(10);
```

Removes all `10`s.

---

##### Remove Single Occurrence

```cpp
auto it = ms.find(10);

if(it != ms.end()) {
    ms.erase(it);
}
```

---

#### 7. size()
Returns total number of elements.

##### Syntax

```cpp
ms.size();
```

---

#### 8. empty()
Checks whether the multiset is empty.

##### Syntax

```cpp
ms.empty();
```

##### Example

```cpp
if(ms.empty()) {
    cout << "Multiset is empty";
}
```

---

### Less Common (But Useful) Functions

#### emplace()
Constructs and inserts element efficiently.

##### Example

```cpp
ms.emplace(50);
```

---

#### upper_bound()
Returns iterator to the first element greater than the given value.

##### Example

```cpp
auto it = ms.upper_bound(10);
```

---

#### lower_bound()
Returns iterator to the first element greater than or equal to the given value.

##### Example

```cpp
auto it = ms.lower_bound(10);
```

---

#### count()
Returns number of occurrences of a value.

##### Example

```cpp
cout << ms.count(10);
```

---

#### equal_range()
Returns a pair of iterators representing the range of duplicate elements.

##### Example

```cpp
auto range = ms.equal_range(10);

for(auto it = range.first; it != range.second; it++) {
    cout << *it << " ";
}
```

---

### Traversing a Multiset

#### Using Range-Based Loop

```cpp
for(auto x : ms) {
    cout << x << " ";
}
```

---

#### Using Iterators

```cpp
for(auto it = ms.begin(); it != ms.end(); it++) {
    cout << *it << " ";
}
```

---

### Difference Between set and multiset

|Feature|set|multiset|
|---|---|---|
|Duplicate Elements|❌ No|✅ Yes|
|Sorted|✅ Yes|✅ Yes|
|Unique Elements|✅ Yes|❌ No|
|Internal Structure|Red-Black Tree|Red-Black Tree|

---

### Time Complexity

|Operation|Complexity|
|---|---|
|Insert|O(log n)|
|Erase|O(log n)|
|Find|O(log n)|
|Count|O(log n + frequency)|

---

### Important Notes

#### Elements are Immutable
You cannot modify elements directly.

Wrong:

```cpp
*it = 100;
```

Because changing values may break ordering.

---

#### Duplicate Elements Stay Sorted

Example:

```cpp
multiset<int> ms = {5,1,5,2};
```

Stored as:

```text
1 2 5 5
```

---

### Multiset in Descending Order

```cpp
multiset<int, greater<int>> ms;
```

Example Output:

```text
9 7 5 3 1
```

---

# Summary

|Feature|Value|
|---|---|
|Ordered|✅|
|Duplicates Allowed|✅|
|Random Access|❌|
|Internal Structure|Red-Black Tree|
|Complexity|O(log n)|

`multiset` is extremely useful when you need:
- sorted data
- duplicate values
- efficient insertion/deletion/search