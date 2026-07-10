**Problem**: Given a **binary tree** `root`, return _the maximum sum of all keys of **any** sub-tree which is also a Binary Search Tree (BST)_.

Assume a BST is defined as follows:
- The left subtree of a node contains only nodes with keys **less than** the node's key.
- The right subtree of a node contains only nodes with keys **greater than** the node's key.
- Both the left and right subtrees must also be binary search trees.

**Example**:
![[Largest Binary Search Tree inside Binary Tree.png|291]]
**Input**: `root = [1,4,3,2,4,2,5,null,null,null,null,null,null,4,6]`
**Output**: 20
**Explanation**: `Maximum sum in a valid Binary search tree is obtained in root node with key equal to 3`.

[Visit Leetcode](https://leetcode.com/problems/maximum-sum-bst-in-binary-tree/)
[Visit GFG](https://www.geeksforgeeks.org/problems/largest-bst/1)

---
### Brute Force Solution
By using Validate function for every node - [[Validate Binary Search Tree]]

**Time Complexity**: O(n<sup>2</sup>)
**Space Complexity**: O(h)

```cpp
class Solution {
  private:
	bool validBST(TreeNode* root, long long minValue, long long maxValue) {
		if (root == nullptr) {
			return true;
		}

		if (root->val >= maxValue || root->val <= minValue) {
			return false;
		}

		return validBST(root->left, minValue, root->val) &&
			   validBST(root->right, root->val, maxValue);
	}

	int subtreeSum(TreeNode* root) {
		if (root == nullptr) {
			return 0;
		}

		return root->val + subtreeSum(root->left) + subtreeSum(root->right);
	}

	void dfs(TreeNode* root, int &ans) {
		if (root == nullptr) {
			return;
		}

		if (validBST(root, LLONG_MIN, LLONG_MAX)) {
			ans = max(ans, subtreeSum(root));
		}

		dfs(root->left, ans);
		dfs(root->right, ans);
	}

  public:
	int maxSumBST(TreeNode* root) {
		int ans = 0;
		dfs(root, ans);
		
		return ans;
	}
};
```

---
### Optimal Solution

**Time Complexity**: O(n)
**Space Complexity**: O(1) + Recursive Stack Space

```cpp
class NodeVal {
  public:
	int minNode, maxNode, sum;
	bool isBST;
	
	NodeVal(int minNode, int maxNode, int sum, bool isBST) {
		this->minNode = minNode;
		this->maxNode = maxNode;
		this->sum = sum;
		this->isBST = isBST;
	}
};

class Solution {
  private:
	int maxSum = 0;
	
	NodeVal helper(TreeNode* root) {
		if (root == nullptr) {
			return NodeVal(INT_MAX, INT_MIN, 0, true);
		}
	
		auto left = helper(root->left);
		auto right = helper(root->right);

		if (left.isBST && right.isBST && left.maxNode < root->val && root->val < right.minNode) {
			int currentSum = left.sum + right.sum + root->val;
			maxSum = max(maxSum, currentSum);
			return NodeVal(min(root->val, left.minNode), max(root->val, right.maxNode), currentSum, true);
		}
	
		return NodeVal(0, 0, 0, false);
	}
	
  public:
	int maxSumBST(TreeNode* root) {
		helper(root);
		return maxSum;
	}
};
```

---