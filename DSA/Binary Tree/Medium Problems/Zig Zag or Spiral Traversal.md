**Problem**: Given the `root` of a binary tree, return _the zigzag level order traversal of its nodes' values_. (i.e., from left to right, then right to left for the next level and alternate between).

**Example**:
![[Zig Zag or Spiral Traversal.png|233]]
**Input**: `root = [3,9,20,null,null,15,7]`
**Output**: `[[3],[20,9],[15,7]]`

[Visit Leetcode](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

---
### Optimal Solution
By Doing a level Order Traversal - [[Level Order Traversal (BFS)]]

**Time Complexity**: O(n)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
	vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
		vector<vector<int>> ans;
		
		if (root == nullptr) {
			return ans;
		}
		
		queue<TreeNode *> q;
		q.push(root);
		
		bool flagLeftToRight = true;
		
		while (!q.empty()) {
			int size = q.size();
			vector<int> level(size);
			
			for (int i = 0; i < size; i++) {
				TreeNode *node = q.front();
				q.pop();
				
				int index = (flagLeftToRight) ? i : size - i - 1;
				
				level[index] = node->val;
				
				if (node->left != nullptr) {
					q.push(node->left);
				}
				
				if (node->right != nullptr) {
					q.push(node->right);
				}
			}
			
			flagLeftToRight = !flagLeftToRight;
			ans.push_back(level);
		}
		
		return ans;
	}
};
```

---
> [!tip]
> We are using O(n) space for returning the answer