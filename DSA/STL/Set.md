A **set** in the [[STL (Standard Template Library)]] is an associative container that stores **unique elements** in **sorted order**.

- Stores **only keys (no values)**
- Elements are **unique**
- Implemented using a **Red-Black Tree**
- Elements are sorted in **ascending order by default**
- Time Complexity: **O(log n)** for insert, delete, search

### Syntax

```cpp
set<object_type> s;
```

#### Example

```cpp
set<int> s;
set<string> s;
```

---

#### Functions in set:

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
