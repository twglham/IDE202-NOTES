# Programming Fundamentals (C)

## Module 1 — Computer Basics & Problem Solving

**How a computer runs a program**
- **CPU** (ALU + control unit + registers) executes instructions; **memory (RAM)** holds the running program + data; **storage** is permanent; **I/O** devices talk to the user.
- Source code → **compiler** → machine code → CPU runs it.
- C is **compiled** (whole program translated first), unlike interpreted languages.

**Problem solving steps**
1. Understand the problem → 2. Plan (algorithm) → 3. Code → 4. Test/debug.

- **Algorithm** = finite, step-by-step, unambiguous instructions.
- **Flowchart** = diagram of the algorithm (oval start/end, rectangle process, diamond decision, parallelogram I/O).
- **Pseudocode** = informal code-like steps.

**First program**
```c
#include <stdio.h>

int main() {
    printf("Hello, world!\n");
    return 0;
}
```
Compile & run: `gcc hello.c -o hello && ./hello`

---

## Module 2 — C Basics: Variables, Types, Operators, I/O

**Data types**
| Type | Size (typical) | Example |
|---|---|---|
| `char` | 1 byte | `'A'` |
| `int` | 4 bytes | `42` |
| `float` | 4 bytes | `3.14f` |
| `double` | 8 bytes | `3.14159` |

**Variables:** must be declared before use. `int age = 20;`
**Constants:** `const int MAX = 100;` or `#define MAX 100`

**Operators**
- Arithmetic: `+ - * / %` (integer division truncates: `7/2 = 3`)
- Relational: `== != < > <= >=`
- Logical: `&& || !`
- Assignment: `= += -= *= /=`
- Increment: `i++` (use then add), `++i` (add then use)

**Type casting:** `(float) a / b` avoids integer division.

**Input / output**
```c
int x;
scanf("%d", &x);        // & is required for scanf
printf("x = %d\n", x);
```
Format specifiers: `%d` int, `%f` float, `%lf` double, `%c` char, `%s` string.

**Common mistakes:** forgetting `&` in `scanf`, using `=` instead of `==`, missing `;`.

---

## Module 3 — Control Flow: Conditionals & Loops

**Conditionals**
```c
if (marks >= 40) printf("Pass");
else if (marks >= 0) printf("Fail");
else printf("Invalid");
```
- `switch` for matching one value against many cases. Don't forget `break`.
```c
switch (op) {
    case '+': r = a + b; break;
    case '-': r = a - b; break;
    default:  printf("Invalid");
}
```
- Ternary: `max = (a > b) ? a : b;`

**Loops**
| Loop | Use when |
|---|---|
| `for` | you know how many times |
| `while` | condition checked first, unknown count |
| `do-while` | must run at least once |

```c
for (int i = 1; i <= 5; i++) printf("%d ", i);
```
- `break` exits the loop; `continue` skips to the next iteration.
- Nested loops → patterns (stars, triangles), tables.
- **Infinite loop** = condition never becomes false (check your update step).

**Classic programs:** factorial, Fibonacci, prime check, sum of digits, reverse a number, palindrome.

---

## Module 4 — Arrays, Strings & Functions

**Arrays:** fixed-size, same type, contiguous, index from `0`.
```c
int a[5] = {1, 2, 3, 4, 5};
for (int i = 0; i < 5; i++) sum += a[i];
```
- No bounds checking in C — going out of range = undefined behavior.
- **2D arrays:** `int m[3][3];` → matrices (loop in rows × cols).

**Strings:** `char` array ending with `'\0'`.
```c
char s[20] = "hello";
```
Useful (`<string.h>`): `strlen`, `strcpy`, `strcmp`, `strcat`.
Read a line with spaces: `fgets(s, sizeof s, stdin);`

**Functions**
```c
int add(int a, int b) {     // definition
    return a + b;
}
int main() {
    int r = add(2, 3);      // call
}
```
- Declare a **prototype** above `main` if the function is defined below it.
- **Pass by value:** function gets a copy, so changes don't affect the original (use pointers to change the original).
- **Scope:** local (inside function) vs global (outside everything).
- **Recursion:** function calls itself; needs a base case.
```c
int fact(int n) { return n <= 1 ? 1 : n * fact(n - 1); }
```

**Searching & sorting (basics)**
- Linear search `O(n)`, binary search `O(log n)` (sorted array only).
- Bubble / selection / insertion sort — all `O(n²)`.

---

## Module 5 — Pointers & Dynamic Memory

**Pointer** = variable that stores a memory **address**.
```c
int x = 10;
int *p = &x;     // p holds address of x
printf("%d", *p); // *p = value at that address -> 10
*p = 20;          // x is now 20
```
- `&` = address of, `*` = dereference.
- Always initialize pointers (`NULL` if nothing yet). Dereferencing `NULL`/garbage → crash.

**Pass by reference (using pointers)**
```c
void swap(int *a, int *b) {
    int t = *a; *a = *b; *b = t;
}
swap(&x, &y);
```

**Arrays & pointers:** array name = address of first element. `a[i]` is the same as `*(a + i)`.

**Dynamic memory** (`<stdlib.h>`) — allocated on the **heap** at runtime:
```c
int *arr = (int *) malloc(n * sizeof(int));
if (arr == NULL) { /* allocation failed */ }
// use arr[i] ...
free(arr);        // always free
arr = NULL;
```
| Function | Does |
|---|---|
| `malloc(size)` | allocate, uninitialized |
| `calloc(n, size)` | allocate, zeroed |
| `realloc(p, size)` | resize |
| `free(p)` | release |

**Bugs to avoid:** memory leak (no `free`), dangling pointer (using after `free`), double free, buffer overflow.

---

## Module 6 — Structures & File Handling

**Structure:** group different types under one name.
```c
struct Student {
    char name[30];
    int roll;
    float cgpa;
};
struct Student s1 = {"Nam", 1, 3.5};
printf("%s", s1.name);          // dot for variable
struct Student *p = &s1;
printf("%d", p->roll);          // arrow for pointer
```
- Array of structs → a small "database" of records.
- `typedef struct {...} Student;` lets you skip the `struct` keyword.

**File handling** (`<stdio.h>`)
```c
FILE *fp = fopen("data.txt", "r");
if (fp == NULL) { printf("Cannot open file"); return 1; }
int x;
while (fscanf(fp, "%d", &x) == 1) printf("%d\n", x);
fclose(fp);
```
| Mode | Meaning |
|---|---|
| `"r"` | read (file must exist) |
| `"w"` | write (creates / **overwrites**) |
| `"a"` | append |
| `"r+"` | read + write |

Functions: `fprintf`, `fscanf`, `fgets`, `fputs`, `fgetc`, `fputc`, `fread`, `fwrite` (binary).
- Always check `fopen` result and always `fclose`.
- Delete a file: `remove("data.txt");` Rename: `rename(old, new);`

---

## Quick Cheat Sheet

- `int main()` returns `0` on success.
- Arrays start at index `0`; strings end with `'\0'`.
- `scanf` needs `&` (except for strings).
- `==` compares, `=` assigns.
- Every `malloc` needs a `free`; every `fopen` needs a `fclose`.
- Pass-by-value by default; use pointers to modify originals.