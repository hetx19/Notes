A **queue** in the [[STL (Standard Template Library)]] is a container adaptor that follows the **FIFO (First In, First Out)** principle.
- Elements are inserted at the **rear (back)**
- Elements are removed from the **front**
- First inserted element is the first one to be removed
- Implemented internally using **deque** by default
- Does not support iterators
- Time Complexity:
    - Push: **O(1)**
    - Pop: **O(1)**
    - Front: **O(1)**
    - Back: **O(1)**

### Syntax

```cpp
queue<object_type> q;
```

#### Example

```cpp
queue<int> q;
queue<string> q;
```

---

#### Functions in queue:
- push()  
    Inserts an element at the rear of the queue.
    
    Syntax

```cpp
q.push(value);
```

Example

```cpp
queue<int> q;

q.push(10);
q.push(20);
q.push(30);
```

Queue:

```text
Front -> 10 20 30 <- Rear
```

---

- pop()  
    Removes the front element from the queue.
    
    Syntax    

```cpp
q.pop();
```

Example

```cpp
queue<int> q;

q.push(10);
q.push(20);
q.push(30);

q.pop();
```

Queue:

```text
Front -> 20 30 <- Rear
```

---

- front()  
    Returns the front element of the queue.
    
    Syntax    

```cpp
q.front();
```

Example

```cpp
queue<int> q;

q.push(10);
q.push(20);

cout << q.front(); // 10
```

---

- back()  
    Returns the last element of the queue.
    
    Syntax    

```cpp
q.back();
```

Example

```cpp
queue<int> q;

q.push(10);
q.push(20);

cout << q.back(); // 20
```

---

- size()  
    Returns the number of elements in the queue.
    
    Syntax    

```cpp
q.size();
```

Example

```cpp
queue<int> q;

q.push(10);
q.push(20);

cout << q.size(); // 2
```

---

- empty()  
    Checks whether the queue is empty.
    
    Syntax

```cpp
q.empty();
```

Example

```cpp
queue<int> q;

if (q.empty()) {
    cout << "Queue is empty";
}
```

---

- swap()  
    Swaps the contents of two queues.
    
    Syntax

```cpp
q1.swap(q2);
```

Example

```cpp
queue<int> q1;
queue<int> q2;

q1.push(10);
q1.push(20);

q2.push(100);

q1.swap(q2);

cout << q1.front(); // 100
cout << q2.front(); // 10
```

---

##### Common Queue Operations

```cpp
queue<int> q;

// Insert Elements
q.push(10);
q.push(20);
q.push(30);

// Access Front Element
cout << q.front(); // 10

// Access Rear Element
cout << q.back(); // 30

// Remove Front Element
q.pop();

// New Front
cout << q.front(); // 20

// Size
cout << q.size(); // 2

// Check Empty
if (q.empty()) {
    cout << "Empty";
}
```

---

##### Traversing a Queue
Since queue does not support iterators, traversal is usually done by creating a copy.

```cpp
queue<int> q;

q.push(10);
q.push(20);
q.push(30);

queue<int> temp = q;

while (!temp.empty()) {
    cout << temp.front() << " ";
    temp.pop();
}
```

Output

```text
10 20 30
```

---

##### Less Common (But Useful) Functions
- emplace()  
    Inserts an element at the rear by constructing it in-place.
    
    Syntax

```cpp
q.emplace(value);
```

Example

```cpp
queue<pair<int, int>> q;

q.emplace(1, 2);
```

- Uses less copying than `push()`.

---

##### Important Notes
- Queue follows **FIFO (First In, First Out)**.
- Elements are inserted at the rear and removed from the front.
- Only the front and rear elements can be accessed directly.
- No random access (`q[0]` is invalid).
- No iterators (`begin()`, `end()` are not available).
- Default underlying container is `deque`.
- Can also be implemented using `list`.

Example

```cpp
queue<int, list<int>> q;
```

---

##### Applications of Queue
- CPU Scheduling
- Printer Queue Management
- Task Scheduling
- Breadth First Search (BFS) - [[BFS]]
- Request Handling in Servers
- Message Queues
- Simulation Systems
- Order Processing Systems