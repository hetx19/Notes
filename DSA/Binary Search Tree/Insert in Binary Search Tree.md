**Problem**: You are given the `root` node of a binary search tree (BST) and a `value` to insert into the tree. Return _the root node of the BST after the insertion_. It is **guaranteed** that the new value does not exist in the original BST.

**Notice** that there may exist multiple valid ways for the insertion, as long as the tree remains a BST after insertion. You can return **any of them**.

**Example**:
![[Insert in Binary Search Tree.png|586]]
**Input**: `root = [4,2,7,1,3], val = 5`
**Output**: `[4,2,7,1,3,5]`

[Visit Leetcode](https://leetcode.com/problems/insert-into-a-binary-search-tree/)
[Visit GFG](https://www.geeksforgeeks.org/problems/insert-a-node-in-a-bst/1)

---
### Brute Force Solution
By using recursion - [[Recursion]]

**Time Complexity**: O(log n)
**Space Complexity**: O(h)

```cpp
class Solution {
  public:
	TreeNode* insertIntoBST(TreeNode* root, int val) {
		if (root == nullptr) {
			return new TreeNode(val);
		}
		
		if (key < root->val) {
	        root->left = insert(root->left, key);
	    } else {
	        root->right = insert(root->right, key);
	    }
	    
	    return root;
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
	TreeNode* insertIntoBST(TreeNode* root, int val) {
		TreeNode *temp = new TreeNode(val)
		if (root == nullptr) {
			return temp;
		}
		
		TreeNode *current = root;
		
		while (current != nullptr) {
	        if (current->val > key && current->left != nullptr) {
	            current = current->left;
	        } else if (current->val < key && current->right != nullptr) {
	            current = current->right;
	        } else {
			    break;
			}
	    }

		if (current->data > key) {
	        current->left = temp;
	    } else {
	        current->right = temp;
	    }
	    
	    return root;
	}
};
```

---