> [!abstract] Overview  
> A **Singly Linked List (SLL)** is a linear data structure made up of nodes. Each node stores a value and a pointer to the **next node**.
> 
> In this note:
> - Introduction to Singly Linked List
> - Insertion at the head
> - Deletion of the head
> - Finding the length
> - Searching in a linked list

---

# 1. Introduction to Singly Linked List

A **Singly Linked List (SLL)** consists of a sequence of nodes where each node contains:
1. **Data** — the value stored in the node.
2. **Next pointer** — the address of the next node.

The last node points to `nullptr`.

```text
HEAD
 ↓
[10 | •] → [20 | •] → [30 | •] → nullptr
```

Here:
- `head` points to the first node.
- `10`, `20`, and `30` are the values stored in the nodes.
- Each node points to the next node.
- The last node points to `nullptr`.

> [!note] Important  
> Unlike an array, the nodes of a linked list **do not need to be stored in contiguous memory locations**.

---

## ListNode Structure in C++

```cpp
struct ListNode {
    int data;
    ListNode* next;

    ListNode(int value) {
        data = value;
        next = nullptr;
    }
};
```

### Explanation

```cpp
int data;
```

Stores the value of the node.

```cpp
ListNode* next;
```

Stores the address of the next node.

```cpp
next = nullptr;
```

Initially, the node does not point to another node.

---

# 2. Creating a Singly Linked List

We can dynamically create nodes using the `new` keyword.

```cpp
ListNode* head = new ListNode(10);

head->next = new ListNode(20);
head->next->next = new ListNode(30);
```

The resulting list:

```text
HEAD
 ↓
10 → 20 → 30 → nullptr
```

An empty linked list is represented by:

```cpp
ListNode* head = nullptr;
```

Which means:

```text
HEAD → nullptr
```

> [!tip] Key Point  
> `head == nullptr` means the linked list is empty.

---

# 3. Insertion at the Head

## Concept

Insertion at the head means adding a new node **before the current first node**.

Before:

```text
HEAD
 ↓
10 → 20 → 30 → nullptr
```

Suppose we want to insert `5`.

After:

```text
HEAD
 ↓
5 → 10 → 20 → 30 → nullptr
```

---

## Algorithm

To insert a node at the head:
1. Create a new node.
2. Make the new node point to the current `head`.
3. Update `head` to point to the new node.
4. Return the new `head`.

```text
Before:

HEAD
 ↓
10 → 20 → 30 → nullptr


Step 1:

newNode
   ↓
   5

HEAD
 ↓
10 → 20 → 30


Step 2:

newNode → 10 → 20 → 30
   ↓
   5


Step 3:

HEAD
 ↓
5 → 10 → 20 → 30 → nullptr
```

---

## C++ Implementation

```cpp
ListNode* insertAtHead(ListNode* head, int value) {
    ListNode* newNode = new ListNode(value);

    newNode->next = head;
    head = newNode;

    return head;
}
```

### Example

```cpp
ListNode* head = nullptr;

head = insertAtHead(head, 30);
head = insertAtHead(head, 20);
head = insertAtHead(head, 10);
```

Result:

```text
HEAD
 ↓
10 → 20 → 30 → nullptr
```

### Why do we write `head = insertAtHead(...)`?

The function creates a **new head** and returns it.

Therefore, the caller must update its `head`:

```cpp
head = insertAtHead(head, 10);
```

Simply writing:

```cpp
insertAtHead(head, 10); // ❌
```

would ignore the returned new head.

---

## Complexity

|Operation|Time|Extra Space|
|---|---|---|
|Insert at Head|**O(1)**|**O(1)**|

Insertion at the head does not require traversing the list.

> [!tip] Key Point  
> **Insertion at the head of a singly linked list is O(1).**

---

# 4. Deletion of the Head

## Concept

Deleting the head means removing the **first node** from the linked list.

Before:

```text
HEAD
 ↓
10 → 20 → 30 → nullptr
```

After deleting `10`:

```text
HEAD
 ↓
20 → 30 → nullptr
```

---

## Algorithm

To delete the head:
1. Check whether the list is empty.
2. Store the current `head` in a temporary pointer.
3. Move `head` to the next node.
4. Delete the old head.
5. Return the new `head`.

---

## C++ Implementation

```cpp
ListNode* deleteHead(ListNode* head) {
    if (head == nullptr) {
        return nullptr;
    }

    ListNode* temp = head;

    head = head->next;

    delete temp;

    return head;
}
```

### Example

```cpp
ListNode* head = new ListNode(10);

head->next = new ListNode(20);
head->next->next = new ListNode(30);

head = deleteHead(head);
```

Result:

```text
HEAD
 ↓
20 → 30 → nullptr
```

---

## Why Use a Temporary Pointer?

Consider:

```cpp
ListNode* temp = head;
head = head->next;
delete temp;
```

The temporary pointer stores the address of the old head.

```text
temp
 ↓
10 → 20 → 30
     ↑
    head
```

After:

```cpp
head = head->next;
```

we have:

```text
temp
 ↓
10     20 → 30
       ↑
      head
```

Now it is safe to delete:

```cpp
delete temp;
```

> [!warning] Important  
> Do **not** delete `head` before saving the next node.
> 
> Incorrect:
> 
> ```cpp
> delete head;
> head = head->next; // ❌ Invalid access
> ```
> 
> Correct:
> 
> ```cpp
> ListNode* temp = head;
> head = head->next;
> delete temp;
> ```

---

## Why Check `head == nullptr`?

An empty list looks like:

```text
HEAD → nullptr
```

There is no node to delete.

Therefore:

```cpp
if (head == nullptr) {
    return nullptr;
}
```

handles the empty-list case safely.

---

## Complexity

|Operation|Time|Extra Space|
|---|---|---|
|Delete Head|**O(1)**|**O(1)**|

> [!tip] Key Point  
> **Deletion of the head of a singly linked list is O(1).**

---

# 5. Find the Length of the Linked List

## Concept

The **length of a linked list** is the total number of nodes present in the list.

Example:

```text
10 → 20 → 30 → 40 → nullptr
```

Length:

```text
4
```

---

## Approach

Start from `head` and traverse the entire list.

For every node:

```text
count++
```

Continue until:

```cpp
temp == nullptr
```

---

## Algorithm
1. Initialize `count = 0`.
2. Start from `head`.
3. While the current node is not `nullptr`:
    - Increment `count`.
    - Move to the next node.
4. Return `count`.

---

## C++ Implementation

```cpp
int getLength(ListNode* head) {
    int count = 0;

    ListNode* temp = head;

    while (temp != nullptr) {
        count++;
        temp = temp->next;
    }

    return count;
}
```

---

## Example

```cpp
ListNode* head = new ListNode(10);

head->next = new ListNode(20);
head->next->next = new ListNode(30);

cout << getLength(head);
```

Output:

```text
3
```

---

## Dry Run

For:

```text
10 → 20 → 30 → nullptr
```

|`temp`|`count`|
|---|---|
|`10`|1|
|`20`|2|
|`30`|3|
|`nullptr`|Stop|

Final result:

```text
3
```

---

## Empty List

For:

```text
HEAD → nullptr
```

the function immediately stops:

```cpp
while (temp != nullptr)
```

and returns:

```text
0
```

Therefore:

```text
Length of empty linked list = 0
```

---

## Complexity

|Operation|Time|Extra Space|
|---|---|---|
|Find Length|**O(n)**|**O(1)**|

We may need to visit every node, so the time complexity is **O(n)**.

---

# 6. Search in a Linked List

## Concept

Searching means checking whether a particular value exists in the linked list.

Example:

```text
10 → 20 → 30 → 40 → nullptr
```

Search for:

```text
30
```

Traversal:

```text
10 ❌ → 20 ❌ → 30 ✅
```

The element is found.

---

## Linear Search

A singly linked list does not support direct/random access like an array.

For example, we cannot directly access:

```cpp
head[2]; // ❌
```

Instead, we must traverse the list from the beginning.

Therefore, searching is performed using **linear search**.

---

## C++ Implementation

```cpp
bool search(ListNode* head, int target) {
    ListNode* temp = head;

    while (temp != nullptr) {
        if (temp->data == target) {
            return true;
        }

        temp = temp->next;
    }

    return false;
}
```

---

## Example

```cpp
ListNode* head = new ListNode(10);

head->next = new ListNode(20);
head->next->next = new ListNode(30);

if (search(head, 20)) {
    cout << "Element found";
} else {
    cout << "Element not found";
}
```

Output:

```text
Element found
```

---

## How Search Works

For:

```text
10 → 20 → 30 → 40 → nullptr
```

Searching for `30`:

```text
Step 1:
10 == 30 ❌

Step 2:
20 == 30 ❌

Step 3:
30 == 30 ✅

Return true
```

If the value does not exist:

```text
10 → 20 → 30 → nullptr
```

Search for `50`:

```text
10 ❌
20 ❌
30 ❌
nullptr → Not found
```

The function returns:

```cpp
false
```

---

## Complexity

|Operation|Time|Extra Space|
|---|---|---|
|Search|**O(n)**|**O(1)**|

### Best Case

The target is at the first node:

```text
target → O(1)
```

### Worst Case

The target is at the last node or does not exist:

```text
target → O(n)
```

> [!tip] Key Point  
> Searching in a singly linked list is **O(n) in the worst case**.

---

# 7. Complete Basic Implementation

The following implementation contains all four operations:

```cpp
#include <iostream>
using namespace std;

struct ListNode {
    int data;
    ListNode* next;

    ListNode(int value) {
        data = value;
        next = nullptr;
    }
};

// Insert at Head
ListNode* insertAtHead(ListNode* head, int value) {
    ListNode* newNode = new ListNode(value);

    newNode->next = head;
    head = newNode;

    return head;
}

// Delete Head
ListNode* deleteHead(ListNode* head) {
    if (head == nullptr) {
        return nullptr;
    }

    ListNode* temp = head;

    head = head->next;

    delete temp;

    return head;
}

// Find Length
int getLength(ListNode* head) {
    int count = 0;

    ListNode* temp = head;

    while (temp != nullptr) {
        count++;
        temp = temp->next;
    }

    return count;
}

// Search
bool search(ListNode* head, int target) {
    ListNode* temp = head;

    while (temp != nullptr) {
        if (temp->data == target) {
            return true;
        }

        temp = temp->next;
    }

    return false;
}
```

---

# 9. Complexity Summary

|Operation|Time Complexity|Extra Space|
|---|---|---|
|Create Node|O(1)|O(1)|
|Insert at Head|**O(1)**|O(1)|
|Delete Head|**O(1)**|O(1)|
|Find Length|**O(n)**|O(1)|
|Search|**O(n)**|O(1)|

> [!important] Remember  
> **Head operations are O(1)** because we have direct access to the first node. 
> **Length and Search are O(n)** because, in the worst case, we need to traverse the entire list.

---

# 10. Important Edge Cases

Always consider the following cases when solving linked-list problems.

## Empty List

```text
HEAD → nullptr
```

- Length = `0`
- Search = `false`
- Delete head = no operation
- Insert at head = creates the first node

---

## Single Node

```text
HEAD
 ↓
10 → nullptr
```

After deleting the head:

```text
HEAD → nullptr
```

---

## Multiple Nodes

```text
10 → 20 → 30 → nullptr
```

After deleting the head:

```text
20 → 30 → nullptr
```

---
# 14. Core Takeaways

> [!success] Remember
> - A singly linked list consists of nodes connected through `next` pointers.     
> - `head` points to the first node.
> - The last node points to `nullptr`.
> - Insertion at the head takes **O(1)** time.
> - Deletion of the head takes **O(1)** time.
> - Finding the length takes **O(n)** time.
> - Searching takes **O(n)** time in the worst case.
> - When a function changes the head and returns it, always write:
> ```cpp
> head = function(head);
> ```
> - Always handle the **empty-list** case.