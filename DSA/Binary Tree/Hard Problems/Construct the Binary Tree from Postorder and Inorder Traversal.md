**Problem**: Given two integer arrays `inorder` and `postorder` where `inorder` is the inorder traversal of a binary tree and `postorder` is the postorder traversal of the same tree, construct and return _the binary tree_.

**Example**:
![[Construct the Binary Tree from Preorder and Inorder Traversal.png|209]]
**Input**: `inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]`
**Output**: `[3,9,20,null,null,15,7]`

[Visit Leetcode](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)
[Visit GFG](https://www.geeksforgeeks.org/problems/tree-from-postorder-and-inorder/1)

---
### Optimal Solution

**Time Complexity**: O(n)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	TreeNode* build(vector<int>& inorder, int inStart, int inEnd, vector<int>& postorder, int postStart, int postEnd, unordered_map<int, int>& mpp) {
		if (postStart > postEnd || inStart > inEnd) {
			return nullptr;
		}
		
		TreeNode* root = new TreeNode(postorder[postEnd]);
		int inRoot = mpp[postorder[postEnd]];
		int numLeft = inRoot - inStart;
		
		root->left = build(inorder, inStart, inRoot - 1, postorder, postStart, postStart + numLeft - 1, mpp);
		root->right = build(inorder, inRoot + 1, inEnd, postorder, postStart + numLeft, postEnd - 1, mpp);
		
		return root;
	}
	
  public:
	TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
		int inSize = inorder.size(), postSize = postorder.size();
		
		if (inSize != postSize) {
			return nullptr;
		}
		
		unordered_map<int, int> mpp;
		for (int i = 0; i < inSize; i++) {
			mpp[inorder[i]] = i;
		}
		
		return build(inorder, 0, inSize - 1, postorder, 0, postSize - 1, mpp);
	}
};
```

---