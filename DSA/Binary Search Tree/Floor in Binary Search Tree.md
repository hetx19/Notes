**Problem**: Given the **root** of a **[binary search tree](https://www.geeksforgeeks.org/binary-search-tree-data-structure/ "BST")** and a number **k**, find the **greatest** number in the binary search tree that is less than or equal to **k**.

**Note:** If no such node value exists that is smaller than _k_, then return **-1**.

**Example**:
**Input**: `root = [10, 7, 15, 2, 8, 11, 16],  k = 14`
![[Floor in Binary Search Tree.png|257]]
**Output**: 11
**Explanation**: The greatest element in the tree which is less than or equal to 14, is 11.

[Visit GFG](https://www.geeksforgeeks.org/problems/closest-neighbor-in-bst/1)

---
### Brute Force Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(log n)
**Space Complexity**: O(log n)

```cpp
class Solution {
  public:
	int floor(TreeNode* root, int k) {
		if (root == nullptr) {
			return -1;
		}
		
		if (root->val == k) {
	        return root->val;
	    }
	    
	    if (root->val > k) {
	        return floor(root->left, k);
	    }
	    
	    int floorValue = floor(root->right, k);
	    
	    return (floorValue <= k && floorValue != -1) ? floorValue : root->val;
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
	int floor(TreeNode* root, int k) {
	    int floorValue = -1;

	    while (root != nullptr) {
	        if (root->val == k) {
	            return root->val;
	        }

	        if (root->val > k) {
	            root = root->left;
	        } else {
	            floorValue = root->val;
	            root = root->right;
	        }
	    }

	    return floorValue;
	}
};
```

---