---
id: LU5_Recursion
aliases: []
tags: []
---

# TMF1434 – Data Structure and Algorithms

<!--toc:start-->
- [TMF1434 – Data Structure and Algorithms](#tmf1434-data-structure-and-algorithms)
  - [LU5: Recursion – Summary Notes](#lu5-recursion-summary-notes)
  - [1. What is Recursion?](#1-what-is-recursion)
  - [2. Recursive Definition](#2-recursive-definition)
  - [3. Classic Example: Factorial](#3-classic-example-factorial)
  - [4. Direct vs Indirect Recursion](#4-direct-vs-indirect-recursion)
  - [5. Tail Recursion](#5-tail-recursion)
  - [6. Problem Solving with Recursion](#6-problem-solving-with-recursion)
    - [6.1 Largest Element in an Array](#61-largest-element-in-an-array)
    - [6.2 Print a Linked List in Reverse Order](#62-print-a-linked-list-in-reverse-order)
    - [6.3 Fibonacci Numbers](#63-fibonacci-numbers)
  - [7. How to Design a Recursive Function](#7-how-to-design-a-recursive-function)
  - [8. Recursion vs Iteration](#8-recursion-vs-iteration)
  - [9. Recursion and Backtracking](#9-recursion-and-backtracking)
    - [Example: 4-Queens / 8-Queens Puzzle](#example-4-queens-8-queens-puzzle)
<!--toc:end-->

## LU5: Recursion – Summary Notes

---

## 1. What is Recursion?

Recursion is the process of solving a problem by **reducing it to smaller versions of itself**. A recursive function is one that calls itself. It provides an elegant alternative to repetitive iterative tasks and is especially powerful for problems that are naturally self-similar (e.g. fractals like the Sierpinski Gasket and Koch Snowflake).

---

## 2. Recursive Definition

Every recursive definition must have:

1. **Base case(s)** – a condition that stops the recursion and is solved directly.
2. **General case** – the problem is expressed in terms of a smaller version of itself, which must eventually reduce to the base case.

> ⚠️ If no base case is reached, the result is **infinite recursion**, which leads to abnormal program termination (stack overflow).

---

## 3. Classic Example: Factorial

**Mathematical definition:**
```
0! = 1                    (base case)
n! = n × (n − 1)!         (general case, if n > 0)
```

**C++ implementation:**
```cpp
int fact(int num) {
    if (num == 0)
        return 1;
    else
        return num * fact(num - 1);
}
```

**Trace of `fact(3)`:**
```
fact(3) → 3 * fact(2)
            → 2 * fact(1)
                → 1 * fact(0)
                    → returns 1
                → returns 1
            → returns 2
        → returns 6
```

---

## 4. Direct vs Indirect Recursion

| Type | Description |
|------|-------------|
| **Direct** | A function calls itself directly |
| **Indirect** | Function A calls Function B, which eventually calls Function A again |

---

## 5. Tail Recursion

A recursive function is **tail recursive** if the **last executed statement is the recursive call** — meaning no pending operation remains after the call returns.

**Tail recursive example:**
```cpp
void printDescend(int num) {
    if (num < 0) return;
    std::cout << " " << num;
    printDescend(num - 1);  // last statement = recursive call
}
```

**Non-tail recursive example (factorial):**
```cpp
int fact(int x) {
    if (x == 0) return 1;
    else return x * fact(x - 1);  // multiplication is pending → NOT tail recursive
}
```

**Tail recursive version of factorial:**
```cpp
int fact_T(int n, int result) {
    if (n == 1) return result;
    return fact_T(n - 1, n * result);
}
int fact(int n) { return fact_T(n, 1); }
```

**Advantages of tail recursion:**
- Easy to convert to an iterative loop
- Smart compilers can auto-optimise it
- Used in languages without explicit loop structures (e.g. Prolog, Lisp)

---

## 6. Problem Solving with Recursion

### 6.1 Largest Element in an Array

**Base case:** List has 1 element → that element is the largest.  
**General case:** Find the largest in `list[a+1]...list[b]`, then compare with `list[a]`.

```cpp
int largest(const int list[], int lowerIndex, int upperIndex) {
    int max;
    if (lowerIndex == upperIndex)
        return list[lowerIndex];
    else {
        max = largest(list, lowerIndex + 1, upperIndex);
        if (list[lowerIndex] >= max)
            return list[lowerIndex];
        else
            return max;
    }
}
```

---

### 6.2 Print a Linked List in Reverse Order

Since a singly linked list can only be traversed forward, recursion is used to defer printing until the end of the list is reached.

**Base case:** List is empty → do nothing.  
**General case:** Print the tail first, then print the current node.

```cpp
void reversePrint(nodeType<Type> *current) const {
    if (current != NULL) {
        reversePrint(current->link);   // print the tail
        cout << current->info << " "; // print the node
    }
}
```

---

### 6.3 Fibonacci Numbers

The Fibonacci sequence: `1, 1, 2, 3, 5, 8, 13, ...`  
Each term is the sum of the two previous terms.

**Recursive definition:**
```
rFibNum(a, b, 1) = a
rFibNum(a, b, 2) = b
rFibNum(a, b, n) = rFibNum(a, b, n-1) + rFibNum(a, b, n-2)   if n > 2
```

**C++ implementation:**
```cpp
int rFibNum(int a, int b, int n) {
    if (n == 1) return a;
    else if (n == 2) return b;
    else return rFibNum(a, b, n - 1) + rFibNum(a, b, n - 2);
}
```

---

## 7. How to Design a Recursive Function

1. Understand the problem requirements.
2. Determine the limiting condition (e.g. list size reaches 0 or 1).
3. Identify and directly solve all **base cases**.
4. Express each **general case** in terms of a smaller version of the problem.

---

## 8. Recursion vs Iteration

Both can usually solve the same problems, but there are trade-offs:

| Aspect | Recursion | Iteration |
|--------|-----------|-----------|
| Readability | Often more elegant | Can be more verbose |
| Performance | Higher overhead (function call stack) | Generally faster |
| Memory usage | More (stack frames per call) | Less |
| Best for | Naturally recursive problems (trees, backtracking) | Simple loops |

**Guideline:** Prefer iteration when the iterative solution is equally clear. Use recursion when the algorithm is naturally recursive (e.g. tree traversal, backtracking).

**Fibonacci comparison (number of recursive calls):**

| n  | Iterative assignments | Recursive calls |
|----|----------------------|-----------------|
| 6  | 15                   | 25              |
| 10 | 27                   | 177             |
| 20 | 57                   | 21,891          |
| 30 | 87                   | 2,692,537       |

> The recursive Fibonacci grows exponentially in calls, while iterative grows linearly — a clear case where iteration is preferred.

---

## 9. Recursion and Backtracking

**Backtracking** is an algorithm strategy that:
1. Builds a solution incrementally (partial solutions).
2. Abandons a partial solution when it cannot lead to a valid result (**dead end**).
3. **Backs up** and tries another option.

### Example: 4-Queens / 8-Queens Puzzle

Place N queens on an N×N chessboard such that no two queens attack each other (no shared row, column, or diagonal).

**Key check — `canPlaceQueen(k, i)`:** Can the k-th queen be placed in column `i` of row `k`?

```cpp
for (int j = 0; j < k; j++)
    if ((queensInRow[j] == i)                      // same column
        || (abs(queensInRow[j] - i) == abs(j - k))) // same diagonal
        return false;
return true;
```

**Backtracking steps (4-Queens example):**
1. Place queen 1 at row 1, col 1.
2. Place queen 2 at the first valid column in row 2.
3. Continue placing queens row by row.
4. If no valid column exists → **backtrack** to the previous row and try the next column.
5. Repeat until all queens are placed (solution found) or all options are exhausted.
