**Problem**: Given the `root` of a binary tree, return _the level order traversal of its nodes' values_. (i.e., from left to right, level by level).

**Note**: A **level order traversal** is a breadth-first search (BFS) of the tree. It visits nodes level by level, starting from the root, and processes all nodes from left to right within each level before moving to the next.

**Example**:
![[Level Order Traversal (BFS).png|226]]

**Input**: `root = [3,9,20,null,null,15,7]`
**Output**: `[[3],[9,20],[15,7]]`

[Visit Leetcode](https://leetcode.com/problems/binary-tree-level-order-traversal/)
[Visit GFG](https://www.geeksforgeeks.org/problems/level-order-traversal/1)

---
### By using a Queue - [[DSA/STL/Queue|Queue]]

**Time Complexity**: O(n)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
	vector<vector<int>> levelOrder(TreeNode* root) {
		vector<vector<int>> ans;
		
		if (root == nullptr) {
			return ans;
		}
		
		queue<TreeNode *> q;
		q.push(root);
		
		while (!q.empty()) {
			vector<int> level;
			int size = q.size();
			
			for (int i = 0; i < size; i++) {
				TreeNode *node = q.front();
				q.pop();
				
				if (node->left != nullptr) q.push(node->left);
				if (node->right != nullptr) q.push(node->right);
				
				level.push_back(node->val);
			}
			
			ans.push_back(level);
		}
		
		return ans;
	}
};
```

> [!tip]
> We are using O(n) space for returning the ans

---