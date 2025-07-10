# Integrating SQLite3 with C++ on Windows (g++)

## Goal

Compile and run a C++ project (`project.cpp`) that uses SQLite3 for data storage, by properly linking the `sqlite3.c` source file.

---

## Project Structure

```bash
project-name/  
├── project.cpp  
├── sqlite3.c  
├── sqlite3.h  
```

---

## What **NOT** to do

### DO NOT include `sqlite3.c` in your C++ code

```cpp
extern "C" {
    #include "sqlite3.h"
    #include "sqlite3.c"  // DON'T add this -> causes MASSIVE compile-time errors
}
```

- Including the `.c` file inside a `.cpp` file leads to:
  - `invalid use of incomplete type`
  - `redefinition of symbol`
  - `forward declaration` issues
  - linking issues (`undefined reference to sqlite3_*`)
  - hundreds of errors due to mismatches between C and C++

---

## The Correct Way

### Step 1: Compile the SQLite source file separately with a C compiler

```bash
gcc -c sqlite3.c -o sqlite3.o
```

- This creates an object file from the C source.

---

### Step 2: Compile the C++ file and link it with `sqlite3.o`

```bash
g++ project.cpp sqlite3.o -o project.exe
```

- This links both files together into a working `.exe`.

---

### Step 3: Run the program

```bash
./project.exe
```

Or in PowerShell:

```powershell
.\project.exe
```

---

## Header usage in the C++ file

Use only the header:

```cpp
extern "C" {
    #include "sqlite3.h"
}
```

- This ensures proper linking with C-style declarations.

---

## All-in-One Commands

**Bash (Linux/macOS/Git Bash/WSL):**

```bash
gcc -c sqlite3.c -o sqlite3.o && g++ project.cpp sqlite3.o -o project.exe && ./project.exe
```

**PowerShell:**

```powershell
gcc -c sqlite3.c -o sqlite3.o ; g++ project.cpp sqlite3.o -o project.exe ; .\project.exe
```

---

## Optional Build Script (Linux/macOS/Git Bash)

**build.sh**:

```bash
#!/bin/bash
gcc -c sqlite3.c -o sqlite3.o
g++ project.cpp sqlite3.o -o project.exe
./project.exe
```

Make it executable:

```bash
chmod +x build.sh
```

Then run:

```bash
./build.sh
```

---
