# Build & Run Scripts for C++ Projects with C Library Dependencies

This guide provides generalized scripts for compiling and running C++ projects that include C source files (like SQLite or other libraries), usable on both **Windows** and **Linux/macOS**.

---

## Windows: `build.bat`

Compile and link a C++ project that uses `.cpp` and `.c` files.

```bat
@echo off
REM Compile C file (e.g., a library)
gcc -c c_library.c -o c_library.o

REM Compile C++ source file
g++ -c main.cpp -o main.o

REM Link object files and create executable
g++ main.o c_library.o -o app.exe -static

echo.
echo Build complete. Press any key to continue...
pause
```

---

## Windows: `run.bat`

Run the compiled executable.

```bat
@echo off
if not exist app.exe (
    echo Executable not found. Please build the project first.
    pause
    exit /b
)
app.exe
pause
```

---

## Linux/macOS: `build.sh`

Make this file executable:

```bash
chmod +x build.sh
```

Then run it:

```bash
./build.sh
```

Script content:

```bash
#!/bin/bash

# Compile the C library
gcc -c c_library.c -o c_library.o

# Compile the C++ source file
g++ -c main.cpp -o main.o

# Link object files into final binary
g++ main.o c_library.o -o app -static

echo
echo "Build complete. Run it with: ./app"
```

---

## Suggested Directory Structure

```bash
project-folder/
├── main.cpp
├── c_library.c
├── c_library.h
├── app.exe         <-- Windows build
├── app             <-- Linux/macOS build
├── build.bat
├── run.bat
├── build.sh
└── .gitignore
```

---

## .gitignore Recommendations

Add these files to `.gitignore` to keep the repo clean:

```bash
app.exe
app
*.o
*.db
*.log
```

---

## Notes

- Always compile `.c` files using `gcc` and `.cpp` files using `g++`.
- Object files (`*.o`) can be safely linked with `g++` in the final step.
- The `-static` flag helps to avoid runtime dependency issues.
- Rename files in the scripts as needed to match the actual project files.

---

This setup works for projects involving mixed-language compilation (C + C++) and provides a portable way for users to build and run software.
