---
id: LU1_Principles_and_CPP_Classes
aliases: []
tags: []
---

# LU1: Principles and C++ Classes

<!--toc:start-->
- [Principles and C++ Classes](#principles-and-c-classes)
- [Principles and C++ Classes](#principles-and-c-classes-1)
  - [Table of Contents](#table-of-contents)
  - [1. Software Life Cycle](#1-software-life-cycle)
  - [2. Software Development Phases](#2-software-development-phases)
    - [Phase 1 — Analysis](#phase-1-analysis)
    - [Phase 2 — Design](#phase-2-design)
    - [Phase 3 — Implementation](#phase-3-implementation)
    - [Phase 4 — Testing & Debugging](#phase-4-testing-debugging)
  - [3. Algorithm Analysis — Big-O Notation](#3-algorithm-analysis-big-o-notation)
    - [Time Efficiency](#time-efficiency)
    - [Space Efficiency](#space-efficiency)
    - [Asymptotic Growth](#asymptotic-growth)
    - [Common Big-O Functions](#common-big-o-functions)
  - [4. C++ Classes](#4-c-classes)
    - [What is a Class?](#what-is-a-class)
    - [Member Access Specifiers](#member-access-specifiers)
    - [UML Diagram (clockType example)](#uml-diagram-clocktype-example)
    - [Declaring & Using Objects](#declaring-using-objects)
    - [Implementing Member Functions](#implementing-member-functions)
    - [Passing Objects as Parameters](#passing-objects-as-parameters)
    - [Constructors](#constructors)
    - [Destructors](#destructors)
    - [Structs](#structs)
  - [5. Summary](#5-summary)
<!--toc:end-->

## Table of Contents

- [Software Life Cycle](#software-life-cycle)
- [Software Development Phase](#software-development-phase)
  - [Analysis](#analysis)
  - [Design](#design)

---

## 1. Software Life Cycle

Three fundamental stages: **Development → Use → Maintenance**

Software is retired when it becomes too expensive to maintain.

---

## 2. Software Development Phases

### Phase 1 — Analysis
- Understand the problem thoroughly
- Identify requirements
- Break complex problems into sub-problems

### Phase 2 — Design
Design an algorithm: a step-by-step process that produces a solution in a **finite** amount of time.

Two design approaches:

| Approach | Also Known As | Description |
|---|---|---|
| **Structured Design** | Top-down, modular programming | Divide → solve sub-problems → combine |
| **Object-Oriented Design (OOD)** | OOP | Identify objects, define their data, operations |

**OOD Three Principles:**

| Principle | Description |
|---|---|
| **Encapsulation** | Combines data + operations into a single unit (class) |
| **Inheritance** | Create new types from existing types |
| **Polymorphism** | Same expression performs different operations depending on the object |


### Phase 3 — Implementation
- Write and compile code
- Implement classes and functions from design phase
- **Pre-condition**: what must be true before a function is called
- **Post-condition**: what is true after a function completes

### Phase 4 — Testing & Debugging

| | Black-Box Testing | White-Box Testing |
|---|---|---|
| **Also called** | Behavioral testing | Clear box testing |
| **Internal knowledge?** | Not required | Required |
| **Done by** | Independent tester | Developer |
| **Applicable levels** | Integration, System, Acceptance | Unit, Integration |
| **Based on** | Requirement specs | Detailed design |

---

## 3. Algorithm Analysis — Big-O Notation

**Big-O** describes the **worst-case** performance/complexity of an algorithm (time or space).

### Time Efficiency
Count the number of operations — not actual clock time (varies by machine). Operation count stays constant across machines.

**Example: Fixed operations**
```cpp
cout << "Enter two numbers";       // 1 op
cin >> num1 >> num2;               // 2 ops
if (num1 >= num2) max = num1;      // 1 + 1 op (either branch)
else max = num2;
cout << max << endl;               // 3 ops
// Total = 8 operations (fixed)
```

**Example: Loop with dominant term**
- 5 ops before loop
- Loop body = 5 ops × n, +1 termination check
- ~9 ops after loop
- Total ≈ **5n + 14** → dominant term is **5n** → **O(n)**

### Space Efficiency
Memory consumed by the algorithm.

| Function | Memory Used |
|---|---|
| `int Sum(int num1, int num2)` | 16 bytes (fixed) |
| `int Sum(int NumArr[], int N)` | 4N + 16 bytes |

### Asymptotic Growth
As n grows large, lower-order terms become insignificant.

- `f(n) = n² + 4n + 20` → dominant term is **n²** → **O(n²)**

### Common Big-O Functions

| g(n) | Growth Rate |
|---|---|
| O(1) | Constant — doesn't depend on n |
| O(log₂n) | Logarithmic — grows slowly |
| O(n) | Linear — proportional to n |
| O(n log₂n) | Faster than linear |
| O(n²) | Quadratic — quadruples when n doubles |
| O(2ⁿ) | Exponential — squares when n doubles |

**Big-O derivation rules:**

| Function | Big-O |
|---|---|
| `f(n) = an + b` | O(n) |
| `f(n) = n² + 5n + 1` | O(n²) |
| `f(n) = 4n⁶ + 3n³ + 1` | O(n⁶) |

**Practical examples:**

| Complexity | Behaviour |
|---|---|
| O(1) | 1 item = 1s, 100 items = 1s |
| O(n) | 1 item = 1s, 100 items = 100s |
| O(n²) | 1 item = 1s, 100 items = 10,000s |

---

## 4. C++ Classes

### What is a Class?
- A collection of a fixed number of components called **members**
- Implements **encapsulation** — data + operations in one unit

```cpp
class classIdentifier {
      class members list
};
```

### Member Access Specifiers

| Specifier | Access |
|---|---|
| `private` | Default. Only accessible within the class |
| `public` | Accessible by anyone |
| `protected` | Accessible by the class and its derived classes |

**General rule:** data members → `private`; operations the user needs → `public`

### UML Diagram (clockType example)
```
clockType
─────────────────────────────
- hr: int
- min: int
- sec: int
─────────────────────────────
+ setTime(int, int, int): void
+ getTime(int&, int&, int&): void const
+ printTime(): void const
+ incrementSeconds(): int
+ incrementMinutes(): int
+ incrementHours(): int
+ equalTime(clockType): bool const
+ clockType(int, int, int)
+ clockType()
```
`-` = private, `+` = public

### Declaring & Using Objects

```cpp
// Default constructor
Student student1;

// Constructor with parameters
Student student2("John", 21);

// Accessing members with dot operator
student2.Age = 20;
```

### Implementing Member Functions
Use the **scope resolution operator** `::`:

```cpp
void clockType::setTime(int hours, int minutes, int seconds) {
      if (0 <= hours && hours < 24) hr = hours;
      else hr = 0;
      // same for min and sec...
}
```

### Passing Objects as Parameters

| Method | Behaviour |
|---|---|
| By value | Copy made; original unaffected |
| By reference | Address passed; changes affect original |
| `const` reference | Address passed; cannot modify value |

### Constructors
- Same name as the class, **no return type**
- Executes automatically when object enters scope
- Can be overloaded (different parameter lists)
- **Default constructor**: no parameters or all default params

```cpp
clockType()
clockType(int h = 0, int m = 0, int s = 0)
```

### Destructors
- Name = `~ClassName`
- No parameters, no return type, one per class
- Automatically runs when object goes out of scope

### Structs
- Special class where all members are `public` by default
- Defined with the `struct` keyword instead of `class`

---

## 5. Summary

- Software life cycle: Analysis -> Design -> Implementation -> Testing
- Algorithm: step-by-step solution obtained in finite time
- OOD principles: Encapsulation, Inheritance, Polymorphism
- Constructors initialise class instance variables automatically
- UML: graphical notation describing a class and its members
- Destructors: no type, auto-execute when object goes out of scope
- Structs: special class with all-public members
