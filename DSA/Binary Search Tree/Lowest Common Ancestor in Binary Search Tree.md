**Problem**: Given a binary search tree (BST), find the lowest common ancestor (LCA) node of two given nodes in the BST.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow **a node to be a descendant of itself**).”

**Example**:
![[Lowest Common Ancestor in Binary Search Tre.png]]
**Input**: `root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8`
**Output**: 6
**Explanation**: The LCA of nodes 2 and 8 is 6.

[Visit Leetcode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)
[Visit GFG](https://www.geeksforgeeks.org/problems/lowest-common-ancestor-in-a-bst/1)

---
### Brute Force Solution
By using same concept as Binary Tree - [[Lowest Common Ancestor in Binary Tree]]

**Time Complexity**: O(n)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
	TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
		if (root == nullptr || root == p || root == q) {
			return root;
		}
		
		TreeNode *left = lowestCommonAncestor(root->left, p, q);
		TreeNode *right = lowestCommonAncestor(root->right, p, q);
		
		if (left == nullptr) {
			return right;
		} else if (right == nullptr) {
			return left;
		}
		
		return root;
	}
};
```

---
### Better Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(log n)
**Space Complexity**: O(h)

```cpp
class Solution {
  public:
	TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
		if (root == nullptr) {
			return nullptr;
		}
		
		if (root == p) {
			return p;
		}
		
		if (root == q) {
			return q;
		}
		
		int current = root->val;
		
		if (current < p->val && current < q->val) {
			return lowestCommonAncestor(root->right, p, q);
		}

		if (current > p->val && current > q->val) {
			return lowestCommonAncestor(root->left, p, q);
		}
		
		return root;
	}
};
```

---
### Optimal Solution

**Time Complexity**: O(log n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
		while (root != nullptr) {
	        if (root->val > p->val && root->val > q->val) {
	            root = root->left;
	        } else if (root->val < p->val && root->val < q->val) {
	            root = root->right;
	        } else {
	            break;
	        }
	    }
	    
	    return root;
	}
};
```

---