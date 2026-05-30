A **stack** in the [[STL (Standard Template Library)]] is a container adaptor that follows the **LIFO (Last In, First Out)** principle.
- Elements are inserted and removed only from the **top**    
- Last inserted element is the first one to be removed
- Implemented internally using **deque** by default
- Does not support iterators
- Time Complexity:
    - Push: **O(1)**
    - Pop: **O(1)**
    - Top: **O(1)**

### Syntax

```cpp
stack<object_type> st;
```

#### Example

```cpp
stack<int> st;
stack<string> st;
```

---

#### Functions in stack:
- push()  
    Inserts an element at the top of the stack.
    
    Syntax

```cpp
st.push(value);
```

Example

```cpp
stack<int> st;

st.push(10);
st.push(20);
st.push(30);
```

Stack:

```text
Top
30
20
10
```

---

- pop()  
    Removes the top element from the stack.
    
    Syntax    

```cpp
st.pop();
```

Example

```cpp
stack<int> st;

st.push(10);
st.push(20);
st.push(30);

st.pop();
```

Stack:

```text
Top
20
10
```
---

- top()  
    Returns the top element of the stack.
    
    Syntax

```cpp
st.top();
```

Example

```cpp
stack<int> st;

st.push(10);
st.push(20);

cout << st.top(); // 20
```

---

- size()  
    Returns the number of elements in the stack.
    
    Syntax    

```cpp
st.size();
```

Example

```cpp
stack<int> st;

st.push(10);
st.push(20);

cout << st.size(); // 2
```

---

- empty()  
    Checks whether the stack is empty.
    
    Syntax    

```cpp
st.empty();
```

Example

```cpp
stack<int> st;

if (st.empty()) {
    cout << "Stack is empty";
}
```

---

- swap()  
    Swaps the contents of two stacks.
    
    Syntax    

```cpp
st1.swap(st2);
```

Example

```cpp
stack<int> st1;
stack<int> st2;

st1.push(10);
st1.push(20);

st2.push(100);

st1.swap(st2);

cout << st1.top(); // 100
cout << st2.top(); // 20
```

---

##### Common Stack Operations

```cpp
stack<int> st;

// Insert Elements
st.push(10);
st.push(20);
st.push(30);

// Access Top Element
cout << st.top(); // 30

// Remove Top Element
st.pop();

// Current Top
cout << st.top(); // 20

// Size of Stack
cout << st.size(); // 2

// Check Empty
if (st.empty()) {
    cout << "Empty";
}
```

---

##### Traversing a Stack
Since stack does not support iterators, traversal is usually done by creating a copy.


```cpp
stack<int> st;

st.push(10);
st.push(20);
st.push(30);

stack<int> temp = st;

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
    Inserts an element at the top by constructing it in-place.

```cpp
st.emplace(value);
```

Example

```cpp
stack<pair<int, int>> st;
st.emplace(1, 2);
```

- Uses less copying than `push()`.

---

##### Important Notes
- Stack follows **LIFO (Last In, First Out)**.
- Only the top element can be accessed directly.
- No random access (`st[0]` is invalid).
- No iterators (`begin()`, `end()` are not available).
- Default underlying container is `deque`.
- Can also be implemented using `vector` or `list`.

Example

```cpp
stack<int, vector<int>> st1;
stack<int, list<int>> st2;
```

---

##### Applications of Stack
- Function Call Management (Call Stack) - [[Recursion]]
- Undo / Redo Operations
- Browser History
- Expression Evaluation
- Parentheses Matching
- DFS (Depth First Search) - [[DFS]]
- Backtracking Algorithms
- Infix to Postfix / Prefix Conversion