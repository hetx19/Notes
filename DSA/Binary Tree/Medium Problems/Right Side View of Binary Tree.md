**Problem**: Given the `root` of a binary tree, imagine yourself standing on the **right side** of it, return _the values of the nodes you can see ordered from top to bottom_.

**Example**:
**Input**: `root = [1,2,3,null,5,null,4]`
**Output**: `[1,3,4]`
**Explanation**:
![[Right Side View of Binary Tree.png|345]]

[Visit Leetcode](https://leetcode.com/problems/binary-tree-right-side-view/)
[Visit GFG](https://www.geeksforgeeks.org/problems/right-view-of-binary-tree/1)

---
### Optimal Solution

**Time Complexity**: O(n)
**Space Complexity**: O(h) + (Recursive Stack Space)

```cpp
class Solution {
  private:
	void rightDFS(TreeNode *node, int level, vector<int>& ans) {
		if (node == nullptr) {
			return;
		}
		
		if (ans.size() == level) {
			ans.push_back(node->val);
		}
		
		rightDFS(node->right, level + 1, ans);
		rightDFS(node->left, level + 1, ans);
	}
	
  public:
	vector<int> rightSideView(TreeNode* root) {
		vector<int> ans;
		rightDFS(root, 0, ans);
		return ans;
	}
};
```

---

---