**Problem**: You are given the `root` of a binary search tree (BST) and an integer `val`.

Find the node in the BST that the node's value equals `val` and return the subtree rooted with that node. If such a node does not exist, return `null`.

**Example**:
![[Search in Binary Search Tree.png|272]]
**Input**: `root = [4,2,7,1,3], val = 2`
**Output**: `[2,1,3]`

[Visit Leetcode](https://leetcode.com/problems/search-in-a-binary-search-tree/)
[Visit GFG](https://www.geeksforgeeks.org/problems/search-a-node-in-bst/1)

---
### Brute Force Solution
By traversing every nodes

**Time Complexity**: O(n)
**Space Complexity**: O(h)

```cpp
class Solution { 
  private:
    TreeNode* search(TreeNode* node, int val) {
        if (node == nullptr) {
            return nullptr;
        }

        if (node->val == val) {
            return node;
        }

        TreeNode* left = search(node->left, val);
        if (left != nullptr) {
            return left;
        }

        return search(node->right, val);
    }

  public:
    TreeNode* searchBST(TreeNode* root, int val) {
        return search(root, val);
    }
};
```

---
### Optimal Solution
By using the BST property

**Time Complexity**: O(log n)
**Space Complexity**: O(h)

```cpp
class Solution {
  public:
    TreeNode* searchBST(TreeNode* root, int val) {
        if (root == nullptr || root->val == val) {
            return root;
        }

        if (val < root->val) {
            return searchBST(root->left, val);
        }

        return searchBST(root->right, val);
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
    TreeNode* searchBST(TreeNode* root, int val) {
        while (root != nullptr && root->val != val) {
            if (val < root->val) {
                root = root->left;
            } else {
                root = root->right;
            }
        }

        return root;
    }
};
```