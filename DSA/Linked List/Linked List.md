> [!abstract] Definition  
> A **Linked List** is a linear data structure where elements are stored in **nodes**. Each node contains data and a pointer/reference to the next node in the sequence.

Unlike arrays, linked-list nodes are **not required to occupy contiguous memory locations**.

---

## 1. Node Structure
A basic singly linked-list node in C++:

```cpp
struct Node {
    int data;
    Node* next;

    Node(int value) {
        data = value;
        next = nullptr;
    }
};
```

Each node contains:
- `data` → stores the value.
- `next` → stores the address of the next node.

Example:
```text
10 → 20 → 30 → nullptr
```

---

## 2. Creating a Linked List

```cpp
Node* head = new Node(10);

head->next = new Node(20);
head->next->next = new Node(30);
```

Result:
```text
head
 ↓
[10 | •] → [20 | •] → [30 | NULL]
```

`head` stores the address of the first node.

---

## 3. Traversing a Linked List
To visit every node:

```cpp
Node* temp = head;

while (temp != nullptr) {
    cout << temp->data << " ";
    temp = temp->next;
}
```

Output:
```text
10 20 30
```

### Complexity
- Time: **O(n)**
- Space: **O(1)**

---

## 4. Insertion

### Insert at Beginning

```cpp
Node* newNode = new Node(5);

newNode->next = head;
head = newNode;
```

Before:
```text
10 → 20 → 30
```

After:
```text
5 → 10 → 20 → 30
```

**Time: O(1)**

---

### Insert at End

```cpp
Node* newNode = new Node(40);

if (head == nullptr) {
    head = newNode;
} else {
    Node* temp = head;

    while (temp->next != nullptr) {
        temp = temp->next;
    }

    temp->next = newNode;
}
```

**Time: O(n)** without a tail pointer.
With a maintained `tail` pointer, insertion at the end can be **O(1)**.

---

### Insert After a Given Node
Suppose we want to insert `25` after `20`.

```cpp
Node* newNode = new Node(25);

newNode->next = temp->next;
temp->next = newNode;
```

Before:
```text
10 → 20 → 30
```

After:
```text
10 → 20 → 25 → 30
```

**Time: O(1)** if the target node is already known.

---

## 5. Deletion

### Delete from Beginning

```cpp
if (head != nullptr) {
    Node* temp = head;
    head = head->next;
    delete temp;
}
```

**Time: O(1)**

---

### Delete from End

```cpp
if (head == nullptr) {
    return;
}

if (head->next == nullptr) {
    delete head;
    head = nullptr;
    return;
}

Node* temp = head;

while (temp->next->next != nullptr) {
    temp = temp->next;
}

delete temp->next;
temp->next = nullptr;
```

**Time: O(n)** for a singly linked list.

---

### Delete a Node by Value

```cpp
if (head == nullptr) {
    return;
}

if (head->data == value) {
    Node* temp = head;
    head = head->next;
    delete temp;
    return;
}

Node* temp = head;

while (temp->next != nullptr &&
       temp->next->data != value) {
    temp = temp->next;
}

if (temp->next != nullptr) {
    Node* toDelete = temp->next;
    temp->next = temp->next->next;
    delete toDelete;
}
```

**Time: O(n)**

---

## 6. Searching

```cpp
bool search(Node* head, int target) {
    Node* temp = head;

    while (temp != nullptr) {
        if (temp->data == target)
            return true;

        temp = temp->next;
    }

    return false;
}
```

**Time: O(n)**

Linked lists do **not support efficient random access**.
For example, accessing the 5th element requires traversing from the head.

---

# Types of Linked Lists

## 1. Singly Linked List

Each node points only to the next node.

```text
10 → 20 → 30 → NULL
```

```cpp
struct Node {
    int data;
    Node* next;
};
```

---

## 2. Doubly Linked List

Each node contains pointers to both previous and next nodes.

```text
NULL ← 10 ⇄ 20 ⇄ 30 → NULL
```

```cpp
struct Node {
    int data;
    Node* prev;
    Node* next;
};
```

### Advantages
- Can traverse in both directions.
- Easier deletion when a node pointer is already available.
- Useful for structures such as browser history and deques.

### Disadvantage
- Requires extra memory for `prev`.

---

## 3. Circular Linked List
The last node points back to the first node.

```text
┌──────────────────────┐
↓.                     │
10 → 20 → 30 ──────────┘
```

There is no `nullptr` at the end.
Useful in applications involving **cyclic processing**, such as round-robin scheduling.

---

# Linked List vs Array

|Feature|Array|Linked List|
|---|---|---|
|Memory|Contiguous|Non-contiguous|
|Random Access|O(1)|O(n)|
|Search|O(n)|O(n)|
|Insert at Beginning|O(n)|O(1)|
|Delete at Beginning|O(n)|O(1)|
|Insert at End|O(1) amortized*|O(n)**|
|Memory Overhead|Low|Higher|
|Cache Locality|Better|Generally worse|
|Size|Usually fixed / dynamically managed|Dynamic|

* For a dynamic array such as `vector`.  
* **O(1)** if a suitable tail pointer is maintained.

---

# Complexity Summary

|Operation|Singly Linked List|
|---|---|
|Access by index|O(n)|
|Search|O(n)|
|Insert at head|O(1)|
|Delete at head|O(1)|
|Insert at tail|O(n)|
|Delete at tail|O(n)|
|Reverse|O(n)|

---

# Important C++ Concepts

## `->` Operator
When working with a pointer to a structure/class:

```cpp
Node* temp = head;

cout << temp->data;
temp = temp->next;
```

This is equivalent to:

```cpp
(*temp).data
(*temp).next
```

---

## `nullptr`

Use `nullptr` to represent a null pointer:

```cpp
Node* head = nullptr;
```

Prefer `nullptr` over the older `NULL` in modern C++.

---

## Dynamic Memory
Nodes are commonly created using:

```cpp
Node* node = new Node(10);
```

and released using:

```cpp
delete node;
```

When implementing linked lists manually, be careful to avoid:
- Memory leaks
- Dangling pointers
- Double deletion
- Losing access to the remaining list

---

# Key Takeaways

> [!tip] Remember
> 
> - A linked list stores data in **nodes connected by pointers**.
>     
> - Accessing an element by index takes **O(n)**.
>     
> - Insertion/deletion at the head takes **O(1)**.
>     
> - The **fast & slow pointer** technique is extremely important.
>     
> - Learn **iterative reversal** thoroughly.
>     
> - Always handle `nullptr` and edge cases carefully.
>     
> - `prev`, `curr`, and `next` are the core pointers used in many linked-list problems.
>     

---