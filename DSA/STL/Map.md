A **map** in the [[STL (Standard Template Library)]] is an associative container that stores elements as **key–value pairs**.

- Each **key** must be unique.
- A `map` maintains its elements in a **self-balancing binary search tree** (typically a **Red-Black Tree**), which keeps the keys sorted in ascending order by default.
- Therefore, the time complexity of insertion, deletion, and search operations in a `map` is **O(log n)**.

### Syntax

```cpp
map <object_type_of_key, object_type_of_value> variable_name;
```

#### Example

```cpp
map<int, int> mpp;
map<string, int> mpp;
map<pair<int, int>, int> mpp;
```

---

#### Functions in map:

- insert()
  Inserts a key–value pair into the map.  
  If the key already exists, insertion fails.

  Syntax

```cpp
  mpp.insert({key, value});
```

Example

```cpp
map<int, string> mpp;
mpp.insert({1, "Apple"});
mpp.insert({2, "Banana"});
mpp[1] = "New Value";  // Updates value if key already exists
```

- begin()
  Returns an iterator pointing to the first (smallest key) element.

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
  Removes all elements from the map.

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
  Returns the number of elements in the map.

  Syntax

  ```cpp
  mpp.size();
  ```

- empty()
  Checks whether the map is empty.

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

- lower_bound()
  Returns an iterator pointing to:
  The first element **whose key is greater than or equal to (`>=`) the given key**.
  If no such element exists, it returns `mpp.end()`.
  Time Complexity: O(log n)

  Syntax

  ```cpp
  mpp.lower_bound(key);
  ```

  Example

```cpp
map<int, string> mpp;
mpp[10] = "A";
mpp[20] = "B";
mpp[30] = "C";

auto it = mpp.lower_bound(20);
cout << it->first;   // Output: 20

auto it2 = mpp.lower_bound(25);
cout << it2->first;  // Output: 30
```

- upper_bound()
  Returns an iterator pointing to:
  The first element **whose key is strictly greater than (`>`) the given key**.
  If no such element exists, it returns `mpp.end()`.
  Time Complexity: O(log n)

  Syntax

  ```cpp
  mpp.upper_bound(key);
  ```

  Example

```cpp
map<int, string> mpp;
mpp[10] = "A";
mpp[20] = "B";
mpp[30] = "C";

auto it = mpp.upper_bound(20);
cout << it->first;   // Output: 30
```

---

##### Less Common (But Useful) Functions

- cbegin() / cend()
  Return constant iterators (cannot modify elements).

- rbegin() / rend()
  Return reverse iterators (iterate in descending order of keys).

- emplace()
  Inserts an element more efficiently by constructing it in-place.

- max_size()
  Returns the maximum number of elements the map can hold.
