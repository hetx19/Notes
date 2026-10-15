**Problem**: You are given an expression _**'exp'**_ in the form of a string where operands will be : (TRUE or FALSE), and operators will be : (AND, OR or XOR).

Now you have to find the number of ways we can parenthesize the expression such that it will evaluate to TRUE.

As the answer can be very large, return the output modulo `1000000007`.

**Note**:

```
‘T’ will represent the operand TRUE.
‘F’ will represent the operand FALSE.
‘|’ will represent the operator OR.
‘&’ will represent the operator AND.
‘^’ will represent the operator XOR.
```

**Example**:
**Input:** `exp = "T|T & F"`
**Output:** 1

---
### Brute Force
By using Recursion - [[Recursion]]

**Time Complexity**: O(4<sup>n</sup>)
**Space Complexity**: O(n)

```cpp
using ll = long long;
const static int MOD = 1000000007;

class Solution {
  private:
	int solve (string &exp, int start, int end, bool isTrue) {
	    if (start > end) {
	        return 0;
	    }

	    if (start == end) {
	        if (isTrue) {
	            return exp[start] == 'T' ? 1 : 0;
	        } else {
	            return exp[start] == 'F' ? 1 : 0;
	        }
	    }

	    ll ways = 0;

	    for (int index = start + 1; index < end; index += 2) {
	        ll lT = solve(exp, start, index - 1, true);
	        ll lF = solve(exp, start, index - 1, false);
	        ll rT = solve(exp, index + 1, end, true);
	        ll rF = solve(exp, index + 1, end, false);

	        if (exp[index] == '&') {
	            if (isTrue) {
	                ways = (ways + (lT * rT) % MOD) % MOD;
	            } else {
	                ways = (ways + (lF * rT) % MOD + (lT * rF) % MOD + (lF * rF) % MOD) % MOD;
	            }
	        } else if (exp[index] == '|') {
	            if (isTrue) {
		           ways = (ways + (lF * rT) % MOD + (lT * rF) % MOD + (lT * rT) % MOD) % MOD;
	            } else {
	                ways = (ways + (lF * rF) % MOD) % MOD;
	            }
	        } else {
	            if (isTrue) {
	                ways = (ways + (lF * rT) % MOD + (lT * rF) % MOD) % MOD;
	            } else {
	                ways = (ways + (lF * rF) % MOD + (lT * rT) % MOD) % MOD;
	            }
	        }
	    }

	    return ways;
	}
	
  public:
	int evaluateExp(string & exp) {
	    int n = exp.size();

	    return solve(exp, 0, n - 1, true);
	}
};
```

---
### Memoization

**Time Complexity**: O(2 x n<sup>3</sup>) → O(n<sup>3</sup>)
**Space Complexity**: O(2 x n<sup>2</sup>) + O(n) → O(n<sup>2</sup>)

```cpp
using ll = long long;
const static int MOD = 1000000007;

class Solution {
  private:
	int solve (string &exp, int start, int end, int isTrue, vector<vector<vector<ll>>>& dp) {
	    if (start > end) {
	        return 0;
	    }

	    if (start == end) {
	        if (isTrue == 1) {
	            return dp[start][end][isTrue] = (exp[start] == 'T' ? 1 : 0);
	        } else {
		        return dp[start][end][isTrue] = (exp[start] == 'F' ? 1 : 0);
	        }
	    }
	    
	    if (dp[start][end][isTrue] != -1) {
		    return dp[start][end][isTrue];
		}

	    ll ways = 0;

	    for (int index = start + 1; index < end; index += 2) {
	        ll lT = solve(exp, start, index - 1, 1, dp);
	        ll lF = solve(exp, start, index - 1, 0, dp);
	        ll rT = solve(exp, index + 1, end, 1, dp);
	        ll rF = solve(exp, index + 1, end, 0, dp);

	        if (exp[index] == '&') {
	            if (isTrue == 1) {
	                ways = (ways + (lT * rT) % MOD) % MOD;
	            } else {
	                ways = (ways + (lF * rT) % MOD + (lT * rF) % MOD + (lF * rF) % MOD) % MOD;
	            }
	        } else if (exp[index] == '|') {
	            if (isTrue == 1) {
		           ways = (ways + (lF * rT) % MOD + (lT * rF) % MOD + (lT * rT) % MOD) % MOD;
	            } else {
	                ways = (ways + (lF * rF) % MOD) % MOD;
	            }
	        } else {
	            if (isTrue == 1) {
	                ways = (ways + (lF * rT) % MOD + (lT * rF) % MOD) % MOD;
	            } else {
	                ways = (ways + (lF * rF) % MOD + (lT * rT) % MOD) % MOD;
	            }
	        }
	    }

	    return dp[start][end][isTrue] = ways;
	}
	
  public:
	int evaluateExp(string & exp) {
	    int n = exp.size();
	    vector<vector<vector<ll>>> dp(n, vector<vector<ll>>(n, vector<ll>(2, -1)));

	    return solve(exp, 0, n - 1, 1, dp);
	}
};
```

---
### Tabulation

**Time Complexity**: O(2 x n<sup>3</sup>) → O(n<sup>3</sup>)
**Space Complexity**: O(2 x n<sup>2</sup>) → O(n<sup>2</sup>)

```cpp
using ll = long long;
const static int MOD = 1000000007;

class Solution {
  public:
	int evaluateExp(string & exp) {
	    int n = exp.size();
	    vector<vector<vector<ll>>> dp(n, vector<vector<ll>>(n, vector<ll>(2, 0)));

	    for (int start = n - 1; start >= 0; start--) {
	        for (int end = 0; end <= n - 1; end++) {
	            if (start > end) {
	                continue;
	            }

	            for (int isTrue = 0; isTrue <= 1; isTrue++) {
	                if (start == end) {
	                    if (isTrue == 1) {
	                        dp[start][end][isTrue] = exp[start] == 'T';
	                    } else {
	                        dp[start][end][isTrue] = exp[start] == 'F';
	                    }
	                    continue;
	                }

	                ll ways = 0;

	                for (int index = start + 1; index < end; index += 2) {
	                    ll lT = dp[start][index - 1][1];
	                    ll lF = dp[start][index - 1][0];
	                    ll rT = dp[index + 1][end][1];
	                    ll rF = dp[index + 1][end][0];

	                    if (exp[index] == '&') {
	                        if (isTrue == 1) {
	                            ways = (ways + (lT * rT) % MOD) % MOD;
	                        } else {
	                            ways = (ways + (lF * rT) % MOD + (lT * rF) % MOD + (lF * rF) % MOD) % MOD;
	                        }
	                    } else if (exp[index] == '|') {
	                        if (isTrue == 1) {
	                            ways = (ways + (lF * rT) % MOD + (lT * rF) % MOD + (lT * rT) % MOD) % MOD;
	                        } else {
	                            ways = (ways + (lF * rF) % MOD) % MOD;
	                        }
	                    } else {
	                        if (isTrue == 1) {
	                            ways = (ways + (lF * rT) % MOD + (lT * rF) % MOD) % MOD;
	                        } else {
	                            ways = (ways + (lF * rF) % MOD + (lT * rT) % MOD) % MOD;
	                        }
	                    }

	                    dp[start][end][isTrue] = ways;
	                }
	            }
	        }
	    }

	    return dp[0][n - 1][1];
	}
};
```