---
id: LU7_Queues
aliases: []
tags: []
---

# LU7 — Queues

<!--toc:start-->
- [LU7 — Queues](#lu7-queues)
  - [1. Queue Concept](#1-queue-concept)
  - [2. Stack vs. Queue](#2-stack-vs-queue)
  - [3. Basic Queue Operations](#3-basic-queue-operations)
  - [4. Queue Applications](#4-queue-applications)
    - [Real-world](#real-world)
    - [Computer Science](#computer-science)
  - [5. Implementing a Queue as an Array](#5-implementing-a-queue-as-an-array)
    - [Required components](#required-components)
    - [How operations work](#how-operations-work)
    - [Problem with the Naïve Approach](#problem-with-the-naïve-approach)
    - [Solution 1 — Shift elements](#solution-1-shift-elements)
    - [Solution 2 — Circular Array *(preferred)*](#solution-2-circular-array-preferred)
  - [6. Detecting Empty vs. Full Queue (Circular Array)](#6-detecting-empty-vs-full-queue-circular-array)
    - [Fix 1 — Use a `count` variable](#fix-1-use-a-count-variable)
    - [Fix 2 — Reserved Slot](#fix-2-reserved-slot)
  - [7. Implementing a Queue as a Linked List](#7-implementing-a-queue-as-a-linked-list)
    - [Why prefer a linked list?](#why-prefer-a-linked-list)
    - [Structure](#structure)
  - [8. STL `queue` Class](#8-stl-queue-class)
  - [9. Priority Queues](#9-priority-queues)
    - [Characteristics](#characteristics)
    - [Implementations](#implementations)
    - [STL `priority_queue`](#stl-priorityqueue)
  - [Quick Review](#quick-review)
<!--toc:end-->

**TMF1434 Data Structure and Algorithms**
*Based on "Data Structures Using C++" by D. S. Malik*

---

## 1. Queue Concept

A **queue** is a linear data structure that follows the **FIFO (First-In, First-Out)** principle.

- Elements are **added at the rear** → *enqueue* operation
- Elements are **removed from the front** → *dequeue* operation
- Middle elements are **inaccessible**
- The element that has been in the queue the longest is always the first to be removed

**Real-life analogy:** A queue at a cashier — the first person to join is the first to be served.

---

## 2. Stack vs. Queue

| Feature | Stack | Queue |
|---|---|---|
| Add operation | `push` — at the **top** | `enqueue` — at the **rear** |
| Remove operation | `pop` — from the **top** | `dequeue` — from the **front** |
| Access points | Single end (top) | Two ends (front & rear) |
| Order | LIFO | FIFO |

---

## 3. Basic Queue Operations

| Operation | Description |
|---|---|
| `initializeQueue` | Create an empty queue |
| `isEmptyQueue` | Check if the queue has no elements |
| `isFullQueue` | Check if the queue has reached max capacity |
| `enqueue` / `addQueue` | Add a new element to the **rear** |
| `dequeue` / `deleteQueue` | Remove the element from the **front** |
| `front` | Return (peek) the first element without removing it |
| `back` | Return (peek) the last element without removing it |
| Destroy | Deallocate all queue memory |

---

## 4. Queue Applications

### Real-world
- Cashier lines in stores
- Tech support hold queues
- People on an escalator
- Traffic queues

### Computer Science
1. OS job scheduling with equal priority (e.g., print queue)
2. Multiprogramming — processes waiting for CPU time
3. Asynchronous data transfer (I/O buffers, pipes, sockets)
4. Shared resource management (CPU scheduling, disk scheduling)
5. Round-robin scheduling

---

## 5. Implementing a Queue as an Array

### Required components
- An **array** to store queue elements
- `queueFront` — index of the first element
- `queueRear` — index of the last element
- `maxQueueSize` — maximum capacity

### How operations work
- **`addQueue`:** Advance `queueRear`, then insert at `queueRear`
- **`deleteQueue`:** Retrieve element at `queueFront`, then advance `queueFront`

### Problem with the Naïve Approach
Given a sequence like `AAADADADADADA...`, `queueRear` eventually reaches the last array index even though the front portion of the array is empty.

- Looks like a full queue (`queueRear == maxQueueSize - 1`)
- Reality: only 2–3 elements remain; space is wasted at the front

### Solution 1 — Shift elements
- When rear overflows, slide all elements back toward index `[0]`
- Simple, but **inefficient** for large queues

### Solution 2 — Circular Array *(preferred)*
- Treat the array as a circle: after index `maxQueueSize - 1`, wrap around to `0`
- Use **modulo arithmetic** to advance indices:

```
queueRear = (queueRear + 1) % maxQueueSize
queueFront = (queueFront + 1) % maxQueueSize
```

Example: `(99 + 1) % 100 = 0` — wraps correctly to the start.

---

## 6. Detecting Empty vs. Full Queue (Circular Array)

**Problem:** After several enqueue/dequeue operations, both `queueFront` and `queueRear` can end up with the **same value** in both an empty queue and a full queue — ambiguous!

### Fix 1 — Use a `count` variable
- Increment `count` on `addQueue`, decrement on `deleteQueue`
- Empty: `count == 0`; Full: `count == maxQueueSize`
- Useful when you frequently need to know the queue size

### Fix 2 — Reserved Slot
- `queueFront` points to the slot **before** the actual first element (reserved, unused)
- `queueRear` still points to the actual last element
- **Empty:** `queueFront == queueRear`
- **Full:** the next slot after `queueRear` (circularly) would be the reserved slot

---

## 7. Implementing a Queue as a Linked List

### Why prefer a linked list?
- Arrays have a fixed size; a linked-list queue is **never truly full**
- Queue grows and shrinks dynamically with each operation
- No wasted space, no circular index arithmetic

### Structure
Maintain **two external pointers**:
- `front` — points to the first node
- `rear` / `back` — points to the last node

```
front → [item1 | •] → [item2 | •] → [item3 | •] → NULL
                                          ↑
                                         rear
```

- **Enqueue:** Create new node, attach at `rear`, advance `rear`
- **Dequeue:** Retrieve `front` node's data, advance `front`, delete old node
- Deletion from the front is simpler than insertion at the back

---

## 8. STL `queue` Class

Included via `#include <queue>`.

| Member function | Description |
|---|---|
| `size()` | Returns the number of elements in the queue |
| `empty()` | Returns `true` if the queue is empty |
| `push(item)` | Inserts `item` at the rear |
| `front()` | Returns the front element (does **not** remove it) |
| `back()` | Returns the rear element (does **not** remove it) |
| `pop()` | Removes the front element |

---

## 9. Priority Queues

A **priority queue** relaxes the strict FIFO rule. Elements with **higher priority** are served first, regardless of arrival order.

**Example:** In a hospital, a patient with life-threatening symptoms is seen before others who arrived earlier.

### Characteristics
- **Enqueue is not always at the rear** — new elements are inserted in priority order
- **Dequeue is still from the front** — the highest-priority element is removed first

### Implementations
- **Ordered linked list** — maintains elements from highest to lowest priority
- **Tree-based structure (heap)** — very efficient; covered in sorting algorithms

### STL `priority_queue`
- Class template: `priority_queue<elemType>`
- Contained in header `<queue>`
- Default priority: uses the **less-than operator (`<`)** — largest element has highest priority
- Custom priority can be defined by overloading `<` or providing a comparison function

---

## Quick Review

1. **FIFO** = First-In, First-Out — the first element added is the first to be removed.
2. When an element is removed, it is taken from the **front**.
3. When an element is added, it is placed at the **rear**.
4. Two core operations every queue performs: **enqueue** (add to rear) and **dequeue** (remove from front).
