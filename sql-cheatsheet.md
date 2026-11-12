# SQL Cheatsheet — From Zero to One

> Objective: Quick revison of SQL concepts.

---

## Table of Contents
1. [What is SQL?](#what-is-sql)
2. [Setup — PostgreSQL on Ubuntu](#setup--postgresql-on-ubuntu)
3. [How psql Works Internally](#how-psql-works-internally)
4. [Writing vs Execution Order](#writing-vs-execution-order)
5. [Naming & Case Rules](#naming--case-rules)
6. [Comments](#comments)
7. [Data Types & Constraints](#data-types--constraints)
8. [CREATE TABLE](#create-table)
9. [INSERT Rows](#insert-rows)
10. [SELECT — Reading Data](#select--reading-data)
11. [WHERE — Filtering Rows](#where--filtering-rows)
12. [NULL Values](#null-values)
13. [ORDER BY, LIMIT, OFFSET](#order-by-limit-offset)
14. [Aggregate Functions](#aggregate-functions)
15. [GROUP BY and HAVING](#group-by-and-having)
16. [Expressions and Aliases (AS)](#expressions-and-aliases-as)
17. [UPDATE Rows](#update-rows)
18. [DELETE Rows](#delete-rows)
19. [Transactions](#transactions)
20. [ACID Properties](#acid-properties)
21. [Locking Mechanism](#locking-mechanism)
22. [Database Isolation Levels](#database-isolation-levels)
23. [ALTER TABLE](#alter-table)
24. [DROP TABLE](#drop-table)
25. [Indexes](#indexes)
26. [JOINs](#joins)
27. [Subqueries](#subqueries)
28. [Schema — Theory vs Practice](#schema--theory-vs-practice)
29. [Normalization (1NF → 2NF → 3NF)](#normalization-1nf--2nf--3nf)
30. [Triggers](#triggers)
31. [CAP Theorem](#cap-theorem)
32. [Common Mistakes & Misconceptions](#common-mistakes--misconceptions)
33. [Quick Reference Cards](#quick-reference-cards)

---

## What is SQL?

**SQL** (Structured Query Language) is the language used to perform **CRUD** operations on a **relational database**.

| CRUD Letter | SQL Keyword |
|-------------|-------------|
| Create      | `INSERT`    |
| Read        | `SELECT`    |
| Update      | `UPDATE`    |
| Delete      | `DELETE`    |

> Think of a relational database as a collection of Excel sheets (tables) that can talk to each other through shared columns.

**Key vocabulary:**
- **Table** = the structure (also called a schema in informal talk)
- **Column** = attribute / property
- **Row** = one record / one instance of data

---

## Setup — PostgreSQL on Ubuntu

### Step 1 — Install PostgreSQL

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
psql --version
sudo systemctl start postgresql.service
sudo systemctl status postgresql.service
```

Expected status output: `active (running)` — if you see this, the server is alive.

### Step 2 — Create your own role and database

The default superuser is `postgres`. Do NOT practice as superuser. Create your own role.

```bash
sudo -u postgres psql
```

Inside the psql shell (replace `veera` with your Linux username — check it with `whoami`):

```sql
CREATE ROLE veera WITH LOGIN PASSWORD 'yourpassword' CREATEDB;
CREATE DATABASE practice OWNER veera;
\q
```

### Step 3 — Connect to your database

```bash
psql -d practice
```

Your prompt should look like: `practice=>` (the `=>` means you are a normal user, not superuser — that's correct).

**Troubleshooting:**
- `role does not exist` → you skipped Step 2.
- `Peer authentication failed` → your Linux username doesn't match the role name.

### Step 4 — Essential psql Commands

| Command          | What it does                   |
|------------------|-------------------------------|
| `\l`             | List all databases             |
| `\c dbname`      | Switch to another database     |
| `\dt`            | List all tables in current DB  |
| `\d tablename`   | Show table structure/columns   |
| `\conninfo`      | Show current connection info   |
| `\q`             | Quit psql (or use Ctrl+D)      |
| `\i ~/file.sql`  | Run a SQL file from inside psql|

> **Common mistake:** Pressing Ctrl+Z does NOT quit psql. It suspends it in the background. Always use `\q` or Ctrl+D. If you accidentally suspend it, run `fg` to bring it back, then `\q`.

### Step 5 — Run SQL from a file

```bash
psql -d practice -f myfile.sql
```

This is the best workflow for learning: write SQL in a file, run it, see results, fix, repeat.

```bash
psql -d practice -f filename.sql              # run a file
psql -d practice -c "SELECT * FROM students;" # run one statement inline
psql -d practice -v ON_ERROR_STOP=1 -f file.sql  # stop on first error
```

**Why use a file?** Write once, run many times. Add `DROP TABLE IF EXISTS` at the top so you can re-run safely.

```sql
DROP TABLE IF EXISTS students;

CREATE TABLE students (
  id    SERIAL PRIMARY KEY,
  name  TEXT NOT NULL,
  city  TEXT,
  marks INT
);

INSERT INTO students (name, city, marks) VALUES
  ('Ravi',  'Kakinada',  85),
  ('Anil',  'Hyderabad', 72),
  ('Sita',  'Kakinada',  91),
  ('Kiran', 'Vizag',     60);

SELECT * FROM students;
```

**Reading psql output:**
- `INSERT 0 4` → 4 rows inserted (the `0` is legacy, ignore it)
- `UPDATE 2` / `DELETE 1` → rows affected
- `ERROR: ...` → that statement failed; psql keeps running the rest by default

---

## How psql Works Internally

```
psql -d practice
= psql -h /var/run/postgresql -p 5432 -U veera -d practice
```

- PostgreSQL is a **server** (runs in background, owns data files in `/var/lib/postgresql/`)
- `psql` is a **client** (connects, sends SQL, prints results)
- They talk through a socket file: `/var/run/postgresql/.s.PGSQL.5432`
- With peer authentication, your Linux username must match the PostgreSQL role name

**Data hierarchy:** Server → Databases → Tables → Rows

---

## Writing vs Execution Order

This is one of the most important things to understand. SQL is written in one order but runs in a different order.

**Writing order (what you type):**

```
SELECT → FROM → JOIN → WHERE → GROUP BY → HAVING → ORDER BY → LIMIT → OFFSET
```

**Execution order (what the database actually does):**

```
FROM → JOIN → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT → OFFSET
```

> This explains why you CANNOT use a `SELECT` alias in a `WHERE` clause — `WHERE` runs before `SELECT`. But you CAN use an alias in `ORDER BY` because `ORDER BY` runs after `SELECT`.

---

## Naming & Case Rules

| Thing              | Rule                                                     |
|--------------------|----------------------------------------------------------|
| SQL keywords       | Case-insensitive (`SELECT` = `select` = `Select`)        |
| Table/column names | Depends on database; in PostgreSQL, lowercased by default|
| String values      | Case-sensitive by default; collation-dependent           |

**Convention:** Write SQL keywords in `UPPERCASE` and table/column names in `lowercase_with_underscores` (snake_case). This is the universal standard.

```sql
SELECT name, marks FROM students WHERE city = 'Kakinada';
```

**Quote rules:**
- Single quotes `' '` → for **string values**
- Double quotes `" "` → for **identifiers** (column names, table names with spaces)

---

## Comments

```sql
-- This is a single-line comment

/*
  This is a
  multi-line comment
*/
```

---

## Data Types & Constraints

### Common Data Types

| Type        | Use for                           | Example             |
|-------------|-----------------------------------|---------------------|
| `INT`       | Whole numbers                     | age, marks          |
| `SERIAL`    | Auto-incrementing integer         | id columns          |
| `TEXT`      | Variable-length text (PostgreSQL) | name, description   |
| `VARCHAR(n)`| Text up to n characters           | email VARCHAR(100)  |
| `BOOLEAN`   | true / false                      | is_active           |
| `DATE`      | Calendar date                     | birth_date          |
| `TIMESTAMP` | Date + time                       | created_at          |
| `NUMERIC`   | Exact decimal numbers             | price, GPA          |
| `FLOAT`     | Approximate decimal numbers       | measurements        |

### Constraints

| Constraint    | Meaning                                           |
|---------------|---------------------------------------------------|
| `PRIMARY KEY` | Uniquely identifies each row; cannot be NULL      |
| `NOT NULL`    | Column must always have a value                   |
| `UNIQUE`      | No two rows can have the same value in this column|
| `DEFAULT val` | Use this value if none is provided on insert      |
| `CHECK (cond)`| Row is rejected if condition is false             |
| `REFERENCES`  | Foreign key — links to another table's primary key|

---

## CREATE TABLE

```sql
CREATE TABLE table_name (
  column_name  DataType  Constraint,
  column_name  DataType  Constraint,
  ...
);
```

**Full example:**

```sql
CREATE TABLE students (
  id         SERIAL PRIMARY KEY,
  name       TEXT NOT NULL,
  city       TEXT DEFAULT 'Unknown',
  marks      INT CHECK (marks >= 0 AND marks <= 100),
  enrolled   BOOLEAN DEFAULT true
);
```

**What each part does:**
- `SERIAL` → PostgreSQL auto-generates 1, 2, 3, ... — you never insert this manually
- `PRIMARY KEY` → unique identifier for each row; cannot be NULL
- `NOT NULL` → field is mandatory
- `DEFAULT` → used when the INSERT doesn't mention this column
- `CHECK` → data validation at the database level

---

## INSERT Rows

### Insert all columns (must match column order)

```sql
INSERT INTO students
VALUES (1, 'Ravi', 'Kakinada', 85, true);
```

### Insert specific columns (recommended — safer)

```sql
INSERT INTO students (name, city, marks)
VALUES ('Ravi', 'Kakinada', 85);
```

### Insert multiple rows at once

```sql
INSERT INTO students (name, city, marks) VALUES
  ('Ravi',  'Kakinada',  85),
  ('Anil',  'Hyderabad', 72),
  ('Sita',  'Kakinada',  91),
  ('Kiran', 'Vizag',     60);
```

> **Common mistake:** Forgetting to match the number of values to the number of columns specified. If you name 3 columns, provide exactly 3 values per row.

---

## SELECT — Reading Data

```sql
SELECT column1, column2 FROM table_name;
SELECT * FROM table_name;   -- * means all columns
```

**Basic examples:**

```sql
SELECT * FROM students;
SELECT name, marks FROM students;
SELECT name, marks FROM students WHERE marks > 70;
SELECT * FROM students ORDER BY marks DESC;
SELECT COUNT(*) FROM students;
SELECT city, AVG(marks) FROM students GROUP BY city;
```

---

## WHERE — Filtering Rows

`WHERE` filters rows **before** any grouping or aggregation. It runs on individual rows.

```sql
SELECT column_name
FROM table_name
WHERE condition
  AND/OR another_condition;
```

### Numeric Operators

| Operator                 | Meaning                            | Example                          |
|--------------------------|------------------------------------|----------------------------------|
| `=`, `!=`, `<>`, `<`, `>`, `<=`, `>=` | Standard comparisons | `marks >= 80`           |
| `BETWEEN x AND y`        | Inclusive range check              | `marks BETWEEN 60 AND 90`        |
| `NOT BETWEEN x AND y`    | Outside the range                  | `marks NOT BETWEEN 1 AND 10`     |
| `IN (...)`               | Matches any value in the list      | `city IN ('Kakinada', 'Vizag')`  |
| `NOT IN (...)`           | Does not match any in the list     | `id NOT IN (1, 3, 5)`            |

### Text Operators

| Operator      | Meaning                                    | Example                      |
|---------------|--------------------------------------------|------------------------------|
| `=`           | Exact match (case-sensitive)               | `name = 'Ravi'`              |
| `!=` or `<>`  | Not equal (case-sensitive)                 | `name != 'Anil'`             |
| `LIKE`        | Pattern match (case-insensitive in some DBs)| `name LIKE 'Ra%'`           |
| `NOT LIKE`    | Does not match the pattern                 | `name NOT LIKE '%a%'`        |
| `%`           | Wildcard: zero or more characters          | `'%AT%'` matches CAT, BATS   |
| `_`           | Wildcard: exactly one character            | `'AN_'` matches AND, not AN  |
| `IN (...)`    | Matches any string in the list             | `city IN ('Delhi', 'Mumbai')`|

> **Key distinction:** `=` is for exact match. `LIKE` is for pattern matching with `%` and `_`. Do not mix them up.

**Combined example:**

```sql
SELECT name, city, marks
FROM students
WHERE city IN ('Kakinada', 'Vizag')
  AND marks BETWEEN 60 AND 100
  AND name LIKE 'R%'
ORDER BY marks DESC;
```

---

## NULL Values

`NULL` means **the value is unknown or missing**. It is NOT zero, NOT an empty string, NOT false.

```sql
-- Check for NULL
SELECT * FROM students WHERE city IS NULL;
SELECT * FROM students WHERE city IS NOT NULL;
```

> **Most missed concept:** You cannot use `= NULL` or `!= NULL`. NULL comparisons always require `IS NULL` or `IS NOT NULL`. Writing `WHERE city = NULL` will silently return 0 rows because any comparison with NULL evaluates to UNKNOWN (not TRUE, not FALSE).

**NULL in expressions:**
- `NULL + 5` = `NULL`
- `NULL = NULL` = `NULL` (not TRUE!)
- Use `COALESCE(col, default_value)` to replace NULLs with a fallback.

```sql
SELECT name, COALESCE(city, 'No City') AS city FROM students;
```

---

## ORDER BY, LIMIT, OFFSET

```sql
SELECT * FROM students
ORDER BY marks DESC, name ASC;
```

```sql
SELECT * FROM students
ORDER BY marks DESC
LIMIT 5;        -- return only 5 rows
```

```sql
SELECT * FROM students
ORDER BY marks DESC
LIMIT 5 OFFSET 10;    -- skip first 10, then return next 5 (pagination)
```

| Keyword    | Meaning                         |
|------------|---------------------------------|
| `ASC`      | Ascending (default, A→Z, 1→100) |
| `DESC`     | Descending (Z→A, 100→1)         |
| `LIMIT n`  | Return at most n rows           |
| `OFFSET n` | Skip the first n rows           |

---

## Aggregate Functions

Aggregate functions **collapse multiple rows into a single value**.

| Function       | What it returns                        | Example                          |
|----------------|----------------------------------------|----------------------------------|
| `COUNT(*)`     | Number of rows                         | `SELECT COUNT(*) FROM students;` |
| `COUNT(col)`   | Number of non-NULL values in column    | `SELECT COUNT(city) FROM students;`|
| `SUM(col)`     | Total sum                              | `SELECT SUM(marks) FROM students;`|
| `AVG(col)`     | Average value                          | `SELECT AVG(marks) FROM students;`|
| `MIN(col)`     | Smallest value                         | `SELECT MIN(marks) FROM students;`|
| `MAX(col)`     | Largest value                          | `SELECT MAX(marks) FROM students;`|

> **Rule:** Aggregates appear in TWO places only:
> 1. After `SELECT`
> 2. After `HAVING`
>
> **Never use an aggregate inside `WHERE`.** Use `HAVING` instead.

---

## GROUP BY and HAVING

### GROUP BY

Groups rows that have the same value in a column, then applies an aggregate function to each group.

```sql
SELECT city, COUNT(*) AS student_count, AVG(marks) AS avg_marks
FROM students
GROUP BY city;
```

Output: one row per unique city with the count and average.

### HAVING

Filters the **groups** after grouping. It's like `WHERE` but for groups.

```sql
SELECT city, COUNT(*) AS student_count
FROM students
GROUP BY city
HAVING COUNT(*) > 1;
```

### WHERE vs HAVING — The critical difference

| Clause   | Filters          | Runs on          | Can use aggregates? |
|----------|------------------|------------------|---------------------|
| `WHERE`  | Individual rows  | Before GROUP BY  | No                  |
| `HAVING` | Groups of rows   | After GROUP BY   | Yes                 |

```sql
SELECT city, AVG(marks) AS avg
FROM students
WHERE marks > 50          -- filter individual rows first
GROUP BY city
HAVING AVG(marks) > 70;   -- then filter groups
```

> **Common mistake:** Using `HAVING` without `GROUP BY`. You can technically do it, but then you're treating the entire result as one group — rarely what you want. If you have no `GROUP BY`, just use `WHERE`.

---

## Expressions and Aliases (AS)

You can write expressions directly in `SELECT`:

```sql
SELECT name, marks * 1.1 AS adjusted_marks FROM students;
SELECT name, UPPER(name) AS name_upper FROM students;
SELECT particle_speed / 2.0 AS half_speed FROM physics_data WHERE ABS(position) * 10.0 > 500;
```

The `AS` keyword renames a column in the output. It does not change the actual table.

```sql
SELECT column_expression AS descriptive_label FROM table_name;
```

> Alias names can be used in `ORDER BY` but NOT in `WHERE` (because `WHERE` runs before `SELECT`).

---

## UPDATE Rows

```sql
UPDATE students
SET marks = 95
WHERE name = 'Anil';
```

Update multiple columns at once:

```sql
UPDATE students
SET marks = 95,
    city  = 'Hyderabad'
WHERE name = 'Anil';
```

> **Most dangerous mistake in SQL:** Running `UPDATE` without a `WHERE` clause. This updates EVERY row in the table. Always double-check your WHERE before running UPDATE or DELETE. Run the equivalent SELECT first to confirm which rows will be affected.

---

## DELETE Rows

```sql
DELETE FROM students
WHERE name = 'Kiran';
```

> Same danger as UPDATE: `DELETE FROM students;` without `WHERE` deletes every row in the table (but the table structure remains).

**Safe practice:** Before any DELETE, run this SELECT first:

```sql
SELECT * FROM students WHERE name = 'Kiran';
```

If those are the rows you want gone, then run the DELETE.

---

## Transactions

A **transaction** is a sequence of one or more SQL statements treated as a **single unit of work**. Either all of them succeed together, or none of them are applied.

Think of it like a shopping cart checkout: either your payment goes through AND the items are reserved AND the invoice is generated — all at once — or if anything fails, none of it happens. You never end up with money deducted but no order placed.

**Classic real-world example:** Bank transfer.

Imagine sending ₹500 from your account to a friend's account. The database must do two things:

```
Step 1: Deduct ₹500 from your account   (1000 → 500)
Step 2: Add   ₹500 to friend's account  ( 200 → 700)
```

If the server crashes after Step 1 but before Step 2 — your ₹500 is gone and your friend got nothing. A transaction prevents this — both steps succeed or neither does.

### Setup for all examples below

```sql
CREATE TABLE accounts (
  id      SERIAL PRIMARY KEY,
  name    TEXT NOT NULL,
  balance INT  NOT NULL CHECK (balance >= 0)
);

INSERT INTO accounts (name, balance) VALUES
  ('Alice', 1000),
  ('Bob',   200);
```

### Transaction Control Statements

| Statement          | What it does                                              |
|--------------------|-----------------------------------------------------------|
| `BEGIN`            | Start a transaction block                                 |
| `COMMIT`           | Save all changes — make them permanent                    |
| `ROLLBACK`         | Undo everything since BEGIN — as if it never ran          |
| `SAVEPOINT name`   | Create a named checkpoint inside a transaction            |
| `ROLLBACK TO name` | Undo back to a savepoint only, not the full transaction   |

### Example 1 — Successful transfer (COMMIT)

```sql
BEGIN;

UPDATE accounts SET balance = balance - 500 WHERE name = 'Alice';
UPDATE accounts SET balance = balance + 500 WHERE name = 'Bob';

COMMIT;
```

After COMMIT: Alice has 500, Bob has 700. Both updates are permanent.

```sql
SELECT * FROM accounts;
-- Alice: 500
-- Bob:   700
```

### Example 2 — Something went wrong (ROLLBACK)

```sql
BEGIN;

UPDATE accounts SET balance = balance - 300 WHERE name = 'Alice';
-- You realize you picked the wrong amount or wrong account

ROLLBACK;
```

After ROLLBACK: Alice still has 1000. The deduction never happened.

### Example 3 — Constraint violation mid-transaction

```sql
BEGIN;

UPDATE accounts SET balance = balance - 300 WHERE name = 'Alice';
-- Alice now has 700 (not committed yet, only visible inside this transaction)

UPDATE accounts SET balance = balance - 800 WHERE name = 'Alice';
-- This would make Alice's balance -100 → violates CHECK (balance >= 0)
-- PostgreSQL raises an ERROR and aborts the entire transaction

-- You MUST rollback now — no more statements are allowed in this transaction
ROLLBACK;
```

> **Important:** Once PostgreSQL raises an error inside a transaction, the whole transaction is aborted. You cannot run more statements. You MUST `ROLLBACK` before starting a new one.

### Example 4 — SAVEPOINT (partial undo)

SAVEPOINT lets you mark a checkpoint inside a transaction. If something goes wrong later, you can roll back only to that point — not all the way to `BEGIN`.

```sql
BEGIN;

INSERT INTO accounts (name, balance) VALUES ('Charlie', 500);
SAVEPOINT after_charlie;        -- mark checkpoint here

INSERT INTO accounts (name, balance) VALUES ('Dave', 300);
-- Oops — Dave was added by mistake

ROLLBACK TO after_charlie;      -- Dave's row is removed; Charlie's row still exists

COMMIT;                         -- Only Charlie's insert is saved permanently
```

```sql
SELECT * FROM accounts;
-- Alice:   1000
-- Bob:     200
-- Charlie: 500   (Dave never made it)
```

> **Common mistake:** Every SQL statement outside a `BEGIN` block is **auto-committed** instantly and permanently. There is no undo. If you accidentally run `DELETE FROM accounts;` without `BEGIN`, the data is gone immediately. Always wrap destructive operations in `BEGIN` first.

---

## ACID Properties

ACID is the set of four guarantees that every database transaction must satisfy. These are the rules that make transactions reliable.

| Letter | Property    | Plain meaning                                                              |
|--------|-------------|----------------------------------------------------------------------------|
| **A**  | Atomicity   | All steps in a transaction succeed together, or none of them do            |
| **C**  | Consistency | A transaction takes the database from one valid state to another valid one |
| **I**  | Isolation   | Concurrent transactions don't interfere with each other                    |
| **D**  | Durability  | Once committed, data survives crashes, power loss, or restarts             |

### Each property explained

**Atomicity** — The "all or nothing" rule.  
If a transaction has 5 statements and the 3rd one fails, the first 2 are automatically rolled back. The database never ends up in a half-done state.

**Consistency** — Rules are never broken.  
Constraints (`NOT NULL`, `FOREIGN KEY`, `CHECK`) are always enforced. A transaction that would break a rule is rejected entirely.

**Isolation** — Transactions don't see each other's in-progress work.  
If two transactions run at the same time, each one behaves as if it's the only one running. (How strictly this is enforced is controlled by Isolation Levels — see the next section.)

**Durability** — Committed data is permanent.  
PostgreSQL writes to a WAL (Write-Ahead Log) before confirming a commit. If the server crashes right after a COMMIT, the data is recovered from the log on restart.

> **Most missed concept:** ACID is a *guarantee set*, not a feature you turn on. It describes how a well-implemented transaction system must behave. When a system claims "ACID compliance," it means all four properties hold.

> NoSQL databases often trade some ACID properties (especially Isolation and sometimes Durability) for speed and scalability. This is the core of the CAP Theorem discussed later.

---

## Locking Mechanism

Locks are how the database enforces isolation when multiple users (or processes) access the same data at the same time. Without locks, two people could update the same row simultaneously and produce garbage results.

**Real-world analogy:** A shared Google Doc with "editing" mode — only one person edits at a time while others can still read it. The lock is the edit access.

### Why locks exist — a concrete problem without them

Imagine two people booking the last seat on a flight simultaneously:

```
Person A reads: seat 14A is available
Person B reads: seat 14A is available
Person A books: UPDATE seat SET status='booked' WHERE seat='14A'
Person B books: UPDATE seat SET status='booked' WHERE seat='14A'
-- Both succeed! Now two people hold the same seat.
```

A lock on the row when Person A reads-to-update would have blocked Person B until Person A either committed (seat gone) or rolled back (seat available again).

### Two fundamental lock types

| Lock Type               | Held by           | Blocks                   | Used when      |
|-------------------------|-------------------|--------------------------|----------------|
| **Shared Lock (S)**     | Multiple readers  | Only exclusive writers   | Reading a row  |
| **Exclusive Lock (X)**  | One writer only   | All readers and writers  | Writing a row  |

- Many transactions can hold a shared lock on the same row simultaneously — reads don't block reads.
- An exclusive lock is held alone — a writer blocks everyone else until COMMIT or ROLLBACK.

### How PostgreSQL acquires locks automatically

You don't usually write lock commands manually. PostgreSQL handles it:

| SQL Statement         | Lock acquired automatically        |
|-----------------------|------------------------------------|
| `SELECT`              | No lock (snapshot-based reads)     |
| `INSERT`              | Exclusive lock on the new row      |
| `UPDATE`              | Exclusive lock on the target row   |
| `DELETE`              | Exclusive lock on the target row   |
| `ALTER TABLE`         | Full table lock                    |

### SELECT FOR UPDATE — explicit row locking

Use this when you need to read a row and you know you're about to update it. It locks the row immediately at SELECT time so no other transaction can modify it between your read and your write.

```sql
-- Session A (runs first)
BEGIN;

SELECT balance FROM accounts WHERE name = 'Alice' FOR UPDATE;
-- Row is now LOCKED — Session B cannot touch Alice's row until Session A commits

UPDATE accounts SET balance = balance - 500 WHERE name = 'Alice';

COMMIT;    -- lock released
```

```sql
-- Session B (runs at the same time, tries to update Alice)
BEGIN;

SELECT balance FROM accounts WHERE name = 'Alice' FOR UPDATE;
-- This WAITS here, blocked, until Session A commits or rolls back
-- Then it proceeds with the updated value

COMMIT;
```

Without `FOR UPDATE`, Session B could read Alice's old balance, compute a new value based on it, and overwrite Session A's change — classic race condition.

### Row-level vs Table-level locks

| Level        | Locks          | Speed    | Contention |
|--------------|----------------|----------|------------|
| Row-level    | One specific row  | Fast     | Low — other rows unaffected |
| Table-level  | Entire table    | Slow     | High — blocks all other operations |

PostgreSQL uses row-level locking by default for `UPDATE`, `DELETE`, `SELECT FOR UPDATE`.  
Table-level locks are used by `ALTER TABLE`, `DROP TABLE`, `TRUNCATE`, `VACUUM FULL`.

### Deadlocks — when two locks wait for each other

A deadlock occurs when:
- Transaction A holds a lock on Row 1 and waits for Row 2
- Transaction B holds a lock on Row 2 and waits for Row 1
- Both wait forever

```sql
-- Session A
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE name = 'Alice';   -- locks Alice
-- now waiting for Bob's row...
UPDATE accounts SET balance = balance + 100 WHERE name = 'Bob';

-- Session B (running simultaneously)
BEGIN;
UPDATE accounts SET balance = balance - 50 WHERE name = 'Bob';      -- locks Bob
-- now waiting for Alice's row...
UPDATE accounts SET balance = balance + 50 WHERE name = 'Alice';

-- DEADLOCK: A waits for B's lock on Bob. B waits for A's lock on Alice.
-- PostgreSQL detects this and kills one transaction with:
-- ERROR: deadlock detected
```

The killed transaction gets a deadlock error and must be retried. The other transaction continues.

**Prevention tip:** Always acquire locks in the same order across all transactions. If every transaction locks Alice before Bob, the circular wait can never happen.

```sql
-- Safe pattern: always lock lower id first
BEGIN;
SELECT * FROM accounts WHERE name IN ('Alice', 'Bob') ORDER BY id FOR UPDATE;
-- Locks both rows in consistent order — no deadlock possible
UPDATE accounts SET balance = balance - 100 WHERE name = 'Alice';
UPDATE accounts SET balance = balance + 100 WHERE name = 'Bob';
COMMIT;
```

> **Lock visibility:** Locks are released when a transaction ends (COMMIT or ROLLBACK). They are never released in the middle of a transaction (unless you use SAVEPOINT with partial rollback).

---

## Database Isolation Levels

Isolation levels let you control the trade-off between data accuracy and performance for a transaction. Stronger isolation = more locking = safer but slower.

**The core question isolation levels answer:** "What can my transaction see that other concurrent transactions are doing?"

### The three problems isolation levels protect against

Understanding these problems first makes the levels much easier to remember.

**Problem 1 — Dirty Read**

You read data that another transaction wrote but hasn't committed yet. If that other transaction rolls back, you read data that never officially existed.

```
Session A: BEGIN; UPDATE accounts SET balance = 9999 WHERE name = 'Alice';
Session B: SELECT balance FROM accounts WHERE name = 'Alice';
           -- Sees 9999 (not committed yet!) -- DIRTY READ
Session A: ROLLBACK;  -- 9999 never existed officially
Session B: made a decision based on a lie
```

**Problem 2 — Non-Repeatable Read**

You read the same row twice inside the same transaction and get different values because another transaction committed a change in between.

```
Session A: BEGIN;
Session A: SELECT balance FROM accounts WHERE name = 'Alice';  -- gets 1000
           -- (some time passes)
Session B: UPDATE accounts SET balance = 500 WHERE name = 'Alice'; COMMIT;
Session A: SELECT balance FROM accounts WHERE name = 'Alice';  -- gets 500 now!
           -- same query, different result within same transaction
Session A: COMMIT;
```

**Problem 3 — Phantom Read**

You run the same WHERE query twice and get different sets of rows because another transaction inserted or deleted matching rows in between.

```
Session A: BEGIN;
Session A: SELECT * FROM accounts WHERE balance > 500;  -- returns 2 rows
           -- (some time passes)
Session B: INSERT INTO accounts (name, balance) VALUES ('Eve', 800); COMMIT;
Session A: SELECT * FROM accounts WHERE balance > 500;  -- returns 3 rows now!
           -- a "phantom" row appeared
Session A: COMMIT;
```

### The four isolation levels

| Level               | Dirty Read | Non-Repeatable Read | Phantom Read | Performance |
|---------------------|------------|----------------------|--------------|-------------|
| `READ UNCOMMITTED`  | Possible   | Possible             | Possible     | Fastest     |
| `READ COMMITTED`    | Prevented  | Possible             | Possible     | Fast (PostgreSQL default) |
| `REPEATABLE READ`   | Prevented  | Prevented            | Possible*    | Moderate    |
| `SERIALIZABLE`      | Prevented  | Prevented            | Prevented    | Slowest     |

*PostgreSQL's `REPEATABLE READ` actually also prevents phantom reads in practice.

### How to set isolation level for a transaction

```sql
BEGIN TRANSACTION ISOLATION LEVEL READ COMMITTED;
-- or
BEGIN TRANSACTION ISOLATION LEVEL REPEATABLE READ;
-- or
BEGIN TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

### Demonstrating READ COMMITTED (default)

```sql
-- Session A
BEGIN;
SELECT balance FROM accounts WHERE name = 'Alice';
-- Returns: 1000

-- Meanwhile Session B runs and commits:
-- UPDATE accounts SET balance = 600 WHERE name = 'Alice'; COMMIT;

SELECT balance FROM accounts WHERE name = 'Alice';
-- Returns: 600  (sees Session B's committed change)
-- This is a non-repeatable read — different values in same transaction
COMMIT;
```

### Demonstrating REPEATABLE READ

```sql
-- Session A
BEGIN TRANSACTION ISOLATION LEVEL REPEATABLE READ;
SELECT balance FROM accounts WHERE name = 'Alice';
-- Returns: 1000  (snapshot taken here)

-- Meanwhile Session B runs and commits:
-- UPDATE accounts SET balance = 600 WHERE name = 'Alice'; COMMIT;

SELECT balance FROM accounts WHERE name = 'Alice';
-- Returns: 1000  (still! PostgreSQL uses the snapshot from transaction start)
COMMIT;
```

### Demonstrating SERIALIZABLE

```sql
-- Both sessions try to do the same thing simultaneously
-- Session A: give 10% bonus to everyone with balance > 500
-- Session B: give 10% bonus to everyone with balance > 500

-- With SERIALIZABLE, one will succeed and the other will get:
-- ERROR: could not serialize access due to concurrent update
-- The failed transaction must be retried
```

SERIALIZABLE is the strictest level. PostgreSQL detects when two transactions would produce a result that couldn't happen if they ran one at a time, and aborts one.

### Setting default isolation level for a session

```sql
SET default_transaction_isolation = 'repeatable read';
```

### When to use which level

| Use case                                               | Recommended level   |
|--------------------------------------------------------|---------------------|
| Dashboard/analytics reads, slight staleness is fine    | `READ COMMITTED`    |
| Generating a consistent report (snapshot of data)      | `REPEATABLE READ`   |
| Financial transfers, inventory updates, booking seats  | `SERIALIZABLE`      |

> **PostgreSQL default is `READ COMMITTED`** — right for most web applications. The database re-reads committed data at each statement, which is a good balance.

> **Common mistake:** Thinking the default is `SERIALIZABLE`. It is not. You have to explicitly opt into higher isolation when exactness matters. For interview prep: know what each level prevents and name an example use case for each.

---

## ALTER TABLE

Add, remove, or rename columns in an existing table.

### Add a column

```sql
ALTER TABLE students
ADD email TEXT DEFAULT 'unknown@example.com';
```

### Remove a column

```sql
ALTER TABLE students
DROP COLUMN email;
```

### Rename a column

```sql
ALTER TABLE students
RENAME COLUMN marks TO score;
```

### Rename the table

```sql
ALTER TABLE students
RENAME TO learners;
```

> **Note:** `DROP COLUMN` is not supported in SQLite. If you're on SQLite, you have to create a new table and copy data over.

---

## DROP TABLE

Removes the entire table — structure and all data — permanently.

```sql
DROP TABLE students;
DROP TABLE IF EXISTS students;   -- safer: doesn't error if table doesn't exist
```

| Command                    | What it removes                      |
|----------------------------|--------------------------------------|
| `DELETE FROM students`     | All rows, but table structure stays  |
| `DROP TABLE students`      | Everything: rows + table structure   |

---

## Indexes

An index is a separate data structure the database maintains to make lookups faster — just like the index at the back of a textbook lets you jump to a topic without reading every page.

**Without an index:** The database reads every row (full table scan) to find matches. With 10 million rows, this is slow.  
**With an index:** The database jumps directly to the matching rows using a B-tree (or other structure).

### Creating an index

```sql
CREATE INDEX idx_students_city ON students (city);
```

The naming convention is `idx_tablename_columnname`.

### Unique index (also enforces uniqueness)

```sql
CREATE UNIQUE INDEX idx_students_email ON students (email);
```

### Multi-column (composite) index

```sql
CREATE INDEX idx_name_city ON students (name, city);
```

Order matters: this index helps queries that filter by `name` alone, or by `name + city` together. It does NOT help queries that filter by `city` alone.

### Dropping an index

```sql
DROP INDEX idx_students_city;
```

### Viewing indexes in PostgreSQL

```sql
\d students        -- shows indexes along with table structure
```

### How indexes affect performance

| Scenario                              | Index helps? |
|---------------------------------------|--------------|
| `WHERE city = 'Kakinada'`             | Yes          |
| `ORDER BY city`                       | Yes          |
| `JOIN ON a.id = b.a_id`               | Yes (on join columns) |
| `WHERE city LIKE '%nada'`             | No (leading wildcard) |
| Small table (< a few thousand rows)   | Usually no   |

### The trade-off

| Benefit                             | Cost                                              |
|-------------------------------------|---------------------------------------------------|
| `SELECT` and `JOIN` queries faster  | `INSERT`, `UPDATE`, `DELETE` slightly slower       |
| Enforces uniqueness (UNIQUE index)  | Extra disk space                                  |

> **Rule of thumb:** Index columns that appear in `WHERE`, `JOIN ON`, and `ORDER BY` clauses. Don't over-index — every index slows down writes.

> **Most missed concept:** Primary keys are automatically indexed. You don't need to manually create an index on `id`. Foreign key columns, however, are NOT automatically indexed in PostgreSQL — you should add them manually.

```sql
CREATE INDEX idx_employees_dept_id ON employees (dept_id);
```

---

## JOINs

Joins combine rows from two or more tables based on a related column.

**Setup for examples:**

```sql
CREATE TABLE departments (
  dept_id   SERIAL PRIMARY KEY,
  dept_name TEXT NOT NULL
);

CREATE TABLE employees (
  emp_id    SERIAL PRIMARY KEY,
  name      TEXT NOT NULL,
  dept_id   INT REFERENCES departments(dept_id)
);
```

### INNER JOIN — only matching rows from both tables

```sql
SELECT e.name, d.dept_name
FROM employees AS e
INNER JOIN departments AS d ON e.dept_id = d.dept_id;
```

Returns only employees who have a matching department.

### LEFT JOIN — all rows from left table, NULLs for unmatched right

```sql
SELECT e.name, d.dept_name
FROM employees AS e
LEFT JOIN departments AS d ON e.dept_id = d.dept_id;
```

Returns all employees; if no matching department, `dept_name` is NULL.

### RIGHT JOIN — all rows from right table, NULLs for unmatched left

```sql
SELECT e.name, d.dept_name
FROM employees AS e
RIGHT JOIN departments AS d ON e.dept_id = d.dept_id;
```

Returns all departments; employees without a department show NULL.

### FULL OUTER JOIN — all rows from both tables

```sql
SELECT e.name, d.dept_name
FROM employees AS e
FULL OUTER JOIN departments AS d ON e.dept_id = d.dept_id;
```

Returns everything. Unmatched rows on either side show NULLs.

### Visual summary

```
Table A     Table B
  [A]         [B]

INNER JOIN  = only the overlapping center
LEFT JOIN   = all of A + overlap (B's unmatched = NULL)
RIGHT JOIN  = all of B + overlap (A's unmatched = NULL)
FULL JOIN   = everything from A and B
```

> **Missed pattern:** When you do a LEFT JOIN and then filter with `WHERE b.column = something`, it effectively becomes an INNER JOIN because NULLs are excluded by the WHERE. Put the condition in the `ON` clause if you truly want a LEFT JOIN behavior.

---

## Subqueries

A subquery is a SELECT statement nested inside another statement.

### Subquery in WHERE

```sql
SELECT name, joindate
FROM members
WHERE joindate = (
  SELECT MAX(joindate) FROM members
);
```

Finds the most recently joined member.

### Subquery in FROM (derived table)

```sql
SELECT city, avg_marks
FROM (
  SELECT city, AVG(marks) AS avg_marks
  FROM students
  GROUP BY city
) AS city_averages
WHERE avg_marks > 75;
```

### Subquery with IN

```sql
SELECT name
FROM students
WHERE city IN (
  SELECT city FROM top_cities WHERE rank <= 5
);
```

> **When to use a subquery vs a JOIN:** Use a subquery when you need to compare against an aggregate (like MAX, AVG). Use a JOIN when you need columns from both tables in the result.

---

## Schema — Theory vs Practice

**Theory meaning of schema:** The structure/blueprint of a table — which columns exist, what data types they have, what constraints are set.

**Practical meaning in PostgreSQL:** A schema is also a **namespace** inside a database. The default schema is `public`. You can have multiple schemas in one database to organize tables (like folders).

```sql
-- Table in default schema
SELECT * FROM students;

-- Table in a specific schema
SELECT * FROM cd.members;   -- 'cd' is a schema, 'members' is a table in it
```

---

## Normalization (1NF → 2NF → 3NF)

Normalization is the process of organizing tables to reduce redundancy and prevent anomalies.

**Three problems normalization solves:**
- **Update anomaly:** You change a value in one row but forget to change it in duplicated rows.
- **Insertion anomaly:** You can't insert new data without having other unrelated data.
- **Deletion anomaly:** Deleting one row accidentally deletes unrelated data.

### Starting point: Unnormalized

| student_id | student_name | courses        |
|------------|--------------|----------------|
| 1          | Ravi         | Python, SQL    |
| 2          | Priya        | Java, Python   |

Problem: `courses` column has multiple values in one cell.

---

### 1NF — First Normal Form

**Rule:** Each cell must contain exactly one atomic value. No lists, no repeating groups.

| student_id | student_name | course |
|------------|--------------|--------|
| 1          | Ravi         | Python |
| 1          | Ravi         | SQL    |
| 2          | Priya        | Java   |
| 2          | Priya        | Python |

Fixed the multi-value issue. But now `Ravi` appears twice — redundancy remains.

---

### 2NF — Second Normal Form

**Rule:** Must be in 1NF, and every non-key column must depend on the ENTIRE primary key (no partial dependencies).

Here, the composite key is `(student_id, course)`. But `student_name` depends only on `student_id` — that's a partial dependency.

**Fix:** Split into two tables.

**Students:**

| student_id (PK) | student_name |
|-----------------|--------------|
| 1               | Ravi         |
| 2               | Priya        |

**Enrollments:**

| student_id (PK, FK) | course (PK) |
|---------------------|-------------|
| 1                   | Python      |
| 1                   | SQL         |
| 2                   | Java        |

---

### 3NF — Third Normal Form

**Rule:** Must be in 2NF, and no non-key column should depend on another non-key column (no transitive dependencies).

Suppose we add `dept_id` and `dept_name` to Students:

| student_id | student_name | dept_id | dept_name        |
|------------|--------------|---------|------------------|
| 1          | Ravi         | 101     | Computer Science |
| 2          | Priya        | 101     | Computer Science |

Dependency chain: `student_id -> dept_id -> dept_name`  
`dept_name` transitively depends on `student_id` through `dept_id`. That's a 3NF violation.

**Fix:** Split out departments.

**Students:**

| student_id | student_name | dept_id |
|------------|--------------|---------|
| 1          | Ravi         | 101     |

**Departments:**

| dept_id (PK) | dept_name        |
|--------------|------------------|
| 101          | Computer Science |

---

### Summary Table

| Normal Form | Problem it solves              | Test question                                               |
|-------------|--------------------------------|-------------------------------------------------------------|
| 1NF         | Multiple values in one cell    | Does any cell hold a list?                                  |
| 2NF         | Partial dependency             | Does any column depend on only PART of a composite key?     |
| 3NF         | Transitive dependency          | Does a non-key column depend on another non-key column?     |

**PostgreSQL implementation of normalized schema:**

```sql
CREATE TABLE departments (
  dept_id   INT PRIMARY KEY,
  dept_name TEXT NOT NULL
);

CREATE TABLE students (
  student_id   INT PRIMARY KEY,
  student_name TEXT NOT NULL,
  dept_id      INT REFERENCES departments(dept_id)
);

CREATE TABLE enrollments (
  student_id INT REFERENCES students(student_id),
  course     TEXT,
  PRIMARY KEY (student_id, course)
);
```

---

## Triggers

A trigger is a function the database automatically runs when a specific event happens on a table — no manual call needed. You define it once; it fires every time that event occurs.

**Think of it as:** A motion-sensor light. You don't flip a switch — just walk in and it turns on automatically. A trigger is the same idea applied to database events.

### Where triggers are useful

- **Audit log:** Automatically record who changed what and when
- **Auto-timestamp:** Set `updated_at` to `NOW()` on every update
- **Validation:** Reject data that breaks a business rule the CHECK constraint can't express
- **Derived columns:** Auto-compute a value based on other columns being changed

### Trigger anatomy — two separate parts

You always need both:

1. **Trigger function** — a PL/pgSQL function that says *what to do*
2. **Trigger** — connects the function to a table and an event, says *when to do it*

### Setup for all examples

```sql
CREATE TABLE students (
  id         SERIAL PRIMARY KEY,
  name       TEXT NOT NULL,
  marks      INT,
  updated_at TIMESTAMP
);

CREATE TABLE audit_log (
  id         SERIAL PRIMARY KEY,
  table_name TEXT,
  action     TEXT,
  row_id     INT,
  changed_at TIMESTAMP DEFAULT NOW()
);

INSERT INTO students (name, marks) VALUES ('Ravi', 85), ('Priya', 90);
```

### Example 1 — Auto-update timestamp on every UPDATE

**Step 1: Write the trigger function**

```sql
CREATE OR REPLACE FUNCTION set_updated_at()
RETURNS TRIGGER AS $$
BEGIN
  NEW.updated_at = NOW();    -- modify the row before it is written
  RETURN NEW;                -- must return NEW so the row is actually saved
END;
$$ LANGUAGE plpgsql;
```

`NEW` is a special variable — it holds the row that is about to be inserted or updated. You can modify it before returning.

**Step 2: Attach it to the table**

```sql
CREATE TRIGGER trg_students_updated_at
BEFORE UPDATE ON students         -- fires BEFORE the UPDATE is written
FOR EACH ROW                      -- fires once per row affected
EXECUTE FUNCTION set_updated_at();
```

**Test it:**

```sql
UPDATE students SET marks = 95 WHERE name = 'Ravi';

SELECT name, marks, updated_at FROM students WHERE name = 'Ravi';
-- Ravi | 95 | 2026-11-12 22:05:00  (timestamp was set automatically)
```

You didn't write `updated_at` anywhere in your UPDATE — the trigger did it.

### Example 2 — Audit log: record every INSERT, UPDATE, DELETE

**Step 1: Write the trigger function**

```sql
CREATE OR REPLACE FUNCTION log_student_changes()
RETURNS TRIGGER AS $$
BEGIN
  INSERT INTO audit_log (table_name, action, row_id, changed_at)
  VALUES ('students', TG_OP, COALESCE(NEW.id, OLD.id), NOW());

  IF TG_OP = 'DELETE' THEN
    RETURN OLD;    -- for DELETE, return OLD (the row that was deleted)
  ELSE
    RETURN NEW;    -- for INSERT/UPDATE, return NEW (the row being saved)
  END IF;
END;
$$ LANGUAGE plpgsql;
```

Built-in trigger variables:
- `TG_OP` — the event that fired: `'INSERT'`, `'UPDATE'`, or `'DELETE'`
- `NEW` — the row being inserted or the new version of an updated row
- `OLD` — the row before an update, or the row being deleted

**Step 2: Attach it**

```sql
CREATE TRIGGER trg_audit_students
AFTER INSERT OR UPDATE OR DELETE ON students
FOR EACH ROW
EXECUTE FUNCTION log_student_changes();
```

**Test it:**

```sql
INSERT INTO students (name, marks) VALUES ('Kiran', 75);
UPDATE students SET marks = 80 WHERE name = 'Kiran';
DELETE FROM students WHERE name = 'Kiran';

SELECT * FROM audit_log;
-- | table_name | action | row_id | changed_at          |
-- | students   | INSERT | 3      | 2026-11-12 22:05:10 |
-- | students   | UPDATE | 3      | 2026-11-12 22:05:11 |
-- | students   | DELETE | 3      | 2026-11-12 22:05:12 |
```

Every operation on `students` is automatically recorded — without writing a single line in your application code.

### Example 3 — BEFORE trigger for validation

Block an insert or update if the data breaks a business rule.

```sql
CREATE OR REPLACE FUNCTION prevent_marks_decrease()
RETURNS TRIGGER AS $$
BEGIN
  IF NEW.marks < OLD.marks THEN
    RAISE EXCEPTION 'Marks cannot be decreased. Old: %, New: %', OLD.marks, NEW.marks;
  END IF;
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_no_marks_decrease
BEFORE UPDATE ON students
FOR EACH ROW
EXECUTE FUNCTION prevent_marks_decrease();
```

**Test it:**

```sql
UPDATE students SET marks = 60 WHERE name = 'Ravi';   -- Ravi had 95
-- ERROR: Marks cannot be decreased. Old: 95, New: 60
```

The UPDATE is blocked entirely. The row is not changed.

### BEFORE vs AFTER — which to use?

| Timing   | When it fires                        | Can modify NEW? | Typical use                              |
|----------|--------------------------------------|-----------------|------------------------------------------|
| `BEFORE` | Before the row is written            | Yes             | Validation, modify data before saving    |
| `AFTER`  | After the row is successfully written | No             | Audit log, cascade to other tables       |

### FOR EACH ROW vs FOR EACH STATEMENT

| Mode                  | Fires how many times?         | Has access to NEW/OLD? |
|-----------------------|-------------------------------|------------------------|
| `FOR EACH ROW`        | Once per affected row         | Yes                    |
| `FOR EACH STATEMENT`  | Once per SQL statement total  | No                     |

If you `UPDATE students SET marks = 0;` (affects 100 rows):
- `FOR EACH ROW` trigger fires 100 times
- `FOR EACH STATEMENT` trigger fires once

### Viewing and removing triggers

```sql
\d students               -- psql: shows the table structure including triggers

DROP TRIGGER trg_audit_students ON students;
DROP FUNCTION log_student_changes();   -- drop function separately if no longer needed
```

> **Common mistake:** In a `BEFORE` trigger, forgetting to `RETURN NEW`. If you `RETURN NULL` from a BEFORE trigger, the operation (INSERT/UPDATE) is silently cancelled — the row is not saved and no error is raised. This is intentional for validation triggers that want to block the write.

> **Another mistake:** Trying to access `NEW` in a DELETE trigger or `OLD` in an INSERT trigger. `NEW` doesn't exist on DELETE (there's no new row). `OLD` doesn't exist on INSERT (there was no previous row).

> **Avoid triggers for application business logic.** They are invisible to developers reading the code and make debugging painful. Use triggers only for database infrastructure concerns: timestamps, audit logs, data consistency. Keep business logic in your application code.

---

## CAP Theorem

CAP Theorem states that any distributed database system can only guarantee **two of the following three** properties at the same time — never all three.

| Letter | Property        | Meaning                                                                                      |
|--------|-----------------|----------------------------------------------------------------------------------------------|
| **C**  | Consistency     | Every read returns the most recent write (or an error) — no stale data                      |
| **A**  | Availability    | Every request gets a response (not necessarily the latest data, but always a response)       |
| **P**  | Partition Tolerance | The system keeps working even when network communication between nodes breaks down       |

### The network partition problem

In a distributed system (data spread across multiple machines/data centers), a **network partition** means two nodes can no longer talk to each other. When this happens, the system must choose:

- **Sacrifice Consistency** → both nodes stay available, but may serve different (stale) data → **AP system**
- **Sacrifice Availability** → refuse to respond until the partition heals → **CP system**

You cannot avoid Partition Tolerance in a real distributed system — networks fail. So the real choice is always **C vs A** when a partition occurs.

### Where real systems land

| System                     | Guarantees | Trade-off                                                |
|----------------------------|------------|----------------------------------------------------------|
| PostgreSQL, MySQL (single node) | CA     | No partition tolerance — not distributed by default      |
| MongoDB (default config)   | AP         | Prioritizes availability; eventual consistency           |
| HBase, Zookeeper           | CP         | Prioritizes consistency; may refuse requests             |
| Cassandra                  | AP         | Tunable consistency per query                            |

### CAP vs ACID

| Concept | Lives in       | About                                              |
|---------|----------------|----------------------------------------------------|
| ACID    | Single-node DB | Guarantees within one transaction on one database  |
| CAP     | Distributed DB | Trade-offs when data is split across multiple nodes|

> **Most missed concept:** CAP Theorem does NOT say you pick two properties permanently. It says during a partition event, you must choose between C and A. Outside of a partition, a well-designed system can provide all three to some degree.

> **Practical relevance for SQL developers:** If you're on PostgreSQL on a single machine (which you are while learning), ACID applies fully. CAP becomes relevant when you're designing systems that replicate data across multiple servers or regions.

---

## Common Mistakes & Misconceptions

| Mistake | Correct Approach |
|---------|-----------------|
| `WHERE marks = NULL` | `WHERE marks IS NULL` |
| Using aggregate in WHERE: `WHERE AVG(marks) > 70` | Use HAVING: `HAVING AVG(marks) > 70` |
| `UPDATE students SET marks = 90` without WHERE | Always add WHERE — this updates every row |
| `DELETE FROM students` without WHERE | Deletes all rows; table structure remains |
| Using alias in WHERE: `WHERE adjusted = 90` | Alias is from SELECT which runs after WHERE; use the full expression |
| `NULL = NULL` returns TRUE | NULL = NULL returns UNKNOWN; use `IS NULL` |
| `LIKE` with `=`: `name LIKE = 'Ra%'` | Never combine LIKE and =; it's just `name LIKE 'Ra%'` |
| Ctrl+Z in psql | Does NOT quit; use `\q` or Ctrl+D |
| `DROP TABLE` to clear rows | Use `DELETE FROM` to keep the table; `DROP TABLE` removes structure too |
| `HAVING` without `GROUP BY` | Usually a logical error; just use WHERE |
| LEFT JOIN with WHERE on right table col | Becomes an INNER JOIN; put the filter in the ON clause instead |

---

## Quick Reference Cards

### Full SELECT Template

```sql
SELECT
  column1,
  column2,
  AGG_FUNC(column3) AS alias
FROM table_name
JOIN other_table ON table_name.id = other_table.fk_id
WHERE row_condition
GROUP BY column1, column2
HAVING group_condition
ORDER BY column1 ASC, column2 DESC
LIMIT 10
OFFSET 20;
```

### CRUD Template

```sql
-- Create
INSERT INTO table (col1, col2) VALUES ('val1', 42);

-- Read
SELECT * FROM table WHERE condition;

-- Update
UPDATE table SET col1 = 'new_val' WHERE condition;

-- Delete
DELETE FROM table WHERE condition;
```

### Table Management Template

```sql
-- Create
CREATE TABLE table_name (id SERIAL PRIMARY KEY, name TEXT NOT NULL);

-- Modify
ALTER TABLE table_name ADD COLUMN email TEXT;
ALTER TABLE table_name DROP COLUMN email;
ALTER TABLE table_name RENAME COLUMN old_name TO new_name;
ALTER TABLE table_name RENAME TO new_table_name;

-- Remove
DROP TABLE IF EXISTS table_name;
```

### Aggregate Quick Reference

```sql
SELECT
  COUNT(*)        AS total_rows,
  COUNT(city)     AS rows_with_city,
  SUM(marks)      AS total_marks,
  AVG(marks)      AS average,
  MIN(marks)      AS lowest,
  MAX(marks)      AS highest
FROM students;
```

### JOIN Quick Reference

```sql
-- INNER JOIN (only matches)
SELECT * FROM a INNER JOIN b ON a.id = b.a_id;

-- LEFT JOIN (all from a, NULLs for unmatched b)
SELECT * FROM a LEFT JOIN b ON a.id = b.a_id;

-- RIGHT JOIN (all from b, NULLs for unmatched a)
SELECT * FROM a RIGHT JOIN b ON a.id = b.a_id;

-- FULL OUTER JOIN (everything)
SELECT * FROM a FULL OUTER JOIN b ON a.id = b.a_id;
```

### String Functions Quick Reference

```sql
SELECT
  UPPER(name),                -- 'ravi' -> 'RAVI'
  LOWER(name),                -- 'RAVI' -> 'ravi'
  LENGTH(name),               -- number of characters
  TRIM(name),                 -- remove leading/trailing spaces
  name || ' ' || city,        -- concatenate strings with ||
  CONCAT(name, ' ', city),    -- same as above
  SUBSTRING(name, 1, 3)       -- first 3 characters
FROM students;
```

### NULL Handling Quick Reference

```sql
SELECT
  COALESCE(city, 'Unknown'),       -- replace NULL with 'Unknown'
  NULLIF(marks, 0)                 -- return NULL if marks = 0 (useful to avoid div by 0)
FROM students;
```

---

> **Learning order:** Setup -> SELECT -> WHERE -> ORDER BY / LIMIT -> Aggregates -> GROUP BY / HAVING -> JOINs -> Subqueries -> Normalization  
> **Interview focus:** JOINs, GROUP BY + HAVING, Subqueries, Window Functions (next step after this sheet)

