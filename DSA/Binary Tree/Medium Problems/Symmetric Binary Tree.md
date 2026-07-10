**Problem**: Given the `root` of a binary tree, _check whether it is a mirror of itself_ (i.e., symmetric around its center).

**Example**:
![[Symmetric Binary Tree.png|275]]
**Input**: `root = [1,2,2,3,4,4,3]`
**Output**: true

[Visit Leetcode](https://leetcode.com/problems/symmetric-tree/)
[Visit GFG](https://www.geeksforgeeks.org/problems/symmetric-tree/1)

---
### Optimal Solution

**Time Complexity**: O(n)
**Space Complexity**: O(h)

```cpp
class Solution {
  private:
	bool isSymmetricHelp(TreeNode* left, TreeNode* right) {
		if (left == nullptr || right == nullptr) {
			return left == right;
		}
		
		if (left->val != right->val) {
			return false;
		}
		
		return isSymmetricHelp(left->left, right->right) && isSymmetricHelp(left->right, right->left);
	}
	
  public:
	bool isSymmetric(TreeNode* root) {
		return root == nullptr || isSymmetricHelp(root->left, root->right);
	}
};
```

---