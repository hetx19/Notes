# What is a Data Structure ?

In computer science, a data structure is a way of organizing and storing data in a computer program so that it can be accessed and used efficiently.

Data structures help in:

- Efficient searching
- Sorting of data
- Insertion of new data
- Deletion of existing data
- Better memory utilization
  [[Data_structure.canvas]]

---

_Linear Data Structure_
A Linear Data Structure organizes data in sequential order, where each element (except the first and the last) has a single predecessor and a single successor.

Examples:

1. Array
2. Linked List
3. Stack
4. Queue

_NON-Linear Data Structure_
A non-linear data structure is a type of data structure in which elements are not arranged sequentially. Instead, data elements are organized in a hierarchical or interconnected manner, where one element can be connected to multiple elements.
Examples).

1. Tree
2. Graph

---

**Algorithms**
An Algorithm is a finite sequence of well-defined steps used to solve a specific problem.

Characteristics of an Algorithm:

- It must be clear and unambiguous
- It must have a finite number of steps
- It must produce an output
- It must terminate

**_How To Compare Two Algorithms_**
Two algorithms are compared based on their **efficiency**. The main factors used for comparison are:

1. Time Complexity: The rate at which the time required to run the code, changes with respect to the input size, is considered the time complexity.

2. Space Complexity: The term space complexity generally refers to the memory space that a code uses while being executed.

---

**_Asymptotic Notation_**
**Asymptotic Notation** is used to represent the time and space complexity of an algorithm in mathematical form for large input sizes.
It helps us analyze the performance of an algorithm as the input size approaches infinity.

There are three main types of asymptotic notations:

1. Big O Notation (O):
   - Represent the **Worst-Case** time complexity
   - Gives the upper bound of running time
   - Most commonly used notation.
     Example:
   - O(1) – Constant Time
   - O(log n) - Logarithmic Time
   - O(n) – Linear Time
   - O(n log n) - Linearithmic Time
   - O(n²) – Quadratic Time

2. Big Ω Notation (Omega):
   - Represent the **Best-Case** time complexity
   - Gives the lower bound of running time
     Example:
   - Ω(1)
   - Ω(n)
3. Big Θ Notation (Theta):
   - Represent the **Average-Case** time complexity
   - Gives the tight bound (both upper and lower bound)
     Example:
   - Θ(n)
   - Θ(n log n)

---

**_Order of growth rate_** (As we go down Time complexity Increases)

- O(1) – Constant Time
- O(log n) - Logarithmic Time
- O(n) – Linear Time
- O(n log n) - Linearithmic Time
- O(n²) – Quadratic Time
- O(n³) - Cubic Time
- O(2ⁿ) - Exponential Time
- O(n!) - Factorial Time

---

STL - [[STL (Standard Template Library)]]
Basic - [[Basic]]
Sorting Techniques - [[Sorting]]
Recursion - [[Recursion]]
Hashing - [[Hashing]]
Array - [[Array]]
Binary Search - [[Binary Search]]
Linked List - [[Linked List]]
Stack - [[DSA/Stack/Stack|Stack]]
Queue - [[DSA/Queue/Queue|Queue]]
Bit Manipulation - [[Bit Manipulation]]
Binary Tree - [[Binary Tree]]
Binary Search Tree - [[Binary Search Tree]]
Graph - [[Graph]]
DisjointSet - [[DisjointSet Data Structure]]
Dynamic Programming - [[Dynamic Programming]]
Greedy - [[Greedy]]
