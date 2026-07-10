**Problem**: Given a binary tree, determine if it is **height-balanced**.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

**Example**:
![[Maximum Depth in Binary Tree.png|241]]

**Input**: `root = [3,9,20,null,null,15,7]`
**Output**: tree

[Visit Leetcode](https://leetcode.com/problems/balanced-binary-tree/description/)
[Visit GFG](https://www.geeksforgeeks.org/problems/check-for-balanced-tree/1)

---
### Brute Force Solution
By calculating difference of left and right height for every sub tree

**Time Complexity**: O(n<sup>2</sup>)
**Space Complexity**: O(N)

```cpp
class Solution {
  private:
	int getHeight(TreeNode* node) {
		if (node == nullptr) {
			return 0;
		}
		
		int leftHeight = getHeight(node->left);
		int rightHeight = getHeight(node->right);
		
		return 1 + max(leftHeight, rightHeight);
	}
	
  public:
	bool isBalanced(TreeNode* root) {
		if (root == nullptr) {
			return true;
		}

		int leftHeight = getHeight(root->left);
		int rightHeight = getHeight(root->right);
		
		if (abs(leftHeight - rightHeight) <= 1 && isBalance(root->left) && isBalance(root->right)) {
			return true;
		}
		
		return false;
	}
};
```

---
### By doing a Level Order Traversal - [[Level Order Traversal (BFS)]]

**Time Complexity**: O(n)
**Space Complexity**: O(h)

```cpp
class Solution {
  private:
	int dfsHeight(TreeNode* node) {
		if (root == nullptr) {
			return 0;
		}
		
		int leftHeight = dfsHeight(root->left);
		
		if (leftHeight == -1) {
			return -1;
		}
		
		int rightHeight = dfsHeight(root->right);
		
		if (rightHeight == -1) {
			return -1;
		}
		
		if (abs(leftHeight - rightHeight) > 1) {
			return -1;
		}
		
		return 1 + max(leftHeight, rightHeight);
	}
  public:
	bool isBalanced(TreeNode* root) {
		return dfsHeight(root) != -1;
	}
};
```
---