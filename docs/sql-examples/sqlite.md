# SQLite — Installation, Usage, and Data Analysis

## Introduction

This project documents my exploration of **SQLite**, a lightweight, file-based relational database engine. The aim was to understand how SQLite works, how it differs from server-based databases, and how to install, configure, and query it locally using real-world data.

The project combines:
- Conceptual understanding of SQLite
- Hands-on installation and configuration
- Debugging common beginner errors
- SQL analysis using a public dataset

---

## What is SQLite?

SQLite is a **database engine** that allows users to interact with a relational database stored in a **single file**. Unlike systems such as PostgreSQL or MySQL, SQLite does not require a separate server process.

This design makes SQLite:
- Extremely portable
- Easy to copy and share
- Ideal for local development, testing, and embedded systems

A database can be moved or backed up simply by copying the file that stores it.

---

## Drawbacks of SQLite

While SQLite is highly accessible, it has limitations:

- Only one user can write to the database at a time
- Security must be managed carefully due to file-based access
- Advanced database features are limited compared to full server systems
- SQLite does **not strictly enforce data types**

For example, SQLite allows values of any type to be inserted into any column, even when a schema defines expected types:

```sql
CREATE TABLE celebs (
  id INTEGER,
  name TEXT,
  age INTEGER
);
``` 
---

## Common Use Cases for SQLite

Despite its drawbacks, SQLite is widely used for:
- Local application development
- Testing and prototyping
- Embedded systems
- Applications where the database lives alongside the code
SQLite is considered one of the most widely deployed software components in the world.

---

## Installing SQLite (macOS)
### **Step 1: Locate SQLite Tools**

After downloading the SQLite tools, I navigated through the filesystem:

```bash
ls
cd Desktop
ls
cd sqlite-tools-osx-x64-3510100
```

This confirmed the location of the SQLite binaries.

---

### **Step 2: Move sqlite3 into the System PATH**

**Initial attempt:**

```bash
mv sqlite3 /usr/local/bin/
```

**Result:**

```text
Permission denied
```

**Corrected attempt using administrator privileges:**

```bash
sudo mv sqlite3 /usr/local/bin/sqlite3
```

This succeeded because `/usr/local/bin` is a protected directory.

---

### Step 3: Verify SQLite Installation

```bash
sqlite3 --version
```

**Output:**

```text
SQLite 3.43.2 2023-10-10 (64-bit)
```

This confirmed:

- SQLite was installed correctly
- The binary was accessible globally
- The interactive SQLite shell could launch

---

### Step 4: Learn the difference between Terminal vs SQLite Shell

I learned the important distinction between:

- Terminal shell (zsh)
- SQLite shell (sqlite>)

Attempting shell commands inside SQLite:

```sql
sqlite> pwd
sqlite> ls
```

**Result:**

- SQLite entered continuation mode (...>)

Exit safely with:

```sql
Ctrl + C
```

**Key learning:**

- Shell commands do not work inside SQLite. 
- SQLite expects valid SQL.
- Ctrl + C safely cancels unfinished SQL

---

### Step 5: Opening the Database (First Mistake)

**Attempt:**

```bash
sqlite3 acs-1-year-2015.sqlite
```

Inside SQLite:

```sql
.schema
```

**Result:**

- No output

**What happened:**

SQLite silently created a new empty database because the file did not exist in the current directory.

---

### Step 6: Locating the Correct Database File

To find the actual database:

```bash
find ~ -name "acs-1-year-2015.sqlite" 2>/dev/null
```

**Result:**

```text
/Users/username/Desktop/acs-1-year-2015.sqlite
/Users/username/acs-1-year-2015.sqlite
```

**Key insight:**

SQLite creates a new database file if the specified file does not exist.

---

### Step 7: Opening the Correct Database

```bash
sqlite3 /Users/username/Desktop/acs-1-year-2015.sqlite
```

### Step 8: Inspecting Database Contents

```sql
.databases
```

**Output:**

```text
main: /Users/username/Desktop/acs-1-year-2015.sqlite r/w
```

```sql
.tables
```

**Output:**

```text
congressional_districts
places
states
```

Then:

```sql
.schema
```

**Result:**

- Full schemas for all tables
- Indexes on key columns for performance

---

## Step 9: First SQL Errors (Debugging Phase)
### Error 1: Column Name Typo

- per_captia_income   ❌
- per_capita_income   ✅

SQLite returned:
Parse error: no such column


```sql
SELECT per_captia_income FROM states;
```

Error:

```text
Parse error: no such column
```

---

Correct column name:

```text
per_capita_income
```

---

### Error 2: Multiple SELECT Statements

Typing multiple SELECT clauses caused:

```text
Parse error: near "SELECT": syntax error
```

**Fix:**

- Press Ctrl + C
- Re-enter a single valid SQL statement

---

## Step 10: Successful Queries
#### Per Capita Income by State

```sql
SELECT name, per_capita_income
FROM states
ORDER BY per_capita_income ASC;
```

**Result (excerpt):**

Puerto Rico|18626
Mississippi|40593
Arkansas|41995
West Virginia|42019
...
California|64500
Massachusetts|70628
New Jersey|72222
Hawaii|73486
District of Columbia|75628
Maryland|75847

**Insight:**

- Significant income disparities across states
- Southern states clustered at the lower end
- Coastal and urban regions ranked highest


#### Median Age by State

```sql
SELECT name, median_age
FROM states
ORDER BY median_age ASC;
```

#### Result (excerpt):

Utah|30.6
Alaska|33.3
District of Columbia|33.8
Texas|34.4
...
Florida|41.8
West Virginia|42.2
New Hampshire|42.8
Vermont|43.1
Maine|44.6

**Insight:**

 - Utah has the youngest population
 - Northeastern states skew older
 - Florida’s age profile reflects retirement migration

---

# Key Takeaways

- Installed and verified SQLite on macOS
- Understood the difference between Terminal and SQLite shell
- Learned how SQLite handles missing database files
- Explored databases using .databases, .tables, and .schema
- Debugged real SQL errors
- Analysed demographic and economic data using SQL
- Interpreted results in a real-world context
