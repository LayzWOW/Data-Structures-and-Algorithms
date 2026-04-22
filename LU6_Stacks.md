---
id: LU6_Stacks
aliases: []
tags: []
---

# TMF1434 – Data Structure and Algorithms

<!--toc:start-->
- [TMF1434 – Data Structure and Algorithms](#tmf1434-data-structure-and-algorithms)
  - [LU6: Stack](#lu6-stack)
  - [What is a Stack?](#what-is-a-stack)
  - [Stack Applications in Computer Science](#stack-applications-in-computer-science)
  - [Stack ADT Operations](#stack-adt-operations)
  - [Implementation 1: Stack as Array (stackType)](#implementation-1-stack-as-array-stacktype)
    - [Core Function Implementations](#core-function-implementations)
    - [Time Complexity (Array-based)](#time-complexity-array-based)
  - [Implementation 2: Stack as Linked List (linkedStackType)](#implementation-2-stack-as-linked-list-linkedstacktype)
    - [Core Function Implementations](#core-function-implementations-1)
    - [Time Complexity (Linked List-based)](#time-complexity-linked-list-based)
  - [Comparison: Array vs Linked List Stack](#comparison-array-vs-linked-list-stack)
  - [Application: Printing a Linked List Backward (Non-recursive)](#application-printing-a-linked-list-backward-non-recursive)
  - [STL `stack` Class](#stl-stack-class)
  - [Quick Review Answers](#quick-review-answers)
<!--toc:end-->

## LU6: Stack

---

## What is a Stack?

A stack is a **Last-In, First-Out (LIFO)** data structure. Elements are added and removed from only one end, called the **top**. Unlike arrays and linked lists (which allow random access), a stack restricts access to just the top element.

**Real-life analogies:** a stack of books, coins, trays, or boxes — you must remove the top item before you can reach items below it.

---

## Stack Applications in Computer Science

- **Balanced symbol checking** — compilers use stacks to detect missing brackets `{}`, `()`, `[]`
- **Undo operations** — text editors push changes onto a stack and pop them to undo
- **Arithmetic expression parsing** — e.g., converting infix to postfix:
  - `(2+3)*(4+5)` → `2 3 + 4 5 + *`

---

## Stack ADT Operations

| Operation | Description | Precondition |
|---|---|---|
| `initializeStack()` | Resets the stack to empty | — |
| `isEmptyStack()` | Returns `true` if stack is empty | — |
| `isFullStack()` | Returns `true` if stack is full | — |
| `push(item)` | Adds an element to the top | Stack must not be full |
| `top()` | Returns (peeks) the top element | Stack must not be empty |
| `pop()` | Removes the top element | Stack must not be empty |

---

## Implementation 1: Stack as Array (stackType)

The array-based stack uses:
- `list` — a pointer to the underlying array
- `maxStackSize` — maximum capacity
- `stackTop` — index tracking the top (ranges from `0` to `maxStackSize`)

**Key rule:** `stackTop` equals the number of elements; `stackTop - 1` is the index of the actual top element.

### Core Function Implementations

**initializeStack:**
```cpp
stackTop = 0;
```

**isEmptyStack:**
```cpp
return (stackTop == 0);
```

**isFullStack:**
```cpp
return (stackTop == maxStackSize);
```

**push:**
```cpp
if (!isFullStack()) {
    list[stackTop] = newItem;  // store at current top index
    stackTop++;                // increment top
} else {
    cout << "Cannot add to a full stack." << endl;
}
```
> **Overflow:** pushing onto a full stack triggers the error message.

**pop:**
```cpp
if (!isEmptyStack())
    stackTop--;  // simply decrement; data is logically removed
else
    cout << "Cannot remove from an empty stack." << endl;
```
> **Underflow:** popping from an empty stack triggers the error message.

**top:**
```cpp
assert(stackTop != 0);
return list[stackTop - 1];
```

### Time Complexity (Array-based)

| Function | Complexity |
|---|---|
| isEmptyStack, isFullStack, initializeStack | O(1) |
| constructor, destructor | O(1) |
| top, push, pop | O(1) |
| copyStack, copy constructor, assignment operator | O(n) |

---

## Implementation 2: Stack as Linked List (linkedStackType)

**Why linked list?** Arrays have fixed size. A linked list allocates memory dynamically, so the stack can grow as needed.

Design decision: push and pop at the **head** of the list (constant time). Removing from the tail would require O(n) traversal.

`stackTop` here is a **pointer** to the top node (not an integer index). An empty stack has `stackTop == NULL`.

### Core Function Implementations

**initializeStack** — deallocates all nodes:
```cpp
nodeType<Type> *temp;
while (stackTop != NULL) {
    temp = stackTop;
    stackTop = stackTop->link;
    delete temp;
}
```

**isEmptyStack / isFullStack:**
```cpp
return (stackTop == NULL);   // isEmptyStack
return false;                // isFullStack — linked stack is never full
```

**push** — inserts at the front of the list:
```cpp
nodeType<Type> *newNode = new nodeType<Type>;
newNode->info = newElement;
newNode->link = stackTop;    // point new node to current top
stackTop = newNode;          // update top to new node
```

**top:**
```cpp
assert(stackTop != NULL);
return stackTop->info;
```

**pop** — removes front node:
```cpp
if (stackTop != NULL) {
    nodeType<Type> *temp = stackTop;
    stackTop = stackTop->link;
    delete temp;
} else {
    cout << "Cannot remove from an empty stack." << endl;
}
```

### Time Complexity (Linked List-based)

| Function | Complexity |
|---|---|
| isEmptyStack, isFullStack | O(1) |
| constructor | O(1) |
| top, push, pop | O(1) |
| initializeStack, destructor | O(n) |
| copyStack, copy constructor, assignment operator | O(n) |

> **Key difference from array:** `initializeStack` and `destructor` are O(n) for linked list (must traverse and free all nodes), but O(1) for array.

---

## Comparison: Array vs Linked List Stack

| Function | Array | Linked List |
|---|---|---|
| initializeStack | O(1) | O(n) |
| destructor | O(1) | O(n) |
| push / pop / top | O(1) | O(1) |
| copyStack / copy constructor | O(n) | O(n) |
| Can overflow? | Yes (fixed size) | No (dynamic) |

---

## Application: Printing a Linked List Backward (Non-recursive)

A stack can replace recursion. To print a linked list in reverse:

**Phase 1 — Push all nodes:**
```cpp
current = first;
while (current != NULL) {
    stack.push(current);
    current = current->link;
}
```

**Phase 2 — Pop and print:**
```cpp
while (!stack.isEmptyStack()) {
    current = stack.top();
    stack.pop();
    cout << current->info << " ";
}
```

For a list `5 → 10 → 15`, this prints `15 10 5`.

---

## STL `stack` Class

C++ STL provides a built-in `stack` class via `#include <stack>`.

| Operation | Effect |
|---|---|
| `size()` | Returns number of elements |
| `empty()` | Returns `true` if empty |
| `push(item)` | Inserts item onto the stack |
| `top()` | Returns top element (no removal) |
| `pop()` | Removes top element |

---

## Quick Review Answers

1. **LIFO** = Last-In, First-Out — the last element pushed is the first to be popped.
2. **pop** retrieves (removes) the **top** element — the most recently pushed one.
3. All stacks perform **push** (add an element) and **pop** (remove the top element).
