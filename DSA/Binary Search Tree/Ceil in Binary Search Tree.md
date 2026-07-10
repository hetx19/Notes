**Problem**: Given a **root** binary search tree and an integer **x** , find the Ceil of **x** in the tree.

**Ceil(x)** is a number that is either equal to x or is immediately greater than x. If Ceil could not be found, return **-1**.

**Example**:
**Input**: `root = [10, 7, 15, 2, 8, 11, 16],  k = 14`
![[Floor in Binary Search Tree.png|257]]
**Output**: 15
**Explanation**: The smallest element in the tree which is greater than or equal to 14, is 15.

[Visit GFG](https://www.geeksforgeeks.org/problems/implementing-ceil-in-bst/1)

---
### Brute Force Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(log n)
**Space Complexity**: O(log n)

```cpp
class Solution {
  public:
	int ceil(TreeNode* root, int k) {
		if (root == nullptr) {
			return -1;
		}
		
		if (root->val == k) {
	        return root->val;
	    }
	    
	    if (root->val < k) {
	        return ceil(root->right, k);
	    }
	    
	    int ceilValue = ceil(root->left, k);
	    
	    return (ceilValue >= k && ceilValue != -1) ? ceilValue : root->val;
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
	int ceil(TreeNode* root, int k) {
	    int ceilValue = -1;

	    while (root != nullptr) {
	        if (root->val == k) {
	            return root->val;
	        }

	        if (root->val < k) {
	            root = root->right;
	        } else {
	            ceilValue = root->val;
	            root = root->left;
	        }
	    }

	    return ceilValue;
	}
};
```

---