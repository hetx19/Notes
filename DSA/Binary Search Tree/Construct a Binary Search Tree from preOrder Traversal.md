**Problem**: Given an array of integers preorder, which represents the **preorder traversal** of a BST (i.e., **binary search tree**), construct the tree and return _its root_.

It is **guaranteed** that there is always possible to find a binary search tree with the given requirements for the given test cases.

A **binary search tree** is a binary tree where for every node, any descendant of `Node.left` has a value **strictly less than** `Node.val`, and any descendant of `Node.right` has a value **strictly greater than** `Node.val`.

A **preorder traversal** of a binary tree displays the value of the node first, then traverses `Node.left`, then traverses `Node.right`.

**Example**:
![[Construct a Binary Search Tree from preOrder Traversal.png|273]]
**Input**: `preorder = [8,5,1,7,10,12]`
**Output**: `[8,5,10,1,7,null,12]`

[Visit Leetcode](https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/)
[Visit GFG](https://www.geeksforgeeks.org/problems/construct-bst-from-post-order/1)

---
### Brute Force Solution
By sorting preOrder gives inorder traversal

**Time Complexity**: O(n log n)
**Space Complexity**: O(n) (hashmap + inorder) + **O(h)** recursion stack.

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
	
	TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
		unordered_map<int, int> inMap;
		
		for (int i = 0; i < inorder.size(); i++) {
			inMap[inorder[i]] = i;
		}
		
		return build(preorder, 0, preorder.size() - 1, inorder, 0, inorder.size() - 1, inMap);
	}
	
  public:
	TreeNode* bstFromPreorder(vector<int>& preorder) {
		vector<int> inorder = preorder;
		sort(inorder.begin(), inorder.end());
		
		return buildTree(preorder, inorder);
	}
};
```

---
### Better Solution
By inserting all nodes in BST - [[Insert in Binary Search Tree]]

**Time Complexity**: O(n log n)
**Space Complexity**: O(h) → For Skewed Tree h = n

```cpp
class Solution {
private:
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        if (root == nullptr) {
            return new TreeNode(val);
        }

        if (val < root->val) {
            root->left = insertIntoBST(root->left, val);
        } else {
            root->right = insertIntoBST(root->right, val);
        }

        return root;
    }

public:
    TreeNode* bstFromPreorder(vector<int>& preorder) {
        TreeNode* root = nullptr;

        for (int val : preorder) {
            root = insertIntoBST(root, val);
        }

        return root;
    }
};
```

---
### Optimal Solution
By setting the range for every node

**Time Complexity**: O(n)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	TreeNode* build(vector<int>& preorder, int& i, int bound) {
		if (i == preorder.size() || preorder[i] > bound) {
			return nullptr;
		}
		
		TreeNode* root = new TreeNode(preorder[i++]);
		
		root->left = build(preorder, i, root->val);
		root->right = build(preorder, i, bound);

		return root;
	}

  public:
	TreeNode* bstFromPreorder(vector<int>& preorder) {
		int i = 0;
		return build(preorder, i, INT_MAX);
	}
};
```