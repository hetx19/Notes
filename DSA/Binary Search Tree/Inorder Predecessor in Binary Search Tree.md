**Problem**: Given a BST, and a reference to a Node **k** in the BST. Find the Inorder Predecessor of the given node in the BST. If there is no predecessor, return -1.

**Example**:  
**Input**: `root = [2, 1, 3], k = 2`  
**Output**: 1 
**Explanation**: `Inorder traversal : 1 2 3 Hence, inorder predecessor of 2 is 1`.

---
### Brute Force Solution
By storing the inorder traversal + Binary Search - [[Lower Bound]]

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
    int inOrderPredecessor(TreeNode* root, TreeNode* k) {
        vector<int> inorder;
        inorderTraversal(root, inorder);

        auto it = lower_bound(inorder.begin(), inorder.end(), k->val);

        if (it == inorder.begin()) {
            return -1;
        }

        --it;
        return *it;
    }
};
```

---
### Better Solution
Perform inorder traversal and return the last value smaller than the value of `k`.

**Time Complexity**: O(n)  
**Space Complexity**: O(1) + Recursive Stack Space

```cpp
class Solution {
  private:
    void inorder(TreeNode* node, int target, int& predecessor) {
        if (node == nullptr) {
            return;
        }

        inorder(node->left, target, predecessor);

        if (node->val < target) {
            predecessor = node->val;
        }

        inorder(node->right, target, predecessor);
    }

  public:
    int inOrderPredecessor(TreeNode* root, TreeNode* k) {
        int predecessor = -1;
        inorder(root, k->val, predecessor);
        return predecessor;
    }
};
```

---
### Optimal Solution
Use the BST property:
- If `k->val <= root->val`, move to the left subtree.
- Otherwise, the current node is a potential predecessor. Store it and move to the right subtree to find a larger valid predecessor.

**Time Complexity**: O(h)  
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
    int inOrderPredecessor(TreeNode* root, TreeNode* k) {
        int predecessor = -1;

        while (root != nullptr) {
            if (k->val <= root->val) {
                root = root->left;
            } else {
                predecessor = root->val;
                root = root->right;
            }
        }

        return predecessor;
    }
};
```

---