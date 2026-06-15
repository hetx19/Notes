### Definition
**Dynamic Programming (DP)** is an optimization technique used to solve problems by breaking them into smaller subproblems and storing the answers of those subproblems so they do not need to be recomputed.

Instead of solving the same problem repeatedly, DP computes it once and reuses the stored result whenever needed.

---

### Key Idea
Many recursive problems contain **overlapping subproblems**.
Consider the Fibonacci recurrence:

```text
F(n) = F(n - 1) + F(n - 2)
```

with:

```text
F(0) = 0
F(1) = 1
```

For example:

```text
F(5)
├── F(4)
│   ├── F(3)
│   └── F(2)
└── F(3)
```

Notice that:

```text
F(3)
```

is calculated multiple times.

Dynamic Programming eliminates this redundant work by storing previously computed results.
![[Dp.png|417]]

---
### Requirements for Dynamic Programming
A problem can be solved using DP if it has:

#### 1. Overlapping Subproblems
The same subproblems are solved repeatedly.

![[fibo.png|218]]

Example:

```text
F(3)
```

appears multiple times in the Fibonacci recursion tree.
![[Fibo recursion tree.png|321]]

---

#### 2. Optimal Substructure
The solution to a problem can be constructed using solutions of smaller subproblems.

Example:

```text
F(n) = F(n - 1) + F(n - 2)
```

---

### DP Approaches
There are two major approaches:

#### 1. Memoization (Top-Down)
Solve the problem recursively and store answers when they are computed.

```text
Main Problem
      ↓
Smaller Problems
      ↓
Base Cases
```

---

#### 2. Tabulation (Bottom-Up)
Start from the base cases and iteratively build the final answer.

```text
Base Cases
      ↓
Smaller Problems
      ↓
Main Problem
```

---

## Part 1: Memoization
### Idea
Memoization stores answers to already solved subproblems.

Before computing a state:
1. Check if it has already been solved.
2. If yes, return the stored answer.
3. Otherwise compute and store it. 

---

### Steps
#### Step 1
Create a DP array:

```cpp
vector<int> dp(n + 1, -1);
```

---

#### Step 2
Check whether the state has already been computed:

```cpp
if(dp[n] != -1) return dp[n];
```

---

#### Step 3
Compute and store the answer:

```cpp
dp[n] = fib(n - 1, dp) + fib(n - 2, dp);
```

---

### C++ Implementation

```cpp
class Solution {
public:
    int fib(int n, vector<int>& dp) {
        if (n <= 1) {
	        return n;
	    }

        if (dp[n] != -1) {
            return dp[n];
        }

        return dp[n] = fib(n - 1, dp) + fib(n - 2, dp);
    }
};
```

---

### Complexity Analysis
#### Time Complexity

```text
O(n)
```

Each state:

```text
0 → n
```

is computed only once.

---

#### Space Complexity

```text
O(n)
```

For:
- DP array
- Recursion stack

---

### Advantages
- Easy conversion from recursion.
- Solves only required states. 

![[Fibo Memoization.png|314]]

---

### Disadvantages
- Uses recursion stack.
- Slightly slower than tabulation.

---

## Part 2: Tabulation
### Idea
Tabulation builds the answer from the base cases.

Instead of recursion:
```text
Base Cases
    ↓
Build Answers
    ↓
Final State
```

---

### Steps
#### Step 1
Create DP array:

```cpp
vector<int> dp(n + 1);
```

---
#### Step 2
Initialize base cases:

```cpp
dp[0] = 0;
dp[1] = 1;
```

---

#### Step 3
Fill remaining states:

```cpp
for(int i = 2; i <= n; i++) {
    dp[i] = dp[i - 1] + dp[i - 2];
}
```

---

### C++ Implementation

```cpp
class Solution {
public:
    int fib(int n) {
        if(n <= 1) {
            return n;
        }

        vector<int> dp(n + 1);

        dp[0] = 0;
        dp[1] = 1;

        for(int i = 2; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }

        return dp[n];
    }
};
```

---

### Complexity Analysis
#### Time Complexity

```text
O(n)
```

---

#### Space Complexity

```text
O(n)
```

---

### Advantages
- No recursion stack.
- Usually faster than memoization.
- Easier to optimize space.

![[Fibo Tabulation.png|352]]

---

### Disadvantages
- Computes all states even if some are unnecessary.
- Requires identifying correct iteration order.

---

### Part 3: Space Optimization
#### Observation
The Fibonacci recurrence is:

```text
dp[i] = dp[i - 1] + dp[i - 2]
```

To calculate:

```text
dp[i]
```

we only need:

```text
dp[i - 1]
dp[i - 2]
```

Therefore storing the entire array is unnecessary.

---

### Optimization
Store only:

```cpp
prev2 = dp[i-2]
prev  = dp[i-1]
```

Compute:

```cpp
curr = prev + prev2;
```

Then update:

```cpp
prev2 = prev;
prev = curr;
```

---

### C++ Implementation

```cpp
class Solution {
public:
    int fib(int n) {
        if(n == 0) {
            return 0;
        }

        if(n == 1) {
            return 1;
        }

        int prev2 = 0;
        int prev = 1;

        for(int i = 2; i <= n; i++) {
            int curr = prev + prev2;
            prev2 = prev;
            prev = curr;
        }

        return prev;
    }
};
```

---

### Complexity Analysis

#### Time Complexity

```text
O(n)
```

---

#### Space Complexity

```text
O(1)
```

---

### DP Progression
Most DP problems are solved in this order:

```text
Recursion
    ↓
Memoization
    ↓
Tabulation
    ↓
Space Optimization
```

---

### Memoization vs Tabulation

|Feature|Memoization|Tabulation|
|---|---|---|
|Approach|Top-Down|Bottom-Up|
|Uses Recursion|Yes|No|
|Uses Stack Space|Yes|No|
|Computes Required States Only|Yes|No|
|Usually Faster|No|Yes|
|Easier to Write|Yes|Moderate|

---

### General DP Recipe
Whenever solving a DP problem:

#### 1. Write Recursive Solution
Identify:

```text
State
Choices
Base Cases
```

---

#### 2. Add Memoization
Store repeated states.

---

#### 3. Convert to Tabulation
Replace recursion with iteration.

---

#### 4. Optimize Space
Keep only states needed by the recurrence.

---

### Common Pitfalls
1. Forgetting base cases.
2. Wrong state definition.
3. Incorrect recurrence relation.
4. Wrong tabulation order.
5. Not checking memoized states before recursion.
6. Missing space optimization opportunities.

---

### Memory Trick

```text
DP = Remember Previous Answers
```

```text
Recursion
    ↓
Memoization
    ↓
Tabulation
    ↓
Space Optimization
```

```text
Overlapping Subproblems + Optimal Substructure = Dynamic Programming
```

---

### Interview Takeaway
Whenever you see:

```text
Maximum
Minimum
Count Ways
Number of Paths
Take / Not Take
Pick / Skip
```

ask yourself:

```text
Can I define a state and reuse subproblem answers?
```

If yes, the problem is likely a **Dynamic Programming** problem.

---

### Problems on Dynamic Programming

**_1 D DP_**
Climbing Stairs - [[Climbing Stairs]]

**__**
**__**
**__**
**__**
