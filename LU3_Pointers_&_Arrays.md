---
id: LU3_Pointers_&_Arrays
aliases: []
tags: []
---

# TMF1434 – LU3: Pointers and Arrays

<!--toc:start-->
- [TMF1434 – Data Structure and Algorithms](#tmf1434-data-structure-and-algorithms)
  - [LEC03: Pointers & Arrays](#lec03-pointers-arrays)
  - [Learning Objectives](#learning-objectives)
  - [1. Pointer Variables](#1-pointer-variables)
    - [Avoiding Ambiguity](#avoiding-ambiguity)
  - [2. The Address Operator `&`](#2-the-address-operator)
  - [ ] [3. The Dereference Operator `*`](#3-the-dereference-operator)
  - [4. The NULL Pointer](#4-the-null-pointer)
  - [5. Dynamic Variables](#5-dynamic-variables)
  - [6. Operator `new`](#6-operator-new)
    - [Single Object](#single-object)
    - [Dynamic Array](#dynamic-array)
      - [Using Array Notation (pointer stays fixed)](#using-array-notation-pointer-stays-fixed)
  - [7. Operator `delete`](#7-operator-delete)
  - [8. Dangling Pointers](#8-dangling-pointers)
    - [How to Avoid](#how-to-avoid)
  - [9. Memory Leaks](#9-memory-leaks)
    - [Example 1 — Inaccessible Object](#example-1-inaccessible-object)
    - [Example 2 — Inaccessible Object](#example-2-inaccessible-object)
  - [10. Array Name as a Constant Pointer](#10-array-name-as-a-constant-pointer)
  - [11. Passing Pointers to Functions](#11-passing-pointers-to-functions)
    - [Passing by Reference](#passing-by-reference)
    - [Example](#example)
  - [12. Pointer as Function Return Value](#12-pointer-as-function-return-value)
  - [13. Dynamic Two-Dimensional Arrays](#13-dynamic-two-dimensional-arrays)
    - [Method 1 — Array of Pointers](#method-1-array-of-pointers)
    - [Method 2 — Pointer to Pointer (`**`)](#method-2-pointer-to-pointer)
  - [14. Shallow vs. Deep Copy](#14-shallow-vs-deep-copy)
    - [Shallow Copy](#shallow-copy)
    - [Deep Copy](#deep-copy)
  - [15. Pointers and Classes](#15-pointers-and-classes)
    - [Destructor](#destructor)
    - [Overloading the Assignment Operator](#overloading-the-assignment-operator)
    - [Copy Constructor](#copy-constructor)
  - [16. Summary — Classes with Dynamic Data](#16-summary-classes-with-dynamic-data)
  - [17. Array-Based Lists](#17-array-based-lists)
    - [Operations on a List](#operations-on-a-list)
  - [18. Sequential Search (`seqSearch`)](#18-sequential-search-seqsearch)
  - [19. Insert (`insert`)](#19-insert-insert)
  - [20. Remove (`remove`)](#20-remove-remove)
  - [21. Time Complexity of List Operations](#21-time-complexity-of-list-operations)
<!--toc:end-->

## LEC03: Pointers & Arrays

---

## Learning Objectives

- Understand the pointer data type and pointer variables
- Declare and manipulate pointer variables
- Use the address-of operator (`&`) and dereference operator (`*`)
- Discover dynamic variables and use `new` / `delete`
- Understand pointer arithmetic and dynamic arrays
- Distinguish between shallow and deep copies
- Explore classes with pointer data members
- Process lists using dynamic arrays

---

## 1. Pointer Variables

A **pointer variable** stores the **address** (not the value) of a data object.

```cpp
data_type *variable_name;

// Example:
int *p;   // p is a pointer to an int
```

### Avoiding Ambiguity

```cpp
int* p, q;    // Only p is a pointer; q is an int
int *p, q;    // Same as above — attach * to the variable name
int *p, *q;   // Both p and q are pointer variables
```

> **Best practice:** Attach `*` to the variable name to avoid confusion.

---

## 2. The Address Operator `&`

`&` is a **unary operator** that returns the memory address of its operand.

```cpp
int x = 25;
int *p;
p = &x;   // p now holds the address of x
```

After this, `p` and `x` refer to the same memory location.

---

## 3. The Dereference Operator `*`

`*` (also called the **indirection operator**) gives access to the object a pointer points to.

```cpp
int x = 25;
int *p;
p = &x;

cout << *p << endl;  // prints 25
*p = 55;             // x is now 55
```

---

## 4. The NULL Pointer

A pointer variable can be in one of three states:
1. **Unassigned** — undefined behavior if used
2. **Points to a data object**
3. **Null pointer** — points to nothing

```cpp
p = 0;      // null pointer
p = NULL;   // equivalent
```

> C++ does **not** automatically initialise pointer variables. Always initialise them.

---

## 5. Dynamic Variables

Variables created **during program execution** are called **dynamic variables**.

Two key operators:
| Operator | Purpose |
|----------|---------|
| `new`    | Allocate memory on the heap (free store) |
| `delete` | Deallocate memory from the heap |

---

## 6. Operator `new`

```cpp
new data_type          // allocate a single object
      new data_type[n]       // allocate an array of n objects
```

### Single Object
```cpp
int *p;
p = new int;   // allocate memory for one int
*p = 28;       // store 28 in that memory

// Or in one line:
int *p = new int;
```

### Dynamic Array
```cpp
int *p;
p = new int[10];  // allocate array of 10 ints

*p = 25;   // stores 25 in first element
p++;       // move pointer to next element
*p = 35;   // stores 35 in second element
```

#### Using Array Notation (pointer stays fixed)
```cpp
for (int j = 0; j < 10; j++)
      p[j] = 0;   // initialise all elements to 0
```

---

## 7. Operator `delete`

```cpp
delete pointer_variable;      // deallocate a single object
delete [] pointer_variable;   // deallocate a dynamic array
```

> Every call to `new` should have a matching call to `delete`.

---

## 8. Dangling Pointers

A **dangling pointer** is one that:
- Has **not been initialised**, or
- Has been **deleted**

```cpp
// Example:
delete intPtrA;
// intPtrB (which also pointed to the same memory) is now dangling!
```

### How to Avoid
- Always initialise pointer variables
- Never use a pointer after it has been deleted

---

## 9. Memory Leaks

A **memory leak** occurs when:
1. Dynamic data is allocated but never deallocated
2. Inaccessible objects are left on the heap

### Example 1 — Inaccessible Object
```cpp
intPtrA = new int;   // original object pointed to by intPtrA is now lost
```

### Example 2 — Inaccessible Object
```cpp
intPtrA = intPtrB;   // original object pointed to by intPtrA is now lost
```

> **Rule:** Every `new` must have a matching `delete`.

---

## 10. Array Name as a Constant Pointer

An array name is a **constant pointer** — its value (the base address) **cannot be changed**.

```cpp
int list[5];
// list holds address 1000 (for example) and this cannot be altered

list[0] = 25;   // stores 25 in first element
list[2] = 78;   // stores 78 in third element
```

---

## 11. Passing Pointers to Functions

Pointer variables can be passed **by value** or **by reference**.

### Passing by Reference
```cpp
void example(int* &p, double *q)
{
      // p is a reference to a pointer to int
}
```

### Example
```cpp
double Total(int* &p, double *num)
{
      *p = *p + 1;
      return *p + *num;
}
```

---

## 12. Pointer as Function Return Value

```cpp
int* testExp(...)
{
      // returns a pointer to int
}
```

---

## 13. Dynamic Two-Dimensional Arrays

### Method 1 — Array of Pointers
```cpp
int *board[4];   // array of 4 int pointers

for (int row = 0; row < 4; row++)
      board[row] = new int[6];   // each row has 6 columns
```

Result: `board` is a 4×6 two-dimensional array.

### Method 2 — Pointer to Pointer (`**`)
```cpp
int **board;
int rows = 4, cols = 6;

board = new int*[rows];          // create array of row pointers
for (int row = 0; row < rows; row++) { 
      board[row] = new int[cols];  // create each rows
}
```

> A 2D array is a **pointer to an array of pointers**, where each pointer points to a row.

---

## 14. Shallow vs. Deep Copy

### Shallow Copy
Only the **pointer** is copied — both pointers share the same data.

```cpp
int *first, *second;
second = first;   // shallow copy — both point to the same memory
```

⚠️ If `delete [] second` is called, `first` becomes **invalid** (dangling pointer).

### Deep Copy
Each pointer has its **own copy** of the data.

```cpp
second = new int[10];

for (int j = 0; j < 10; j++) { second[j] = first[j]; }  // deep copy — independent data
```

---

## 15. Pointers and Classes

Classes with pointer members need three special functions:

| Function | Purpose |
|----------|---------|
| **Destructor** | Frees dynamic memory when object goes out of scope |
| **Overloaded Assignment Operator** | Performs deep copy during assignment |
| **Copy Constructor** | Performs deep copy during initialisation |

### Destructor
```cpp
pointerDataClass::~pointerDataClass()
{
      delete [] p;   // deallocate the dynamic array
}
```

### Overloading the Assignment Operator

**Prototype:**
```cpp
const className& operator=(const className&);
```

**Definition:**
```cpp
const className& className::operator=(const className& rightObject)
{
      if (this != &rightObject)   // avoid self-assignment
      {
            // copy rightObject into this object (deep copy)
      }
      return *this;
}
```

### Copy Constructor

**Prototype:**
```cpp
className(const className& otherObject);
```

The copy constructor executes automatically when:
1. An object is declared and initialised from another object
2. An object is passed **by value** to a function
3. A function **returns** an object

---

## 16. Summary — Classes with Dynamic Data

Classes that use dynamic memory typically require:

- ✅ One or more **constructors**
- ✅ A **destructor** (cleanup the free store)
- ✅ A **copy constructor** (deep copy on initialisation)
- ✅ An **overloaded assignment operator** (deep copy on assignment)

---

## 17. Array-Based Lists

An **array** is used to represent a list — a collection of elements of the same type, ordered by position. Duplicate values may occur.

### Operations on a List

| Operation | Description |
|-----------|-------------|
| Create | Initialise to empty |
| isEmpty | Check if list has no elements |
| isFull | Check if list is at max capacity |
| listSize | Return number of elements |
| insert | Add an item (no duplicates) |
| remove | Delete an item |
| search | Find an item (sequential search) |
| retrieveAt | Get item at a position |
| replaceAt | Replace item at a position |
| clearList | Empty the list |

---

## 18. Sequential Search (`seqSearch`)

```cpp
template <class elemType>
int arrayListType<elemType>::seqSearch(const elemType& item) const
{
      int loc;
      bool found = false;

      for (loc = 0; loc < length; loc++)
            if (list[loc] == item)
            {
                  found = true;
                  break;
            }

      if (found) { return loc; }
      else { return -1; }
}
```

---

## 19. Insert (`insert`)

```cpp
template <class elemType>
void arrayListType<elemType>::insert(const elemType& insertItem)
{
      int loc;

      if (length == 0) { list[length++] = insertItem; }
      else if (length == maxSize) {cerr << "Cannot insert in a full list." << endl; }
      else {
            loc = seqSearch(insertItem);
            if (loc == -1) { list[length++] = insertItem; }
            else { cerr << "the item to be inserted is already in the list." << endl; }
      }
}
```

---

## 20. Remove (`remove`)

```cpp
template <class elemType>
void arrayListType<elemType>::remove(const elemType& removeItem)
{
      int loc;

      if (length == 0) { cerr << "Cannot delete from an empty list." << endl; }
      else {
            loc = seqSearch(removeItem);
            if (loc != -1) { removeAt(loc); }
            else { cout << "The item to be deleted is not in the list." << endl; }
      }
}
```

---

## 21. Time Complexity of List Operations

| Function | Time Complexity |
|----------|----------------|
| `isEmpty` | O(1) |
| `isFull` | O(1) |
| `listSize` | O(1) |
| `maxListSize` | O(1) |
| `print` | O(n) |
| `isItemAtEqual` | O(1) |
| `insertAt` | O(n) |
| `insertEnd` | O(1) |
| `removeAt` | O(n) |
| `retrieveAt` | O(1) |
| `replaceAt` | O(n) |
| `clearList` | O(1) |
| `constructor` | O(1) |
| `destructor` | O(1) |
| `copy constructor` | O(n) |
| `overloaded assignment` | O(n) |
| `seqSearch` | O(n) |
| `insert` | O(n) |
| `remove` | O(n) |

---

*References: "Data Structures Using C++" by D. S. Malik; "C++ by Example" by Greg Perry*
