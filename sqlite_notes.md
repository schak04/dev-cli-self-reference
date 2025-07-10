# SQLite Notes for C++ Developers

## What is SQLite?

- SQLite is a **self-contained**, **serverless**, **zero-configuration** SQL database engine.
- It reads and writes directly to ordinary disk files (`.db` or `.sqlite`).
- Being extremely lightweight and embedded, it is ideal for desktop apps, mobile apps, and CLI tools.
- It uses **SQL** (Structured Query Language) to interact with the database.

---

## How SQLite Stores Data

- Each SQLite database is a **single file** on disk.
- The structure contains:
  - Tables
  - Indexes
  - Views
  - Triggers
- There is no need for a server; the `sqlite3` library handles file I/O internally.

---

## SQLite CLI Basics (for quick testing/debugging)

```bash
sqlite3 my_database.db       # opens or creates the DB file
.tables                      # lists all tables
.schema tablename            # shows schema for a table
.quit                        # exit
```

### Example SQL

**Creating a table:**

```sql
CREATE TABLE tasks (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    title TEXT NOT NULL,
    description TEXT,
    priority INTEGER,
    status TEXT
);
```

**Inserting a row:**

```sql
INSERT INTO tasks (title, description, priority, status)
VALUES ('Test Task', 'Just testing', 1, 'incomplete');
```

**Selecting rows:**

```sql
SELECT * FROM tasks ORDER BY priority DESC;
```

---

## SQLite with C/C++

### 1. Include SQLite

Both the `.cpp` file and `sqlite3.c` must be compiled, and then this must be included:

```cpp
#include "sqlite3.h"
```

---

### 2. Open a Database

```cpp
sqlite3* db;
int rc = sqlite3_open("tasks.db", &db);
if (rc) {
    std::cerr << "Can't open database: " << sqlite3_errmsg(db) << std::endl;
} else {
    std::cout << "Opened database successfully\n";
}
```

---

### 3. Execute a Simple SQL Command

```cpp
char* errMsg = 0;
std::string sql = "CREATE TABLE IF NOT EXISTS tasks ("
                  "id INTEGER PRIMARY KEY AUTOINCREMENT,"
                  "title TEXT NOT NULL,"
                  "description TEXT,"
                  "priority INTEGER,"
                  "status TEXT);";

int rc = sqlite3_exec(db, sql.c_str(), 0, 0, &errMsg);
if (rc != SQLITE_OK) {
    std::cerr << "SQL error: " << errMsg << std::endl;
    sqlite3_free(errMsg);
} else {
    std::cout << "Table created successfully\n";
}
```

---

### 4. Insert Using `sqlite3_exec()`

```cpp
std::string sql = "INSERT INTO tasks (title, description, priority, status) "
                  "VALUES ('Study SQLite', 'Learn basics', 3, 'incomplete');";
rc = sqlite3_exec(db, sql.c_str(), 0, 0, &errMsg);
```

> Vulnerable to SQL injection if user input is being inserted directly. Prefer prepared statements below.

---

### 5. Query and Read Results with Callback

```cpp
int callback(void* NotUsed, int argc, char** argv, char** azColName) {
    for (int i = 0; i < argc; i++) {
        std::cout << azColName[i] << ": " << (argv[i] ? argv[i] : "NULL") << "\n";
    }
    std::cout << "\n";
    return 0;
}

std::string sql = "SELECT * FROM tasks;";
sqlite3_exec(db, sql.c_str(), callback, 0, &errMsg);
```

---

### 6. Prepared Statements (Safe & Efficient)

```cpp
sqlite3_stmt* stmt;
std::string sql = "INSERT INTO tasks (title, description, priority, status) "
                  "VALUES (?, ?, ?, ?);";

if (sqlite3_prepare_v2(db, sql.c_str(), -1, &stmt, NULL) == SQLITE_OK) {
    sqlite3_bind_text(stmt, 1, "Read book", -1, SQLITE_STATIC);
    sqlite3_bind_text(stmt, 2, "C++ for Dummies", -1, SQLITE_STATIC);
    sqlite3_bind_int(stmt, 3, 2);
    sqlite3_bind_text(stmt, 4, "incomplete", -1, SQLITE_STATIC);

    if (sqlite3_step(stmt) == SQLITE_DONE) {
        std::cout << "Task inserted successfully.\n";
    } else {
        std::cerr << "Failed to insert task.\n";
    }

    sqlite3_finalize(stmt);
}
```

---

## Error Handling

Check return values of all functions like `sqlite3_exec()`, `sqlite3_prepare_v2()`, `sqlite3_step()`, etc.

Use:

```cpp
sqlite3_errmsg(db);
```

to get readable error messages from SQLite.

---

## Closing the Database

```cpp
sqlite3_close(db);
```

---

## Common SQL Operations CheatSheet

| Operation     | SQL |
|---------------|-----|
| Create table  | `CREATE TABLE ...` |
| Insert row    | `INSERT INTO ...` |
| Select rows   | `SELECT * FROM ...` |
| Update row    | `UPDATE ... SET ... WHERE ...` |
| Delete row    | `DELETE FROM ... WHERE ...` |
| Drop table    | `DROP TABLE ...` |

---

## Extra Tips for C++ Integration

- Prefer **prepared statements** if using user input.
- Use wrapper functions like `addTaskToDB()`, `updateTaskInDB()` etc.
- Avoid keeping DB open longer than needed.
- Use `sqlite3_reset(stmt)` to reuse a prepared statement.

---

## Resources

- [SQLite Official Docs](https://www.sqlite.org/docs.html)
- [sqlite3 C/C++ API Reference](https://www.sqlite.org/c3ref/intro.html)

---
