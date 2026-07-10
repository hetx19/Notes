**Problem**: Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return _the **root node reference** (possibly updated) of the BST_.

Basically, the deletion can be divided into two stages:

1. Search for a node to remove.
2. If the node is found, delete the node.

**Example**:
![[Delete a node in Binary Search Tree.png]]
**Input**: `root = [5,3,6,2,4,null,7], key = 3`
**Output**: `[5,4,6,2,null,null,7]`

[Visit Leetcode](https://leetcode.com/problems/delete-node-in-a-bst/)
[Visit GFG](https://www.geeksforgeeks.org/problems/delete-a-node-from-bst/1)

---
### Optimal Solution

**Time Complexity**: O(log n)
**Space Complexity**: O(log n)

```cpp
class Solution {
  private:
	TreeNode* findLastRight(TreeNode* root) {
		if (root->right == nullptr) {
			return root;
		}
		
		return findLastRight(root->right);
	}
	
	TreeNode* helper(TreeNode* root) {
		if (root->left == nullptr) {
			return root->right;
		} else if (root->right == nullptr) {
			return root->left;
		}
		
		TreeNode* rightChild = root->right;
		TreeNode* lastRightChild = findLastRight(root->left);
		lastRightChild->right = rightChild;
		
		return root->left;
	}
	
  public:
	TreeNode* deleteNode(TreeNode* root, int key) {
		if (root == nullptr) {
			return nullptr;
		}
		
		if (root->val == key) {
			return helper(root);
		}
		
		TreeNode* dummy = root;
		
		while (root != nullptr) {
			if (root->val > key) {
				if (root->left != nullptr && root->left->val == key) {
					root->left= helper(root->left);
					break;
				} else {
					root = root->left;
				}
			} else {
				if (root->right != nullptr && root->right->val == key) {
					root->right= helper(root->right);
					break;
				} else {
					root = root->right; 
				}
			}
		}
		
		return dummy;
	}
};
```

---