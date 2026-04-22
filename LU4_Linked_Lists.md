---
id: LU4_Linked_Lists
aliases: []
tags: []
---

# LU4 – Linked Lists

<!--toc:start-->
- [LU4 – Linked Lists](#lu4-linked-lists)
  - [1. What is a Linked List?](#1-what-is-a-linked-list)
    - [Key Terminology](#key-terminology)
    - [Node Declaration (C++)](#node-declaration-c)
  - [2. Variations of Linked Lists](#2-variations-of-linked-lists)
  - [3. Building a Linked List](#3-building-a-linked-list)
    - [Forward (insert at tail)](#forward-insert-at-tail)
    - [Backward (insert at head)](#backward-insert-at-head)
  - [4. Traversing a Linked List](#4-traversing-a-linked-list)
  - [5. Inserting a Node](#5-inserting-a-node)
    - [Into the Middle (after node `p`)](#into-the-middle-after-node-p)
    - [With Two Pointers (`p` and `q`)](#with-two-pointers-p-and-q)
  - [6. Deleting a Node (Two Pointers)](#6-deleting-a-node-two-pointers)
  - [7. Unordered Linked List Operations](#7-unordered-linked-list-operations)
    - [Search](#search)
    - [Insert at Head (`insertFirst`)](#insert-at-head-insertfirst)
    - [Insert at Tail (`insertLast`)](#insert-at-tail-insertlast)
    - [Delete a Node — Cases to Handle](#delete-a-node-cases-to-handle)
  - [8. Ordered Linked List Operations](#8-ordered-linked-list-operations)
    - [Search](#search-1)
    - [Insert — Cases](#insert-cases)
    - [Delete — Cases](#delete-cases)
  - [9. Doubly Linked List](#9-doubly-linked-list)
  - [10. Circular Linked List](#10-circular-linked-list)
  - [11. Header and Trailer Nodes](#11-header-and-trailer-nodes)
  - [12. Pointer Syntax Reference](#12-pointer-syntax-reference)
<!--toc:end-->

**TMF1434 Data Structures and Algorithms**

---

## 1. What is a Linked List?

A linked list is a data structure made up of a collection of **nodes**. Each node has two components:

- **Data** – the actual value stored
- **Link** – a pointer to the next node in the list (NULL for the last node)

### Key Terminology
- **Head** – a separate pointer that holds the address of the first node
- **Tail** – the last node in the list; its link is NULL

### Node Declaration (C++)
```cpp
struct nodeType {
    int info;       // data
    nodeType *link; // pointer to next node
};
nodeType *head;
```

---

## 2. Variations of Linked Lists

| Type | Description |
|------|-------------|
| **Singly Linked** | Each node has one pointer to the next node |
| **Circular Linked** | Like singly linked, but the last node points back to the first |
| **Doubly Linked** | Each node has two pointers: one to the next node, one to the previous |

---

## 3. Building a Linked List

### Forward (insert at tail)
- Requires two pointers: `*first` and `*last`
- New nodes are always appended to the end

```cpp
newNode = new nodeType;
newNode->info = num;
newNode->link = NULL;
if (first == NULL) {
    first = newNode;
    last = newNode;
} else {
    last->link = newNode;
    last = newNode;
}
```

### Backward (insert at head)
- Requires only one pointer: `*first`
- New nodes are always prepended to the front

```cpp
newNode = new nodeType;
newNode->info = num;
newNode->link = first;
first = newNode;
```

---

## 4. Traversing a Linked List

Never traverse using `head` directly — use a separate `current` pointer so you don't lose the reference to the start of the list.

```cpp
current = head;
while (current != NULL) {
    // process current node
    current = current->link;
}
```

---

## 5. Inserting a Node

### Into the Middle (after node `p`)

**Order matters when using a single pointer!**

```cpp
newNode = new nodeType;       // Step 1: create node
newNode->info = 50;           // Step 2: store data
newNode->link = p->link;      // Step 3: point newNode to p's next  ← must be FIRST
p->link = newNode;            // Step 4: update p to point to newNode
```

> ⚠️ If you do Step 4 before Step 3, `p->link` gets overwritten and the rest of the list is lost.

### With Two Pointers (`p` and `q`)
When you already have both `p` (before) and `q` (after), the order does **not** matter:

```cpp
p->link = newNode;
newNode->link = q;
```

---

## 6. Deleting a Node (Two Pointers)

```cpp
q = p->link;          // Step 2: q points to node to delete
p->link = q->link;    // Step 3: bypass the node
delete q;             // Step 4: free memory
```

---

## 7. Unordered Linked List Operations

### Search
Traverse the list comparing each node's info to the search item. Returns `true` if found, `false` otherwise.

### Insert at Head (`insertFirst`)
```cpp
newNode->info = newItem;
newNode->link = first;
first = newNode;
if (last == NULL) last = newNode; // was empty list
count++;
```

### Insert at Tail (`insertLast`)
```cpp
newNode->info = newItem;
newNode->link = NULL;
if (first == NULL) { first = newNode; last = newNode; }
else { last->link = newNode; last = newNode; }
count++;
```

### Delete a Node — Cases to Handle
1. **Empty list** → error, cannot delete
2. **Delete first node** (2a: only one node, 2b: multiple nodes)
3. **Delete middle or last node** (3a: not last, 3b: is last)
4. **Item not in list** → error message

---

## 8. Ordered Linked List Operations

Elements are maintained in sorted order. Uses two traversal pointers: `current` and `trailCurrent`.

### Search
Stop early when `current->info >= searchItem` (no need to traverse the whole list).

### Insert — Cases
1. **Empty list** → newNode becomes first and last
2. **New item < first item** → insert before `first`
3. **Insert somewhere in list** → find correct position, link in newNode
   - 3a: item goes at end (`current == NULL`)
   - 3b: item goes in middle

### Delete — Cases
1. **Empty list** → error
2. **Delete first node** → adjust `first`
3. **Delete middle/last** → use `trailCurrent->link = current->link`
4. **Item not found** → error message

---

## 9. Doubly Linked List

Each node stores pointers to both its **next** and **previous** nodes.

```cpp
struct nodeType {
    int info;
    nodeType *prev;   // predecessor
    nodeType *next;   // successor
};
```

- Can be traversed **in both directions**
- `first->prev` is NULL; `last->next` is NULL

---

## 10. Circular Linked List

The last node's link points back to the first node (no NULL tail).

- Convenient to let `first` point to the **last** node
  - `first` → last node
  - `first->link` → first node
- Empty list: `first = NULL`

---

## 11. Header and Trailer Nodes

A technique to simplify insertion/deletion edge cases:

- **Header node** – placed at the start, holds a value *smaller* than any data value (e.g., `"A"`)
- **Trailer node** – placed at the end, holds a value *larger* than any data value (e.g., `"zzzzzzzz"`)

This ensures you never need to insert before the first real node or after the last, removing special-case logic.

```
first → [A] → [Bell] → [Grant] → [zzzzzzzz] → NULL
         ^header                   ^trailer
```

---

## 12. Pointer Syntax Reference

| Expression | Meaning |
|---|---|
| `head` | Address of first node |
| `head->info` | Data in first node |
| `head->link` | Address of second node |
| `head->link->info` | Data in second node |
| `(*ptr).member` | Same as `ptr->member` |

---

*Textbook: "Data Structures Using C++" by D.S. Malik*
