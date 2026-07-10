**Problem**: You are given the **root** of a binary tree, and your task is to return its **bottom view**. The bottom view of a binary tree is the set of nodes visible when the tree is viewed from the bottom.

**Note:** If there are **multiple** bottom-most nodes for a horizontal distance from the root, then the **latter** one in the level order traversal is considered.

**Example**:
**Input**: `[1, 2, 3, 4, 5, N, 6]`
**Output**: `[4, 2, 5, 3, 6]`
**Explanation**: The Green colored nodes represents the top view in the below Binary tree

![[Bottom View of Binary Tree.png|260]]

[Visit GFG](https://www.geeksforgeeks.org/problems/bottom-view-of-binary-tree/1)

---

### Optimal Solution
By doing a BFS Traversal - [[Level Order Traversal (BFS)]]

**Time Complexity**: O(n)
**Space Complexity**: O(n / 2) + O(n / 2)

```cpp
class Solution {
  public:
	vector<int> bottomView(TreeNode *root) {
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
			mpp[line] = node->val;
			
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