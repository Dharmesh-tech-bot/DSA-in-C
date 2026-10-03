# 📘 Data Structures in C — Complete Interview Preparation Guide

> **Language:** C (C99/C11 Standard)

> **Audience:** Freshers & early-career engineers targeting strong fundamentals in C

> **Goal:** Interview-ready knowledge with actual C implementations

> **Style:** Revision-friendly, code-focused, interview-oriented

---

## 🛠️ C Essentials Before You Start

Before diving into data structures, these C concepts are **non-negotiable prerequisites**.

### Memory Management in C

```c
#include <stdlib.h>

/* Allocate memory */
int *ptr = (int *)malloc(sizeof(int));         // single int
int *arr = (int *)malloc(n * sizeof(int));     // array of n ints

/* Zero-initialized allocation */
int *arr = (int *)calloc(n, sizeof(int));

/* Resize existing allocation */
arr = (int *)realloc(arr, new_size * sizeof(int));

/* Always free what you allocate */
free(ptr);
ptr = NULL;  // Avoid dangling pointer
```

> **Interview Rule:**
>
>  Every `malloc`/`calloc`/`realloc` must have a matching `free`. Always check if returned pointer is `NULL` (allocation failure).

### Pointers — The Backbone of DSA in C

```c
int x = 10;
int *p = &x;        // p holds address of x
*p = 20;            // dereference: x is now 20

/* Pointer to pointer — used in linked list head modifications */
void insert(Node **head) { ... }  // pass &head from caller

/* NULL pointer — end of list / uninitialized */
Node *head = NULL;
```

### Struct — Defining Node Types

```c
/* Basic node structure */
typedef struct Node {
    int data;
    struct Node *next;
} Node;

/* Creating a node on heap */
Node *createNode(int val) {
    Node *newNode = (Node *)malloc(sizeof(Node));
    if (newNode == NULL) { /* handle error */ return NULL; }
    newNode->data = val;
    newNode->next = NULL;
    return newNode;
}
```

---

## 📌 Table of Contents

1. [Time Complexity Fundamentals](#-time-complexity-fundamentals)
2. [Array](#1--array)
3. [Stack](#2--stack)
4. [Queue](#3--queue)
5. [Singly Linked List](#4--singly-linked-list)
6. [Doubly Linked List](#5--doubly-linked-list)
7. [Circular Linked List](#6--circular-linked-list)
8. [Skip List](#7--skip-list)
9. [Hash Table](#8--hash-table)
10. [Binary Search Tree (BST)](#9--binary-search-tree-bst)
11. [Cartesian Tree](#10--cartesian-tree)
12. [B-Tree](#11--b-tree)
13. [Red-Black Tree](#12--red-black-tree)
14. [Splay Tree](#13--splay-tree)
15. [AVL Tree](#14--avl-tree)
16. [KD Tree](#15--kd-tree)
17. [Final Comparison Table](#-final-comparison-table)

---

## ⏱ Time Complexity Fundamentals

### Big-O, Big-Theta, Big-Omega — What They Mean in C Context

---

### 🔹 Big-O (O) — Upper Bound / Worst Case

- Describes the **maximum** time — "never slower than this."
- **C Example:** `scanf` reading `n` integers into an array — `O(n)` worst case.

### 🔹 Big-Omega (Ω) — Lower Bound / Best Case

- Describes the **minimum** time — "always at least this fast."
- **C Example:** Accessing `arr[0]` is always `Ω(1)`.

### 🔹 Big-Theta (Θ) — Tight Bound

- Algorithm grows **exactly** at this rate in all cases.
- **C Example:** `memcpy(dest, src, n)` is `Θ(n)` — always copies all `n` bytes.

---

### Complexity Classes (Fastest → Slowest)

```
O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(2ⁿ) < O(n!)
```

### Best, Average, Worst — Table

| Case | Definition | C Example |
|---|---|---|
| **Best** | Most favourable input | `arr[0] == target` → found immediately |
| **Average** | Expected over all inputs | Target found at middle on average |
| **Worst** | Most unfavourable input | Target is last element or not present |

---
---

## 1. 📦 Array

### 🔹 Technical Definition

An **Array** in C is a contiguous block of memory holding `n` elements of the **same type**, declared with a fixed size at compile time (static) or allocated with `malloc` at runtime (dynamic). Element at index `i` is accessed as `arr[i]`, which the compiler translates to `*(arr + i)` — pointer arithmetic over the base address.

---

### 🔹 Intuition / Core Idea

- In C, an array name is a **pointer to its first element**: `arr == &arr[0]`.
- Accessing any element is O(1) because the address is computed as:
  `base_address + i × sizeof(element_type)`.
- C does **not** perform bounds checking — accessing out-of-range indices is **undefined behaviour**.

---

### 🔹 Key Properties in C

- `int arr[5]` — static array, stored on the **stack**, size fixed at compile time.
- `int *arr = malloc(n * sizeof(int))` — dynamic array on the **heap**, size flexible.
- `sizeof(arr)` on a static array gives total bytes; on a pointer, gives pointer size (8 bytes on 64-bit) — a common trap.
- Arrays are **passed as pointers** to functions — size information is lost (array decay).
- 2D array: `int matrix[rows][cols]` — row-major storage; `matrix[i][j]` = `*(matrix + i * cols + j)`.

---

### 🔹 Operations & Time Complexity

| Operation | Best | Average | Worst |
|---|---|---|---|
| Access by Index (`arr[i]`) | O(1) | O(1) | O(1) |
| Search (Linear) | O(1) | O(n) | O(n) |
| Search (Binary — sorted) | O(1) | O(log n) | O(log n) |
| Insert at End (dynamic) | O(1) | O(1) amortized | O(n) on resize |
| Insert at Middle | O(n) | O(n) | O(n) |
| Delete at End | O(1) | O(1) | O(1) |
| Delete at Middle | O(n) | O(n) | O(n) |
| Traversal | O(n) | O(n) | O(n) |

**Space Complexity:** `O(n)`

---

### 🔹 C Implementation

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

/* ── Static Array ── */
int staticArr[5] = {10, 20, 30, 40, 50};

/* ── Dynamic Array ── */
int *arr = (int *)malloc(n * sizeof(int));
if (!arr) { perror("malloc failed"); exit(EXIT_FAILURE); }

/* ── Access ── O(1) */
int val = arr[i];  /* same as *(arr + i) */

/* ── Linear Search ── O(n) */
int linearSearch(int *arr, int n, int target) {
    for (int i = 0; i < n; i++)
        if (arr[i] == target) return i;
    return -1;
}

/* ── Binary Search (sorted array) ── O(log n) */
int binarySearch(int *arr, int n, int target) {
    int low = 0, high = n - 1;
    while (low <= high) {
        int mid = low + (high - low) / 2;  /* Avoids overflow vs (low+high)/2 */
        if (arr[mid] == target)  return mid;
        else if (arr[mid] < target) low = mid + 1;
        else                        high = mid - 1;
    }
    return -1;
}

/* ── Insert at index i (shift right) ── O(n) */
void insertAt(int *arr, int *size, int i, int val) {
    for (int j = *size; j > i; j--)
        arr[j] = arr[j - 1];
    arr[i] = val;
    (*size)++;
}

/* ── Delete at index i (shift left) ── O(n) */
void deleteAt(int *arr, int *size, int i) {
    for (int j = i; j < *size - 1; j++)
        arr[j] = arr[j + 1];
    (*size)--;
}

/* ── Free dynamic array ── */
free(arr);
arr = NULL;
```

> **C Trap:** `int mid = (low + high) / 2` can **integer overflow** if `low + high > INT_MAX`. Always use `low + (high - low) / 2`.

---

### 🔹 2D Array in C

```c
/* Stack-allocated 2D array */
int matrix[3][4];
matrix[1][2] = 5;  /* row 1, col 2 */

/* Heap-allocated 2D array (array of pointers) */
int **matrix = (int **)malloc(rows * sizeof(int *));
for (int i = 0; i < rows; i++)
    matrix[i] = (int *)malloc(cols * sizeof(int));

/* Free 2D heap array */
for (int i = 0; i < rows; i++) free(matrix[i]);
free(matrix);
```

---

### 🔹 Variations / Modifications

- **Static Array** — `int arr[N]` — stack allocated, cannot resize.
- **Dynamic Array** — `malloc` + `realloc` for resizing (C's version of `ArrayList`).
- **2D / Multidimensional** — Stack: `int a[R][C]`, Heap: pointer-to-pointer.
- **VLA (Variable Length Array)** — C99 feature: `int arr[n]` on stack; avoid in production (stack overflow risk, removed in C11 as mandatory).
- **Bit Array** — `unsigned int bits[n/32 + 1]`; use bitwise ops for space-efficient booleans.

---

### 🔹 Advantages & Disadvantages

| Advantages | Disadvantages |
|---|---|
| O(1) random access | No bounds checking in C — silent UB on overflow |
| Cache-friendly (contiguous memory) | Static arrays: fixed size |
| Supports pointer arithmetic | Dynamic resize: manual `realloc` + error handling |
| Direct memory control via pointers | `sizeof` trap when array decays to pointer |

---

### 🔹 Interview Tips (C-Specific)

- **Common Questions:** Reverse an array in-place, rotate array by `k`, find duplicates using XOR, merge two sorted arrays.
- **C-Specific Traps Interviewers Test:**
  - `sizeof(arr)` inside a function receiving array as parameter — gives pointer size, not array size. Always pass `n` separately.
  - Off-by-one in binary search — use `low + (high - low) / 2`.
  - `realloc` failure — returns `NULL` but original pointer is still valid; don't overwrite it directly.
  - Stack overflow from large static arrays — declare large arrays globally or use heap.
- **Mistakes to Avoid:**
  - `int arr[-1]` — accessing negative index is UB in C.
  - Not initializing dynamic array values (`calloc` gives zero-initialization; `malloc` does not).

---

### 🔹 Real-World Use Cases

- **Kernel Buffers:** Linux kernel uses C arrays for socket receive/send buffers.
- **Image Processing:** Pixel data stored as `uint8_t` arrays.
- **Embedded Systems:** Sensor readings, ADC buffers as static arrays.
- **NumPy (CPython backend):** C arrays under the hood.

---
---

## 2. 📚 Stack

### 🔹 Technical Definition

A **Stack** in C is a LIFO (Last In, First Out) abstract data structure implemented either via a static/dynamic array or a singly linked list. In C, there is no built-in stack — it must be manually implemented using a `struct` holding the array (or head pointer) and a `top` index.

---

### 🔹 Intuition / Core Idea

- Think of a **stack of plates** — add and remove only from the top.
- In C programs, the **call stack** is a native stack maintained by the CPU/OS for function frames, local variables, and return addresses.
- Used for: expression evaluation, balanced parentheses, DFS, undo mechanisms.

---

### 🔹 Key Properties in C

- **Array-based:** Fixed capacity; `top` index tracks the top element; faster due to cache locality.
- **Linked list-based:** Dynamic capacity; each push allocates a new node; pop frees it.
- **Stack overflow:** In array-based, when `top == capacity - 1`. In recursion, when the call stack exhausts system stack space (`ulimit -s` on Linux).
- The **program call stack** in C is managed by the runtime — local variables, return addresses, saved registers are pushed/popped automatically.

---

### 🔹 Operations & Time Complexity

| Operation | Best | Average | Worst |
|---|---|---|---|
| Push | O(1) | O(1) | O(1) |
| Pop | O(1) | O(1) | O(1) |
| Peek / Top | O(1) | O(1) | O(1) |
| Search | O(1) | O(n) | O(n) |
| isEmpty | O(1) | O(1) | O(1) |

**Space Complexity:** `O(n)`

---

### 🔹 C Implementation — Array-Based Stack

```c
#include <stdio.h>
#include <stdlib.h>
#define MAX 100

/* ── Stack Structure ── */
typedef struct {
    int data[MAX];
    int top;
} Stack;

/* ── Initialize ── */
void initStack(Stack *s) {
    s->top = -1;
}

/* ── isEmpty ── */
int isEmpty(Stack *s) {
    return s->top == -1;
}

/* ── isFull ── */
int isFull(Stack *s) {
    return s->top == MAX - 1;
}

/* ── Push ── O(1) */
void push(Stack *s, int val) {
    if (isFull(s)) { printf("Stack Overflow\n"); return; }
    s->data[++(s->top)] = val;
}

/* ── Pop ── O(1) */
int pop(Stack *s) {
    if (isEmpty(s)) { printf("Stack Underflow\n"); return -1; }
    return s->data[(s->top)--];
}

/* ── Peek ── O(1) */
int peek(Stack *s) {
    if (isEmpty(s)) { printf("Stack Empty\n"); return -1; }
    return s->data[s->top];
}
```

---

### 🔹 C Implementation — Linked List-Based Stack

```c
typedef struct Node {
    int data;
    struct Node *next;
} Node;

typedef struct {
    Node *top;
} Stack;

void initStack(Stack *s) { s->top = NULL; }

/* ── Push ── O(1) */
void push(Stack *s, int val) {
    Node *newNode = (Node *)malloc(sizeof(Node));
    if (!newNode) { perror("malloc"); return; }
    newNode->data = val;
    newNode->next = s->top;
    s->top = newNode;
}

/* ── Pop ── O(1) */
int pop(Stack *s) {
    if (!s->top) { printf("Underflow\n"); return -1; }
    Node *temp = s->top;
    int val = temp->data;
    s->top = temp->next;
    free(temp);          /* Always free in C */
    return val;
}

/* ── Free entire stack ── */
void freeStack(Stack *s) {
    while (s->top) pop(s);
}
```

---

### 🔹 Variations / Modifications

- **Min Stack in C** — Maintain a parallel `minStack` array; track running minimum at each push level.
- **Monotonic Stack** — During push, pop all elements that violate the monotonic order (increasing or decreasing); powerful for "next greater element" problems.
- **Dynamic Stack** — Use `realloc` to double capacity when full instead of fixed `MAX`.

---

### 🔹 Advantages & Disadvantages

| Advantages | Disadvantages |
|---|---|
| O(1) push/pop/peek | No random access |
| Array-based is cache-friendly | Array-based: fixed `MAX` unless `realloc` is used |
| Linked list-based: truly dynamic | Linked list: `malloc`/`free` per push/pop overhead |
| Simple and intuitive | Memory leak if `free` is forgotten on pop |

---

### 🔹 Interview Tips (C-Specific)

- **Common Questions:** Balanced parentheses, Next Greater Element, Infix to Postfix, Evaluate Postfix, Reverse a string using stack.
- **C-Specific Traps:**
  - Forgetting to `free` nodes in linked list pop — interviewers watch for this.
  - Using `s->top` (arrow operator) vs `(*s).top` — both work but `->` is idiomatic C.
  - Stack underflow check before pop — silent array access on `top == -1` gives garbage in C.
- **Mistakes to Avoid:**
  - Not checking `NULL` after `malloc` in push.
  - Comparing `top == MAX` instead of `top == MAX - 1` for overflow check.

---

### 🔹 Real-World Use Cases

- **C Compiler:** Function call frames — local variables, return address, parameters pushed onto the call stack.
- **Expression Parsing:** Infix-to-postfix conversion in calculators.
- **Recursive → Iterative Conversion:** Explicitly simulate call stack.
- **Undo in Vim / Editors:** State stored on a stack.

---
---

## 3. 🚶 Queue

### 🔹 Technical Definition

A **Queue** in C is a FIFO (First In, First Out) abstract data structure where elements are enqueued at the **rear** and dequeued from the **front**. C has no built-in queue — implemented as a circular array (preferred) or a linked list with `head` and `tail` pointers.

---

### 🔹 Intuition / Core Idea

- Think of a **print spooler** — jobs processed in the order submitted.
- The circular array is the preferred C implementation — prevents the "front drift" problem where dequeuing from a simple array wastes space at the front.

---

### 🔹 Key Properties in C

- **Circular Array Queue:** `front` and `rear` indices wrap around using `% capacity`.
- **Linked List Queue:** `head` = dequeue side, `tail` = enqueue side. Maintain both pointers for O(1) enqueue/dequeue.
- **No STL in C** — you must implement from scratch.
- `front == rear` can mean either empty or full in a circular array — resolve by tracking `size` separately.

---

### 🔹 Operations & Time Complexity

| Operation | Best | Average | Worst |
|---|---|---|---|
| Enqueue | O(1) | O(1) | O(1) |
| Dequeue | O(1) | O(1) | O(1) |
| Peek / Front | O(1) | O(1) | O(1) |
| Search | O(1) | O(n) | O(n) |
| isEmpty / isFull | O(1) | O(1) | O(1) |

**Space Complexity:** `O(n)`

---

### 🔹 C Implementation — Circular Array Queue

```c
#include <stdio.h>
#include <stdlib.h>
#define MAX 100

typedef struct {
    int data[MAX];
    int front, rear, size;
} Queue;

void initQueue(Queue *q) {
    q->front = 0;
    q->rear  = -1;
    q->size  = 0;
}

int isEmpty(Queue *q) { return q->size == 0; }
int isFull(Queue *q)  { return q->size == MAX; }

/* ── Enqueue ── O(1) */
void enqueue(Queue *q, int val) {
    if (isFull(q)) { printf("Queue Full\n"); return; }
    q->rear = (q->rear + 1) % MAX;   /* circular wrap */
    q->data[q->rear] = val;
    q->size++;
}

/* ── Dequeue ── O(1) */
int dequeue(Queue *q) {
    if (isEmpty(q)) { printf("Queue Empty\n"); return -1; }
    int val = q->data[q->front];
    q->front = (q->front + 1) % MAX; /* circular wrap */
    q->size--;
    return val;
}

/* ── Peek ── O(1) */
int front(Queue *q) {
    if (isEmpty(q)) return -1;
    return q->data[q->front];
}
```

---

### 🔹 C Implementation — Linked List Queue

```c
typedef struct Node {
    int data;
    struct Node *next;
} Node;

typedef struct {
    Node *head;   /* dequeue side */
    Node *tail;   /* enqueue side */
} Queue;

void initQueue(Queue *q) { q->head = q->tail = NULL; }

/* ── Enqueue ── O(1) */
void enqueue(Queue *q, int val) {
    Node *newNode = (Node *)malloc(sizeof(Node));
    if (!newNode) { perror("malloc"); return; }
    newNode->data = val;
    newNode->next = NULL;
    if (!q->tail) { q->head = q->tail = newNode; return; }
    q->tail->next = newNode;
    q->tail = newNode;
}

/* ── Dequeue ── O(1) */
int dequeue(Queue *q) {
    if (!q->head) { printf("Queue Empty\n"); return -1; }
    Node *temp = q->head;
    int val = temp->data;
    q->head = q->head->next;
    if (!q->head) q->tail = NULL;  /* queue became empty */
    free(temp);
    return val;
}

/* ── Free entire queue ── */
void freeQueue(Queue *q) {
    while (q->head) dequeue(q);
}
```

---

### 🔹 Variations / Modifications

- **Circular Queue** — Circular array as above; preferred over linear array.
- **Deque (Double-Ended Queue)** — Supports insert/delete at both ends; implement with doubly linked list.
- **Priority Queue** — Min-heap backed; requires `heapify` operations.
- **Monotonic Deque** — Used for Sliding Window Maximum; maintain a deque of indices where values are monotonically decreasing.

---

### 🔹 Advantages & Disadvantages

| Advantages | Disadvantages |
|---|---|
| O(1) enqueue and dequeue | No random access |
| Circular array: no wasted space | Linked list: `malloc`/`free` overhead per element |
| FIFO naturally models real-world queues | Fixed size in array-based (unless `realloc`) |

---

### 🔹 Interview Tips (C-Specific)

- **Common Questions:** BFS on graph (adjacency list in C), Implement Queue using 2 Stacks, First Non-Repeating Character, Level Order Traversal of Binary Tree.
- **C-Specific Traps:**
  - Circular wrap — `(rear + 1) % MAX` not `rear + 1`.
  - Not updating `tail = NULL` when the last element is dequeued from linked list queue — causes dangling pointer on next enqueue.
  - Forgetting to `free` dequeued node.
- **Mistakes to Avoid:**
  - Using a plain array without circular logic — dequeue creates unusable gaps at the front.
  - Not handling the case where `head == tail` (single element queue) during dequeue.

---

### 🔹 Real-World Use Cases

- **OS Process Scheduling:** `struct task_struct` queues in Linux kernel.
- **BFS in Graph Algorithms:** Level-order traversal of trees/graphs.
- **I/O Buffering:** Kernel uses circular ring buffers (queues) for network packet handling.
- **Message Queues in IPC:** `mqueue_open` in C POSIX API.

---
---

## 4. 🔗 Singly Linked List

### 🔹 Technical Definition

A **Singly Linked List** in C is a dynamic data structure of `Node` structs, each containing a `data` field and a `next` pointer to the next node. The list is accessed from a `head` pointer. The last node's `next` is `NULL`. All memory is heap-allocated via `malloc` and must be manually freed.

---

### 🔹 Intuition / Core Idea

- In C, linked lists shine where **dynamic memory** and **pointer manipulation** matter — you can insert/delete at the head in O(1) without shifting.
- Each node is an independent heap allocation — unlike arrays, nodes are not contiguous.
- `NULL` is the sentinel marking the end of the list.

---

### 🔹 Key Properties in C

- `Node **head` (double pointer) is passed to functions that modify the head — essential C pattern.
- `->` operator: `node->data` is `(*node).data` — always use `->` for pointer-to-struct.
- **Memory ownership:** The creator of a node is responsible for freeing it.
- **Segfault risk:** Dereferencing `NULL` or a freed pointer — two most common bugs.

---

### 🔹 Operations & Time Complexity

| Operation | Best | Average | Worst |
|---|---|---|---|
| Access by Index | O(1) (head) | O(n) | O(n) |
| Search | O(1) | O(n) | O(n) |
| Insert at Head | O(1) | O(1) | O(1) |
| Insert at Tail (with tail ptr) | O(1) | O(1) | O(1) |
| Insert at Index | O(n) | O(n) | O(n) |
| Delete at Head | O(1) | O(1) | O(1) |
| Delete at Tail | O(n) | O(n) | O(n) |
| Delete by Value | O(1) | O(n) | O(n) |
| Traversal | O(n) | O(n) | O(n) |

**Space Complexity:** `O(n)` + pointer overhead (8 bytes per `next` on 64-bit)

---

### 🔹 C Implementation

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int data;
    struct Node *next;
} Node;

/* ── Create Node ── */
Node *createNode(int val) {
    Node *n = (Node *)malloc(sizeof(Node));
    if (!n) { perror("malloc"); return NULL; }
    n->data = val;
    n->next = NULL;
    return n;
}

/* ── Insert at Head ── O(1) */
void insertAtHead(Node **head, int val) {
    Node *newNode = createNode(val);
    newNode->next = *head;
    *head = newNode;
}

/* ── Insert at Tail ── O(n) without tail ptr */
void insertAtTail(Node **head, int val) {
    Node *newNode = createNode(val);
    if (!*head) { *head = newNode; return; }
    Node *curr = *head;
    while (curr->next) curr = curr->next;
    curr->next = newNode;
}

/* ── Delete by Value ── O(n) */
void deleteByValue(Node **head, int val) {
    if (!*head) return;
    /* Special case: head node matches */
    if ((*head)->data == val) {
        Node *temp = *head;
        *head = (*head)->next;
        free(temp);
        return;
    }
    Node *prev = *head, *curr = (*head)->next;
    while (curr) {
        if (curr->data == val) {
            prev->next = curr->next;
            free(curr);
            return;
        }
        prev = curr;
        curr = curr->next;
    }
}

/* ── Reverse ── O(n) — Iterative */
void reverse(Node **head) {
    Node *prev = NULL, *curr = *head, *next = NULL;
    while (curr) {
        next = curr->next;   /* save next */
        curr->next = prev;   /* reverse link */
        prev = curr;         /* move prev forward */
        curr = next;         /* move curr forward */
    }
    *head = prev;
}

/* ── Floyd's Cycle Detection ── O(n) */
int hasCycle(Node *head) {
    Node *slow = head, *fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) return 1;  /* cycle detected */
    }
    return 0;
}

/* ── Find Middle (Two Pointer) ── O(n) */
Node *findMiddle(Node *head) {
    Node *slow = head, *fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
    }
    return slow;
}

/* ── Print List ── */
void printList(Node *head) {
    while (head) {
        printf("%d -> ", head->data);
        head = head->next;
    }
    printf("NULL\n");
}

/* ── Free Entire List ── ESSENTIAL */
void freeList(Node **head) {
    Node *curr = *head, *next;
    while (curr) {
        next = curr->next;
        free(curr);
        curr = next;
    }
    *head = NULL;
}
```

---

### 🔹 Variations / Modifications

- **Sorted Linked List:** Insert node at correct position — traverse until `curr->data > val`.
- **Recursive Reverse:** Cleaner code but uses O(n) call stack — risky for very large lists in C.
- **XOR Linked List:** Each node stores `XOR(prev_addr, next_addr)` — halves pointer memory; complex but impressive to mention in interviews.

---

### 🔹 Advantages & Disadvantages

| Advantages | Disadvantages |
|---|---|
| O(1) head insert/delete | O(n) access by index — no pointer arithmetic shortcut |
| True dynamic size (heap) | `malloc`/`free` overhead per node |
| No pre-allocation | Cache-unfriendly (non-contiguous) |
| Elegant recursive solutions | Memory leak risk if `free` is forgotten |

---

### 🔹 Interview Tips (C-Specific)

- **Common Questions:** Reverse linked list (iterative + recursive), detect cycle, find middle, merge two sorted lists, remove Nth from end, check palindrome.
- **C-Specific Traps:**
  - Always use `Node **head` (double pointer) when the function may change `head` — single pointer won't update the caller's `head`.
  - Three-pointer reversal (`prev`, `curr`, `next`) — must be memorized.
  - `free(curr)` then access `curr->next` — **use-after-free** bug; always save `next` before freeing.
- **Mistakes to Avoid:**
  - `if (head->data == val)` without checking `head != NULL` first — segfault.
  - Forgetting to call `freeList` — memory leak that interviewers look for.

---

### 🔹 Real-World Use Cases

- **Linux Kernel:** `list_head` embedded linked list for process and file descriptor management.
- **Memory Allocators:** `free` list of available memory blocks.
- **Hash Table Chaining:** Each bucket is a singly linked list of entries.
- **Adjacency List (Graphs):** Each vertex's edges stored as a linked list.

---
---

## 5. ↔️ Doubly Linked List

### 🔹 Technical Definition

A **Doubly Linked List** in C is a linked list where each `Node` holds three fields: `data`, a `next` pointer (successor), and a `prev` pointer (predecessor). Both `head` and `tail` pointers are maintained. In C, every insertion and deletion must update **both** `prev` and `next` correctly — missing either causes a broken or corrupt list.

---

### 🔹 Intuition / Core Idea

- The `prev` pointer enables **O(1) deletion** when given a direct node pointer — no need to traverse to find the predecessor.
- This is the reason LRU Cache uses a DLL — moving a node to the front and removing from the tail both require O(1) pointer updates.

---

### 🔹 Key Properties in C

- Two pointers per node: `next` and `prev` — double the pointer memory of a singly linked list.
- `head->prev = NULL` and `tail->next = NULL` always.
- Using **sentinel dummy nodes** for `head` and `tail` eliminates edge-case handling in insert/delete.

---

### 🔹 Operations & Time Complexity

| Operation | Best | Average | Worst |
|---|---|---|---|
| Insert at Head | O(1) | O(1) | O(1) |
| Insert at Tail | O(1) | O(1) | O(1) |
| Delete given Node Pointer | O(1) | O(1) | O(1) |
| Delete by Value | O(1) | O(n) | O(n) |
| Search | O(1) | O(n) | O(n) |
| Traversal (Fwd/Rev) | O(n) | O(n) | O(n) |

**Space Complexity:** `O(n)` — 2 pointers per node

---

### 🔹 C Implementation

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int data;
    struct Node *prev;
    struct Node *next;
} Node;

typedef struct {
    Node *head;
    Node *tail;
} DLL;

void initDLL(DLL *list) { list->head = list->tail = NULL; }

Node *createNode(int val) {
    Node *n = (Node *)malloc(sizeof(Node));
    if (!n) { perror("malloc"); return NULL; }
    n->data = val;
    n->prev = n->next = NULL;
    return n;
}

/* ── Insert at Head ── O(1) */
void insertHead(DLL *list, int val) {
    Node *newNode = createNode(val);
    if (!list->head) {
        list->head = list->tail = newNode;
        return;
    }
    newNode->next = list->head;
    list->head->prev = newNode;
    list->head = newNode;
}

/* ── Insert at Tail ── O(1) */
void insertTail(DLL *list, int val) {
    Node *newNode = createNode(val);
    if (!list->tail) {
        list->head = list->tail = newNode;
        return;
    }
    newNode->prev = list->tail;
    list->tail->next = newNode;
    list->tail = newNode;
}

/* ── Delete a given Node (O(1) given pointer) ── */
void deleteNode(DLL *list, Node *node) {
    if (!node) return;
    if (node->prev) node->prev->next = node->next;
    else            list->head = node->next;   /* deleting head */

    if (node->next) node->next->prev = node->prev;
    else            list->tail = node->prev;   /* deleting tail */

    free(node);
}

/* ── Forward Traversal ── */
void printForward(DLL *list) {
    Node *curr = list->head;
    while (curr) { printf("%d <-> ", curr->data); curr = curr->next; }
    printf("NULL\n");
}

/* ── Backward Traversal ── */
void printBackward(DLL *list) {
    Node *curr = list->tail;
    while (curr) { printf("%d <-> ", curr->data); curr = curr->prev; }
    printf("NULL\n");
}

/* ── Free DLL ── */
void freeDLL(DLL *list) {
    Node *curr = list->head, *next;
    while (curr) { next = curr->next; free(curr); curr = next; }
    list->head = list->tail = NULL;
}
```

---

### 🔹 LRU Cache in C (DLL + Hash Table Concept)

```c
/* Conceptual structure for LRU Cache node */
typedef struct LRUNode {
    int key, value;
    struct LRUNode *prev, *next;
} LRUNode;

/*
 * Full LRU Cache implementation requires:
 * 1. DLL: stores (key, value) nodes in usage order (MRU at head, LRU at tail)
 * 2. Hash Table: maps key → LRUNode* for O(1) lookup
 * get(key):  Find in hash table → move node to head → return value
 * put(key):  If exists → update + move to head
 *            If new    → insert at head
 *            If full   → remove tail node + its hash entry → insert new at head
 */
```

---

### 🔹 Advantages & Disadvantages

| Advantages | Disadvantages |
|---|---|
| O(1) delete given node pointer | 2× pointer memory per node |
| O(1) insert/delete at both ends | More pointer updates per operation — bug-prone in C |
| Bidirectional traversal | Sentinel nodes add setup complexity |

---

### 🔹 Interview Tips (C-Specific)

- **Common Questions:** LRU Cache, Flatten Multilevel DLL, Design Browser History.
- **C-Specific Traps:**
  - Updating only `next` and forgetting `prev` (or vice versa) — corrupts the list silently.
  - Edge cases: deleting the only node, deleting `head`, deleting `tail` — all need separate pointer updates.
  - Always `free` the deleted node — not doing so is a memory leak.
- **Mistakes to Avoid:**
  - Not setting `list->tail = node->prev` when deleting the tail node.
  - Accessing `node->prev->next` without checking `node->prev != NULL` — segfault.

---

### 🔹 Real-World Use Cases

- **LRU Cache:** Used in OS page replacement, CPU caches, Redis eviction.
- **Linux `task_struct`:** Process doubly linked list for scheduler.
- **Text Editors:** Some editors represent document content as a DLL of lines.
- **Browser History:** Forward/back navigation.

---
---

## 6. 🔄 Circular Linked List

### 🔹 Technical Definition

A **Circular Linked List** in C is a singly (or doubly) linked list where the **last node's `next` pointer points back to the first node** (head) instead of `NULL`. There is no `NULL` terminator — traversal must be controlled by comparing the current node to the starting node. A **tail pointer** is typically maintained for O(1) access to both ends.

---

### 🔹 Intuition / Core Idea

- Like a **circular race track** — after the last position, you're back to the start.
- No `NULL` in the list — loop termination is `current == start_node`.
- Preferred for round-robin scheduling where you cycle through nodes repeatedly.

---

### 🔹 Key Properties in C

- **Infinite traversal risk** — without a stop condition, a `while (curr->next != NULL)` loop runs forever.
- Maintaining a **tail pointer** (not head) gives O(1) access to both tail and head (`tail->next`).
- To detect whether a given list is circular: use Floyd's cycle detection on it.

---

### 🔹 Operations & Time Complexity

| Operation | Best | Average | Worst |
|---|---|---|---|
| Insert at Beginning | O(1) | O(1) | O(1) |
| Insert at End (with tail) | O(1) | O(1) | O(1) |
| Insert at Middle | O(n) | O(n) | O(n) |
| Delete at Beginning | O(1) | O(1) | O(1) |
| Delete at End | O(n) | O(n) | O(n) |
| Search | O(1) | O(n) | O(n) |
| Traversal | O(n) | O(n) | O(n) |

**Space Complexity:** `O(n)`

---

### 🔹 C Implementation

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int data;
    struct Node *next;
} Node;

Node *createNode(int val) {
    Node *n = (Node *)malloc(sizeof(Node));
    if (!n) { perror("malloc"); return NULL; }
    n->data = val;
    n->next = NULL;
    return n;
}

/* ── Insert at Beginning (using tail pointer) ── O(1) */
void insertBeginning(Node **tail, int val) {
    Node *newNode = createNode(val);
    if (!*tail) {                    /* empty list */
        *tail = newNode;
        newNode->next = newNode;     /* points to itself */
        return;
    }
    newNode->next = (*tail)->next;   /* new node points to current head */
    (*tail)->next = newNode;         /* tail points to new head */
}

/* ── Insert at End (using tail pointer) ── O(1) */
void insertEnd(Node **tail, int val) {
    insertBeginning(tail, val);      /* insert at beginning first */
    *tail = (*tail)->next;           /* then advance tail */
}

/* ── Delete Beginning ── O(1) */
void deleteBeginning(Node **tail) {
    if (!*tail) return;
    Node *head = (*tail)->next;
    if (head == *tail) {             /* only one node */
        free(*tail);
        *tail = NULL;
        return;
    }
    (*tail)->next = head->next;
    free(head);
}

/* ── Traverse ── O(n) */
void traverse(Node *tail) {
    if (!tail) { printf("Empty\n"); return; }
    Node *start = tail->next;   /* head = tail->next */
    Node *curr  = start;
    do {
        printf("%d -> ", curr->data);
        curr = curr->next;
    } while (curr != start);    /* stop when back at start — NOT curr != NULL */
    printf("(back to head)\n");
}

/* ── Free Circular List ── */
void freeCircular(Node **tail) {
    if (!*tail) return;
    Node *head = (*tail)->next;
    (*tail)->next = NULL;        /* break the circle first */
    Node *curr = head, *next;
    while (curr) {
        next = curr->next;
        free(curr);
        curr = next;
    }
    *tail = NULL;
}
```

> **Critical C Note:** Always **break the circle** (`tail->next = NULL`) before freeing — otherwise the `while (curr)` loop runs forever.

---

### 🔹 Advantages & Disadvantages

| Advantages | Disadvantages |
|---|---|
| O(1) insert at head and tail (with tail ptr) | Infinite loop if stop condition is wrong |
| No null checks during cyclic traversal | Must break circle before freeing memory |
| Natural for cyclic processing | Slightly more complex than regular linked list |

---

### 🔹 Interview Tips (C-Specific)

- **Common Questions:** Josephus Problem, Round-Robin Simulation, Split Circular List into Two.
- **C-Specific Traps:**
  - Loop condition `while (curr != start)` — not `while (curr)` or `while (curr->next)`.
  - Forgetting `tail->next = NULL` before `freeList` — causes infinite free loop.
  - Single-node edge case: `newNode->next = newNode` (points to itself).
- **Mistakes to Avoid:**
  - Maintaining `head` pointer instead of `tail` — you lose O(1) tail access.

---

### 🔹 Real-World Use Cases

- **OS Round-Robin Scheduler:** Processes in a circular queue; CPU cycles through.
- **Ring Buffer (Circular Buffer):** Fixed-size I/O buffer; `read` and `write` pointers wrap around.
- **Multiplayer Games:** Turn management — circular player list.
- **Token Ring Network:** IEEE 802.5 — token passed in a circular list of nodes.

---
---

## 7. 🚀 Skip List

### 🔹 Technical Definition

A **Skip List** in C is a probabilistic, multi-layered sorted linked list. Level 0 contains all elements; each higher level is a sparse subset where each node is promoted with probability `p = 0.5`. Forward pointers at each level allow large-range traversals, enabling expected O(log n) search without tree rotations.

---

### 🔹 Intuition / Core Idea

- Think of **express highway lanes** — some nodes have "express" pointers that skip over many nodes.
- In C, each node holds an array of `next[]` pointers — one per level it participates in.
- Randomness replaces balancing rotations (AVL/RBT) — elegant and simpler to implement correctly.

---

### 🔹 Key Properties in C

- Each node: `int key`, `int level`, `Node *forward[]` (flexible array of forward pointers).
- `MAX_LEVEL` — maximum number of levels; typically `log₂(n_max)` ≈ 16–32.
- **Random level generation:** Flip a coin (random bit); promote while heads and within `MAX_LEVEL`.
- **Header node:** A sentinel node with `MAX_LEVEL` forward pointers, all initially pointing to `NIL` (a tail sentinel).

---

### 🔹 Operations & Time Complexity

| Operation | Best | Average | Worst |
|---|---|---|---|
| Search | O(1) | O(log n) | O(n) |
| Insert | O(1) | O(log n) | O(n) |
| Delete | O(1) | O(log n) | O(n) |
| Range Query | O(log n) | O(log n + k) | O(n + k) |
| Traversal | O(n) | O(n) | O(n) |

**Space Complexity:** `O(n)` expected, `O(n log n)` worst case

---

### 🔹 C Implementation

```c
#include <stdio.h>
#include <stdlib.h>
#include <limits.h>
#define MAX_LEVEL 16
#define P 0.5

typedef struct SkipNode {
    int key;
    struct SkipNode *forward[MAX_LEVEL]; /* forward[i] = next node at level i */
} SkipNode;

typedef struct {
    SkipNode *header;
    int level;           /* current max level in use */
} SkipList;

/* ── Random Level Generator ── */
int randomLevel() {
    int lvl = 0;
    while ((double)rand() / RAND_MAX < P && lvl < MAX_LEVEL - 1)
        lvl++;
    return lvl;
}

/* ── Create Node ── */
SkipNode *createSkipNode(int key, int level) {
    SkipNode *n = (SkipNode *)malloc(sizeof(SkipNode));
    if (!n) { perror("malloc"); return NULL; }
    n->key = key;
    for (int i = 0; i < MAX_LEVEL; i++) n->forward[i] = NULL;
    return n;
}

/* ── Initialize Skip List ── */
SkipList *createSkipList() {
    SkipList *sl = (SkipList *)malloc(sizeof(SkipList));
    sl->level = 0;
    sl->header = createSkipNode(INT_MIN, MAX_LEVEL);
    return sl;
}

/* ── Search ── O(log n) expected */
int search(SkipList *sl, int key) {
    SkipNode *curr = sl->header;
    for (int i = sl->level; i >= 0; i--) {
        while (curr->forward[i] && curr->forward[i]->key < key)
            curr = curr->forward[i];
    }
    curr = curr->forward[0];
    return curr && curr->key == key;
}

/* ── Insert ── O(log n) expected */
void insert(SkipList *sl, int key) {
    SkipNode *update[MAX_LEVEL];   /* update[i] = predecessor at level i */
    SkipNode *curr = sl->header;

    for (int i = sl->level; i >= 0; i--) {
        while (curr->forward[i] && curr->forward[i]->key < key)
            curr = curr->forward[i];
        update[i] = curr;
    }

    int newLevel = randomLevel();
    if (newLevel > sl->level) {
        for (int i = sl->level + 1; i <= newLevel; i++)
            update[i] = sl->header;
        sl->level = newLevel;
    }

    SkipNode *newNode = createSkipNode(key, newLevel);
    for (int i = 0; i <= newLevel; i++) {
        newNode->forward[i] = update[i]->forward[i];
        update[i]->forward[i] = newNode;
    }
}

/* ── Delete ── O(log n) expected */
void delete(SkipList *sl, int key) {
    SkipNode *update[MAX_LEVEL];
    SkipNode *curr = sl->header;

    for (int i = sl->level; i >= 0; i--) {
        while (curr->forward[i] && curr->forward[i]->key < key)
            curr = curr->forward[i];
        update[i] = curr;
    }
    curr = curr->forward[0];

    if (!curr || curr->key != key) return;  /* not found */

    for (int i = 0; i <= sl->level; i++) {
        if (update[i]->forward[i] != curr) break;
        update[i]->forward[i] = curr->forward[i];
    }
    free(curr);
    while (sl->level > 0 && !sl->header->forward[sl->level]) sl->level--;
}

/* ── Print Skip List (all levels) ── */
void printSkipList(SkipList *sl) {
    for (int i = sl->level; i >= 0; i--) {
        SkipNode *curr = sl->header->forward[i];
        printf("Level %d: ", i);
        while (curr) { printf("%d -> ", curr->key); curr = curr->forward[i]; }
        printf("NULL\n");
    }
}

/* ── Free Skip List ── ESSENTIAL */
void freeSkipList(SkipList *sl) {
    if (!sl) return;
    SkipNode *curr = sl->header->forward[0]; /* level 0 has all nodes */
    while (curr) {
        SkipNode *next = curr->forward[0];
        free(curr);
        curr = next;
    }
    free(sl->header); /* free header sentinel */
    free(sl);         /* free the list struct */
}
```

---

### 🔹 Advantages & Disadvantages

| Advantages | Disadvantages |
|---|---|
| Simpler than balanced BSTs in C | No worst-case O(log n) — probabilistic only |
| Easy concurrent version (CAS-based) | `forward[]` array of pointers — higher memory than BST node |
| O(log n) expected for all ops | Requires `rand()` — nondeterministic behavior |

---

### 🔹 Interview Tips (C-Specific)

- **C-Specific Traps:** Memory for the `forward[]` array — use flexible array member or fixed-size array.
- **Key Points:** Explain `update[]` array during insert/delete — this is what interviewers test.
- Mention Redis uses skip list for its `ZSET` implementation — shows real-world awareness.
- **Mistakes to Avoid:** Claiming O(log n) worst-case — it is **expected** only.

---

### 🔹 Real-World Use Cases

- **Redis Sorted Sets:** C-implemented skip list for O(log n) ranked operations.
- **LevelDB / RocksDB memtable:** Written in C++ but concept is a skip list.
- **Java `ConcurrentSkipListMap`:** Lock-free concurrent sorted map.

---
---

## 8. 🗂️ Hash Table

### 🔹 Technical Definition

A **Hash Table** in C is an array of buckets where each entry stores a key-value pair. A **hash function** maps a key to an array index. **Collisions** (multiple keys hashing to the same index) are resolved via **chaining** (linked list at each bucket) or **open addressing** (probing within the array). C has no built-in hash map — it must be implemented manually.

---

### 🔹 Intuition / Core Idea

- The hash function is a bridge between an arbitrary key and a fixed array index.
- A good hash function distributes keys **uniformly** across all buckets, minimizing collisions.
- Load factor `α = n / m` (elements / buckets) must stay below `0.7` for good performance.

---

### 🔹 Key Properties in C

- **Hash function for integers:** `hash = key % TABLE_SIZE` (use prime TABLE_SIZE to reduce clustering).
- **Hash function for strings:** Polynomial rolling hash — `hash = Σ (str[i] × 31^i) % TABLE_SIZE`.
- **Chaining:** Each bucket is a `Node *` (head of a linked list).
- **Open Addressing (Linear Probing):** On collision, probe `(hash + 1) % m`, `(hash + 2) % m`, etc.
- **Tombstone:** A special marker for deleted slots in open addressing — required to maintain probe chains.

---

### 🔹 Operations & Time Complexity

| Operation | Best | Average | Worst |
|---|---|---|---|
| Insert | O(1) | O(1) | O(n) |
| Search / Lookup | O(1) | O(1) | O(n) |
| Delete | O(1) | O(1) | O(n) |
| Traversal | O(m) | O(m+n) | O(m+n) |

**Space Complexity:** `O(n)` for entries, `O(m)` for bucket array

---

### 🔹 C Implementation — Chaining

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define TABLE_SIZE 53   /* prime number */

/* ── Entry Node ── */
typedef struct Entry {
    char *key;
    int   value;
    struct Entry *next;
} Entry;

/* ── Hash Table ── */
typedef struct {
    Entry *buckets[TABLE_SIZE];
} HashTable;

/* ── Hash Function (djb2 for strings) ── */
unsigned int hashFunction(const char *key) {
    unsigned long hash = 5381;
    int c;
    while ((c = *key++))
        hash = ((hash << 5) + hash) + c;   /* hash * 33 + c */
    return hash % TABLE_SIZE;
}

/* ── Initialize ── */
void initHashTable(HashTable *ht) {
    for (int i = 0; i < TABLE_SIZE; i++)
        ht->buckets[i] = NULL;
}

/* ── Insert ── O(1) average */
void insert(HashTable *ht, const char *key, int value) {
    unsigned int idx = hashFunction(key);
    Entry *curr = ht->buckets[idx];
    /* Update if key already exists */
    while (curr) {
        if (strcmp(curr->key, key) == 0) { curr->value = value; return; }
        curr = curr->next;
    }
    /* New entry — prepend to chain */
    Entry *newEntry = (Entry *)malloc(sizeof(Entry));
    if (!newEntry) { perror("malloc"); return; }
    newEntry->key   = strdup(key);   /* allocates and copies key string */
    newEntry->value = value;
    newEntry->next  = ht->buckets[idx];
    ht->buckets[idx] = newEntry;
}

/* ── Search ── O(1) average */
int search(HashTable *ht, const char *key, int *outValue) {
    unsigned int idx = hashFunction(key);
    Entry *curr = ht->buckets[idx];
    while (curr) {
        if (strcmp(curr->key, key) == 0) { *outValue = curr->value; return 1; }
        curr = curr->next;
    }
    return 0;  /* not found */
}

/* ── Delete ── O(1) average */
void delete(HashTable *ht, const char *key) {
    unsigned int idx = hashFunction(key);
    Entry *curr = ht->buckets[idx], *prev = NULL;
    while (curr) {
        if (strcmp(curr->key, key) == 0) {
            if (prev) prev->next = curr->next;
            else      ht->buckets[idx] = curr->next;
            free(curr->key);
            free(curr);
            return;
        }
        prev = curr;
        curr = curr->next;
    }
}

/* ── Free Hash Table ── */
void freeHashTable(HashTable *ht) {
    for (int i = 0; i < TABLE_SIZE; i++) {
        Entry *curr = ht->buckets[i];
        while (curr) {
            Entry *next = curr->next;
            free(curr->key);
            free(curr);
            curr = next;
        }
        ht->buckets[i] = NULL;
    }
}
```

---

### 🔹 Open Addressing (Linear Probing) — Conceptual

```c
/* Tombstone marker for deleted slots */
#define EMPTY    0
#define OCCUPIED 1
#define DELETED  2

typedef struct {
    int key;
    int value;
    int state;    /* EMPTY, OCCUPIED, or DELETED (tombstone) */
} Slot;

Slot table[TABLE_SIZE];

/* Insert: find first EMPTY or DELETED slot starting from hash(key) */
/* Search: probe until key found OR EMPTY slot (DELETED does not stop search) */
/* Delete: set state = DELETED (tombstone) — do NOT set EMPTY */
```

---

### 🔹 Variations / Modifications

- **Chaining** — Linked list per bucket; simpler; handles high load factors.
- **Open Addressing (Linear/Quadratic/Double Hash)** — All data in array; more cache-friendly.
- **Cuckoo Hashing** — Two hash functions; O(1) worst-case lookup.
- **Consistent Hashing** — Distributed systems; nodes on a virtual ring; used in Memcached, DynamoDB.

---

### 🔹 Advantages & Disadvantages

| Advantages | Disadvantages |
|---|---|
| O(1) average insert/search/delete | O(n) worst case with bad hash / many collisions |
| Flexible key types with custom hash | Unordered — no sorted traversal |
| Foundation of many algorithms | Manual `free` for keys and entries in C |
| Cache-friendly (open addressing) | Tombstone accumulation in open addressing |

---

### 🔹 Interview Tips (C-Specific)

- **Common Questions:** Two Sum (using hash map), check duplicate in array, frequency counting, implement hash map from scratch.
- **C-Specific Traps:**
  - `strdup(key)` allocates memory — must `free(entry->key)` when deleting.
  - Using `int` keys vs `char *` keys — different hash functions and comparison (`==` vs `strcmp`).
  - Load factor management — ask "what happens when table gets full?" in interviews.
  - `TABLE_SIZE` should be **prime** to reduce clustering with modulo hashing.
- **Mistakes to Avoid:**
  - Forgetting tombstone in open addressing — breaks probe chains on lookup.
  - `free(entry)` without first `free(entry->key)` — memory leak.

---

### 🔹 Real-World Use Cases

- **C Standard Library `hsearch` / POSIX `hcreate`:** Basic hash table APIs in C.
- **GCC Symbol Table:** Hash table for variable/function name lookup during compilation.
- **DNS Cache:** Hash table mapping domain names to IP addresses.
- **Linux Kernel Routing Table:** Hash table for fast packet forwarding decisions.

---
---

## 9. 🌳 Binary Search Tree (BST)

### 🔹 Technical Definition

A **Binary Search Tree** in C is a binary tree where each node satisfies the BST property: `left->data < node->data < right->data` for all nodes recursively. Implemented with heap-allocated `Node` structs having `data`, `left`, and `right` pointer fields. Operations exploit the ordering to achieve O(log n) average complexity.

---

### 🔹 Intuition / Core Idea

- At every node, you make a **binary decision** — go left (smaller) or right (larger). This halves the search space in a balanced tree.
- **In-order traversal** (LNR) always produces sorted output — a fundamental BST property.

---

### 🔹 Key Properties in C

- BST **height** determines performance: balanced → O(log n); sorted insertion → O(n) (linked list degeneracy).
- Recursive C functions naturally mirror BST structure — each function call is a subtree.
- Returning `Node *` from recursive insert/delete is the cleanest C pattern.

---

### 🔹 Operations & Time Complexity

| Operation | Best | Average | Worst |
|---|---|---|---|
| Search | O(1) | O(log n) | O(n) |
| Insert | O(log n) | O(log n) | O(n) |
| Delete | O(log n) | O(log n) | O(n) |
| Min / Max | O(log n) | O(log n) | O(n) |
| In-order Traversal | O(n) | O(n) | O(n) |

**Space Complexity:** `O(n)` + O(h) for recursive call stack

---

### 🔹 C Implementation

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int data;
    struct Node *left;
    struct Node *right;
} Node;

Node *createNode(int val) {
    Node *n = (Node *)malloc(sizeof(Node));
    if (!n) { perror("malloc"); return NULL; }
    n->data  = val;
    n->left  = n->right = NULL;
    return n;
}

/* ── Search ── O(log n) avg */
Node *search(Node *root, int key) {
    if (!root || root->data == key) return root;
    if (key < root->data) return search(root->left,  key);
    else                  return search(root->right, key);
}

/* ── Insert ── O(log n) avg */
Node *insert(Node *root, int val) {
    if (!root) return createNode(val);
    if (val < root->data)       root->left  = insert(root->left,  val);
    else if (val > root->data)  root->right = insert(root->right, val);
    /* val == root->data: duplicate, ignore */
    return root;
}

/* ── Find Minimum ── */
Node *findMin(Node *root) {
    while (root && root->left) root = root->left;
    return root;
}

/* ── Delete ── O(log n) avg — 3 Cases */
Node *delete(Node *root, int key) {
    if (!root) return NULL;

    if (key < root->data)
        root->left = delete(root->left, key);
    else if (key > root->data)
        root->right = delete(root->right, key);
    else {
        /* Case 1: Leaf node */
        if (!root->left && !root->right) {
            free(root);
            return NULL;
        }
        /* Case 2: One child */
        if (!root->left) {
            Node *temp = root->right;
            free(root);
            return temp;
        }
        if (!root->right) {
            Node *temp = root->left;
            free(root);
            return temp;
        }
        /* Case 3: Two children — replace with in-order successor */
        Node *successor = findMin(root->right);
        root->data = successor->data;          /* copy successor's data */
        root->right = delete(root->right, successor->data); /* delete successor */
    }
    return root;
}

/* ── In-order Traversal (sorted output) ── */
void inorder(Node *root) {
    if (!root) return;
    inorder(root->left);
    printf("%d ", root->data);
    inorder(root->right);
}

/* ── Validate BST ── O(n) — Pass min/max bounds */
int validate(Node *root, long min, long max) {
    if (!root) return 1;
    if (root->data <= min || root->data >= max) return 0;
    return validate(root->left,  min, root->data) &&
           validate(root->right, root->data, max);
}
/* Call: validate(root, LONG_MIN, LONG_MAX) */

/* ── Height of Tree ── */
int height(Node *root) {
    if (!root) return 0;
    int lh = height(root->left);
    int rh = height(root->right);
    return 1 + (lh > rh ? lh : rh);
}

/* ── Free Entire Tree (post-order) ── */
void freeTree(Node *root) {
    if (!root) return;
    freeTree(root->left);
    freeTree(root->right);
    free(root);
}
```

---

### 🔹 Variations / Modifications

- **Augmented BST:** Each node stores subtree size — supports O(log n) kth-smallest queries.
- **Threaded BST:** `NULL` pointers replaced with in-order predecessor/successor — O(1) next element without a stack.
- **Balanced Variants:** AVL Tree, Red-Black Tree, Splay Tree — prevent O(n) degeneration.

---

### 🔹 Advantages & Disadvantages

| Advantages | Disadvantages |
|---|---|
| In-order gives sorted output | O(n) worst case on sorted input |
| Recursive C code mirrors structure cleanly | No automatic balancing |
| Supports floor, ceil, predecessor, successor | Not cache-friendly (scattered heap allocations) |

---

### 🔹 Interview Tips (C-Specific)

- **Common Questions:** Validate BST, Lowest Common Ancestor of BST, Kth Smallest in BST, Convert BST to Sorted Array, BST Iterator.
- **C-Specific Traps:**
  - Validate BST: Do **not** just check `node->left->data < node->data` — pass `(min, max)` bounds through recursion.
  - Three-case delete is always tested — know it cold.
  - `freeTree` must be **post-order** — free children before parent.
  - LCA in BST: if both keys < root → go left; both > root → go right; otherwise root is LCA. O(log n) for balanced BST.
- **Mistakes to Avoid:**
  - Forgetting to return the modified root from recursive insert/delete.
  - Not handling `NULL` root in every recursive function — base case is always `if (!root) return ...`.

---

### 🔹 Real-World Use Cases

- **Compilers:** Symbol table lookups with sorted key space.
- **In-memory Sorted Sets:** Used before switching to self-balancing trees.
- **Expression Trees:** Arithmetic expressions represented as binary trees.

---
---

## 10. 🃏 Cartesian Tree

### 🔹 Technical Definition

A **Cartesian Tree** in C is a binary tree built from an array where: (1) in-order traversal yields the original array sequence (BST on indices), and (2) it satisfies the min-heap property — every parent has a smaller value than its children. Uniquely defined for distinct-element arrays. Built in O(n) using a stack. The LCA of two nodes corresponds to the Range Minimum Query between those array indices.

---

### 🔹 Intuition / Core Idea

- The **root is always the minimum** of the array. Its left subtree is the Cartesian tree of elements to its left; its right subtree handles elements to its right.
- The stack-based O(n) construction maintains the **rightmost path** of the tree.
- Key insight for interviews: **LCA in Cartesian Tree = RMQ in original array.**

---

### 🔹 Operations & Time Complexity

| Operation | Best | Average | Worst |
|---|---|---|---|
| Build from Array | O(n) | O(n) | O(n) |
| Search by Index | O(log n) | O(log n) | O(n) |
| Range Min Query (via LCA) | O(log n) | O(log n) | O(n) |

**Space Complexity:** `O(n)`

---

### 🔹 C Implementation — O(n) Stack-Based Build

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int val, idx;
    struct Node *left, *right;
} Node;

Node *createNode(int val, int idx) {
    Node *n = (Node *)malloc(sizeof(Node));
    n->val = val; n->idx = idx;
    n->left = n->right = NULL;
    return n;
}

/* ── Build Cartesian Tree — O(n) using Stack ── */
Node *buildCartesianTree(int *arr, int n) {
    Node **stack = (Node **)malloc(n * sizeof(Node *));
    int top = -1;
    Node *root = NULL;

    for (int i = 0; i < n; i++) {
        Node *curr = createNode(arr[i], i);
        Node *last = NULL;

        /* Pop all nodes with value greater than current */
        while (top >= 0 && stack[top]->val > curr->val) {
            last = stack[top--];
        }
        /* Last popped becomes left child of current */
        curr->left = last;

        /* Current becomes right child of new stack top */
        if (top >= 0) stack[top]->right = curr;

        stack[++top] = curr;
    }
    root = stack[0];   /* bottom of stack = root */
    free(stack);
    return root;
}

/* In-order traversal should reproduce original array */
void inorder(Node *root) {
    if (!root) return;
    inorder(root->left);
    printf("arr[%d]=%d ", root->idx, root->val);
    inorder(root->right);
}

/* ── Free Cartesian Tree (post-order) ── ESSENTIAL */
void freeCartesianTree(Node *root) {
    if (!root) return;
    freeCartesianTree(root->left);
    freeCartesianTree(root->right);
    free(root);
}

/* ── Safe build with null check ── */
/*
    Node *root = NULL;
    if (n > 0)
        root = buildCartesianTree(arr, n);  // stack[0] safe only when n > 0
    // use root...
    freeCartesianTree(root);
*/
```

> **C Note:** `buildCartesianTree` assumes `n > 0`. Always guard the call — `stack[0]` on an empty input is undefined behaviour.

---

### 🔹 Variations / Modifications

- **Max-Cartesian Tree** — Heap is max instead of min; pop nodes with smaller value; root = array maximum.
- **Treap** — Cartesian Tree where keys are data values and priorities are random; acts as randomized BST.
- **Implicit Treap** — BST key is the implicit array index; supports O(log n) array split/merge — used in competitive programming.

---

### 🔹 Interview Tips (C-Specific)

- **Typically conceptual** — explain the LCA = RMQ insight and O(n) build.
- **C-Specific:** Stack used is an array of `Node *` — must allocate it.
- **Mistakes to Avoid:** Confusing Cartesian Tree (deterministic, from array) with Treap (uses random priorities).

---

### 🔹 Real-World Use Cases

- **Range Minimum Query:** Combined with LCA algorithms for O(1) RMQ after O(n) preprocessing.
- **Treap implementations:** Randomized BST with heap property.
- **String Algorithms:** Cartesian tree of LCP array used in suffix arrays.

---
---

## 11. 🗄️ B-Tree

### 🔹 Technical Definition

A **B-Tree of order `m`** is a self-balancing multi-way search tree where every non-root node has between `⌈m/2⌉ - 1` and `m - 1` keys. All leaves are at the **same depth**. Designed to minimize **disk I/O** — each node is sized to fit a disk block, maximizing keys per read. In C, nodes are typically large structs or heap-allocated arrays of keys and child pointers.

---

### 🔹 Intuition / Core Idea

- Unlike a BST (1 key, 2 children per node), a B-Tree node holds **many keys** and **many children** — the branching factor is high, so the tree is wide and short.
- A short tree means fewer disk accesses to reach any leaf. This is the entire point.

---

### 🔹 Key Properties in C

- B-Tree is designed for **on-disk** storage — each node ≈ one disk block (4KB typical).
- **Order m:** max `m` children, max `m-1` keys per node.
- **B+ Tree** (most used in databases): only leaf nodes hold data; internal nodes hold routing keys only; leaves linked as a sorted linked list for range scans.

---

### 🔹 Operations & Time Complexity

| Operation | Best | Average | Worst |
|---|---|---|---|
| Search | O(log n) | O(log n) | O(log n) |
| Insert | O(log n) | O(log n) | O(log n) |
| Delete | O(log n) | O(log n) | O(log n) |
| Range Query | O(log n + k) | O(log n + k) | O(log n + k) |

**Space Complexity:** `O(n)`

---

### 🔹 C Node Structure (Order 5 B-Tree)

```c
#define ORDER 5                    /* max children per node */
#define MAX_KEYS (ORDER - 1)       /* max keys per node = 4 */
#define MIN_KEYS (ORDER / 2 - 1)   /* min keys per non-root = 1 */

typedef struct BTreeNode {
    int   keys[MAX_KEYS];              /* sorted keys */
    struct BTreeNode *children[ORDER]; /* child pointers */
    int   n;                           /* current number of keys */
    int   isLeaf;                      /* 1 if leaf, 0 if internal */
} BTreeNode;

BTreeNode *createBTreeNode(int isLeaf) {
    BTreeNode *node = (BTreeNode *)calloc(1, sizeof(BTreeNode));
    if (!node) { perror("calloc"); return NULL; }
    node->isLeaf = isLeaf;
    node->n = 0;
    return node;
}

/* ── Search in B-Tree ── O(log n) */
BTreeNode *search(BTreeNode *root, int key) {
    int i = 0;
    /* Find first key >= key using linear scan (use binary search for large ORDER) */
    while (i < root->n && key > root->keys[i]) i++;

    if (i < root->n && root->keys[i] == key) return root;   /* found */
    if (root->isLeaf) return NULL;                           /* not found */
    return search(root->children[i], key);                  /* go down */
}

/* ── Split child y of x at index i ── O(1) pointer work, O(t) copies */
/*
 * Called when x->children[i] is full (has MAX_KEYS keys).
 * Splits it into two nodes. Median promoted to x.
 */
void splitChild(BTreeNode *x, int i) {
    int t = ORDER / 2;                       /* minimum degree */
    BTreeNode *y = x->children[i];           /* full child to split */
    BTreeNode *z = createBTreeNode(y->isLeaf); /* new right sibling */

    z->n = t - 1;
    /* Copy last (t-1) keys of y into z */
    for (int j = 0; j < t - 1; j++)
        z->keys[j] = y->keys[j + t];
    /* Copy last t children of y into z (if not leaf) */
    if (!y->isLeaf)
        for (int j = 0; j < t; j++)
            z->children[j] = y->children[j + t];
    y->n = t - 1;                            /* shrink y */

    /* Shift x's children right to make room for z */
    for (int j = x->n; j >= i + 1; j--)
        x->children[j + 1] = x->children[j];
    x->children[i + 1] = z;

    /* Shift x's keys right and insert the median */
    for (int j = x->n - 1; j >= i; j--)
        x->keys[j + 1] = x->keys[j];
    x->keys[i] = y->keys[t - 1];            /* promote median */
    x->n++;
}

/* ── Insert into non-full node ── O(log n) */
void insertNonFull(BTreeNode *x, int key) {
    int i = x->n - 1;
    if (x->isLeaf) {
        /* Shift keys right to find insertion position */
        while (i >= 0 && key < x->keys[i]) {
            x->keys[i + 1] = x->keys[i];
            i--;
        }
        x->keys[i + 1] = key;
        x->n++;
    } else {
        /* Find the child to descend into */
        while (i >= 0 && key < x->keys[i]) i--;
        i++;
        if (x->children[i]->n == MAX_KEYS) { /* child is full */
            splitChild(x, i);
            if (key > x->keys[i]) i++;       /* decide which new child */
        }
        insertNonFull(x->children[i], key);
    }
}

/* ── B-Tree Insert ── O(log n) */
BTreeNode *btreeInsert(BTreeNode *root, int key) {
    if (root->n == MAX_KEYS) {              /* root is full — split it */
        BTreeNode *newRoot = createBTreeNode(0);
        newRoot->children[0] = root;
        splitChild(newRoot, 0);             /* split old root */
        insertNonFull(newRoot, key);
        return newRoot;                     /* new root after split */
    }
    insertNonFull(root, key);
    return root;
}

/* ── Free B-Tree (post-order) ── ESSENTIAL */
void freeBTree(BTreeNode *root) {
    if (!root) return;
    if (!root->isLeaf)
        for (int i = 0; i <= root->n; i++)
            freeBTree(root->children[i]);
    free(root);
}

/*
 * Delete is the most complex operation — involves:
 * 1. Key in leaf: remove directly, shift keys left.
 * 2. Key in internal node: replace with predecessor/successor (from leaf), delete from leaf.
 * 3. Underflow (n < MIN_KEYS): borrow from sibling (rotate via parent key) OR merge sibling + parent key.
 * Merging reduces parent's key count — may propagate to root, shrinking tree height.
 * Describe this logic verbally in interviews; full C code is 150+ lines.
 */
```

---

### 🔹 Advantages & Disadvantages

| Advantages | Disadvantages |
|---|---|
| O(log n) guaranteed all ops | Complex split/merge implementation in C |
| Minimal disk I/O — high branching factor | Overkill for in-memory data |
| Always balanced — no degenerate case | Large node structs — more struct memory per allocation |

---

### 🔹 Interview Tips (C-Specific)

- **In interviews:** Mostly conceptual — describe the node structure, explain split/merge logic, and compare B-Tree vs B+ Tree.
- **Key Insight:** "I would size each `BTreeNode` struct to fit exactly one disk block (4096 bytes). With 4-byte integer keys and 8-byte pointers, order can be tuned accordingly."
- **B+ Tree vs B-Tree:** In B+ Tree, internal nodes store only routing keys (no data); all data at leaves; leaf nodes linked — enables O(k) range scans after O(log n) search.
- **Mistakes to Avoid:** Calling it "Binary Tree" — B-Tree is a multi-way (not binary) tree.

---

### 🔹 Real-World Use Cases

- **PostgreSQL / MySQL InnoDB / SQLite:** Primary indexes are B+ Trees.
- **NTFS / HFS+ / ext4:** File system directory trees use B-Tree structures.
- **MongoDB WiredTiger:** B-Tree storage engine in C.
- **Oracle Database:** B* Tree variant for indexes.

---
---

## 12. 🔴⚫ Red-Black Tree

### 🔹 Technical Definition

A **Red-Black Tree** in C is a self-balancing BST where each node has a color field (`RED` or `BLACK`) and satisfies five invariants that collectively bound the tree height to `2 log₂(n+1)`. All operations remain O(log n) in the worst case. Used internally by `std::map`/`std::set` (C++) and Linux kernel's CFS scheduler.

---

### 🔹 Intuition / Core Idea

- Instead of precisely tracking heights (like AVL), Red-Black Trees use **color encoding** as a lightweight balancing signal. The color rules prevent any path from being more than twice the length of another.
- Fewer rotations per insert/delete than AVL Trees — better for write-heavy workloads.

---

### 🔹 The 5 Red-Black Properties

1. Every node is `RED` or `BLACK`.
2. The **root** is always `BLACK`.
3. All **NIL leaves** (null sentinels) are `BLACK`.
4. A `RED` node's children must both be `BLACK` (no two consecutive reds).
5. Every root-to-NIL path has the same **black-height** (count of black nodes).

---

### 🔹 Operations & Time Complexity

| Operation | Best | Average | Worst |
|---|---|---|---|
| Search | O(log n) | O(log n) | O(log n) |
| Insert | O(log n) | O(log n) | O(log n) |
| Delete | O(log n) | O(log n) | O(log n) |
| Rotation | O(1) | O(1) | O(1) |

**Space Complexity:** `O(n)` + 1 bit per node for color

---

### 🔹 C Node Structure & Rotation

```c
typedef enum { RED, BLACK } Color;

typedef struct RBNode {
    int   data;
    Color color;
    struct RBNode *left, *right, *parent;
} RBNode;

/* ── NIL Sentinel (global) ── */
/* NIL_NODE children/parent initialized to themselves via initNIL() below */
RBNode NIL_NODE = { 0, BLACK, NULL, NULL, NULL };
RBNode *NIL = &NIL_NODE;

/* Call once at program start — NIL must point to itself for rotation safety */
void initNIL(void) {
    NIL_NODE.left = NIL_NODE.right = NIL_NODE.parent = NIL;
}

RBNode *createRBNode(int val) {
    RBNode *n = (RBNode *)malloc(sizeof(RBNode));
    if (!n) { perror("malloc"); return NULL; }
    n->data   = val;
    n->color  = RED;     /* new nodes always RED */
    n->left   = n->right = n->parent = NIL;
    return n;
}

/* ── Left Rotation ── O(1) */
void leftRotate(RBNode **root, RBNode *x) {
    RBNode *y = x->right;
    x->right = y->left;
    if (y->left != NIL) y->left->parent = x;
    y->parent = x->parent;
    if (x->parent == NIL)        *root = y;
    else if (x == x->parent->left) x->parent->left  = y;
    else                           x->parent->right = y;
    y->left   = x;
    x->parent = y;
}

/* ── Right Rotation ── O(1) (mirror of left) */
void rightRotate(RBNode **root, RBNode *x) {
    RBNode *y = x->left;
    x->left = y->right;
    if (y->right != NIL) y->right->parent = x;
    y->parent = x->parent;
    if (x->parent == NIL)         *root = y;
    else if (x == x->parent->right) x->parent->right = y;
    else                            x->parent->left  = y;
    y->right  = x;
    x->parent = y;
}

/* ── Insert Fixup — fix "two consecutive reds" after insertion ── */
void insertFixup(RBNode **root, RBNode *z) {
    while (z->parent->color == RED) {
        if (z->parent == z->parent->parent->left) {
            RBNode *uncle = z->parent->parent->right;
            if (uncle->color == RED) {
                /* Case 1: Uncle RED → recolor, move problem up */
                z->parent->color         = BLACK;
                uncle->color             = BLACK;
                z->parent->parent->color = RED;
                z = z->parent->parent;
            } else {
                if (z == z->parent->right) {
                    /* Case 2: Uncle BLACK, z is right child → left-rotate parent */
                    z = z->parent;
                    leftRotate(root, z);
                }
                /* Case 3: Uncle BLACK, z is left child → right-rotate grandparent */
                z->parent->color         = BLACK;
                z->parent->parent->color = RED;
                rightRotate(root, z->parent->parent);
            }
        } else {
            /* Mirror: parent is right child of grandparent */
            RBNode *uncle = z->parent->parent->left;
            if (uncle->color == RED) {
                z->parent->color         = BLACK;
                uncle->color             = BLACK;
                z->parent->parent->color = RED;
                z = z->parent->parent;
            } else {
                if (z == z->parent->left) {
                    z = z->parent;
                    rightRotate(root, z);
                }
                z->parent->color         = BLACK;
                z->parent->parent->color = RED;
                leftRotate(root, z->parent->parent);
            }
        }
    }
    (*root)->color = BLACK; /* Property 2: root is always BLACK */
}

/* ── RB Insert = BST insert + insertFixup ── O(log n) */
void rbInsert(RBNode **root, int val) {
    RBNode *z = createRBNode(val);
    RBNode *y = NIL, *x = *root;
    while (x != NIL) {             /* standard BST descent */
        y = x;
        x = (z->data < x->data) ? x->left : x->right;
    }
    z->parent = y;
    if (y == NIL)             *root    = z;
    else if (z->data < y->data) y->left  = z;
    else                         y->right = z;
    insertFixup(root, z);          /* restore RB properties */
}

/* ── Free RB Tree (post-order, skip NIL sentinel) ── ESSENTIAL */
void freeRBT(RBNode *root) {
    if (!root || root == NIL) return;
    freeRBT(root->left);
    freeRBT(root->right);
    free(root);
}
```

---

### 🔹 Advantages & Disadvantages

| Advantages | Disadvantages |
|---|---|
| O(log n) worst case — all operations | Most complex BST to implement correctly in C |
| Fewer rotations than AVL on insert/delete | More memory per node (color + parent pointer) |
| Used in Linux kernel — battle-tested | Hard to debug — color violations are subtle |

---

### 🔹 Interview Tips (C-Specific)

- **In interviews:** Mostly conceptual — state the 5 properties, explain insert-fix cases.
- **Key Statement:** "I would implement this as `RBNode` structs on the heap with a global NIL sentinel. The NIL sentinel eliminates null checks in rotation code."
- **Tricky Points:** Parent pointer is essential for rotations in C — unlike recursive AVL.
- **Mistakes to Avoid:** Forgetting to make the root `BLACK` after every insert — property 2 violation.

---

### 🔹 Real-World Use Cases

- **Linux CFS Scheduler (`kernel/sched/fair.c`):** Processes keyed by virtual runtime in RB-Tree.
- **`std::map` / `std::set` in GCC libstdc++:** Red-Black Tree backed.
- **Nginx:** Timer management using RB-Trees.
- **Java `TreeMap`:** Red-Black Tree in Java standard library.

---
---

## 13. 🔁 Splay Tree

### 🔹 Technical Definition

A **Splay Tree** in C is a self-adjusting BST where every access (search, insert, delete) **splays** (moves) the accessed node to the root via rotations. No balance metadata is stored per node. Achieves amortized O(log n) for all operations. Exploits temporal locality — recently accessed nodes remain near the root.

---

### 🔹 Intuition / Core Idea

- Think of a **self-organizing bookshelf** — the book you just touched goes back to the front.
- Three splay cases: **Zig** (parent is root → single rotate), **Zig-Zig** (same direction → double rotate parent first), **Zig-Zag** (opposite direction → two opposite rotations).

---

### 🔹 Operations & Time Complexity

| Operation | Best | Average (Amortized) | Worst (Single Op) |
|---|---|---|---|
| Search | O(1) | O(log n) | O(n) |
| Insert | O(1) | O(log n) | O(n) |
| Delete | O(1) | O(log n) | O(n) |
| Splay | O(1) | O(log n) | O(n) |

**Space Complexity:** `O(n)` — no balance fields stored

---

### 🔹 C Implementation

```c
typedef struct SNode {
    int data;
    struct SNode *left, *right;
} SNode;

SNode *createSNode(int val) {
    SNode *n = (SNode *)malloc(sizeof(SNode));
    if (!n) { perror("malloc"); return NULL; }
    n->data = val; n->left = n->right = NULL;
    return n;
}

/* ── Right Rotation ── */
SNode *rightRotate(SNode *x) {
    SNode *y = x->left;
    x->left  = y->right;
    y->right = x;
    return y;   /* y is new root of this subtree */
}

/* ── Left Rotation ── */
SNode *leftRotate(SNode *x) {
    SNode *y = x->right;
    x->right = y->left;
    y->left  = x;
    return y;
}

/* ── Splay(root, key): Bring key to root ── O(log n) amortized */
SNode *splay(SNode *root, int key) {
    if (!root || root->data == key) return root;

    if (key < root->data) {
        if (!root->left) return root;
        /* Zig-Zig (Left-Left) */
        if (key < root->left->data) {
            root->left->left = splay(root->left->left, key);
            root = rightRotate(root);
        }
        /* Zig-Zag (Left-Right) */
        else if (key > root->left->data) {
            root->left->right = splay(root->left->right, key);
            if (root->left->right) root->left = leftRotate(root->left);
        }
        return root->left ? rightRotate(root) : root;
    } else {
        if (!root->right) return root;
        /* Zig-Zig (Right-Right) */
        if (key > root->right->data) {
            root->right->right = splay(root->right->right, key);
            root = leftRotate(root);
        }
        /* Zig-Zag (Right-Left) */
        else if (key < root->right->data) {
            root->right->left = splay(root->right->left, key);
            if (root->right->left) root->right = rightRotate(root->right);
        }
        return root->right ? leftRotate(root) : root;
    }
}

/* ── Insert ── */
SNode *insert(SNode *root, int key) {
    if (!root) return createSNode(key);
    root = splay(root, key);
    if (root->data == key) return root;  /* duplicate */

    SNode *newNode = createSNode(key);
    if (key < root->data) {
        newNode->right = root;
        newNode->left  = root->left;
        root->left     = NULL;
    } else {
        newNode->left  = root;
        newNode->right = root->right;
        root->right    = NULL;
    }
    return newNode;
}

/* ── Search ── splays found/last-accessed node to root */
SNode *search(SNode *root, int key) {
    return splay(root, key);
}

/* ── Delete ── O(log n) amortized */
SNode *deleteSplay(SNode *root, int key) {
    if (!root) return NULL;
    root = splay(root, key);
    if (root->data != key) return root;   /* key not found */

    if (!root->left) {
        SNode *right = root->right;
        free(root);
        return right;
    }
    /* Splay max of left subtree to its root, attach right subtree */
    SNode *left = splay(root->left, key); /* key not in left, splays max to top */
    left->right = root->right;
    free(root);
    return left;
}

/* ── Free Splay Tree (post-order) ── ESSENTIAL */
void freeSplay(SNode *root) {
    if (!root) return;
    freeSplay(root->left);
    freeSplay(root->right);
    free(root);
}
```

---

### 🔹 Advantages & Disadvantages

| Advantages | Disadvantages |
|---|---|
| Simplest node structure — no balance fields | No per-operation worst-case guarantee |
| Excellent for temporal-locality workloads | Every access restructures the tree — write overhead |
| O(log n) amortized all ops | Poor for adversarial access sequences |

---

### 🔹 Interview Tips (C-Specific)

- **Key Points:** Zig-Zig uses double rotation in the **same** direction (not two single rotations from bottom up). This is critical — two single rotations would not give amortized O(log n).
- **C-Specific:** Return the new root from `splay()` — the root changes on every access.
- **Mistakes to Avoid:** Claiming O(log n) worst-case — amortized only.

---

### 🔹 Real-World Use Cases

- **Windows NT VM Manager:** Virtual address regions stored in splay trees.
- **Network Routers:** Flow table caching using temporal locality.
- **Caching systems:** Self-organizing due to recency — recently accessed data near root.

---
---

## 14. ⚖️ AVL Tree

### 🔹 Technical Definition

An **AVL Tree** in C is a self-balancing BST where each node stores its `height` and maintains the invariant: `|height(left) - height(right)| ≤ 1` for every node. After every insert or delete, balance is restored via at most one or two **rotations**. Guarantees O(log n) for all operations with stricter balance than Red-Black Trees.

---

### 🔹 Intuition / Core Idea

- After every BST insert/delete, walk up the tree and check the **balance factor** at each node.
- If |BF| > 1, apply the appropriate rotation (LL, RR, LR, RL).
- The height is bounded by `1.44 log₂(n)` — the strictest balance among common BSTs.

---

### 🔹 Key Properties in C

- **Balance Factor (BF)** = `height(right) - height(left)` ∈ {-1, 0, +1}.
- Each node stores `int height` — updated after every insert/delete on the return path of recursion.
- Insert requires **at most 1 rotation** (single or double). Delete may require O(log n) rotations.
- Height of null node = -1 (or 0 with adjusted formula); height of leaf = 0.

---

### 🔹 Operations & Time Complexity

| Operation | Best | Average | Worst |
|---|---|---|---|
| Search | O(log n) | O(log n) | O(log n) |
| Insert | O(log n) | O(log n) | O(log n) |
| Delete | O(log n) | O(log n) | O(log n) |
| Rotation | O(1) | O(1) | O(1) |

**Space Complexity:** `O(n)` + O(1) extra per node for `height`

---

### 🔹 C Implementation

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct AVLNode {
    int data, height;
    struct AVLNode *left, *right;
} AVLNode;

/* ── Helpers ── */
int height(AVLNode *n) { return n ? n->height : 0; }
int max(int a, int b)  { return a > b ? a : b; }

int getBalance(AVLNode *n) {
    return n ? height(n->right) - height(n->left) : 0;
}

void updateHeight(AVLNode *n) {
    if (n) n->height = 1 + max(height(n->left), height(n->right));
}

AVLNode *createAVLNode(int val) {
    AVLNode *n = (AVLNode *)malloc(sizeof(AVLNode));
    if (!n) { perror("malloc"); return NULL; }
    n->data = val; n->height = 1;
    n->left = n->right = NULL;
    return n;
}

/* ── Right Rotation (LL Case) ── O(1) */
AVLNode *rightRotate(AVLNode *y) {
    AVLNode *x  = y->left;
    AVLNode *T2 = x->right;
    x->right = y;
    y->left  = T2;
    updateHeight(y);   /* update child before parent */
    updateHeight(x);
    return x;           /* x is new subtree root */
}

/* ── Left Rotation (RR Case) ── O(1) */
AVLNode *leftRotate(AVLNode *x) {
    AVLNode *y  = x->right;
    AVLNode *T2 = y->left;
    y->left  = x;
    x->right = T2;
    updateHeight(x);
    updateHeight(y);
    return y;
}

/* ── Balance a Node ── */
AVLNode *balance(AVLNode *node) {
    updateHeight(node);
    int bf = getBalance(node);

    /* LL Case: left-heavy, left child is left-heavy or balanced */
    if (bf < -1 && getBalance(node->left) <= 0)
        return rightRotate(node);

    /* LR Case: left-heavy, left child is right-heavy */
    if (bf < -1 && getBalance(node->left) > 0) {
        node->left = leftRotate(node->left);
        return rightRotate(node);
    }

    /* RR Case: right-heavy, right child is right-heavy or balanced */
    if (bf > 1 && getBalance(node->right) >= 0)
        return leftRotate(node);

    /* RL Case: right-heavy, right child is left-heavy */
    if (bf > 1 && getBalance(node->right) < 0) {
        node->right = rightRotate(node->right);
        return leftRotate(node);
    }

    return node;   /* already balanced */
}

/* ── Insert ── O(log n) */
AVLNode *insert(AVLNode *root, int val) {
    if (!root) return createAVLNode(val);
    if (val < root->data)      root->left  = insert(root->left,  val);
    else if (val > root->data) root->right = insert(root->right, val);
    else return root;   /* duplicate */
    return balance(root);
}

/* ── Find Minimum ── */
AVLNode *findMin(AVLNode *root) {
    while (root->left) root = root->left;
    return root;
}

/* ── Delete ── O(log n) */
AVLNode *delete(AVLNode *root, int val) {
    if (!root) return NULL;
    if (val < root->data)      root->left  = delete(root->left,  val);
    else if (val > root->data) root->right = delete(root->right, val);
    else {
        if (!root->left || !root->right) {
            AVLNode *temp = root->left ? root->left : root->right;
            free(root);
            return temp;
        }
        AVLNode *successor = findMin(root->right);
        root->data  = successor->data;
        root->right = delete(root->right, successor->data);
    }
    return balance(root);
}

/* ── In-order Traversal ── */
void inorder(AVLNode *root) {
    if (!root) return;
    inorder(root->left);
    printf("%d(h=%d) ", root->data, root->height);
    inorder(root->right);
}

/* ── Free Tree ── */
void freeAVL(AVLNode *root) {
    if (!root) return;
    freeAVL(root->left);
    freeAVL(root->right);
    free(root);
}
```

---

### 🔹 AVL vs Red-Black Tree Comparison

| Property | AVL Tree | Red-Black Tree |
|---|---|---|
| Balance | Strict (|BF| ≤ 1), height ≤ 1.44 log n | Relaxed, height ≤ 2 log n |
| Search | Faster (shorter tree) | Slightly slower |
| Insert/Delete | Slower (more rotations) | Faster (fewer rotations) |
| Metadata per node | `height` (integer) | `color` (1 bit) |
| Best Use Case | Read-heavy workloads | Write-heavy workloads |
| Implementation | Moderate complexity | High complexity |

---

### 🔹 Advantages & Disadvantages

| Advantages | Disadvantages |
|---|---|
| Strictest balance — fastest search | More rotations on insert/delete vs RBT |
| Simpler than Red-Black Tree in C | `height` field adds 4 bytes per node |
| Clean recursive implementation | Delete may need O(log n) rotations |

---

### 🔹 Interview Tips (C-Specific)

- **Common Questions:** Insert sequence and draw the resulting AVL tree, explain which rotation applies, compare with RBT.
- **C-Specific Traps:**
  - Update height of child **before** parent in rotations — order matters.
  - Use `height(node->left)` helper that returns 0 for `NULL` — avoids null dereference.
  - `balance()` function that subsumes `updateHeight` + rotation — clean pattern.
- **Mistakes to Avoid:**
  - Not updating heights after rotations — tree has stale height values.
  - Applying single rotation when double is needed (LR/RL cases).

---

### 🔹 Real-World Use Cases

- **In-memory sorted indexes:** When reads greatly outnumber writes.
- **Computational geometry:** Event queues in sweep-line algorithms.
- **Real-time systems:** Predictable O(log n) with strict bounds.
- **Some DBMS in-memory indexes:** When write volume is controlled.

---
---

## 15. 🌐 KD Tree

### 🔹 Technical Definition

A **KD Tree (K-Dimensional Tree)** in C is a binary tree that partitions `k`-dimensional space by alternating splitting hyperplanes across each dimension. Each node represents a `k`-dimensional point and divides the remaining points along dimension `depth % k`. Enables efficient nearest-neighbor search (NNS) and range queries in multidimensional space.

---

### 🔹 Intuition / Core Idea

- For 2D points (k=2): alternate splitting by x (even depth) and y (odd depth).
- At each level, you partition space into two half-planes — left subtree holds points below/left of the splitting value, right subtree holds points above/right.
- **Backtracking** is essential in NNS — after reaching a leaf, unwind and check if the other half could contain a closer point.

---

### 🔹 Key Properties in C

- Each node stores a k-dimensional point as `int point[K]` (or `double point[K]` for float).
- **Splitting dimension:** `dim = depth % K`.
- **Balanced build:** Find median along current dimension — sort and take midpoint. O(n log n) build.
- **Curse of dimensionality:** For K > ~20, nearly all points become equidistant — KD Tree pruning becomes ineffective, approaching O(n) brute force.

---

### 🔹 Operations & Time Complexity

| Operation | Best | Average | Worst |
|---|---|---|---|
| Build (balanced) | O(n log n) | O(n log n) | O(n log n) |
| Insert | O(log n) | O(log n) | O(n) |
| Exact Search | O(log n) | O(log n) | O(n) |
| Nearest Neighbor (NNS) | O(log n) | O(log n) | O(n) |
| Range Search (2D) | O(√n + r) | O(√n + r) | O(n) |

**Space Complexity:** `O(n)`

---

### 🔹 C Implementation (2D KD Tree)

```c
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#include <float.h>

#define K 2   /* 2-dimensional */

typedef struct KDNode {
    int point[K];
    struct KDNode *left, *right;
} KDNode;

KDNode *createKDNode(int point[K]) {
    KDNode *n = (KDNode *)malloc(sizeof(KDNode));
    if (!n) { perror("malloc"); return NULL; }
    for (int i = 0; i < K; i++) n->point[i] = point[i];
    n->left = n->right = NULL;
    return n;
}

/* ── Comparator for qsort — uses g_sortDim to pick dimension ── */
static int g_sortDim;  /* set before each qsort call */
int cmpByDim(const void *a, const void *b) {
    const int *pa = (const int *)a;   /* points to point[0] of element a */
    const int *pb = (const int *)b;
    return pa[g_sortDim] - pb[g_sortDim];
}

/*
 * Build Balanced KD Tree — O(n log n)
 * points: 2D array, n rows, K columns (each row is one k-dim point)
 * Sorts along current dimension, picks median as node, recurses on halves.
 */
KDNode *buildKDTree(int (*points)[K], int n, int depth) {
    if (n <= 0) return NULL;
    if (n == 1) return createKDNode(points[0]);  /* leaf */

    int dim = depth % K;
    g_sortDim = dim;

    /* Sort along current dimension to find the true median */
    qsort(points, n, sizeof(points[0]), cmpByDim);

    int mid = n / 2;                            /* median index */
    KDNode *node = createKDNode(points[mid]);
    node->left  = buildKDTree(points,           mid,         depth + 1);
    node->right = buildKDTree(points + mid + 1, n - mid - 1, depth + 1);
    return node;
}

/*
 * NOTE: g_sortDim is a global — not thread-safe.
 * For thread-safe builds, pass dimension as a parameter via a wrapper struct
 * and use qsort_r (Linux/BSD) or equivalent.
 */

/* ── Euclidean Distance Squared ── */
double distSq(int a[K], int b[K]) {
    double d = 0;
    for (int i = 0; i < K; i++) d += (double)(a[i] - b[i]) * (a[i] - b[i]);
    return d;
}

/* ── Nearest Neighbor Search ── O(log n) avg */
void NNS(KDNode *root, int target[K], int depth,
         KDNode **best, double *bestDist) {
    if (!root) return;

    double d = distSq(root->point, target);
    if (d < *bestDist) {
        *bestDist = d;
        *best     = root;
    }

    int dim  = depth % K;
    double diff = target[dim] - root->point[dim];

    /* Visit nearer side first */
    KDNode *near = diff < 0 ? root->left  : root->right;
    KDNode *far  = diff < 0 ? root->right : root->left;

    NNS(near, target, depth + 1, best, bestDist);

    /* Check if far side could contain a closer point */
    if ((double)diff * diff < *bestDist)  /* splitting hyperplane within best dist */
        NNS(far, target, depth + 1, best, bestDist);
}

/* ── Usage ── */
/*
    int target[K] = {3, 4};
    KDNode *best = NULL;
    double bestDist = DBL_MAX;
    NNS(root, target, 0, &best, &bestDist);
    printf("Nearest: (%d, %d)\n", best->point[0], best->point[1]);
*/

/* ── Free KD Tree ── */
void freeKDTree(KDNode *root) {
    if (!root) return;
    freeKDTree(root->left);
    freeKDTree(root->right);
    free(root);
}
```

---

### 🔹 Variations / Modifications

- **Ball Tree** — Hypersphere bounding (better for high dimensions than hyperplanes).
- **R-Tree** — Bounding rectangles; used in PostGIS/spatial databases.
- **Approximate NNS (HNSW, Annoy, FAISS):** Vector databases for AI embeddings — when exact NNS is too slow for high dimensions.

---

### 🔹 Advantages & Disadvantages

| Advantages | Disadvantages |
|---|---|
| O(log n) avg NNS for low K (≤ 20) | Degrades to O(n) in high dimensions |
| O(n log n) build; O(n) space | Backtracking logic easy to miss in C |
| No extra index overhead | Dynamic insert can unbalance the tree |

---

### 🔹 Interview Tips (C-Specific)

- **Common Questions:** Design a "find nearest restaurant" system, explain NNS with backtracking, range search in 2D.
- **C-Specific Traps:**
  - Forgetting to check if `diff * diff < *bestDist` — the hyperplane check in NNS. Without this, you miss closer points on the other side.
  - `DBL_MAX` from `<float.h>` as initial bestDist — never assume a maximum.
  - `freeKDTree` is post-order — same pattern as BST free.
- **Mistakes to Avoid:**
  - Claiming O(log n) guaranteed for NNS — it is average case; adversarial point layouts can cause O(n).
  - Forgetting the backtrack step — the most common NNS implementation error.

---

### 🔹 Real-World Use Cases

- **scikit-learn `KNeighborsClassifier`:** C-backed KD Tree for k-NN classification.
- **Robotics (ROS / PCL):** Point cloud nearest-neighbor matching in LiDAR processing (C/C++).
- **Game Engines (C-based):** Collision detection, spatial partitioning.
- **GPS/Mapping:** Find nearest POI (point of interest) in geographic databases.

---
---

## 📊 Final Comparison Table

> Worst-case complexity unless marked †(average case)

| Data Structure | Access | Search | Insert | Delete | Space | C-Specific Note |
|---|---|---|---|---|---|---|
| **Array** | O(1) | O(n) | O(n) | O(n) | O(n) | Pointer arithmetic; bounds unchecked |
| **Stack** | O(n) | O(n) | O(1) | O(1) | O(n) | Array or LL; manual `free` for LL |
| **Queue** | O(n) | O(n) | O(1) | O(1) | O(n) | Circular array avoids front drift |
| **Singly LL** | O(n) | O(n) | O(1) head | O(1) head | O(n) | `Node **` for head modification |
| **Doubly LL** | O(n) | O(n) | O(1) ends | O(1) †given ref | O(n) | Update both `prev` and `next` |
| **Circular LL** | O(n) | O(n) | O(1) † | O(1) head | O(n) | Break circle before `free` |
| **Skip List** | O(n) | O(log n)† | O(log n)† | O(log n)† | O(n) | `forward[]` array per node |
| **Hash Table** | O(n) | O(1)† | O(1)† | O(1)† | O(n) | `strdup`+`free` for string keys |
| **BST** | O(n) | O(n) | O(n) | O(n) | O(n) | Recursive; O(h) call stack |
| **Cartesian Tree** | O(n) | O(n) | O(n) | O(n) | O(n) | Stack-based O(n) build |
| **B-Tree** | O(log n) | O(log n) | O(log n) | O(log n) | O(n) | Large node structs; disk-aligned |
| **Red-Black Tree** | O(log n) | O(log n) | O(log n) | O(log n) | O(n) | Parent ptr + NIL sentinel |
| **Splay Tree** | O(n)† | O(log n)† | O(log n)† | O(log n)† | O(n) | No height/color field |
| **AVL Tree** | O(log n) | O(log n) | O(log n) | O(log n) | O(n) | `height` field; cleaner recursion |
| **KD Tree** | O(n) | O(n) | O(log n)† | O(log n)† | O(n) | `point[K]` array; backtrack in NNS |

> **†** = average/amortized case (not worst case)

---

## 🔧 C-Specific Common Pitfalls & Golden Rules

### Memory Management Rules

```c
/* RULE 1: Every malloc needs a free */
Node *n = malloc(sizeof(Node));
/* ... use n ... */
free(n);
n = NULL;   /* avoid dangling pointer */

/* RULE 2: Check malloc return */
Node *n = malloc(sizeof(Node));
if (!n) { perror("malloc"); exit(EXIT_FAILURE); }

/* RULE 3: Free strings before freeing struct */
free(entry->key);   /* free heap string first */
free(entry);        /* then free the struct */

/* RULE 4: Free tree post-order */
void freeTree(Node *root) {
    if (!root) return;
    freeTree(root->left);    /* free children first */
    freeTree(root->right);
    free(root);              /* then parent */
}

/* RULE 5: Break circular list before freeing */
tail->next = NULL;   /* break the cycle */
/* then free normally */
```

### Pointer Patterns for Linked Structures

```c
/* Pattern 1: Double pointer for head modification */
void insertHead(Node **head, int val) {
    Node *n = createNode(val);
    n->next = *head;
    *head = n;
}
/* Call: insertHead(&head, 42); */

/* Pattern 2: Arrow operator for struct pointer */
node->data  /* same as (*node).data  — always use -> */

/* Pattern 3: Three-pointer reversal */
Node *prev = NULL, *curr = head, *next;
while (curr) {
    next = curr->next;   /* 1. save next */
    curr->next = prev;   /* 2. reverse */
    prev = curr;         /* 3. advance */
    curr = next;         /* 4. advance */
}
head = prev;
```

### Quick Selection Guide (C Context)

```
Need O(1) indexed access?                  → Array (arr[i])
LIFO with fixed max size?                  → Stack (array-based, no malloc per op)
FIFO?                                      → Circular Array Queue
Dynamic insertions, O(1) at head?          → Singly Linked List
O(1) delete given pointer + LRU?           → Doubly Linked List
Round-robin / cyclic processing?           → Circular Linked List
Fast key-value store in C?                 → Hash Table (chaining)
Sorted dynamic set, simple to implement?   → Skip List
Sorted dynamic set, strict O(log n)?       → AVL Tree
Write-heavy sorted map?                    → Red-Black Tree
Disk-based index / database?              → B+ Tree
Temporal locality / self-organizing?       → Splay Tree
Spatial nearest-neighbor (low dim)?        → KD Tree
Array-based range minimum?                 → Cartesian Tree
```

---

## 📎 C Standard Library Functions Cheat Sheet

```c
/* Memory */
#include <stdlib.h>
malloc(size)            /* allocate uninitialized heap memory */
calloc(n, size)         /* allocate + zero-initialize n elements */
realloc(ptr, new_size)  /* resize allocation */
free(ptr)               /* deallocate */

/* String */
#include <string.h>
strcmp(a, b)            /* compare strings: 0 if equal */
strcpy(dst, src)        /* copy string */
strdup(str)             /* allocate + copy string (POSIX) */
memcpy(dst, src, n)     /* copy n bytes */
memset(ptr, val, n)     /* fill n bytes with val */

/* Math */
#include <math.h>
sqrt(x), pow(x, y), fabs(x)
/* Link with -lm flag: gcc prog.c -lm */

/* I/O */
#include <stdio.h>
printf, scanf, fprintf, fscanf
perror("msg")           /* print error + errno message */

/* Limits */
#include <limits.h>
INT_MAX, INT_MIN, LONG_MAX, LONG_MIN
#include <float.h>
DBL_MAX, FLT_MAX        /* used for initial "best distance" in NNS */
```

---

## 🏁 Complexity Memory Aid (C Edition)

```
Array:        arr[i] = O(1) | middle insert/delete = O(n)
Stack/Queue:  push/pop/enqueue/dequeue = O(1) always
Linked Lists: head ops = O(1) | index access = O(n) | no arr[i]
Hash Table:   O(1) avg — need good hash + low load factor
BST:          O(log n) avg | O(n) worst (unbalanced/sorted insert)
AVL Tree:     O(log n) ALL — guaranteed, strict balance
Red-Black:    O(log n) ALL — guaranteed, relaxed balance
B-Tree:       O(log n) ALL — disk-optimized, multi-key nodes
Splay Tree:   O(log n) AMORTIZED — restructures on every access
Skip List:    O(log n) EXPECTED — probabilistic, no hard guarantee
KD Tree:      O(log n) AVG for NNS | O(n) worst | must backtrack
```

---

> **Compilation Tip:** Compile C DSA programs with: `gcc -Wall -Wextra -g -o prog prog.c -lm`
> - `-Wall -Wextra`: enables all warnings — catch pointer and type errors early
> - `-g`: include debug symbols for `gdb`
> - `-lm`: link math library for `sqrt`, `pow`

---

*📝 Study Tip: Implement each data structure from scratch in C — no internet, no reference. The act of managing pointers, malloc, and free manually cements deep understanding that carries into any language.*

---

> **Last Updated:** 2025 | Language: C (C99/C11)
> **Covers:** 15 Data Structures with full C implementations
> **Target:** Fresher to mid-level C/Systems engineering interviews


---
---

## 🧩 Common C DSA Coding Patterns

These patterns appear repeatedly across problems. Knowing them by name and shape is as important as knowing the data structures themselves.

---

### Pattern 1: Two Pointer

```c
/* Classic: Find pair summing to target in sorted array */
int left = 0, right = n - 1;
while (left < right) {
    int sum = arr[left] + arr[right];
    if (sum == target) { /* found */ break; }
    else if (sum < target) left++;
    else                   right--;
}
```

- **When:** Sorted array, finding pairs, palindrome check, partitioning.
- **Complexity:** O(n) time, O(1) space.

---

### Pattern 2: Fast / Slow Pointer (Floyd's Tortoise & Hare)

```c
/* Detect cycle / find middle of linked list */
Node *slow = head, *fast = head;
while (fast && fast->next) {
    slow = slow->next;
    fast = fast->next->next;
    if (slow == fast) { /* cycle */ break; }
}
/* After loop (no cycle): slow == middle node */
```

- **When:** Linked list cycle detection, finding middle, finding cycle start.
- **Complexity:** O(n) time, O(1) space.

---

### Pattern 3: Sliding Window

```c
/* Maximum sum subarray of size k */
int sum = 0, maxSum = 0;
for (int i = 0; i < k; i++) sum += arr[i];   /* first window */
maxSum = sum;
for (int i = k; i < n; i++) {
    sum += arr[i] - arr[i - k];               /* slide: add new, drop old */
    if (sum > maxSum) maxSum = sum;
}
```

- **When:** Subarray/substring of fixed or variable size, max/min in window.
- **Complexity:** O(n) time, O(1) space for fixed window.

---

### Pattern 4: Monotonic Stack

```c
/* Next Greater Element for each arr[i] */
int result[n];
int stack[n]; int top = -1;           /* stack of indices */
for (int i = 0; i < n; i++) {
    while (top >= 0 && arr[stack[top]] < arr[i])
        result[stack[top--]] = arr[i];  /* arr[i] is next greater */
    stack[++top] = i;
}
while (top >= 0) result[stack[top--]] = -1; /* no greater element */
```

- **When:** Next/previous greater/smaller element, largest rectangle, trapping rain water.
- **Complexity:** O(n) time, O(n) space.

---

### Pattern 5: Recursive Tree Traversal Template

```c
/* Generic DFS on binary tree — adapt return type and logic */
int dfs(Node *root) {
    if (!root) return BASE_CASE;          /* base case */
    int left  = dfs(root->left);          /* recurse left */
    int right = dfs(root->right);         /* recurse right */
    return COMBINE(root->data, left, right); /* combine */
}
```

- **Variants:** Height, diameter, max path sum, validate BST, LCA.
- **Key:** What to return and how to combine determines the problem solved.

---

### Pattern 6: BFS Level-Order Template (Queue in C)

```c
/* Level-order traversal of binary tree */
Queue q; initQueue(&q);
if (root) enqueue(&q, root);

while (!isEmpty(&q)) {
    int levelSize = q.size;            /* snapshot: nodes at this level */
    for (int i = 0; i < levelSize; i++) {
        Node *curr = (Node *)dequeue(&q);
        printf("%d ", curr->data);
        if (curr->left)  enqueue(&q, curr->left);
        if (curr->right) enqueue(&q, curr->right);
    }
    printf("\n");                      /* newline = new level */
}
```

- **When:** Shortest path, level order output, graph BFS.
- **Note:** Queue holds `void *` in a generic version; cast at enqueue/dequeue.

---

### Pattern 7: Merge Two Sorted Linked Lists

```c
/* Merges two sorted lists — classic interview pattern */
Node *mergeSorted(Node *a, Node *b) {
    if (!a) return b;
    if (!b) return a;
    if (a->data <= b->data) {
        a->next = mergeSorted(a->next, b);
        return a;
    }
    b->next = mergeSorted(a, b->next);
    return b;
}
/* Iterative version is preferred in C to avoid O(n) stack depth */
```

---

### Pattern 8: Hash Table as Frequency Counter

```c
/* Count character frequencies (for ASCII printable chars) */
int freq[128] = {0};            /* O(1) space since fixed size */
char *str = "interview";
for (int i = 0; str[i]; i++)
    freq[(int)str[i]]++;

/* Check if two strings are anagrams */
/* ... fill freq for s1, decrement for s2, check all zero */
```

- **When:** Frequency counting, duplicate detection, anagram check, Two Sum.

---
---

## 🔬 Debugging C DSA Programs

### Valgrind — Memory Error Detector

```bash
# Compile with debug symbols
gcc -Wall -Wextra -g -o prog prog.c -lm

# Run with Valgrind
valgrind --leak-check=full --track-origins=yes ./prog

# Key Valgrind messages:
# "Invalid read/write"     → out-of-bounds array access
# "Use of uninitialised"   → uninitialized variable / malloc not zeroed
# "definitely lost"        → memory leak (forgot free)
# "Invalid free"           → double free or freeing non-heap memory
```

### GDB — Step-Through Debugger

```bash
gdb ./prog

# Essential GDB commands for DSA debugging:
(gdb) run                          # start program
(gdb) break main                   # set breakpoint at main
(gdb) break linkedlist.c:42        # break at line 42
(gdb) next                         # execute next line (step over)
(gdb) step                         # step into function call
(gdb) print head->data             # print value
(gdb) print *node                  # print struct contents
(gdb) display head                 # auto-print head every step
(gdb) backtrace                    # print call stack (find crash location)
(gdb) continue                     # resume until next breakpoint
(gdb) watch head                   # break when head changes
```

### AddressSanitizer — Fast Runtime Checks (no Valgrind needed)

```bash
gcc -Wall -g -fsanitize=address -fsanitize=undefined -o prog prog.c
./prog
# Catches: buffer overflow, use-after-free, stack overflow, UB
# Much faster than Valgrind; use during development
```

### Common Crash Causes & Fixes

| Symptom | Likely Cause | Fix |
|---|---|---|
| Segfault on pointer access | Dereferencing NULL | Add `if (!ptr)` check before use |
| Segfault inside recursion | Stack overflow (deep recursion) | Add depth limit or use iterative |
| Infinite loop in linked list | Circular list traversed without stop | Check `curr != start` not `curr != NULL` |
| Wrong output after delete | Use-after-free | Save `next` before `free(curr)` |
| Memory leak on exit | Missing `free` | Add `freeTree/freeList/freeHashTable` |
| Random wrong values | `malloc` not zeroed | Use `calloc` or `memset` after `malloc` |
| Double free crash | Freed same pointer twice | Set `ptr = NULL` after `free` |

---
---

## 📘 C vs Java DSA — Key Differences

> Helpful if you know Java and are learning C implementations.

| Concept | Java | C |
|---|---|---|
| **Memory** | GC handles it automatically | Manual `malloc` / `free` — you own it |
| **Linked List Node** | `class Node { int data; Node next; }` | `typedef struct Node { int data; struct Node *next; } Node;` |
| **Head modification** | Pass object reference — works | Must use `Node **head` (double pointer) |
| **NULL check** | `if (node == null)` | `if (!node)` or `if (node == NULL)` |
| **Array size in function** | Passed via object; `arr.length` available | Size must be passed separately — arrays decay to pointers |
| **No bounds check** | `ArrayIndexOutOfBoundsException` thrown | Silent undefined behaviour — segfault or corruption |
| **Stack** | `java.util.Stack` / `Deque` | Implement from scratch with array or linked list |
| **Queue** | `java.util.LinkedList` as Queue | Implement circular array or LL-based queue |
| **HashMap** | `java.util.HashMap` | Implement hash table manually (chaining or open addressing) |
| **TreeMap** | `java.util.TreeMap` (Red-Black Tree) | Implement RBT or AVL manually |
| **Generics** | `Node<T>` — type-safe | Use `void *` — cast at use point; no type safety |
| **String comparison** | `str1.equals(str2)` | `strcmp(str1, str2) == 0` |
| **String copy** | Assignment works | `strcpy` or `strdup` (allocates) |
| **Recursion depth** | Larger default stack; tunable with `-Xss` | Default ~8MB stack; deep recursion risks segfault |

### C `void *` for Generic Data Structures

```c
/* Generic node — works for any data type */
typedef struct Node {
    void *data;          /* pointer to any data */
    struct Node *next;
} Node;

/* Usage — must cast at insertion and retrieval */
int *val = malloc(sizeof(int));
*val = 42;
Node *n = createNode(val);
int retrieved = *(int *)n->data;  /* cast back to int * */
free(n->data);                    /* free the int separately */
free(n);
```

> **Interview Note:** In Java interviews, `List<Integer>` is natural. In C interviews, be explicit about `void *`, casting, and who owns the memory.

---
---

## 🗺️ DSA Practice Roadmap in C

Follow this sequence. Each level builds on the previous.

### Level 1: Foundations (Week 1-2)

- [ ] Static array: linear search, binary search, reverse, rotate
- [ ] Dynamic array: implement with `malloc` + `realloc` doubling
- [ ] Array-based Stack: push/pop/peek with overflow check
- [ ] Circular Queue: enqueue/dequeue with modulo wrap
- **Goal:** Comfortable with pointers, `malloc`, `free`, `sizeof`

### Level 2: Linked Structures (Week 3-4)

- [ ] Singly Linked List: all operations + reverse + cycle detection
- [ ] Doubly Linked List: all operations + LRU Cache concept
- [ ] Circular Linked List: Josephus problem
- [ ] Hash Table: chaining with `strdup`/`free` for string keys
- **Goal:** Double pointer pattern, `->` operator, memory ownership

### Level 3: Trees — Basic (Week 5-6)

- [ ] BST: insert/search/delete (all 3 cases) + validate + LCA
- [ ] BST traversals: inorder (iterative + recursive), level-order with queue
- [ ] Binary Tree: height, diameter, max path sum, mirror
- **Goal:** Recursive tree thinking, base-case discipline, `freeTree` post-order

### Level 4: Trees — Balanced (Week 7-9)

- [ ] AVL Tree: insert with 4 rotation cases + balance factor
- [ ] Heap/Priority Queue: min-heap with array representation
- [ ] Cartesian Tree: O(n) build with stack
- [ ] Skip List: insert/search/delete with `update[]` array
- **Goal:** Understand why O(n) BST degenerates; how rotations fix it

### Level 5: Advanced Structures (Week 10-12)

- [ ] Red-Black Tree: 5 properties + insertFixup (3 cases + mirrors)
- [ ] B-Tree: node struct + search + split logic
- [ ] KD Tree: build with qsort + NNS with backtracking
- [ ] Splay Tree: splay operation (Zig, Zig-Zig, Zig-Zag)
- **Goal:** Can explain any structure verbally and code the core operation

### Daily Practice Template

```
Day 1: Study data structure — implement from scratch
Day 2: Solve 2 easy problems using that structure
Day 3: Solve 1 medium problem
Day 4: Review — can you code it without looking? Time yourself.
Day 5: Explain it out loud as if in an interview
```

---
---

## 💡 Interview Answer Templates (C-Specific)

Use these sentence starters when explaining your C implementation in an interview.

### When explaining a data structure:

> "In C, I'd implement this as a `struct` with [fields]. Memory would be heap-allocated via `malloc` and freed via a dedicated `free_<name>` function. The key invariant to maintain is [property]."

### When asked about time complexity:

> "This operation is O(log n) **average case** — not worst case — because [reason for degradation]. In the worst case, [scenario], making it O(n)."

### When asked about memory:

> "Each node allocates [X] bytes: [Y bytes for data] + [Z bytes per pointer on 64-bit]. The total structure uses O(n) space. I always set freed pointers to NULL to avoid dangling pointer bugs."

### When asked to compare two structures:

> "Both achieve O(log n), but [A] stores [metadata] per node for [guarantee], while [B] relies on [mechanism] for [benefit]. For [use case], I'd prefer [A/B] because [reason]."

---

> **Last Updated:** 2025 | Language: C (C99/C11)
> **Covers:** 15 Data Structures + Patterns + Debugging + Practice Roadmap
> **Target:** Fresher to mid-level C/Systems engineering interviews
