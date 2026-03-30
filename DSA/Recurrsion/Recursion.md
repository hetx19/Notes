Recursion is a programming technique where a function calls itself to solve a smaller version of the same problem.
It helps break complex problems into simpler subproblems.

## Basic Idea

```code
Big Problem → Smaller Problem → Smaller Problem → ... → Base Case
```

A recursive function must have:

1. Base Case → Condition to stop recursion → To avoid the infinite recursion
2. Recursive Case → Function calls itself with smaller input

---

### What is Stack Overflow in Recursion?

- When a recursive function is called, each call is stored in the recursion stack and waits until the inner calls finish.
- A recursive function completes only when the **base case** is reached, allowing control to return to the previous (parent) function.
- If no base case is provided, the function keeps calling itself indefinitely, causing a Stack Overflow (memory limit exceeded), which may result in a Segmentation Fault error.
  ![[Stack_space.png|228]]

---

### Recursion Tree:

A recursive tree is basically a representative form of recursion which depicts how functions are called and returned as a series of events happening consecutively. It is a pictorial description of the process of recursion as illustrated below:

![[recursion_tree.png|243]]

---

### Types of recursion

1. Direct Recursion: The function calls itself directly

- Example

```cpp
void fun() {
    // some code
    fun(); // direct recursive call
}
```

2. Indirect Recursion: The function **calls another function**, which eventually calls the original function.

- Example

```cpp
void funA() { funB(); }
void funB() { funA(); }
```

3. Tail Recursion: - The recursive call is the last operation in the function. Optimizable by compilers (can reduce stack usage).

- Example

```cpp
void tailRecursion(int n) {
    if (n == 0) return;
    cout << n << " ";
    tailRecursion(n - 1); // last operation
}
```

4. Head Recursion: - There is some operation after the recursive call.

- Example

```cpp
void headRecursion(int n) {
    if (n == 0) return;
    headRecursion(n - 1);
    cout << n << " "; // operation after recursion
}
```

5. Tree Recursion: A function makes multiple recursive calls.

- Example

```cpp
int fib(int n) {
    if (n <= 1) {
	    return n;
    }

    return fib(n - 1) + fib(n - 2);
}
```

---

### Practice problems on Recursion

1. Print your name n times - [[Print Name N times]]
2. Print number from 1 to N (Normal / Backtrack) - [[Print Numbers from 1 to N]]
3. Sum of first N natural number - [[Sum of first N natural number]]
4. Factorial of N - [[Factorial]]
5. Reverse an array - [[Reverse an Array]]
6. Check for Palindrome string - [[Palindrome String]]
7. Find N Fibonacci number - [[Nth Fibonacci number]]

