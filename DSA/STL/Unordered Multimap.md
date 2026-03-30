An **`unordered_multimap`** in the **C++ [[STL (Standard Template Library)]]** is an associative container that stores elements as **key–value pairs**.

- **Keys are NOT unique** → multiple elements can have the **same key**.
- Unlike `multimap`, an `unordered_multimap` **does NOT keep elements sorted**.
- It internally uses a **hash table** for storage.
- Therefore, the **average time complexity** of insertion, deletion, and search operations is **O(1)**.

### Syntax

```cpp
unordered_multimap <object_type_of_key, object_type_of_value> variable_name;
```

#### Example

```cpp
unordered_multimap<int, int> mpp;
unordered_multimap<string, int> mpp;
unordered_multimap<int, pair<int, int>> mpp;
```

---

#### Functions in unordered_multimap:

- insert()
  Inserts a key–value pair into the unordered_multimap.  
  Duplicate keys are **allowed**.

  Syntax

```cpp
mpp.insert({key, value});
```

Example

```cpp
unordered_multimap<int, string> mpp;
mpp.insert({1, "Apple"});
mpp.insert({1, "Banana"}); // allowed
```

- begin()
  Returns an iterator pointing to the **first element**. (Note: Order is **not guaranteed**.)

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
  Removes all elements from the unordered_multimap.

  Syntax

  ```cpp
  mpp.clear();
  ```

- find()
  Returns an iterator to **one occurrence** of the key. If not found, returns `mpp.end()`.

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
  Removes an element by key or by iterator.

  Syntax

  ```cpp
  mpp.erase(iterator);
  mpp.erase(key);
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
  Returns the number of elements in the unordered_multimap.

  Syntax

  ```cpp
  mpp.size();
  ```

- empty()
  Checks whether the unordered_multimap is empty.

  Syntax

  ```cpp
  mpp.empty();
  ```

  Example

```cpp
if(mpp.empty()) {
    cout << "Map is empty";
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
auto range = mpp.equal_range(1);
for (auto it = range.first; it != range.second; it++) {
    cout << it->second << " ";
}
```

- bucket_count()
  Returns the **number of buckets** in the hash table.

  Example

```cpp
mpp.bucket_count();
```

- load_factor()
  Returns the **average number of elements per bucket**.

  Example

```cpp
mpp.load_factor();
```

