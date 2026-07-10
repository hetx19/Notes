**Problem**: Given the `root` of a binary tree, return _the length of the **diameter** of the tree_.

The **diameter** of a binary tree is the **length** of the longest path between any two nodes in a tree. This path may or may not pass through the `root`.

The **length** of a path between two nodes is represented by the number of edges between them.

**Example**:
![[Diameter of Binary Tree.png|283]]

**Input**: `root = [1,2,3,4,5]`
**Output**: 3
**Explanation**: `3 is the length of the path [4,2,1,3] or [5,2,1,3]`.

[Visit Leetcode](https://leetcode.com/problems/diameter-of-binary-tree/)
[Visit GFG](https://www.geeksforgeeks.org/problems/diameter-of-binary-tree/1)

---

### Brute Force Solution
By measuring depth of left and right side for every node

```
Total Width = 1 + Depth of Left Side + Depth of Right Side
```

**Time Complexity**: O(n<sup>2</sup>)
**Space Complexity**: O(h)

```cpp
class Solution {
  private:
	int height(TreeNode* node) {
	    if (node == nullptr) {
		    return 0;
		}
		
	    return 1 + max(height(node->left), height(node->right));
	}

  public:
	int diameterOfBinaryTree(TreeNode* root) {
	    if (root == nullptr) {
		    return 0;
		}

	    int left = height(root->left);
	    int right = height(root->right);

	    int throughRoot = left + right;

	    int leftDiameter = diameterOfBinaryTree(root->left);
	    int rightDiameter = diameterOfBinaryTree(root->right);

	    return max({throughRoot, leftDiameter, rightDiameter});
	}
};
```

---

### Optimal Solution
By using a DFS Traversal

```cpp
class Solution {
  private:
	int getHeight(TreeNode *node, int& diameter) {
		if (node == nullptr) {
			return 0;
		}
		
		int leftHeight = getHeight(node->left, diameter);
		int rightHeight = getHeight(node->right, diameter);
		
		diameter = max(diameter, leftHeight + rightHeight);
		
		return 1 + max(leftHeight, rightHeight);
	}
	
  public:
	int diameterOfBinaryTree(TreeNode *root) {
		int diameter = 0;
		getHeight(root, diameter);
		return diameter;
	}
};
```

---
