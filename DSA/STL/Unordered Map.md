A **`unordered_map`** in the **C++ [[STL (Standard Template Library)]]** is an associative container that stores elements as **key–value pairs**.

- Each **key must be unique**.
- Unlike `map`, an `unordered_map` **does not keep elements sorted**.
- It internally uses a **hash table** for storage.
- Therefore, the **average time complexity** of insertion, deletion, and search operations is **O(1)**.
- In the **worst case**, the complexity can degrade to **O(n)** due to hash collisions.

### Syntax

```cpp
unordered_map <object_type_of_key, object_type_of_value> variable_name;
```

#### Example

```cpp
unordered_map<int, int> mpp;
unordered_map<string, int> mpp;
unordered_map<pair<int, int>, int> mpp; // requires custom hash
```

---

#### Functions in unordered_map:

- insert()
  Inserts a key–value pair into the unordered_map.  
  If the key already exists, insertion fails.

  Syntax

```cpp
  mpp.insert({key, value});
```

Example

```cpp
unordered_map<int, string> mpp;
mpp.insert({1, "Apple"});
mpp.insert({2, "Banana"});
```

- begin()
  Returns an iterator pointing to the **first element** in the unordered map.  
  (Note: Order is **not guaranteed**.)

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
  Removes all elements from the unordered_map.

  Syntax

  ```cpp
  mpp.clear();
  ```

- find()
  Returns an iterator to the element with the given key. If not found, returns `mpp.end()`.

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
  Returns the number of elements in the unordered_map.

  Syntax

  ```cpp
  mpp.size();
  ```

- empty()
  Checks whether the unordered_map is empty.

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
  Returns **1 if the key exists**, otherwise **0**.

  Example

```cpp
if(mpp.count(5)) {
    cout << "Key exists";
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
