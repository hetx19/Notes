**Problem**:Given an integer `numRows`, return the first `numRows` rows of **Pascal's Triangle**.

In Pascal's Triangle, each number is the sum of the two numbers directly above it.

```text
            1
          1   1
        1   2   1
      1   3   3   1
    1   4   6   4   1
```

**Example**:
```text
Input: numRows = 5
Output: [[1], [1,1], [1,2,1], [1,3,3,1], [1,4,6,4,1]]
```

[Visit Leetcode](https://leetcode.com/problems/pascals-triangle/)

---

### Theory
Pascal's Triangle can be solved in three different ways:

#### Category 1: Find a Particular Element
Given row `n` and column `r`, find the value at that position.

The element is:
$$  
nCr = \frac{n!}{r!(n-r)!}  
$$

**Example**:
```text
Row = 5
Col = 2

Value = 5C2 = 10
```

Instead of factorials, use the multiplicative formula:

$$  
nCr = \prod_{i=0}^{r-1}\frac{n-i}{i+1}  
$$

---

#### Category 1: Find nCr
##### Optimal Approach

##### Intuition
Compute the answer directly using the multiplicative formula.

##### Time Complexity
```text
O(r)
```

##### Space Complexity
```text
O(1)
```

##### Code
```cpp
class Solution {
public:
    int nCr(int n, int r) {
        long long ans = 1;

        for (int i = 0; i < r; i++) {
            ans = ans * (n - i);
            ans = ans / (i + 1);
        }

        return (int)ans;
    }
};
```

---

### Category 2: Print a Particular Row

Example:
```text
Input: n = 5
Output: [1, 4, 6, 4, 1]
```

---

#### Brute Force
##### Intuition
Generate each element separately using the `nCr()` function.

##### Time Complexity
```text
O(n²)
```

##### Space Complexity
```text
O(n)
```

##### Code
```cpp
class Solution {
private:
    int nCr(int n, int r) {
        long long ans = 1;

        for (int i = 0; i < r; i++) {
            ans = ans * (n - i);
            ans = ans / (i + 1);
        }

        return (int)ans;
    }

public:
    vector<int> getRow(int n) {
        vector<int> row;

        for (int c = 0; c < n; c++) {
            row.push_back(nCr(n - 1, c));
        }

        return row;
    }
};
```

---

#### Optimal Approach
##### Observation
If one element is known, the next element can be generated directly.

For a row having index `(n - 1)`:

$$  
nCr = nC(r-1)\times\frac{n-r+1}{r}  
$$

**Example**:
```text
1
1 × 4 / 1 = 4
4 × 3 / 2 = 6
6 × 2 / 3 = 4
4 × 1 / 4 = 1
```

Result:
```text
1 4 6 4 1
```

##### Time Complexity
```text
O(n)
```

##### Space Complexity
```text
O(n)
```

##### Code
```cpp
class Solution {
public:
    vector<int> getRow(int n) {
        vector<int> row;

        long long ans = 1;
        row.push_back(1);

        for (int col = 1; col < n; col++) {
            ans = ans * (n - col);
            ans = ans / col;

            row.push_back((int)ans);
        }

        return row;
    }
};
```

---

### Category 3: Print Entire Pascal's Triangle
#### Optimal Approach (Using Row Generation)

##### Intuition
Generate each row independently using the optimal row-generation method.

##### Time Complexity
```text
O(numRows²)
```

##### Space Complexity
```text
O(numRows²)
```

##### Code
```cpp
class Solution {
private:
    vector<int> generateRow(int rowNum) {
        vector<int> row;

        long long ans = 1;
        row.push_back(1);

        for (int col = 1; col < rowNum; col++) {
            ans = ans * (rowNum - col);
            ans = ans / col;

            row.push_back((int)ans);
        }

        return row;
    }

public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> triangle;

        for (int row = 1; row <= numRows; row++) {
            triangle.push_back(generateRow(row));
        }

        return triangle;
    }
};
```

---

### Alternative DP Solution - [[Dynamic Programming]]
#### Intuition
Every element is formed using:

```text
current[i] =
previous[i - 1] + previous[i]
```

The first and last element of every row are always `1`.

##### Time Complexity
```text
O(numRows²)
```

##### Space Complexity
```text
O(numRows²)
```

##### Code
```cpp
class Solution { 
  public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> triangle;

        for (int row = 0; row < numRows; row++) {

            vector<int> curr(row + 1, 1);

            for (int col = 1; col < row; col++) {
                curr[col] = triangle[row - 1][col - 1] + triangle[row - 1][col];
            }

            triangle.push_back(curr);
        }

        return triangle;
    }
};
```

---
### Complexity Summary

| Problem           | Approach               | Time  | Space |
| ----------------- | ---------------------- | ----- | ----- |
| Find nCr          | Multiplicative Formula | O(r)  | O(1)  |
| Print Row         | Brute Force            | O(n²) | O(n)  |
| Print Row         | Optimal                | O(n)  | O(n)  |
| Complete Triangle | Row Generation         | O(n²) | O(n²) |
| Complete Triangle | DP                     | O(n²) | O(n²) |

---