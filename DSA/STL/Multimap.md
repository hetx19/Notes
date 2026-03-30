A **`multimap`** in the **C++ [[STL (Standard Template Library)]]** is an associative container that stores elements as **key–value pairs**.

- **Keys are NOT unique** → multiple elements can have the **same key**.
- Unlike `unordered_map`, a `multimap` **keeps elements sorted** based on keys.
- It is typically implemented using a **balanced binary search tree (like Red-Black Tree)**.
- Elements with the same key are stored **adjacent to each other**, but their internal order is **not strictly guaranteed** (implementation-dependent).
- Therefore, the **time complexity** of insertion, deletion, and search operations is **O(log n)**.

### Syntax

```cpp
multimap <object_type_of_key, object_type_of_value> variable_name;
```

#### Example

```cpp
multimap<int, int> mpp;
multimap<string, int> mpp;
multimap<int, pair<int, int>> mpp;
```

---

#### Functions in multimap:

- insert()
  Inserts a key–value pair into the multimap.  
  Duplicate keys are **allowed**.

  Syntax

```cpp
mpp.insert({key, value});
```

Example

```cpp
multimap<int, string> mpp;
mpp.insert({1, "Apple"});
mpp.insert({1, "Banana"}); // allowed
// ❌ Not allowed
mpp[1] = "Apple";  // ERROR
```

- begin()
  Returns an iterator pointing to the **first (smallest key)** element.

  Syntax

  ```cpp
  mpp.begin();
  ```

  Example

```cpp
auto it = mpp.begin();
cout << it->first << " " << it->second;
```

- end()
  Returns an iterator pointing **just after** the last element.

  Syntax

  ```cpp
  mpp.end();
  ```

  Example

```cpp
for (auto it = mpp.begin(); it != mpp.end(); it++) {
    cout << it->first << " ";
}
```

- clear()
  Removes all elements from the multimap.

  Syntax

  ```cpp
  mpp.clear();
  ```

- find()
  Returns an iterator to the **first element with the given key (in sorted order)**. If not found, returns `mpp.end()`.

  Syntax

  ```cpp
  mpp.find(key);
  ```

  Example

```cpp
if(mpp.find(2) != mpp.end()) {
    cout << "Found";
}
```

- erase()
  removes **all elements** with that key.

  Syntax

  ```cpp
  mpp.erase(key);      // removes ALL elements with that key
  mpp.erase(iterator); // removes ONLY that specific element
  ```

  Example

```cpp
mpp.erase(2);              // erase by key

auto it = mpp.find(10);
if (it != mpp.end()) {
    mpp.erase(it);         // erase by iterator
}
```

- size()
  Returns the number of elements in the multimap.

  Syntax

  ```cpp
  mpp.size();
  ```

- empty()
  Checks whether the multimap is empty.

  Syntax

  ```cpp
  mpp.empty();
  ```

  Example

```cpp
if(mpp.empty()) {
    cout << "Multimap is empty";
}
```

---

##### Less Common (But Useful) Functions

- emplace()
  Inserts an element **more efficiently** by constructing it in place.

  Example

  ```cpp
  mpp.emplace(1, "Apple");
  ```

- upper_bound()
  Returns iterator to the **first element > key**.

  Example

```cpp
auto it = mpp.upper_bound(2);
```

- lower_bound()
  Returns iterator to the **first element ≥ key**.

  Example

```cpp
auto it = mpp.lower_bound(2);
```

- count()
  Returns the **number of elements** with a given key.

  Example

```cpp
cout << mpp.count(1); // may be > 1
```

- equal_range()
  Returns a pair of iterators representing the range of elements with a given key.

  Example

```cpp
auto range = mpp.equal_range(key);
for (auto it = range.first; it != range.second; it++) {
    cout << it->second;
}
```
