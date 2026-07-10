**Problem**: Given a BST, and a reference to a Node **k** in the BST. Find the Inorder Successor of the given node in the BST. If there is no successor, return -1.

**Example**:
**Input**: `root = [2, 1, 3], k = 2`
**Output**: 3 
**Explanation**: `Inorder traversal : 1 2 3 Hence, inorder successor of 2 is 3`.

[Visit GFG](https://www.geeksforgeeks.org/problems/inorder-successor-in-bst/1)

---
### Brute Force Solution
By Storing the inorder traversal + Binary Search - [[Upper Bound]]

**Time Complexity**: O(n) + O(log n)
**Space Complexity**: O(n) + Recursive Stack Space

```cpp
class Solution {
  private:
    void inorderTraversal(TreeNode* node, vector<int>& inorder) {
        if (node == nullptr) {
            return;
        }

        inorderTraversal(node->left, inorder);
        inorder.push_back(node->val);
        inorderTraversal(node->right, inorder);
    }

  public:
    int inOrderSuccessor(TreeNode* root, TreeNode* k) {
        vector<int> inorder;
        inorderTraversal(root, inorder);

        auto it = upper_bound(inorder.begin(), inorder.end(), k->val);

        return (it == inorder.end()) ? -1 : *it;
    }
};
```

---
### Better Solution
Perform inorder traversal and return the first value greater than value of k

**Time Complexity**: O(n)
**Space Complexity**: O(1) + Recursive Stack Space

```cpp
class Solution {
  private:
    void inorder(TreeNode* node, int target, int& successor) {
        if (node == nullptr || successor != -1) {
            return;
        }

        inorder(node->left, target);

        if (successor != -1) {
            return;
        }

        if (node->val > target) {
            successor = node->val;
            return;
        }

        inorder(node->right, target);
    }

  public:
    int inOrderSuccessor(TreeNode* root, TreeNode* k) {
        int successor = -1;
        inorder(root, k->val, successor);
        return successor;
    }
};
```

---
### 

**Time Complexity**: O(h) 
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
    int inOrderSuccessor(TreeNode* root, TreeNode* k) {
        int successor = -1;
        
        while (root != nullptr) {
	        if (k->val >= root->val) {
		        root = root->right;
	        } else {
		        successor = root->val
		        root = root->left;
	        }
        }
        
        return successor;
    }
};
```

---