**Problem**: Given the `root` of a binary tree, _determine if it is a valid binary search tree (BST)_.

A **valid BST** is defined as follows:

- The left subtree of a node contains only nodes with keys **strictly less than** the node's key.
- The right subtree of a node contains only nodes with keys **strictly greater than** the node's key.
- Both the left and right subtrees must also be binary search trees.

**Example**:
![[Validate Binary Search Tree.png]]
**Input**: `root = [2,1,3]`
**Output**: true

[Visit Leetcode](https://leetcode.com/problems/validate-binary-search-tree/)
[Visit GFG](https://www.geeksforgeeks.org/problems/check-for-bst/1)

---
### Optimal Solution
By checking range for Every Node

**Time Complexity**: O(n)
**Space Complexity**: O(h)

```cpp
class Solution {
  private:
	bool validBST(TreeNode* root, long long minValue, long long maxValue) {
		if (root == nullptr) {
			return true;
		}
		
		if (root->val >= maxValue || root->val <= minValue) {
			return false;
		}
		
		return (validBST(root->left, minValue, root->val) && validBST(root->right, root->val, maxValue));
	}
	
  public:
	bool isValidBST(TreeNode* root) {
		return validBST(root, LLONG_MIN, LLONG_MAX);
	}
};
```

---