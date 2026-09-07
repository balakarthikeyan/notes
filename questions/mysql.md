# MySQL Fundamentals & Data Architecture

## What are DDL, DML, and DCL?

**DDL (Data Definition Language):** Used to define, alter, and manage database schemas and structures.
* `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `COMMENT`, `RENAME`

**DML (Data Manipulation Language):** Used to insert, retrieve, modify, delete, and inspect data within database objects.
* `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `MERGE`, `CALL`, `EXPLAIN PLAN`, `LOCK TABLE`

**DCL (Data Control Language):** Used to manage database rights, user permissions, and access controls.
* `GRANT`, `REVOKE`

### Example:

```sql
-- DDL: Create structure
CREATE TABLE account (id INT PRIMARY KEY, balance DECIMAL(10,2));
-- DML: Insert data
INSERT INTO account VALUES (1, 500.00);
-- DCL: Grant read permission
GRANT SELECT ON account TO 'app_user'@'localhost';
```

**Output:** `Query OK, 1 row affected (0.01 sec)`

---

## What is the difference between Data Definition Language (DDL) and Data Manipulation Language (DML)?

* **Data Definition Language (DDL):** Commands used to define or alter the structure of the database. Common DDL commands include `CREATE`, `ALTER`, `DROP`, and `TRUNCATE`.
* **Data Manipulation Language (DML):** Commands used to manipulate or modify stored records. Common DML commands include `INSERT`, `UPDATE`, and `DELETE`.

---

## What is the difference between TRUNCATE and DELETE?

* **`DELETE`:** A **DML** command used to remove specific or all rows from a table. It supports the `WHERE` clause. Operations are logged row-by-row and can be rolled back within a transaction.
* **`TRUNCATE`:** A **DDL** command that removes all records from a table by dropping, resets `AUTO_INCREMENT` counters and recreating the underlying storage pages. It is significantly faster than `DELETE`. In MySQL, `TRUNCATE` operations reset auto-increment counters and cannot be rolled back in the same way standard DML transactions operate.

### Legacy vs Modern Implementations:

```sql
CREATE TABLE logs (id INT AUTO_INCREMENT PRIMARY KEY, msg VARCHAR(50));
INSERT INTO logs (msg) VALUES ('Err1'), ('Err2');

-- Modern / Transactional DELETE
START TRANSACTION;
DELETE FROM logs WHERE id = 1;
ROLLBACK; -- Row restored

-- TRUNCATE reset behavior
TRUNCATE TABLE logs; -- Empties table & resets AUTO_INCREMENT to 1
```

---

## Explain difference between TIMESTAMP and DATETIME?

Both `TIMESTAMP` and `DATETIME` store date and time in the standard `YYYY-MM-DD HH:MM:SS` format.

* **`DATETIME`:** Stores the literal date and time value provided without any timezone offsets conversion. (`1000-01-01 00:00:00` to `9999-12-31 23:59:59`). Occupies 5 bytes in modern MySQL.
* **`TIMESTAMP`:** Stores UTC timestamp seconds since Epoch (`1970-01-01 00:00:01` to `2038-01-19 03:14:07`). Converts the provided time to UTC before storing and converts it back to the current server or session time zone upon retrieval. This facilitates serving users across different geographic regions.

### Example:

```sql
CREATE TABLE tz_demo (
  dt DATETIME,
  ts TIMESTAMP
);
SET time_zone = '+00:00';
INSERT INTO tz_demo VALUES ('2026-08-01 12:00:00', '2026-08-01 12:00:00');
SET time_zone = '+05:30';
SELECT dt, ts FROM tz_demo;
```

**Output:**

| dt | ts |
| --- | --- |
| `2026-08-01 12:00:00` | `2026-08-01 17:30:00` |

---

## What is difference between BLOB and TEXT in MySQL?

* **Data Type Intent:** `BLOB` is designed for raw binary data (e.g., images, files), while `TEXT` is designed for character text streams governed by collation rules (UTF-8, ASCII).
* **Encoding & Sorting:** `TEXT` stores character strings with collations and supports text-based sorting and comparisons. `BLOB` stores binary byte strings without character set encodings.

---

## What are different TEXT data types in MySQL? What is difference between TEXT and VARCHAR?

### Text Data Types & Max Sizes

* `TINYTEXT`: Up to 255 bytes (characters)
* `TEXT`: Up to 65,535 bytes (~64 KB)
* `MEDIUMTEXT`: Up to 16,777,215 bytes (~16 MB)
* `LONGTEXT`: Up to 4,294,967,295 bytes (~4 GB)

### Differences Between `VARCHAR` and `TEXT`

* **Storage Allocation:** `VARCHAR` data is stored inline inside the database table row pages. `TEXT` data columns are stored off-page with a reference pointer retained in the main table row.
* **Indexing Constraints:** `TEXT` columns require an explicit prefix length when creating indexes.
* **Default Values:** `VARCHAR` supports inline default values. `TEXT` columns historically did not support default values (though modern MySQL 8.0+ permits expression-based defaults).

---

## Describe BLOB in MySQL?

A **BLOB (Binary Large Object)** is used to store unstructured binary data inside MySQL.

### BLOB Data Types & Capacities

* `TINYBLOB`: Up to 255 bytes
* `BLOB`: Up to 65,535 bytes (~64 KB)
* `MEDIUMBLOB`: Up to 16,777,215 bytes (~16 MB)
* `LONGBLOB`: Up to 4,294,967,295 bytes (~4 GB)

---

## How are VARCHAR and CHAR different?

* **Storage Allocation:** `CHAR` allocates a fixed length of memory based on the defined column size; shorter values are padded with trailing spaces. `VARCHAR` allocates variable length based on the actual string length plus a 1-byte or 2-byte prefix.
* **Performance:** `CHAR` offers slightly better performance for fixed-size entries (e.g., country codes, hashes) because memory offsets are static.


### Comparison:

| Column Type | Input String | Bytes Stored |
| --- | --- | --- |
| `CHAR(10)` | `'MySQL'` | 10 bytes (padded) |
| `VARCHAR(10)` | `'MySQL'` | 6 bytes (5 bytes text + 1 byte prefix) |

---

## How can ENUM be used in MySQL?

`ENUM` is a string object whose value is chosen from a list of permitted values declared explicitly at table creation.

### Example:

```sql
CREATE TABLE Student (
  rollnumber INT NOT NULL,
  name VARCHAR(25) NOT NULL,
  country ENUM('USA', 'UK', 'Australia'),
  PRIMARY KEY(rollnumber)
);

INSERT INTO Student VALUES (6, 'John', 'USA');
SELECT name, country, country + 0 AS enum_index FROM Student;
```

**Output:**

| name | country | enum_index |
| --- | --- | --- |
| `John` | `USA` | `1` |

---

## What is AUTO INCREMENT in MySQL?

`AUTO_INCREMENT` automatically generates a unique sequential integer value for newly inserted rows. A table can have only **one** `AUTO_INCREMENT` column, and it must be defined as an indexed column (typically a `PRIMARY KEY` or `UNIQUE` constraint).

### Example:

```sql
CREATE TABLE Student (
  studentid INT NOT NULL AUTO_INCREMENT,
  rollnumber INT NOT NULL,
  name VARCHAR(25) NOT NULL,
  country ENUM('USA', 'UK', 'Australia'),
  PRIMARY KEY(studentid)
);

INSERT INTO Student (rollnumber, name, country) VALUES (101, 'Alice', 'UK');
SELECT * FROM Student;
```

**Output:**

| studentid | rollnumber | name | country |
| --- | --- | --- | --- |
| `1` | `101` | `Alice` | `UK` |

---

## What is self referencing foreign key?

A self-referencing foreign key occurs when a foreign key constraint in a table points directly back to the primary key of the **same** table. This pattern is commonly used for hierarchical or tree structures (such as employee-manager relationships).

### Example:

```sql
CREATE TABLE Employee (
  employee_id CHAR(9) NOT NULL,
  name VARCHAR(25) NOT NULL,
  manager_id CHAR(9) NULL,
  salary DECIMAL(10,2) NULL,
  PRIMARY KEY(employee_id),
  FOREIGN KEY (manager_id) REFERENCES Employee(employee_id) ON DELETE CASCADE
);

INSERT INTO Employee VALUES ('E1', 'CEO', NULL, 150000.00);
INSERT INTO Employee VALUES ('E2', 'Dev Lead', 'E1', 90000.00);
SELECT e.name AS Employee, m.name AS Manager 
FROM Employee e LEFT JOIN Employee m ON e.manager_id = m.employee_id;
```

**Output:**

| Employee | Manager |
| --- | --- |
| `CEO` | `NULL` |
| `Dev Lead` | `CEO` |

---

## What is normalization?

Normalization is the process of organizing relational database structures to minimize data redundancy and prevent data anomalies (insertion, update, and deletion anomalies).

### Normal Forms

* **1NF (First Normal Form):** Ensures atomic values and eliminates repeating groups.
* **2NF (Second Normal Form):** Satisfies 1NF and eliminates partial dependencies (all non-key attributes must depend fully on the primary key).
* **3NF (Third Normal Form):** Satisfies 2NF and eliminates transitive dependencies (non-key attributes must not depend on other non-key attributes).
* **BCNF (Boyce-Codd Normal Form):** A stricter variant of 3NF addressing anomalies in tables with multiple candidate keys.
* **4NF (Fourth Normal Form):** Eliminates multi-valued dependencies.
* **5NF (Fifth Normal Form):** Eliminates join dependencies.

---

# Querying, Joins & Advanced SQL Constructs

## What are subqueries?

A subquery is a `SELECT` statement nested inside another SQL statement (`SELECT`, `INSERT`, `UPDATE`, or `DELETE`). It acts as an intermediate calculation or filtering step for the primary query.

### Example:

```sql
CREATE TABLE emp (id INT, salary INT);
INSERT INTO emp VALUES (1, 3000), (2, 5000), (3, 4000);

SELECT * FROM emp WHERE salary > (SELECT AVG(salary) FROM emp);
```

**Output:**

| id | salary |
| --- | --- |
| `2` | `5000` |

---

## What is Join?

A join merges columns from two or more tables using related key fields.

### Example Setup:

```sql
CREATE TABLE A (id INT, val VARCHAR(5));
CREATE TABLE B (id INT, val VARCHAR(5));
INSERT INTO A VALUES (1, 'A1'), (2, 'A2');
INSERT INTO B VALUES (2, 'B2'), (3, 'B3');
```

* **Inner Join:** Returns rows where there is a matching value in both tables.
```sql
SELECT A.id, A.val, B.val FROM A INNER JOIN B ON A.id = B.id;
```
**Output:** `(2, 'A2', 'B2')`

* **Left Join (Left Outer Join):** Returns all records from the left table and matching records from the right table (unmatched right side values return `NULL`).
```sql
SELECT A.id, A.val, B.val FROM A LEFT JOIN B ON A.id = B.id;
```
**Output:** `(1, 'A1', NULL)`, `(2, 'A2', 'B2')`

* **Right Join (Right Outer Join):** Returns all records from the right table and matching records from the left table (unmatched left side values return `NULL`).
```sql
SELECT B.id, A.val, B.val FROM A RIGHT JOIN B ON A.id = B.id;
```
**Output:** `(2, 'A2', 'B2')`, `(3, NULL, 'B3')`

* **Full Outer Join:** Returns all records when there is a match in either table (MySQL does not support `FULL OUTER JOIN` natively; it is typically simulated using `LEFT JOIN` ... `UNION` ... `RIGHT JOIN`).
```sql
SELECT A.id, A.val, B.val FROM A LEFT JOIN B ON A.id = B.id
UNION
SELECT B.id, A.val, B.val FROM A RIGHT JOIN B ON A.id = B.id;
```

* **Self Join:** Merges a table with itself to evaluate relationships among rows within the same table.

---

## Write the Syntactical Flow of MySQL Query

The logical execution order of an SQL query runs as follows:

1. `FROM`: Assembles initial baseline datasets and executes table joins.
2. `WHERE`: Filters individual rows prior to grouping.
3. `GROUP BY`: Aggregates the filtered row sets into summary groups.
4. `HAVING`: Filters aggregate calculations derived from `GROUP BY`.
5. `SELECT`: Evaluates projection list calculations and outputs columns.
6. `ORDER BY`: Sorts the final output rows.

---

## What are the most common functions in MySQL Server?

* **String Functions:** `CONCAT()`, `LEFT()`, `RIGHT()`, `SUBSTRING()`, `LENGTH()`, `LTRIM()`, `RTRIM()`, `TRIM()`, `REPLACE()`, `LOWER()`, `UPPER()`
* **Numeric Functions:** `ABS()`, `ROUND()`
* **Date & Time Functions:** `DATEDIFF()`, `CURRENT_TIMESTAMP()`, `DATE_ADD()`, `DAY()`, `MONTH()`, `YEAR()`
* **Aggregate Functions:** `COUNT()`, `SUM()`, `AVG()`, `MIN()`, `MAX()`
* **Flow Control & Null Functions:** `CAST()`, `CONVERT()`, `COALESCE()`, `IFNULL()`, `NULLIF()` 

*(Note: `ISNULL()` is standard in T-SQL; MySQL uses `IFNULL()` or the unary operator `IS NULL`)*.

---

## What is Coalesce() and IsNull()?

* `COALESCE(...)`: Evaluates the argument list in sequence and returns the **first non-null** value.
* `ISNULL(...)`: Evaluates an expression and returns a replacement value if the expression is `NULL`. 

*(In MySQL, `IFNULL(expr1, expr2)` performs this role, whereas MySQL's `ISNULL(expr)` returns `1` if the expression is `NULL`, else `0`)*.


### Example:

```sql
SELECT COALESCE(NULL, NULL, 'Found') AS c_val, IFNULL(NULL, 'Alt') AS i_val, ISNULL(1/0) AS is_null;
```

**Output:**

| c_val | i_val | is_null |
| --- | --- | --- |
| `Found` | `Alt` | `1` |

---

## Explain the ANY and ALL keywords?

`ANY` and `ALL` compare a single value against a range or result set of values returned by a subquery:

* **`ANY`:** Returns `TRUE` if the comparison evaluates to true for **at least one** value in the subquery result.
* **`ALL`:** Returns `TRUE` only if the comparison evaluates to true for **all** values in the subquery result.

### Example:

```sql
CREATE TABLE s1 (val INT); INSERT INTO s1 VALUES (10), (20), (30);
SELECT 25 > ANY(SELECT val FROM s1) AS match_any, 25 > ALL(SELECT val FROM s1) AS match_all;
```

**Output:**

| match_any | match_all |
| --- | --- |
| `1` | `0` |

---

## Example of UPSERT logic using MySQL?

Use `ON DUPLICATE KEY UPDATE` to perform an `INSERT`, or an `UPDATE` if a primary key or unique index collision occurs:

### Example:

```sql
CREATE TABLE User (
  userid INT PRIMARY KEY,
  name VARCHAR(50),
  mobilenumber VARCHAR(15) UNIQUE
);

INSERT INTO User (userid, name, mobilenumber) VALUES (1, 'Alice', '876876876');

-- UPSERT operation
INSERT INTO User (userid, name, mobilenumber) 
VALUES (1, 'Alice', '876876876') 
ON DUPLICATE KEY UPDATE name='Alice', mobilenumber='876876876';
```

**Output:** `Query OK, 2 rows affected (0.00 sec)` *(Note: MySQL reports 2 rows affected for updates triggered by duplicate key violations).*

---

## Explain the concept of Common Table Expressions (CTE) in MySQL?

Introduced in **MySQL 8.0**, a CTE generates a named temporary result set available within the execution scope of a single `SELECT`, `INSERT`, `UPDATE`, or `DELETE` statement.


### Legacy vs Modern Implementations:

```sql
-- LEGACY (MySQL 5.7 Workaround via Subquery)
SELECT * FROM (SELECT id, name FROM emp WHERE salary > 4000) AS high_pay;

-- MODERN (MySQL 8.0 CTE Standard)
WITH high_pay AS (
  SELECT id, name FROM emp WHERE salary > 4000
)
SELECT * FROM high_pay;
```

---

## Explain the concept of Prepared Statements in MySQL?

Prepared Statements separate query parsing and compilation from parameter values, eliminating SQL injection vectors and optimizing performance for repeated queries.

### Architecture:

```
[Client] -- (1) PREPARE query string --> [MySQL Server: Parse & Plan Compilation]
[Client] -- (2) EXECUTE with Params ---> [MySQL Server: Rapid Execution]
```

### Workflow:

1. **Prepare:** Synthesize the query template.
```sql
PREPARE stmt_name FROM 'SELECT name FROM User WHERE userid = ?';
```

2. **Execute:** Bind input parameters and execute.
```sql
SET @id = '11112227';
EXECUTE stmt_name USING @id;
```

3. **Deallocate:** Release server memory resources.
```sql
DEALLOCATE PREPARE stmt_name;
```

### Why use Prepared Statements?

* **Prevent SQL Injection:** Input variables are bound strictly as literal data, preventing arbitrary command execution.
* **Performance:** Statement parsing and execution plan compilation are performed once and reused across subsequent executions.
* **Scalability:** Reduces CPU overhead on the database server during high-volume operations.
* **Code Maintainability:** Separates operational SQL logic from runtime variable values.

---

## What is a pivot table, and how do you create one in MySQL?

A pivot table summarizes row data by rotating distinct key values into dynamic tabular columns. Because MySQL does not contain a native `PIVOT` clause, pivoting is achieved using aggregate functions combined with `CASE` conditional statements:

### Example:

```sql
CREATE TABLE sales (year INT, product VARCHAR(10), amount INT);
INSERT INTO sales VALUES (2025, 'A', 100), (2025, 'B', 200), (2026, 'A', 150);

SELECT 
  year,
  SUM(CASE WHEN product = 'A' THEN amount ELSE 0 END) AS Prod_A,
  SUM(CASE WHEN product = 'B' THEN amount ELSE 0 END) AS Prod_B
FROM sales GROUP BY year;
```

**Output:**

| year | Prod_A | Prod_B |
| --- | --- | --- |
| `2025` | `100` | `200` |
| `2026` | `150` | `0` |

---

## What do ROLLUP, GROUPING SETS, and CUBE Do in T-SQL?

> **Compatibility Note:** The following constructs originate from standard SQL / T-SQL. MySQL supports `WITH ROLLUP` as an extension to `GROUP BY`. MySQL does **not** natively support explicit `GROUPING SETS` or `CUBE` keywords.

* **`ROLLUP`:** Generates multiple hierarchical grouping sets, appending subtotal and grand total summary rows to the output. Supported in MySQL via `GROUP BY col WITH ROLLUP`.
* **`GROUPING SETS`:** *(T-SQL feature)* Defines explicit multiple grouping sets within a single query without computing unnecessary combinations.
* **`CUBE`:** *(T-SQL feature)* Calculates all possible combinations of grouping columns, generating complete dimensional subtotals.

---

# Indexing, Storage Engines & Locking

## What is Index?

An index is a secondary data structure (most commonly a **B-Tree**) managed alongside table data. It stores sorted key values with row pointers to accelerate retrieval speeds.

### Example:

```sql
CREATE INDEX idx_user_mobile ON User(mobilenumber);
```

### Key Considerations

* **Candidate Columns:** Target columns frequently used in `WHERE`, `JOIN`, and `ORDER BY` operations.
* **Storage Overhead:** Indexes increase disk consumption.
* **Write Latency:** Too many indexes degrade write speeds during `INSERT`, `UPDATE`, and `DELETE` operations.

---

## What is MySQL Storage Engines?

A storage engine is the software module underlying a database server responsible for handling disk I/O, record layout, indexing structures, locking mechanics, and transaction execution.

---

## Storage Engine Comparison: MyISAM vs. InnoDB

```
+-----------------------------------------------------------------+
|                         MySQL SERVER ENGINE                     |
+-----------------------------------------------------------------+
|   InnoDB (Modern Default)       |    MyISAM (Legacy Storage)    |
|   - Row-Level Locking           |    - Table-Level Locking      |
|   - ACID Compliant (Transactions)|   - Non-Transactional        |
|   - Foreign Key Constraints     |    - No Foreign Key Support   |
+-----------------------------------------------------------------+

```

| Feature | MyISAM (Legacy Engine) | InnoDB (Modern Default Engine) |
| --- | --- | --- |
| **Default Version** | Default prior to MySQL 5.5 | Default in MySQL 5.5+ |
| **ACID Compliance** | No | Yes |
| **Transactions** | No support (`COMMIT`/`ROLLBACK` unavailable) | Full transactional support |
| **Locking Level** | Table-level locking | Row-level locking |
| **Foreign Keys** | Not supported | Fully supported |
| **Crash Recovery** | Manual repair tools (`myisamchk`) | Automatic crash recovery logs |

### Other MySQL Storage Engines

* **MEMORY:** Keeps all data in RAM for rapid read/write operations; data disappears upon server restart.
* **CSV:** Stores data directly in plain text comma-separated text files.
* **FEDERATED:** Interconnects local proxy tables to remote database instances without local storage.
* **MERGE:** Combines multiple identical `MyISAM` tables into a single logical entity.
* **ARCHIVE:** Compresses historical records using `zlib`; optimized for high-volume logs with no indexing support.
* **BLACKHOLE:** Discards all written data while recording log entries; used in replication and performance testing.

---

## Explain the use of FEDERATED tables in MySQL?

`FEDERATED` tables enable access to remote MySQL database tables without copying physical files locally.

```sql
CREATE TABLE fed_user (
  userid INT PRIMARY KEY,
  name VARCHAR(50)
) ENGINE=FEDERATED
CONNECTION='mysql://user:password@remote_host:3306/federated_db/remote_table';
```

---

## Explain the lock in the MySQL server?

Locks maintain transaction isolation and control concurrent access to shared resources.

> **Note:** Detailed lock mode names like **Bulk Update (BU)** and **Schema Locks (Sch-M/Sch-S)** below originate from **SQL Server**, though equivalent underlying concepts exist in MySQL InnoDB (e.g., Metadata Locks, Intention Locks).

* **Shared (S) Lock:** Held for read-only operations. Multiple transactions can hold shared locks simultaneously on the same record.
* **Update (U) Lock:** Placed on resources being evaluated for modification to prevent conversion deadlocks prior to acquiring an Exclusive Lock.
* **Exclusive (X) Lock:** Imposed during data modifications (`INSERT`, `UPDATE`, `DELETE`) to ensure only one transaction modifies the resource at a time.
* **Bulk Update (BU) Lock:** Used during bulk-copy operations when table lock hints are set.
* **Intent Locks (IS / IX / SIX):** Signal intent to acquire lock states further down a table hierarchy to prevent parent table locks from conflicting with granular child row locks.
* **Key-Range Locks:** Applied in `SERIALIZABLE` isolation levels to block phantom reads within range queries.
* **Schema (Sch) Lock:** Prevents structural DDL updates (`Sch-M`) while concurrent data access is active, or permits queries while holding structural stability locks (`Sch-S`).

---

## What is the difference between a deadlock and a Livelock?

* **Deadlock:** Occurs when two or more transactions hold locks while waiting to acquire locks held by each other, bringing all involved processes to a complete standstill.
* **Livelock:** Occurs when two or more processes continuously change their execution states in response to each other without making operational progress.

---

## What is autocommit in MySQL?

* **Autocommit Enabled (`autocommit = 1`):** Every individual SQL statement runs inside its own implicit transaction and commits instantly upon execution.
* **Autocommit Disabled (`autocommit = 0`):** Changes remain temporary until an explicit `COMMIT` command is issued.
* **Transaction Controls:** You can start an explicit transaction regardless of autocommit state using `START TRANSACTION;` and complete it using `COMMIT;` or `ROLLBACK;`.

---

## What is the use of SAVEPOINT in MySQL?

`SAVEPOINT` sets a checkpoint within an active transaction, allowing partial rollbacks to that marker without cancelling the entire transaction.

```sql
START TRANSACTION;
UPDATE Accounts SET balance = balance - 100 WHERE id = 1;

SAVEPOINT point1;

UPDATE Accounts SET balance = balance + 100 WHERE id = 2;
-- Roll back only to point1 if the second update encounters issues:
ROLLBACK TO point1;

COMMIT;
```

---

## Explain the CASCADE and RESTRICT keywords?

Used with foreign key constraints to define automatic actions when parent rows undergo modification:

* **`CASCADE`:** Updating or deleting a row in the parent table automatically updates or deletes matching rows in the child table.
* **`RESTRICT`:** Blocks updates or deletions of a parent row if dependent child records exist.

---

# Stored Programs, Triggers & Scheduling

## What different stored objects are supported in MySQL?

* **VIEW:** A virtual table based on an underlying `SELECT` statement.
* **STORED PROCEDURE:** A routine containing business logic invoked using the `CALL` statement. It can process input/output parameters and return multiple datasets.
* **STORED FUNCTION:** A routine that performs calculations and returns a **single scalar value**. It can be called inside standard SQL queries (`SELECT`, `WHERE`).
* **TRIGGER:** SQL code executed automatically before or after an `INSERT`, `UPDATE`, or `DELETE` event on a specified table.
* **EVENT:** A task executed automatically according to a predefined time schedule.

---

## What are MySQL Triggers?

Triggers execute automatically in response to DML operations (`BEFORE`/`AFTER`  `INSERT`, `UPDATE`, `DELETE`) on an associated base table.

### Trigger Types

* **Row-Level Trigger:** Executes once for **every individual row** modified by a query.
* **Statement-Level Trigger:** *(Standard SQL feature)* Executes **once per statement**, regardless of how many rows are affected. *(Note: MySQL supports row-level triggers natively).*


### Example:

```sql
CREATE TABLE audit (msg VARCHAR(100));

DELIMITER //
CREATE TRIGGER trg_after_emp_insert
AFTER INSERT ON emp
FOR EACH ROW
BEGIN
  INSERT INTO audit VALUES (CONCAT('Added employee ID: ', NEW.id));
END //
DELIMITER ;
```

---

## How many tables can a trigger associate to in MySQL?

A trigger can be associated with only **one base table**. Triggers cannot be directly associated with temporary tables or virtual views.

---

## What is the use of DELIMITER command in MySQL?

The default client SQL statement terminator is `;`. When creating stored procedures or triggers that contain multiple internal statements, the default delimiter must be temporarily redefined to prevent premature execution.

```sql
DELIMITER //

CREATE PROCEDURE GetUsers()
BEGIN
  SELECT * FROM User;
END //

DELIMITER ;
```

---

## What is MySQL Event Scheduler, and how do you create a scheduled event?

The Event Scheduler manages background execution of scheduled tasks directly within MySQL.

### Setup & Management Commands

```sql
-- Enable the Event Scheduler globally
SET GLOBAL event_scheduler = ON;

-- Verify Event Scheduler Status
SHOW VARIABLES LIKE 'event_scheduler';

-- Inspect Active Events
SHOW EVENTS;

-- Disable or Enable an Event
ALTER EVENT clean_logs DISABLE;
ALTER EVENT clean_logs ENABLE;

-- Update Execution Schedule
ALTER EVENT clean_logs ON SCHEDULE EVERY 1 DAY;

-- Remove Event
DROP EVENT IF EXISTS clean_logs;
```

### Event Definition Example

```sql
CREATE EVENT clean_logs
ON SCHEDULE EVERY 10 SECOND
DO
  -- you can call procedure or direct statement
  CALL RemovelogsProcedure(); / DELETE FROM audit WHERE msg IS NOT NULL;
```

---

# Performance Optimization & Monitoring

## What are the major Optimization Considerations?

* **Use `EXPLAIN` Statements:** Inspect access types, key usage, joint operations, and scanned row counts.
* **Strategic Use of Temporary Tables:** Useful for breaking complex multi-step queries into isolated stages.
* **Denormalization:** Selectively duplicating data elements across tables to remove expensive `JOIN` operations in read-heavy workloads.
* **Hardware Resources:** Increasing RAM, CPU cores, and storage speed (NVMe SSDs) directly improves baseline throughput.

---

## Query Caching (Legacy Marker)

> **Legacy Marker:** The built-in Query Cache was deprecated in MySQL 5.7.20 and **completely removed in MySQL 8.0** due to scalability bottlenecks on multi-core hardware. Production applications now use external caching systems such as Redis or Memcached.

The legacy Query Cache stored raw query strings and their output datasets directly in memory to serve subsequent identical queries instantly.

---

## What is profiling in MySQL and how do you use it?

Profiling measures resource consumption (CPU time, I/O waits) for executed queries.

### Program Setup:

```sql
-- Enable session profiling
SET profiling = 1;

-- Run target query
SELECT * FROM User WHERE userid = '11112227';

-- Inspect query execution metrics
SHOW PROFILES;
SHOW PROFILE FOR QUERY 1;
```

---

## How do you use the Performance Schema in MySQL?

The **Performance Schema** monitors low-level server operations, memory allocation, and query execution timing.

```sql
-- Query event waits from Performance Schema
SELECT * FROM performance_schema.events_statements_summary_by_digest 
ORDER BY SUM_TIMER_WAIT DESC LIMIT 5;
```

---

## What is the MySQL slow query log, and how do you use it?

The slow query log captures queries whose execution time exceeds the defined threshold (`long_query_time`).

### Configuration Setup (`my.cnf` or `my.ini`):

```ini
[mysqld]
slow_query_log = ON
slow_query_log_file = /var/log/mysql/slow-query.log
long_query_time = 2
```

---

## What does OPTIMIZE TABLE command do in MySQL?

`OPTIMIZE TABLE` defragmentation reclaims unused disk space and rebuilds index pages after heavy `DELETE` or `UPDATE` operations on `InnoDB` or `MyISAM` tables.

```sql
OPTIMIZE TABLE my_table;
```

---

# High Availability, Replication & Scaling

## What is Database Replication?

Replication copies data from a primary source server to one or more replica servers to support load distribution, read scaling, offline backups, and disaster recovery.

---

## Replication Modes: Synchronous, Asynchronous, and Semi-Synchronous

* **Synchronous Replication:** The primary node writes data locally and waits for all replicas to commit the transaction before returning success to the application client.
* *Data Consistency:* Extremely Strong | *Latency:* Higher
* **Asynchronous Replication:** The primary node commits transactions locally and writes to replication logs without waiting for replicas to confirm receipt.
* *Data Consistency:* Moderate (risk of failover data loss) | *Latency:* Lowest
* **Semi-Synchronous Replication:** The primary node waits for at least **one** replica to receive and acknowledge the transaction log entry before completing the commit operation.
* *Data Consistency:* Balanced | *Latency:* Moderate

---

## Requirements for Setting Up MySQL Replication

* **Matching Server Versions:** Maintain consistent version levels between primary and replica instances.
* **Network Connectivity:** Uninhibited network communication over port 3306 (or custom configured ports).
* **User Privileges:** Replication accounts require explicit privileges (`REPLICATION SLAVE` on source, `REPLICATION CLIENT` on replica).
* **Binary Logging:** Enabled binary log settings (`log_bin`) on the source instance.
* **Unique Server IDs:** Unique `server_id` integer settings assigned within configuration files for each instance.
* **Baseline Data Snapshot:** A point-in-time database backup (via `mysqldump` or enterprise utilities) to seed the replica.
* **Disk Capacity:** Adequate disk storage capacity for binary logs and replicated data files.

---

## Master-Slave vs. Master-Master Replication

* **Master-Slave (Source-Replica):** Unidirectional flow where a single primary node processes all writes, and one or more replica nodes sync data to handle read workloads.
* **Master-Master (Dual Source):** Bidirectional flow where multiple instances handle write operations simultaneously and sync updates to each other. Requires conflict resolution strategies for primary key auto-increments and simultaneous row updates.

---

## What is Sharding in MySQL?

Sharding horizontally partitions massive datasets across separate database servers based on a designated **shard key**.

```
                [ App / Proxy Layer ]
                         |
       +-----------------+-----------------+
       |                                   |
[ Shard 1: User 1-1000 ]          [ Shard 2: User 1001-2000 ]

```

### Common Sharding Architectures

* **Key/Hash-Based Sharding:** Uses a hash function applied to the shard key to assign records evenly across shards.
* **Range-Based Sharding:** Assigns records to specific servers according to defined value ranges (e.g., ID ranges or date windows).
* **Directory-Based Sharding:** Maintains a central lookup table mapping shard key values to their designated server nodes.

### Horizontal vs. Vertical Sharding

* **Horizontal Sharding:** Splits rows of a single table across multiple server nodes.
* **Vertical Sharding:** Splits distinct columns or functional tables into separate databases.

---

## What is MySQL Clustering?

MySQL Cluster (NDB Cluster) is an auto-sharded, high-availability architecture utilizing the **NDB** storage engine.

### Core Components

* **Data Nodes (NDB):** Store partitioned, in-memory copies of data across independent nodes.
* **SQL Nodes (MySQL Server):** Conventional MySQL instances that route SQL queries to data nodes.
* **Management Nodes (NDB_MGMD):** Handle cluster configurations, heartbeat monitoring, and node memberships.

---

## What is a MySQL Proxy?

A MySQL Proxy acts as an intermediate middleware layer positioned between application clients and database instances, providing load balancing, failover handling, query routing, and connection pooling.

---

## List different ways to perform MySQL backup

* **`mysqldump`:** Logical command-line utility exporting SQL DDL/DML scripts.
```bash
mysqldump -u username -p database_name > backup.sql
```
* **`mysqlhotcopy`:** *(Legacy utility)* Rapid snapshot backup designed specifically for `MyISAM` storage files.
* **MySQL Enterprise Backup:** Commercial tool performing online hot backups for `InnoDB` engines.
* **MySQL Workbench:** GUI-driven logical data exporter tool.
* **File System/Physical Snapshots:** Copying database data directory files directly while locking tables or stopping the database service.
* **Replication/Cloning:** Maintaining live replication replicas dedicated to backup tasks.
* **Third-Party Tools:** Utility solutions such as Percona XtraBackup.

---

## What is MySQL Scaling?

Scaling strategies expand operational capacity to accommodate growing data volumes and traffic demands:

1. **Vertical Scaling (Scale Up):** Upgrading hardware components (RAM, CPU, disk speeds) on existing server nodes.
2. **Horizontal Scaling (Scale Out):** Adding additional server instances using replication or multi-node clusters.
3. **Sharding:** Distributing rows or tables across multiple distinct database instances.
4. **Query Optimization:** Refining indexes and tuning slow SQL queries to reduce resource consumption.
5. **Caching:** Offloading frequent read queries to memory-based caches like Redis or Memcached.
6. **Partitioning:** Structuring internal tables into logical physical subsets.

---

## Partitioning Mechanics

Partitioning divides large single tables into separate physical files on disk while presenting them as a single logical entity to SQL queries.

### Supported Partition Types

* **Range Partitioning:** Assigns rows based on defined value ranges.
* **List Partitioning:** Assigns rows based on explicit enumerated value sets.
* **Hash Partitioning:** Distributes rows using a user-defined mathematical expression or column hash.
* **Key Partitioning:** Uses MySQL's internal hashing algorithms (such as MD5) on key columns.

```sql
-- Partitioning Example
CREATE TABLE Orders (
  order_id INT NOT NULL,
  order_date DATE NOT NULL
)
PARTITION BY RANGE (YEAR(order_date)) (
  PARTITION p2024 VALUES LESS THAN (2025),
  PARTITION p2025 VALUES LESS THAN (2026),
  PARTITION p2026 VALUES LESS THAN MAXVALUE
);
```

---

## Architectural Database Comparisons

### Compare MySQL and PostgreSQL

* **Database Model:** MySQL is a relational database management system (RDBMS). PostgreSQL is an Object-Relational Database Management System (ORDBMS) supporting table inheritance and function overloading.
* **ACID & Reliability:** PostgreSQL is natively ACID-compliant across all configurations. MySQL requires the `InnoDB` storage engine for full ACID compliance.
* **Complex Queries & Performance:** MySQL delivers fast execution for simple read workloads. PostgreSQL handles complex analytical queries, concurrent writes, and heavy CTE/windowing expressions efficiently.
* **Data Types & NoSQL Support:** PostgreSQL provides comprehensive native support for JSONB, indexing, geospatial data (PostGIS), XML, arrays, and custom types. MySQL offers standard JSON data types and spatial extensions.
* **Indexing:** PostgreSQL supports B-tree, Hash, GiST, GIN, BRIN, and SP-GiST indexes. MySQL supports B-tree, R-tree (spatial), and full-text indexes.

---

### Comparison between MySQL and Oracle Database

* **Licensing Model:** MySQL is open-source (GPL) with commercial options. Oracle Database is a proprietary enterprise product (with a restricted free Express Edition).
* **Scalability & Scope:** Oracle is engineered for high-concurrency enterprise data systems. MySQL is widely deployed across web applications and medium-to-large business platforms.
* **Procedural Languages:** Oracle uses `PL/SQL`. MySQL uses standard procedural SQL routines.
* **Security & User Management:** Oracle offers granular role-based security policies, profiles, and auditing. MySQL uses host-authenticated user accounts (`user@host`) and standard database grant tables.
* **Backup & Recovery Capabilities:** Oracle integrates advanced utilities such as RMAN (Recovery Manager) and Data Pump. MySQL uses `mysqldump`, enterprise utilities, or tools like Percona XtraBackup.

---

## Understanding Table and Index Scans in MySQL

MySQL accesses rows through two primary retrieval methods evaluated by the Query Optimizer based on cost:

* **Table Scan (`type: ALL`):** MySQL reads every page of the clustered index B+Tree sequentially from start to finish. Used when no indexes apply, when functions wrap indexed columns, or when matching large portions of a table makes sequential I/O cheaper.

* **Index Range Scan (`type: range`):** Navigates down a B+Tree index to a starting point and scans horizontally along leaf nodes to retrieve a range of values (e.g., `BETWEEN`, `>`, `<`), followed by primary key lookups if non-indexed columns are needed.
  * **Example:** `SELECT * FROM orders WHERE order_date BETWEEN '2026-01-01' AND '2026-01-31';`

* **Full Index Scan (`type: index`):** Scans the entire B+Tree of a secondary index. Faster than a full table scan because secondary index pages are smaller than full-row data pages.
  * **Example:** `SELECT status FROM users;` (where `status` is indexed).

* **Covering Index Scan (`Using index`):** Resolves the query entirely within the secondary index without fetching full row pages from the clustered index (eliminating bookmark lookups).

---

## Difference Between `EXPLAIN` and `EXPLAIN ANALYZE`

* **`EXPLAIN`:** Generates a static estimate of the execution plan constructed by the Query Optimizer **without running the query**. Returns a tabular layout showing index choices, scan types, and estimated row counts. Safe for production checks on DML statements.
* **`EXPLAIN ANALYZE`:** Introduced in MySQL 8.0.18, this command **actually executes the query** and outputs a tree-structured plan detailing real-time execution statistics alongside the optimizer's original estimates.

### Example:

```sql
-- Standard EXPLAIN (Dry Run)
EXPLAIN SELECT * FROM users WHERE status = 'active' ORDER BY created_at DESC LIMIT 10;

-- EXPLAIN ANALYZE (Actual Execution Tree)
EXPLAIN ANALYZE SELECT * FROM users WHERE status = 'active' ORDER BY created_at DESC LIMIT 10;
```

### `EXPLAIN ANALYZE` Output Tree:

```text
-> Limit: 10 row(s)  (cost=5234.50 rows=10) (actual time=12.450..12.453 rows=10 loops=1)
    -> Sort: users.created_at DESC, limit input to 10 row(s)  (cost=5234.50 rows=50120) (actual time=12.449..12.451 rows=10 loops=1)
        -> Index lookup on users using idx_status (status='active')  (cost=2120.10 rows=50120) (actual time=0.082..8.310 rows=48950 loops=1)

```

### Key Columns to Identify

* **`type`**: Indicates the access method (`ALL` = Table Scan, `range` = Index Range Scan, `index` = Full Index Scan, `ref` = Non-unique index lookup).
* **`possible_keys`**: Indexes MySQL considered using.
* **`key`**: The actual index chosen by the optimizer.
* **`rows`**: Estimated number of records MySQL needs to examine to produce the result.
* **`Extra`**: Look for `Using index` (Covering Index) vs. `Using where` (Post-filtering after retrieval).

---

## MySQL Locks: Types, Usage, Scenarios, and `mysqldump`

**Q: How do locks work in MySQL, what are the different types, how are they used in real-time scenarios, and what is the role of `LOCK TABLES` in `mysqldump`?**

**A:**
Locks coordinate simultaneous data access across sessions to maintain transaction isolation and prevent dirty reads or data corruption.

### Core Lock Types:

* **Shared (S) / Exclusive (X):** Read locks ($S$) allow concurrent readers; write locks ($X$) block all other access.
* **Intention Locks (IS / IX):** Set at the table level to signal pending row-level locks.
* **InnoDB Index Locks:** Record Locks target specific index entries; Gap Locks target empty spaces between keys; Next-Key Locks combine both to prevent phantom reads.

### Real-Time Scenario (Preventing Overbooking):

Using `SELECT ... FOR UPDATE` acquires exclusive row locks to prevent race conditions during concurrent inventory decrements:

```sql
START TRANSACTION;
-- Locks product 101 exclusively; concurrent reads with FOR UPDATE will wait
SELECT stock FROM inventory WHERE product_id = 101 FOR UPDATE;
UPDATE inventory SET stock = stock - 1 WHERE product_id = 101;
COMMIT;
```

### `LOCK TABLES` in `mysqldump`:

* `mysqldump` uses `LOCK TABLES` by default to issue a global read lock, ensuring snapshot consistency for non-transactional engines (like MyISAM) while blocking incoming application writes.
* **Modern Practice:** For transactional InnoDB tables, use `--single-transaction` instead. It creates a point-in-time snapshot using MVCC without holding table locks or blocking write operations:

```bash
mysqldump --single-transaction -u root -p my_database > backup.sql
```

---

## Updating Large Datasets in the Backend

Executing a single monolithic `UPDATE` statement causes lock contention, undo/redo log bloat, and replication lag. The solution is **dataset chunking** by primary key ranges.

### Backend Batching Pattern (SQL / TypeScript):

```sql
-- SQL Iteration Loop
WHILE @current_id < @max_id DO
    UPDATE large_table 
    SET status = 'PROCESSED', updated_at = NOW()
    WHERE id > @current_id AND id <= (@current_id + 5000) AND status = 'PENDING';
      
    SET @current_id = @current_id + 5000;
    DO SLEEP(0.05); -- Pause to give priority to live traffic
END WHILE;
```

### Strategic Options:

1. **Temporary Table Joins:** Load calculated updates into a temporary table and execute an indexed join update.
2. **Chunked Workers:** Use application-level workers (Node.js/Python/Go) to update records in indexed batches of 1,000–5,000 records.
3. **Queue Pipelines:** Publish chunked ID batches into message brokers (RabbitMQ/Kafka) for asynchronous worker consumption.

---

# Senior Developer Interview Cheat Sheet & Architecture Summary

### Core Architecture & Execution Flow

* **InnoDB Internals:** Uses a **Buffer Pool** for caching data and indexes, a **Redo Log** for crash recovery (WAL protocol), **Undo Logs** for MVCC and rollbacks, and a **Doublewrite Buffer** to prevent partial page writes.
* **Query Processing:** `FROM` → `WHERE` → `GROUP BY` → `HAVING` → `SELECT` → `ORDER BY` → `LIMIT`.

### Indexing & Performance Rules

* **Indexing Engine:** Primary keys use clustered indexes (data stored directly in B+Tree leaf nodes). Secondary indexes store primary key values as row pointers.
* **Leftmost Prefix Rule:** Composite indexes `(A, B, C)` optimize queries filtering on `(A)`, `(A, B)`, or `(A, B, C)`, but skip optimization for filters relying solely on `(B)` or `(C)`.
* **Optimization Quick Hits:** Cover queries using indexes to avoid secondary lookup steps (`Using index`); avoid functions on indexed columns in `WHERE` clauses (which cause full table scans); use `EXPLAIN FORMAT=JSON` to inspect actual join costs.

### High-Value Architectural Differences (MySQL 5.7 vs 8.0)

* **Features Introduced in MySQL 8.0:** Common Table Expressions (`WITH`), Window Functions (`OVER`), Invisible Indexes, Descending Indexes, Native Roles, and the `utf8mb4_0900_ai_ci` default collation.
* **Features Removed in MySQL 8.0:** Query Cache, MyISAM system tables (migrated to transactional InnoDB data dictionary), and legacy spatial syntax. `caching_sha2_password` replaced `mysql_native_password` as the default authentication plugin.