**Problem**: Given the `root` of a binary search tree, and an integer `k`, return _the_ `kth` _smallest value (**1-indexed**) of all the values of the nodes in the tree_.

**Example**:
![[kth smallest in Binary Search Tree.png|195]]
**Input**: `root = [3,1,4,null,2], k = 1`
**Output**: 1

[Visit Leetcode](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)
[Visit GFG](https://www.geeksforgeeks.org/problems/find-k-th-smallest-element-in-bst/1)

---
### Optimal Solution

**Time Complexity**: O(k)
**Space Complexity**: O(h)

```cpp
class Solution { 
  private:
    void inOrder(TreeNode* root, int k, int& count, int& result) {
        if (root == nullptr) {
            return;
        }
        
        inOrder(root->left, k, count, result);
        count++;
        
        if (count == k) {
            result = root->val;
            return;
        }
        
        inOrder(root->right, k, count, result);
    }

  public:
    int kthSmallest(TreeNode* root, int k) {
	    int count = 0, result = 0;
        inOrder(root, k, count, result);
        return result;
    }
};
```

---