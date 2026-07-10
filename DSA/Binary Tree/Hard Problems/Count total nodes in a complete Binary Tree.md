Given the `root` of a **complete** binary tree, return the number of the nodes in the tree.

According to **[Wikipedia](http://en.wikipedia.org/wiki/Binary_tree#Types_of_binary_trees)**, every level, except possibly the last, is completely filled in a complete binary tree, and all nodes in the last level are as far left as possible. It can have between `1` and `2h` nodes inclusive at the last level `h`.

Design an algorithm that runs in less than `O(n)` time complexity.

**Example**:
![[Count total nodes in a complete Binary Tree.png|266]]
**Input**: `root = [1,2,3,4,5,6]`
**Output**: 6

[Visit Leetcode](https://leetcode.com/problems/count-complete-tree-nodes/)
[Visit GFG](https://www.geeksforgeeks.org/problems/count-number-of-nodes-in-a-binary-tree/1)

---
### Optimal Solution

**Time Complexity**: O((log n)<sup>2</sup>)
**Space Complexity**: O((log n))

```cpp
class Solution { 
  private:
	int findHeightLeft(TreeNode* node) {
		int height = 0;
		
		while (node != nullptr) {
			height++;
			node = node->left;
		}
		
		return height;
	}
	
	int findHeightRight(TreeNode* node) {
		int height = 0;
		
		while (node != nullptr) {
			height++;
			node = node->right;
		}
		
		return height;
	}
	
  public:
	int countNodes(TreeNode* root) {
		if (root == nullptr) {
			return 0;
		}
		
		int leftHeight = findHeightLeft(root);
		int rightHeight = findHeightRight(root);
		
		if (leftHeight == rightHeight) {
			return (1 << lh) - 1;
		}
		
		return 1 + countNodes(root->left) + countNodes(root->right);
	}
};
```

---
