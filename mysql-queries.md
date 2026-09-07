# MySQL Technical Guide & Advanced Query Cookbook

## MySQL Data Manipulation, Utilities & Bulk Operations

### Creating a Table from a SELECT Query

Creates a new table structure based on the output schema of a `SELECT` statement and instantly populates it with the target dataset.

#### Example:

```sql
CREATE TABLE TempCustomer AS
SELECT first_name, last_name
FROM ExternalCustomer AS c
WHERE c.create_date >= '2015-08-01';
```

**Output:** `Query OK, 50 rows affected (0.03 sec)`

---

### Inserting Data Using Queries

Inserts static row entries or copies datasets dynamically across tables using subqueries.

#### Example:

```sql
-- Single row insert
INSERT INTO Customer (first_name, last_name) VALUES ('John', 'Smith');

-- Dynamic bulk insert from query
INSERT INTO Customer (first_name, last_name, address) 
SELECT first_name, last_name, address FROM TempCustomer;
```

**Output:** `Query OK, 51 rows affected (0.02 sec)`

---

### Updating Tables from a SELECT Query

Updates target table records dynamically using either multi-table `JOIN` operations or correlated subqueries.

#### Legacy vs Modern Implementations:

```sql
-- MULTI-TABLE JOIN UPDATE (Preferred for performance)
UPDATE Customer AS c
LEFT JOIN TempCustomer AS tc ON c.customer_id = tc.customer_id
SET c.first_name = tc.first_name, 
    c.last_name = tc.last_name, 
    c.address = tc.address
WHERE c.customer_id > 5;

-- CORRELATED SUBQUERY UPDATE (Legacy / Standard SQL)
UPDATE Customer
SET first_name = (
    SELECT TempCustomer.first_name 
    FROM TempCustomer 
    WHERE TempCustomer.customer_id = Customer.customer_id
);
```

**Output:** `Query OK, 12 rows affected (0.01 sec)`

---

### Deleting Data with Constraints (Foreign Keys & Cascading)

Enforces relational integrity by automatically removing child records when a parent record is deleted.

#### Architecture & Workflow:

```
[ Parent Table: Customer (customer_id) ]
                |
     (DELETE parent record)
                |
                v
  [ Foreign Key Constraint: ON DELETE CASCADE ]
                |
                v
[ Child Table: Order (customer_id) automatically purged ]
```

#### Example:

```sql
ALTER TABLE `Order`
ADD CONSTRAINT fk_order_customer
FOREIGN KEY (customer_id)
REFERENCES Customer (customer_id)
ON DELETE CASCADE;
```

**Output:** `Query OK, 0 rows affected (0.04 sec)`

---

### Importing Data from a CSV File (`LOAD DATA INFILE`)

High-performance bulk ingestion engine for streaming structured flat files directly into MySQL tables.

#### Example:

```sql
LOAD DATA INFILE '/var/lib/mysql-files/customer.csv'
INTO TABLE Customer
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS
(customer_id, @firstname, last_name, address)
SET first_name = @firstname;
```

**Output:** `Query OK, 1000 rows affected (0.08 sec)`

---

### Exporting Data to a CSV File (`INTO OUTFILE`)

Dumps active server datasets directly into delimited text files on the host file system.

#### Example:

```sql
SELECT customer_id, first_name, last_name
FROM Customer
WHERE create_date >= '2015-08-01'
INTO OUTFILE '/var/lib/mysql-files/customers.csv'
FIELDS ENCLOSED BY '"' ESCAPED BY '"'
LINES TERMINATED BY ';';

```

**Output:** `Query OK, 250 rows fetched (0.01 sec)`

---

# Advanced Analytics & Window Function Queries

## Find Top N Salaries per Department

Ranks employees per department based on salary metrics.

### Example:

```sql
WITH RankedEmployees AS (
    SELECT 
        employee_id, 
        department_id, 
        salary,
        ROW_NUMBER() OVER (PARTITION BY department_id ORDER BY salary DESC) AS `rank`
    FROM employees
)
SELECT employee_id, department_id, salary 
FROM RankedEmployees 
WHERE `rank` <= 5 
ORDER BY department_id, `rank`;

```

**Output Sample:**

| employee_id | department_id | salary | rank |
| --- | --- | --- | --- |
| `102` | `10` | `15000.00` | `1` |
| `108` | `10` | `12000.00` | `2` |
| `201` | `20` | `18000.00` | `1` |

---

## Find Nth Highest Value Without `TOP` or `LIMIT`

Calculates positional values using self-correlated scalar count subqueries.

### Example (Finding 3rd Highest Donation, N=3):

```sql
SELECT F1.Donation 
FROM StudentDonationTable F1 
WHERE 3 - 1 = ( 
    SELECT COUNT(DISTINCT F2.Donation) 
    FROM StudentDonationTable F2 
    WHERE F2.Donation > F1.Donation 
);

```

**Output:**

| Donation |
| --- |
| `7500.00` |

---

## Fetch Even or Odd Rows from a Table

### Legacy vs Modern Implementations:

```sql
-- LEGACY / SESSION VARIABLE APPROACH (MySQL 5.7 & Below)
-- Fetch Even Rows
SELECT * FROM (
    SELECT *, @rowNumber := @rowNumber + 1 AS rn 
    FROM users JOIN (SELECT @rowNumber := 0) r
) s WHERE rn % 2 = 0;

-- Fetch Odd Rows
SELECT * FROM (
    SELECT *, @rowNumber := @rowNumber + 1 AS rn 
    FROM users JOIN (SELECT @rowNumber := 0) r
) s WHERE rn % 2 = 1;

-- MODERN WINDOW FUNCTION APPROACH (MySQL 8.0+)
WITH SequencedUsers AS (
    SELECT *, ROW_NUMBER() OVER () AS rn FROM users
)
SELECT * FROM SequencedUsers WHERE rn % 2 = 0; -- Replace 0 with 1 for Odd
```

---

## Period-over-Period Metric & Percentage Change Aggregation
Calculates comparative rolling financial metrics between discrete date intervals using CTE projections.

### Example:
```sql
CREATE TABLE sales (
    product_name VARCHAR(50),
    sales_date DATE,
    revenue DECIMAL(10, 2)
);

INSERT INTO sales (product_name, sales_date, revenue) VALUES 
    ('Product A', '2023-03-01', 1000),
    ('Product A', '2023-03-15', 1500),
    ('Product A', '2023-03-30', 2000), 
    ('Product B', '2023-03-01', 800),
    ('Product B', '2023-03-15', 1200),
    ('Product B', '2023-03-30', 1600),
    ('Product C', '2023-03-01', 500),
    ('Product C', '2023-03-15', 750),
    ('Product C', '2023-03-30', 1000);

WITH sales_last_30_days AS (
    SELECT product_name, SUM(revenue) AS total_revenue
    FROM sales
    WHERE sales_date BETWEEN '2023-03-10' AND '2023-04-09'
    GROUP BY product_name
),
sales_previous_30_days AS (
    SELECT product_name, SUM(revenue) AS total_revenue
    FROM sales
    WHERE sales_date BETWEEN '2023-02-08' AND '2023-03-09'
    GROUP BY product_name
)
SELECT
    s.product_name,
    s.total_revenue,
    ((s.total_revenue - p.total_revenue) / p.total_revenue) * 100 AS revenue_change
FROM sales_last_30_days s
JOIN sales_previous_30_days p ON s.product_name = p.product_name;

```

**Output:**

| product_name | total_revenue | revenue_change |
| --- | --- | --- |
| `Product A` | `3500.00` | `250.0000` |
| `Product B` | `2800.00` | `250.0000` |
| `Product C` | `1750.00` | `250.0000` |

---

# Programmable Objects, Cursors & Advanced Features

## Dynamic Automated Event Execution with Stored Procedures

Combines a `PROCEDURE` with the MySQL `EVENT` engine to run background scheduled tasks.

### Example:

```sql
CREATE DATABASE IF NOT EXISTS `event`;

CREATE TABLE `event`.timestamp_diff (
    id INT AUTO_INCREMENT PRIMARY KEY,
    created_at TIMESTAMP,
    time_difference INT
);

DELIMITER //
CREATE PROCEDURE `event`.insert_timestamp_row()
BEGIN
    DECLARE last_timestamp TIMESTAMP;
    SELECT MAX(created_at) INTO last_timestamp FROM `event`.timestamp_diff;
    INSERT INTO `event`.timestamp_diff (created_at, time_difference)
    VALUES (CURRENT_TIMESTAMP, TIMESTAMPDIFF(SECOND, last_timestamp, CURRENT_TIMESTAMP));
END //
DELIMITER ;

CREATE EVENT insert_timestamp_event
ON SCHEDULE EVERY 10 SECOND
DO
    CALL `event`.insert_timestamp_row();
```

---

## Stored Functions vs. Stored Procedures
- **Stored Procedure:** Invoked via `CALL` statement; handles multi-step operations without forcing a return value.
- **Stored Function:** Invoked inside standard SQL expressions; must declare return types and pass back scalar outputs.

### Syntax:

```sql
CREATE PROCEDURE procedure_name [ ([IN | OUT | INOUT] parameter_name datatype [, parameter datatype]) ]    
BEGIN    
    Declaration_section
    Executable_section
END

CREATE FUNCTION function_name [ (parameter datatype [, parameter datatype]) ]   
RETURNS return_datatype  
[characteristics > DETERMINISTIC | NO SQL | READS SQL DATA]
BEGIN  
  Declaration_section
  Executable_section
END
```

### Example:
```sql
DELIMITER $$  
CREATE FUNCTION Customer_Occupation(age INT)   
RETURNS VARCHAR(20)  
DETERMINISTIC  
BEGIN  
    DECLARE customer_occupation VARCHAR(20);  
    IF age > 35 THEN  
        SET customer_occupation = 'Scientist';  
    ELSEIF (age <= 35 AND age >= 30) THEN  
        SET customer_occupation = 'Engineer';  
    ELSE  
        SET customer_occupation = 'Actor';  
    END IF;
    RETURN (customer_occupation);
END$$  
-- Stored Function Call
CREATE PROCEDURE GetCustomerDetail()  
BEGIN  
    SELECT name, age, Customer_Occupation(age) FROM customer ORDER BY age;  
END$$  
DELIMITER ;

-- Stored Function Call in Procedure
CALL GetCustomerDetail();
```

---

## Cursors in Stored Routines
Iterates row-by-row over query result sets inside procedures.

### Core Properties:
- **Read-only:** Cannot update underlying table data directly through the cursor pointer.
- **Non-scrollable:** Moves forward only, one row at a time.
- **Asensitive:** Operates on direct pointers to live data rather than temporary copies.

### Workflow Architecture:

```

[ DECLARE Cursor ] -> [ OPEN Cursor ] -> [ FETCH Row into Variables ] -> [ LEAVE on HANDLER ] -> [ CLOSE Cursor ]

```

### Example:
```sql
DELIMITER //
CREATE PROCEDURE curdemo()
BEGIN
  DECLARE done INT DEFAULT FALSE;
  DECLARE a CHAR(16);
  DECLARE b, c INT;
  
  DECLARE cur1 CURSOR FOR SELECT id, data FROM test.t1;
  DECLARE cur2 CURSOR FOR SELECT i FROM test.t2;
  DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;

  OPEN cur1;
  OPEN cur2;

  read_loop: LOOP
    FETCH cur1 INTO a, b;
    FETCH cur2 INTO c;
    IF done THEN
      LEAVE read_loop;
    END IF;
    IF b < c THEN
      INSERT INTO test.t3 VALUES (a, b);
    ELSE
      INSERT INTO test.t3 VALUES (a, c);
    END IF;
  END LOOP;

  CLOSE cur1;
  CLOSE cur2;
END //
DELIMITER ;
```

---

## Full-Text Search Engine Integration
Provides relevance-ranked keyword and phrase searching across text-heavy columns.

### Modes of Operation:
- **Natural Language Mode:** Evaluates match relevance scores automatically.
- **Boolean Mode:** Supports fine-grained operators (`+` must match, `-` exclude, `*` wildcard).
- **Query Expansion Mode:** Automatically expands search criteria with related terms.

### Example:
```sql
-- Index Definition
CREATE FULLTEXT INDEX full_text_idx ON articles(title, author, content);

-- Alter Index Definition
ALTER TABLE articles ADD FULLTEXT INDEX full_text_idx (title, author, content);

-- Check Index
SHOW INDEX FROM articles;

-- FULLTEXT Ssearch
SELECT * FROM articles WHERE MATCH(title, author, content) AGAINST('john guide ubuntu');

-- 1. Natural Language Search with Relevance Scores
SELECT MATCH(title, author, content) AGAINST('john guide ubuntu' IN NATURAL LANGUAGE MODE) AS score, articles.* 
FROM articles
WHERE MATCH(title, author, content) AGAINST('john guide ubuntu' IN NATURAL LANGUAGE MODE)
ORDER BY score DESC;

-- 2. Boolean Mode Search (Must include 'learn', exclude 'basic')
SELECT * FROM articles WHERE MATCH(title, author, content) AGAINST('+learn -basic' IN BOOLEAN MODE);

-- 3. Query Expansion Search
SELECT * FROM articles WHERE MATCH(title, author, content) AGAINST('performance tuning' WITH QUERY EXPANSION);
```

---

# Table Partitioning Architectures

Partitions split large physical tables into smaller internal segments while maintaining a single logical table interface.

## 1. Range Partitioning
Segments data based on numeric or date value ranges.

```sql
CREATE TABLE products (
    product_id INT,
    product_name VARCHAR(100),
    price DECIMAL(10,2),
    PRIMARY KEY (product_id)
)
PARTITION BY RANGE (price) (
    PARTITION pLow VALUES LESS THAN (50),
    PARTITION pMedium VALUES LESS THAN (100),
    PARTITION pHigh VALUES LESS THAN (200),
    PARTITION pPremium VALUES LESS THAN MAXVALUE
);
```

---

## 2. List Partitioning
Segments data based on explicit discrete value lists.

```sql
CREATE TABLE orders (
    order_id INT,
    order_date DATE,
    status VARCHAR(50),
    amount DECIMAL(10,2),
    PRIMARY KEY (order_id, order_date)
)
PARTITION BY LIST COLUMNS(status) (
    PARTITION pPending VALUES IN ('Pending'),
    PARTITION pShipped VALUES IN ('Shipped'),
    PARTITION pDelivered VALUES IN ('Delivered'),
    PARTITION pCancelled VALUES IN ('Cancelled')
);
```

---

## 3. Hash Partitioning
Distributes records evenly across partitions using an integer hash function.

```sql
CREATE TABLE customers (
    customer_id INT,
    customer_name VARCHAR(100),
    signup_date DATE,
    PRIMARY KEY (customer_id)
)
PARTITION BY HASH(customer_id) PARTITIONS 4;
```

---

## 4. Key Partitioning
Similar to Hash Partitioning, but uses MySQL's internal hashing algorithms on key columns.

```sql
CREATE TABLE customers_key (
    customer_id INT,
    customer_name VARCHAR(100),
    signup_date DATE,
    PRIMARY KEY (customer_id)
)
PARTITION BY KEY(customer_id) PARTITIONS 4;
```

---

## Partition Indexing Strategy & Metadata Inspection

- **Local Indexes:** Scoped individually to each partition to optimize localized range operations.
- **Partition Verification:** Query `INFORMATION_SCHEMA.PARTITIONS` to inspect row distributions across physical partitions.

### Example:
```sql
CREATE TABLE sales_partitioned (
    id INT,
    sale_date DATE,
    amount DECIMAL(10,2),
    PRIMARY KEY (id, sale_date),
    INDEX idx_amount (amount)
)
PARTITION BY RANGE (YEAR(sale_date)) (
    PARTITION p2021 VALUES LESS THAN (2022),
    PARTITION p2022 VALUES LESS THAN (2023),
    PARTITION p2023 VALUES LESS THAN (2024)
);

-- Metadata Inspection Query
SELECT TABLE_NAME, PARTITION_NAME, TABLE_ROWS, DATA_LENGTH
FROM INFORMATION_SCHEMA.PARTITIONS
WHERE TABLE_NAME = 'sales_partitioned' AND TABLE_SCHEMA = DATABASE();
```

---

# Senior Interview Architecture & Concept Summary

### File Systems & Large Datasets
- **`LOAD DATA INFILE` vs. Row Inserts:** `LOAD DATA INFILE` bypasses standard SQL parsing layers to stream data directly into table pages, delivering significantly faster bulk ingestion speeds.
- **Full-Text Indexing:** Uses inverted indexes to evaluate phrase queries and relevance scores, outperforming `LIKE '%term%'` full table scans on text columns.

### Analytics & Procedural Logic
- **Window Functions vs. Variables:** MySQL 8.0 window functions (`ROW_NUMBER()`, `RANK()`) calculate positional metrics cleanly without requiring session-level user variables (`@rowNumber`).
- **Cursors:** Useful for complex procedural tasks inside stored programs, but operate row-by-row; set-based SQL operations should always be preferred for performance.

### Partitioning & Storage Optimization
- **Partition Pruning:** Allows the query optimizer to scan only relevant physical partitions based on target conditions, drastically reducing disk I/O on massive tables.
- **Partition Key Constraints:** Primary and unique keys on partitioned tables must include every column specified in the partitioning expression.

```