A **priority queue** in the [[STL (Standard Template Library)]] is a container adaptor that stores elements such that the **highest priority element** is always at the top.
- By default, it is a **Max Heap**
- Largest element is always at the top
- Implemented using a **Heap**
- Internally uses a **vector**
- Does not support iterators
- Time Complexity:
    - Push: **O(log n)**
    - Pop: **O(log n)**
    - Top: **O(1)**

### Syntax

```cpp
priority_queue<object_type> pq;
```

#### Example

```cpp
priority_queue<int> pq;
priority_queue<string> pq;
```

---

### Max Heap (Default)
Elements are arranged such that the largest element remains at the top.

```cpp
priority_queue<int> pq;

pq.push(10);
pq.push(30);
pq.push(20);

cout << pq.top(); // 30
```

Heap Representation:

```text
      30
     /  \
   10    20
```

---

### Min Heap
A min heap can be created using `greater<>`.

#### Syntax

```cpp
priority_queue<int, vector<int>, greater<int>> pq;
```

#### Example

```cpp
priority_queue<int, vector<int>, greater<int>> pq;

pq.push(10);
pq.push(30);
pq.push(20);

cout << pq.top(); // 10
```

---

#### Functions in priority_queue:
- push()  
    Inserts an element into the priority queue.
    
    Syntax

```cpp
pq.push(value);
```

Example

```cpp
priority_queue<int> pq;

pq.push(10);
pq.push(20);
pq.push(30);
```

---

- pop()  
    Removes the top element from the priority queue.
    
    Syntax

```cpp
pq.pop();
```

Example

```cpp
priority_queue<int> pq;

pq.push(10);
pq.push(20);
pq.push(30);

pq.pop();

cout << pq.top(); // 20
```

---

- top()  
    Returns the element with the highest priority.
    
    Syntax

```cpp
pq.top();
```

Example

```cpp
priority_queue<int> pq;

pq.push(10);
pq.push(50);
pq.push(30);

cout << pq.top(); // 50
```

---

- size()  
    Returns the number of elements in the priority queue.
    
    Syntax

```cpp
pq.size();
```

Example

```cpp
priority_queue<int> pq;

pq.push(10);
pq.push(20);

cout << pq.size(); // 2
```

---

- empty()  
    Checks whether the priority queue is empty.
    
    Syntax

```cpp
pq.empty();
```

Example

```cpp
priority_queue<int> pq;

if (pq.empty()) {
    cout << "Priority Queue is empty";
}
```

---

- swap()  
    Swaps the contents of two priority queues.
    
    Syntax

```cpp
pq1.swap(pq2);
```

Example

```cpp
priority_queue<int> pq1;
priority_queue<int> pq2;

pq1.push(10);
pq1.push(20);

pq2.push(100);

pq1.swap(pq2);

cout << pq1.top(); // 100
```

---

##### Common Priority Queue Operations

```cpp
priority_queue<int> pq;

// Insert Elements
pq.push(10);
pq.push(30);
pq.push(20);

// Largest Element
cout << pq.top(); // 30

// Remove Largest
pq.pop();

// New Largest
cout << pq.top(); // 20

// Size
cout << pq.size(); // 2

// Check Empty
if (pq.empty()) {
    cout << "Empty";
}
```

---

##### Traversing a Priority Queue
Since priority queue does not support iterators, traversal is usually done by creating a copy.

```cpp
priority_queue<int> pq;

pq.push(10);
pq.push(30);
pq.push(20);

priority_queue<int> temp = pq;

while (!temp.empty()) {
    cout << temp.top() << " ";
    temp.pop();
}
```

Output

```text
30 20 10
```

---

##### Less Common (But Useful) Functions

- emplace()  
    Inserts an element by constructing it in-place.
    
    Syntax    

```cpp
pq.emplace(value);
```

Example

```cpp
priority_queue<pair<int, int>> pq;

pq.emplace(1, 2);
```

- Uses less copying than `push()`.

---

##### Priority Queue of Pairs
By default, pairs are compared lexicographically.

```cpp
priority_queue<pair<int, int>> pq;

pq.push({2, 100});
pq.push({5, 10});
pq.push({5, 20});

cout << pq.top().first << " " << pq.top().second;
```

Output

```text
5 20
```

Explanation:
- Compare first element
- If equal, compare second element

---

##### Custom Min Heap of Pairs

```cpp
priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
```

---

##### Important Notes
- Default priority queue is a **Max Heap**.
- Smallest element can be accessed first using a **Min Heap**.
- No random access (`pq[0]` is invalid).
- No iterators (`begin()`, `end()` are not available).
- Internally implemented using a **Heap Data Structure**.
- Underlying container is usually `vector`.

---

##### Applications of Priority Queue
- Dijkstra's Algorithm - [[Dijkstra's Algo]]
- Prim's Algorithm - [[Minimum Spanning Tree (MST)]]
- Huffman Coding
- CPU Scheduling
- Event Simulation
- Task Scheduling
- K Largest / K Smallest Elements
- Merge K Sorted Arrays
- Median in Data Stream Problems