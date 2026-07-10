### Definition
A **Binary Search Tree (BST)** is a special type of binary tree in which each node follows the **BST property**:

- All nodes in the **left subtree** have values **less than** the root.
- All nodes in the **right subtree** have values **greater than** the root.
- Both left and right subtrees are also Binary Search Trees.

---

### Properties

- Each node has at most **two children**.
- Left child < Parent < Right child.
- No duplicate values (in the standard BST).
- Searching, insertion, and deletion depend on tree height.
- Inorder traversal of a BST always gives elements in **sorted order**.

---

### Structure
![[BST.png|353]]

---

### Terminology

- **Root:** Topmost node.
- **Parent:** Node having child nodes.
- **Child:** Node connected below a parent.
- **Leaf Node:** Node with no children.
- **Internal Node:** Node having at least one child.
- **Subtree:** Tree formed by any node and its descendants.
- **Height:** Longest path from root to leaf.
- **Depth:** Distance from root to a node.

---

### Operations on BST

#### 1. Searching

##### Algorithm

1. Compare key with root.
2. If equal, return node.
3. If key < root, move left.
4. If key > root, move right.
5. Repeat until found or NULL.
##### Time Complexity

- Best: O(1)
- Average: O(log n)
- Worst: O(n)

---

### 2. Insertion

##### Steps

- Insert first node as root.
- Compare with current node.
- Move left if smaller.
- Move right if greater.
- Insert when NULL is reached.
##### Complexity

- Best: O(1)
- Average: O(log n)
- Worst: O(n)

---

### 3. Deletion
Deletion is the most important BST operation.

#### Case 1: Leaf Node

Delete 20

Before

```
     30
    /
   20
```

After

```
     30
```

---

## Case 2: Node with One Child

Delete 60

Before

```
      70
     /
    60
      \
      65
```

After

```
      70
     /
    65
```

---

## Case 3: Node with Two Children

Replace with:
- Inorder Successor (smallest in right subtree)
OR
- Inorder Predecessor (largest in left subtree)

Example

Delete 50

Before

```
       50
      /  \
    30    70
         /  \
       60   80
```

Replace by inorder successor (60)

After

```
       60
      /  \
    30    70
            \
            80
```

### Complexity

- Best: O(1)
- Average: O(log n)
- Worst: O(n)

---

### Traversals

#### 1. Inorder
Left → Root → Right

Example
```
        50
       /  \
     30    70
    / \    / \
   20 40 60 80
```

Output
```
20 30 40 50 60 70 80
```

Sorted order

---

#### 2. Preorder
Root → Left → Right

Output
```
50 30 20 40 70 60 80
```

---

#### 3. Postorder
Left → Right → Root

Output
```
20 40 30 60 80 70 50
```

---

#### 4. Level Order
Visit level by level.

Output
```
50 30 70 20 40 60 80
```

---

### Time Complexity

| Operation | Average | Worst |
|-----------|----------|--------|
| Search | O(log n) | O(n) |
| Insert | O(log n) | O(n) |
| Delete | O(log n) | O(n) |
| Traversal | O(n) | O(n) |

---

### Space Complexity

| Operation | Space |
|-----------|-------|
| Search | O(1) |
| Insert (Recursive) | O(h) |
| Delete (Recursive) | O(h) |
| Traversal | O(h) |

where **h = height of tree**

---

### Advantages

- Fast searching.
- Fast insertion.
- Fast deletion.
- Data remains sorted.
- Inorder traversal produces sorted output.
- Dynamic data structure (no fixed size).

---

### Disadvantages

- Can become skewed.
- Worst-case performance becomes O(n).
- More memory than arrays due to pointers.
- Balancing may be required.

---

### Applications

- Database indexing.
- Dictionary implementation.
- Symbol tables.
- File system organization.
- Searching applications.
- Auto-complete systems.
- Routing tables.
- Memory management.
- Priority searching.

---

### Difference: Binary Tree vs Binary Search Tree

| Binary Tree | Binary Search Tree |
|-------------|--------------------|
| No ordering rule | Left < Root < Right |
| Search is O(n) | Search is O(log n) average |
| Inorder not sorted | Inorder always sorted |
| General-purpose tree | Efficient searching tree |

---

### Example BST
```
             50
           /    \
         30      70
        /  \    /  \
      20   40  60   80
     /           \
   10            65
```

### Inorder
```
10 20 30 40 50 60 65 70 80
```

### Preorder
```
50 30 20 10 40 70 60 65 80
```

### Postorder
```
10 20 40 30 65 60 80 70 50
```

### Level Order
```
50 30 70 20 40 60 80 10 65
```

---

### Problems on Binary Search Tree - [[Problems on Binary Search Tree]]