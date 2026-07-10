**Problem**: You are given the `root` of a binary search tree (BST), where the values of **exactly** two nodes of the tree were swapped by mistake. _Recover the tree without changing its structure_.

**Example**:
![[Recover Binary Search Tree.png|279]]
**Input**: `root = [1,3,null,null,2]`
**Output**: `[3,1,null,null,2]`
**Explanation**: `3 cannot be a left child of 1 because 3 > 1. Swapping 1 and 3 makes the BST valid.`

[Visit Leetcode](https://leetcode.com/problems/recover-binary-search-tree/)
[Visit GFG](https://www.geeksforgeeks.org/problems/fixed-two-nodes-of-a-bst/1)

---
### Brute Force Solution
By sorting The inorder traversal

**Time Complexity**: O(2n + n logn)
**Space Complexity**: O(n) + Recursive Stack Space

```cpp
class Solution {
  private:
    void inorderTraversal(TreeNode* node, vector<int>& inorder) {
        if (node == nullptr) {
	        return;
	    }

        inorderTraversal(node->left);
        inorderValues.push_back(node->val);
        inorderTraversal(node->right);
    }

    void restoreInorder(TreeNode* root, vector<int>& inorder, int& index) {
        if (root == nullptr) {
	        return;
	    }

        restoreInorder(root->left, inorder, index);
        root->val = inorderValues[index++];
        restoreInorder(root->right, inorder, index);
    }

  public:
    void recoverTree(TreeNode* root) {
	    int index = 0;
	    vector<int> inorder;
        inorderTraversal(root, inorder);

        sort(inorder.begin(), inorder.end());

        restoreInorder(root, inorder, index);
    }
};
```

---
### Optimal Solution
Maintaining 4 pointers

**Time Complexity**: O(n)
**Space Complexity**: O(1)

```cpp
class Solution {
  private:
	TreeNode *first, *prev, *middle, *last;

	void inOrder(TreeNode *root) {
		if (root == nullptr) {
			return;
		}

		inOrder(root->left);
		
		if (prev != nullptr && (root->val < prev->val)) {
			if (first == nullptr) {
				first = prev;
				middle = root;
			} else {
				last = root;
			}
		}
		
		prev = root;
		inOrder(root->right);
	}
	
  public:
	void recoverTree(TreeNode* root) {
		first = middle = last = nullptr;
		prev = new TreeNode(INT_MIN);

		inOrder(root);
		
		if (first != nullptr && last != nullptr) {
			swap(first->val, last->val);
		} else if (first != nullptr && middle != nullptr) {
			swap(first->val, middle->val);
		}
	}
};
```