**Problem**: Given the `root` of a binary tree, return _its maximum depth_.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

**Example**:
![[Maximum Depth in Binary Tree.png|199]]

**Input**: `root = [3,9,20,null,null,15,7]`
**Output**: 3

[Visit Leetcode](https://leetcode.com/problems/maximum-depth-of-binary-tree/)
[Visit GFG](https://www.geeksforgeeks.org/problems/height-of-binary-tree/1)

---
### By using Recursion - [[Recursion]]

**Time Complexity**: O(n)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
	int maxDepth(TreeNode* root) {
		if (root == nullptr) {
			return 0;
		}

		int leftHeight = maxDepth(root->left);
		int rightHeight = maxDepth(root->right);
		
		return 1 + max(leftHeight, rightHeight);
	}
};
```

---
### By doing a Level Order Traversal - [[Level Order Traversal (BFS)]]

**Time Complexity**: O(n)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
	int maxDepth(TreeNode* root) {
		if (root == nullptr) {
			return 0;
		}

		queue<TreeNode *> q;
		q.push(root);
		int depth = 0;
		
		while (!q.empty()) {
			int n = q.size();
			
			for (int i = 0; i < n; i++) {
				TreeNode *temp = q.front();
				q.pop();
				
				if (temp->left != nullptr) {
					q.push(temp->left);
				}
				
				if (temp->right != nullptr) {
					q.push(temp->right);
				}
			}
			
			depth++;
		}
		
		return depth;
	}
};
```