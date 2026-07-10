**Problem**: Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return _the binary tree_.

**Example**:
![[Construct the Binary Tree from Preorder and Inorder Traversal.png|209]]
**Input**: `preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]`
**Output**: `[3,9,20,null,null,15,7]`

[Visit Leetcode](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
[Visit GFG](https://www.geeksforgeeks.org/problems/construct-tree-1/1)

---
### Optimal Solution

**Time Complexity**: O(n)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	TreeNode* build(vector<int>& preorder, int preStart, int preEnd, vector<int>& inorder, int inStart, int inEnd, unordered_map<int, int>& inMap) {
		if (preStart > preEnd || inStart > inEnd) {
			return nullptr;
		}
	
		TreeNode* root = new TreeNode(preorder[preStart]);
		int inRoot = inMap[root->val];
		int left = inRoot - inStart;
	
		root->left = build(preorder, preStart + 1, preStart + left, inorder, inStart, inRoot - 1, inMap);
		root->right = build(preorder, preStart+ left + 1, preEnd, inorder, inRoot + 1, inEnd, inMap);
	
		return root;
	}
	
  public:
	TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
		unordered_map<int, int> inMap;
		
		for (int i = 0; i < inorder.size(); i++) {
			inMap[inorder[i]] = i;
		}
		
		return build(preorder, 0, preorder.size() - 1, inorder, 0, inorder.size() - 1, inMap);
	}
};
```

---