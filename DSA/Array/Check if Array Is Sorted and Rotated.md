**Problem**: Given an array `nums`, return `true` *if the array was originally sorted in non-decreasing order, then rotated **some** number of positions (including zero)*. Otherwise, return `false`.

There may be **duplicates** in the original array.

**Note:** An array `A` rotated by `x` positions results in an array `B` of the same length such that `B[i] == A[(i+x) % A.length]` for every valid index `i`.

**Example**:

**Input:** nums = `[3,4,5,1,2]`
**Output:** true
**Explanation:** `[1,2,3,4,5]` is the original sorted array.
You can rotate the array by x = 2 positions to begin on the element of value 3: `[3,4,5,1,2]`.

[Vist_leetcode](https://leetcode.com/problems/check-if-array-is-sorted-and-rotated/description/)
[Vist_GFG]()

---

### Optimal Approach:

**Intuition**: There can be **at most ONE "drop" point**
(where `nums[i] > nums[i+1]`)

```cpp
class Solution {
public:
    bool check(vector<int> &nums) {
        int n = nums.size(), cnt = 0;

        for (int i = 0; i < n; i++) {
            if (nums[i] > nums[(i + 1) % n]) cnt++;
        }

        return cnt <= 1;
    }
}
```

