**Problem**: Given the `root` of a binary tree, imagine yourself standing on the **left side** of it, return _the values of the nodes you can see ordered from top to bottom_.

**Example**:
**Input**: `root = [1,2,3,null,5,null,4]`
**Output**: `[1,2,5]`
**Explanation**:
![[Right Side View of Binary Tree.png|345]]

[Visit GFG](https://www.geeksforgeeks.org/problems/left-view-of-binary-tree/1)

---
### Optimal Solution

**Time Complexity**: O(n)
**Space Complexity**: O(h) + (Recursive Stack Space)

```cpp
class Solution {
  private:
	void leftDFS(TreeNode *node, int level, vector<int>& ans) {
		if (node == nullptr) {
			return;
		}
		
		if (ans.size() == level) {
			ans.push_back(node->val);
		}
		
		leftDFS(node->left, level + 1, ans);
		leftDFS(node->right, level + 1, ans);
	}
	
  public:
	vector<int> rightSideView(TreeNode* root) {
		vector<int> ans;
		leftDFS(root, 0, ans);
		return ans;
	}
};
```

---

---