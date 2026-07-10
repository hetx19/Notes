A **Binary Tree** is a hierarchical data structure in which each node has **at most two children**:
- Left Child
- Right Child

Unlike a Binary Search Tree (BST), a Binary Tree **does not follow any ordering rule**.

---

#### Structure
![[Binary Tree.png|415]]

Each node contains:
```cpp
struct TreeNode {
	int val;
	TreeNode *left;
	TreeNode *right;
	
	TreeNode() : val(0), left(nullptr), right(nullptr) {}
	
	TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
	
	TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
```

---

#### Terminology

```
Root      → Topmost node
Parent    → Node having children
Child     → Descendant of a parent
Leaf      → Node with no children
Edge      → Connection between two nodes
Height    → Longest path from node to leaf
Depth     → Distance from root to node
Level     → Nodes having same depth
Subtree   → Tree rooted at a node
```

---

#### Example

```
        1
      /   \
     2     3
    / \     \
   4   5     6
```

Here,

```
Root = 1
Leaves = 4, 5, 6
Height = 2
Number of Nodes = 6
Number of Edges = 5
```

---

### Types of Binary Trees

#### Full Binary Tree

Every node has either:
- 0 children
- 2 children

![[Full Binary Tree.png|310]]

---

#### Perfect Binary Tree
- Every internal node has 2 children.
- All leaf nodes are at the same level.

![[Perfect Binary Tree.png|319]]

Properties

Nodes = 2<sup>(h+1)</sup> - 1
Leaves = 2<sup>h</sup>

---

#### Complete Binary Tree
- Every level is completely filled except possibly the last.
- Last level is filled from left to right.

![[Complete Binary Tree.png|247]]

Used in:
```
Heap
Priority Queue
```

---

#### Balanced Binary Tree
For every node,

```
|Height(Left) - Height(Right)| <= 1
```

![[Balanced Binary Tree.png|498]]

Examples:

```
AVL Tree
Red-Black Tree
```

---

#### Degenerate (Skewed) Tree
Every node has only one child.

![[Skewed tree.png|353]]

Time complexity becomes:
```
O(n)
```

instead of
```
O(log n)
```

---

### Binary Tree Traversals

There are two major categories.

```
DFS (Depth First Search)
1. Preorder
2. Inorder
3. Postorder
   
BFS (Breadth First Search)
1. Level Order
```

---

### Preorder Traversal

```
Root → Left → Right
```

Example
```
        1
      /   \
     2     3
    / \ 
   4   5
```

Output

```
1 2 4 5 3
```

Code - [[Pre Order Traversal (DFS)]]

---

### Inorder Traversal

```
Left → Root → Right
```

Output
```
4 2 5 1 3
```

Code - [[In Order Traversal (DFS)]]

---

### Postorder Traversal

```
Left → Right → Root
```

Output
```
4 5 2 3 1
```

Code - [[Post Order Traversal (DFS)]]

---

### Level Order Traversal (BFS)
Uses a queue.
```
Level by Level
```

Output

```
1 2 3 4 5 6
```

Code - [[Level Order Traversal (BFS)]]

---

### Time Complexity

|Traversal|Time|Space|
|---|---|---|
|Preorder|O(n)|O(h)|
|Inorder|O(n)|O(h)|
|Postorder|O(n)|O(h)|
|Level Order|O(n)|O(n)|

Where,
```
n = Number of Nodes
h = Height of Tree
```

---

### Height of Binary Tree

Definition
```
Height = Number of edges on the longest path from root to a leaf.
```

Recursive Solution - [[Maximum Depth in Binary Tree]]

---

### Counting Nodes

```cpp
class Solution {
  public:
	int countNodes(TreeNode* root) {
		if(root == nullptr) {
			return 0;
		}
	
		return 1 + countNodes(root->left) + countNodes(root->right);
	}
};
```

Time
```
O(n)
```

---

### Counting Leaf Nodes

```cpp
class Solution {
  public:
	int countLeaves(TreeNode* root) {
		if(root == nullptr) {
			return 0;
		}
		
		if(root->left == nullptr && root->right == nullptr) {
			return 1;
		}
		
		return countLeaves(root->left) + countLeaves(root->right);
	}
};
```

---

### Common Mistakes

#### Forgetting Base Case

❌

```
preorder(root->left);
```

without checking
```
root == nullptr
```

Always write
```cpp
if(root == nullptr) {
	return;
}
```

---

#### Confusing Height Definition
Some problems define height as:

```
Number of Edges
```

Others define it as:

```
Number of Nodes
```

Read the problem carefully.

---

#### Wrong Traversal Order

```
Preorder  → Root Left Right
Inorder   → Left Root Right
Postorder → Left Right Root
```

---

#### Stack Overflow
Recursive DFS may overflow for highly skewed trees.
Use iterative traversal when needed.

---

### Binary Tree Template

```cpp

void dfs(TreeNode* root) {
	if(root == nullptr) {
		return;    // Process current node
	}
	
	dfs(root->left);
	dfs(root->right);
}
```

---

### Key Takeaways
- Every node has at most **two children**.
- Binary Trees have **no ordering property**.
- DFS includes **Preorder, Inorder, and Postorder** traversals.
- BFS is performed using **Level Order Traversal** with a queue.
- Most Binary Tree problems are solved using **recursion**.
- Traversals generally take **O(n)** time.
- Recursive DFS uses **O(h)** auxiliary space, where **h** is the height of the tree.
- Clearly define whether height is measured in **edges** or **nodes** before implementing.

---
### Problems On Binary Tree - [[Problems on Binary Tree]]