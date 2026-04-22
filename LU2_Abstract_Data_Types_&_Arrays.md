---
id: LU2_Abstract_Data_Types_and_Arrays
aliases: []
tags: []
---

# TMF1434 — LU2: Abstract Data Types & Arrays

<!--toc:start-->
- [TMF1434 — LU2: Abstract Data Types & Arrays](#tmf1434-lu2-abstract-data-types-arrays)
  - [1. Abstract Data Types (ADTs)](#1-abstract-data-types-adts)
    - [Implementation of an ADT](#implementation-of-an-adt)
    - [ADT vs Data Structure](#adt-vs-data-structure)
  - [2. ADT Example — Airplane Seats](#2-adt-example-airplane-seats)
  - [3. Arrays](#3-arrays)
    - [Array as an ADT](#array-as-an-adt)
    - [Array Types](#array-types)
    - [One-Dimensional Static Array](#one-dimensional-static-array)
    - [One-Dimensional Dynamic Array](#one-dimensional-dynamic-array)
    - [Display Function Example](#display-function-example)
    - [Multidimensional Arrays (2D)](#multidimensional-arrays-2d)
    - [Three-Dimensional Array](#three-dimensional-array)
  - [4. Data Abstraction & Classes](#4-data-abstraction-classes)
    - [clockType ADT Definition](#clocktype-adt-definition)
  - [5. Identifying Classes in OOD](#5-identifying-classes-in-ood)
<!--toc:end-->

---

## 1. Abstract Data Types (ADTs)

An **ADT** = a collection of data items **together with the operations** on that data.

- "Abstract" = define **what** can be done, not **how** it's implemented
- Focus on logical behaviour, not internal details

**Two things to identify when solving a problem:**
1. The collection of data items
2. The basic operations that must be performed on them

### Implementation of an ADT
Consists of:
- **Storage structures** to hold the data
- **Algorithms** for each operation

### ADT vs Data Structure

| Term | Level | Example |
|---|---|---|
| ADT | Logical/conceptual | Stack |
| Data Structure | Implementation | Array or Linked List |

> A Stack is an ADT; it can be *implemented* using an array or a linked list.

---

## 2. ADT Example — Airplane Seats

Problem: 10 seats, need to list available and reserve seats.

| Solution | Approach | Problem |
|---|---|---|
| Solution 1 | 10 individual variables | Repetitive, doesn't scale |
| Solution 2 | Array of 10 elements | Cleaner, scalable with loops |

**Solution 2 — List available seats:**
```
For number from 0 to max_seats-1:
If seat[number] == ' ':
Display number
```

**Solution 2 — Reserve a seat:**
```
Read seat number
If seat[number] == ' ':
seat[number] = 'X'
Else:
Display "seat is occupied"
```

The ADT here:
- **Data**: list of seats
- **Operations**: scan seat status, change seat status

---

## 3. Arrays

### Array as an ADT

| ADT Property | C++ Array |
|---|---|
| Fixed size | Specify capacity |
| Ordered | Indices 0, 1, 2, …, capacity-1 |
| Same type | Specify element type |
| Direct access | Subscript operator `[]` |

### Array Types

| Type | Description |
|---|---|
| One-dimensional | Single index |
| Multi-dimensional | More than one index |
| Static | Memory allocated at compile time |
| Dynamic | Memory allocated at runtime (`new`) |

---

### One-Dimensional Static Array

```cpp
int b[10];
int b[10] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
int c[10] = {1, 2, 3};     // rest auto-filled with 0
int d[10] = {0};            // all zeros
int e[10] = {7};            // first = 7, rest = 0
char name[10] = "John Doe";
```

### One-Dimensional Dynamic Array

```cpp
int size = 10;
int *Arr = new int[size];

// or
int *Arr = nullptr;
Arr = new int[size];
```

### Display Function Example

```cpp
void display(int arr[], int num_values) {
      for (int i = 0; i < num_values; i++)
            cout << arr[i] << " ";
}
```

---

### Multidimensional Arrays (2D)

```cpp
// Declaration
int scoreTable[30][5];

// Declaration with initialisation
int scoreTable[2][5] = {{80,80,80,80,80},{60,60,60,60,60}};

// Access element: row 0, column 2
scoreTable[0][2] = 100;
```

- `scoreTable[2]` → entire row 2
- `scoreTable[1][3]` → row 1, column 3

### Three-Dimensional Array

```cpp
typedef double ThreeDimArray[NUM_ROWS][NUM_COLS][NUM_RANKS];
```

Use case: multiple pages of a student grade book (rows = students, columns = tests, ranks = pages/semesters)

---

## 4. Data Abstraction & Classes

**Data abstraction** = separating logical data properties from implementation details.

An ADT includes:
- Type name
- Domain (what values it holds)
- Set of operations

### clockType ADT Definition
```
dataTypeName: clockType

domain:
Each value is a time of day in hours, minutes, and seconds.

operations:
Set the time.
Return the time.
Print the time.
Increment by one second.
Increment by one minute.
Increment by one hour.
Compare two times for equality.
```

C++ classes are specifically designed to implement ADTs.

---

## 5. Identifying Classes in OOD

1. Start with the **problem description**
2. **Nouns** → candidate classes / objects
3. **Verbs** → candidate operations / methods
