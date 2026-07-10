**Problem**: Given the **root** of a Binary Search Tree, find the **minimum element** in this given BST.

**Example**:
**Input**: `root = [5, 4, 6, 3, N, N, 7, 1]`
![[Find Minimum in Binary Search Tree.png|257]]
**Output**: 1

[Visit GFG](https://www.geeksforgeeks.org/problems/minimum-element-in-bst/1)

---
### Brute Force Solution
By Storing inorder traversal

**Time Complexity**: O(n)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	void inOrderTraversal(TreeNode* node, vector<int>& inOrder) {
		if (node == nullptr) {
			return;
		}
		
		inOrder(node->left, inOrder);
		inOrder.push_back(node->val);
		inOrder(node->right, inOrder);
	}
	
  public:
    int minValue(TreeNode* root) {
	    vector<int> inOrder;
	    inOrderTraversal(root, inOrder);
	    
	    return (inOrder.size() > 0) ? inOrder[0] : -1;
    }
};
```

---
### Better Approach
By using Recursion - [[Recursion]]

**Time Complexity**: O(log n)
**Space Complexity**: O(h)

```cpp
class Solution {
  public:
	int minValue(TreeNode* root) {
		if (root->left != nullptr) {
			return root->val;
		}
		
		return minValue(root->left);
	}
};
```

---
### Optimal Solution

**Time Complexity**: O(log n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int minValue(TreeNode* root) {
		if (root == nullptr) {
	        return -1;
	    }

	    TreeNode* current = root;
    
	    while (current->left != nullptr) {
	        current = current->left;
	    }

	    return current->val;
	}
};
```

---