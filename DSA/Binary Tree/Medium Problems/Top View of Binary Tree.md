**Problem**: You are given the **root** of a binary tree, and your task is to return its **top view**. The top view of a binary tree is the set of nodes visible when the tree is viewed from the top.

**Note:**

- Return the nodes from the leftmost node to the rightmost node.
- If multiple nodes overlap at the same horizontal position, only the topmost (closest to the root) node is included in the view.

**Example**:
**Input**: `root = [1, 2, 3]`
**Output**: `[2, 1, 3]`
**Explanation**: The Green colored nodes represents the top view in the below Binary tree
![[Top View of Binary Tree.png|243]]

[Visit GFG](https://www.geeksforgeeks.org/problems/top-view-of-binary-tree/1)

---

### Optimal Solution
By doing a BFS Traversal - [[Level Order Traversal (BFS)]]

**Time Complexity**: O(n)
**Space Complexity**: O(n / 2) + O(n / 2)

```cpp
class Solution {
  public:
	vector<int> topView(TreeNode *root) {
		vector<int> ans;
		
		if (root == nullptr) {
			return ans;
		}
		
		map<int, int> mpp;
		queue<pair<TreeNode *, int>> q;
		q.push({root, 0});
		
		while (!q.empty()) {
			auto it = q.front();
			q.pop();
			TreeNode *node = it.first;
			
			int line = it.second;
			
			if (mpp.find(line) == mpp.end()) {
				mpp[line] = node->val;
			}
			
			if (node->left != nullptr) {
				q.push({node->left, line - 1});
			}
			
			if (node->right != nullptr) {
				q.push({node->right, line + 1});
			}
		}
		
		for (auto &it : mpp) {
			ans.push_back(it.second);
		}
		
		return ans;
	}
};
```

---