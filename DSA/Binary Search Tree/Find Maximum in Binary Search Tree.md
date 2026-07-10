**Problem**: Given the **root** of a Binary Search Tree, find the **maximum element** in this given BST.

**Example**:
**Input**: `root = [5, 4, 6, 3, N, N, 7, 1]`
![[Find Minimum in Binary Search Tree.png|257]]
**Output**: 7

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
	    
	    int n = inOrder.size();
	    
	    return (n > 0) ? inOrder[n - 1] : -1;
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
    
	    while (current->right != nullptr) {
	        current = current->right;
	    }

	    return current->val;
	}
};
```

---