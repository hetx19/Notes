**Problem**: Given a binary tree of nodes 'N', you need to modify the value of its nodes, such that the tree holds the Children sum property.

A binary tree is said to follow the children sum property if, for every node of that tree, the value of that node is equal to the sum of the value(s) of all of its children nodes( left child and the right child).

**Note :**
```
 1. You can only increment the value of the nodes, in other words, the modified value must be at least equal to the original value of that node.
 2. You can not change the structure of the original binary tree.
 3. A binary tree is a tree in which each node has at most two children.      
 4. You can assume the value can be 0 for a NULL node and there can also be an empty tree.
```

**Example**:
**Input**: `root = [35, 20, 15, 15, 5, 10, 5]`
![[Children Sum Property in Binary Tree.png|300]]

---
### Optimal Solution

**Time Complexity**: O(n)
**Space Complexity**: O(h)

```cpp
class Solution {
  private:
	int helper(TreeNode* root){
		if (root == nullptr) {
			return 0;
		}
		
		int child = 0;
		
		if (root->left != nullptr) {
			child += root->left->data;
		}

		if (root->right != nullptr) {
			child += root->right->data;
		}
		
		
		if (child >= root->data) {
			root->data = child;
		} else {
			if (root->left != nullptr) {
				root->left->data = root->data;
			}
			
			if (root->right != nullptr) {
				root->right->data = root->data;
			}
		}
		
		int left = helper(root->left);
		int right = helper(root->right);
		
		if (left == 0 && right == 0) {
			return root->data;
		}
		
		if (left + right > root->data) {
			root->data = right + left;
			return root->data;
		}
	}

  public:
	void changeTree(TreeNode* root) {
		helper(root);
	}
};
```

---