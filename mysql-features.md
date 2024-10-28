## What is Stored Function & Stored procedure in MySQL?

A `Stored procedure` does not return a value. Instead, it is invoked with a CALL statement to perform an operation such as modifying a table or processing retrieved records.
A `Stored function` is invoked within an expression and returns a single value directly to the caller to be used in the expression.

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

-- Example

DELIMITER $$  
CREATE FUNCTION Customer_Occupation(  
    age int  
)   
RETURNS VARCHAR(20)  
DETERMINISTIC  
BEGIN  
    DECLARE customer_occupation VARCHAR(20);  
    IF age > 35 THEN  
        SET customer_occupation = 'Scientist';  
    ELSE IF (age <= 35 AND   
            age >= 30) THEN  
        SET customer_occupation = 'Engineer';  
    ELSE IF age < 30 THEN  
        SET customer_occupation = 'Actor';  
    END IF;
    RETURN (customer_occupation);
END$$  
DELIMITER;

-- Stored Function Call
SELECT name, age, Customer_Occupation(age)  
FROM customer ORDER BY age;  

DELIMITER $$  
CREATE PROCEDURE GetCustomerDetail()  
BEGIN  
    SELECT name, age, Customer_Occupation(age) FROM customer ORDER BY age;  
END$$  
DELIMITER ;  

-- Stored Function Call in Procedure
CALL GetCustomerDetail(); 
```

## What is cursor used in MySQL?

Cursor is used in stored programs like stored procedure and stored functions. Cursor is a type of loop which can iterate through result of a SELECT query inside stored programs. Here are some properties of cursor in MySQL.

- `Read-only` - In MySQL, cursor is read only which means data of table cannot be updated using cursor.
- `Non-scrollable` - It is non-scrollable which means one can traverse in forward direction with one entry at a time without skipping entries or moving in reverse direction.
- `Asensitive` - It is asensitive which means cursor will not make temporary copy of the data rather use original data. One issue with cursor being asensitive is that if some other process changes the data while cursor is using it, result might not be predictable.

```sql
CREATE PROCEDURE curdemo()
BEGIN
  DECLARE done INT DEFAULT FALSE;
  DECLARE a CHAR(16);
  DECLARE b, c INT;
  DECLARE cur1 CURSOR FOR SELECT id,data FROM test.t1;
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
      INSERT INTO test.t3 VALUES (a,b);
    ELSE
      INSERT INTO test.t3 VALUES (a,c);
    END IF;
  END LOOP;

  CLOSE cur1;
  CLOSE cur2;
END;
```

## What is Full-Text Search ?

`Full-Text Search` is a method of searching and matching text in a database that goes beyond simple pattern matching using SQL’s LIKE operator. It enables you to search for words or phrases within the text, consider relevance, and return results based on their significance.

Key Benefits of Full-Text Search in MySQL:

- `Faster Search:` Full-Text Search is optimized for large text fields, providing faster and more efficient search results compared to regular SQL queries with LIKE.
- `Relevance Ranking:` MySQL’s Full-Text Search assigns relevance scores to match results, allowing you to prioritize the most relevant entries.
- `Phrase and Word Matching:` You can search for complete phrases, individual words, or a combination of both, making it versatile for various use cases.
- `Stopwords Handling:` Full-Text Search can ignore common words (stopwords) like "the" or "and" to focus on more meaningful keywords.
- `Boolean Operators:` You can use Boolean operators (AND, OR, NOT) to fine-tune your search queries.

```sql
-- WAY 1
CREATE FULLTEXT INDEX full_text_idx ON articles(title, author, content);

-- WAY 2
ALTER TABLE articles ADD FULLTEXT INDEX full_text_idx (title, author, content);

-- check index
SHOW INDEX FROM articles;

-- full-text search
SELECT * FROM articles WHERE MATCH(title, author, content) AGAINST('john guide ubuntu');

-- natural language mode with relevance score
SELECT MATCH(title, author, content) AGAINST('john guide ubuntu' IN NATURAL LANGUAGE MODE) as score, articles.* 
FROM articles
WHERE MATCH(title, author, content) AGAINST('john guide ubuntu' IN NATURAL LANGUAGE MODE)
ORDER BY score DESC;

-- boolean mode
SELECT * FROM articles
WHERE MATCH(title, author, content) AGAINST('+learn -basic' IN BOOLEAN MODE);

-- query mode
SELECT * FROM articles
WHERE MATCH(title, author, content) AGAINST('performance tuning' WITH QUERY EXPANSION);
```

- `Boolean mode :` Allows the use of boolean operators (+, -, *, etc.) and wildcard characters to create detailed search combinations.
- `Natural Language mode:` By default, MySQL uses natural language search mode, which provides a relevance score for each match. The relevance score is a metric used to assess the quality of the match between the search results and the keywords. A score of 0 signifies complete irrelevance. 
- `Query Expansion mode:` This search mode extends search results by including related words (automatically) in the search query. 

## Partitioning Implementation

`Range Partitioning`
Imagine a table of log data that grows rapidly and often needs to be queried for specific periods.

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

`List Partitioning`
```sql
CREATE TABLE orders (
    order_id INT,
    order_date DATE,
    status VARCHAR(50),
    amount DECIMAL(10,2),
    PRIMARY KEY (order_id, order_date)
)
PARTITION BY LIST (status) (
    PARTITION pPending VALUES IN ('Pending'),
    PARTITION pShipped VALUES IN ('Shipped'),
    PARTITION pDelivered VALUES IN ('Delivered'),
    PARTITION pCancelled VALUES IN ('Cancelled')
);
```

`Hash Partitioning`
```sql
CREATE TABLE customers (
    customer_id INT,
    customer_name VARCHAR(100),
    signup_date DATE,
    PRIMARY KEY (customer_id)
)
PARTITION BY HASH(customer_id) PARTITIONS 4;
```

`Key Partitioning`
```sql
CREATE TABLE customers (
    customer_id INT,
    customer_name VARCHAR(100),
    signup_date DATE,
    PRIMARY KEY (customer_id)
)
PARTITION BY KEY(customer_id) PARTITIONS 4;
```

### Indexing Strategy for partition
Using indexing plays a crucial role in optimizing queries on partitioned tables. You can create indexes on partitioned tables just like on regular tables

`Local Indexes:` These are created separately for each partition. They are helpful when queries are likely to target specific partitions.
```sql
CREATE TABLE sales (
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
```

`Global Indexes:` These span all partitions and can be more complex to manage but are necessary for specific queries involving multiple partitions.
```sql
CREATE TABLE sales (
    id INT,
    sale_date DATE,
    amount DECIMAL(10,2),
    PRIMARY KEY (id, sale_date),
    INDEX idx_global_amount (amount)
)
PARTITION BY RANGE (YEAR(sale_date)) (
    PARTITION p2021 VALUES LESS THAN (2022),
    PARTITION p2022 VALUES LESS THAN (2023),
    PARTITION p2023 VALUES LESS THAN (2024)
);
```

check partition 

```sql
SELECT
  TABLE_NAME,
  PARTITION_NAME,
  SUBPARTITION_NAME,
  PARTITION_ORDINAL_POSITION,
  SUBPARTITION_ORDINAL_POSITION,
  TABLE_ROWS,
  DATA_LENGTH,
  INDEX_LENGTH
FROM
  INFORMATION_SCHEMA.PARTITIONS
WHERE
  TABLE_NAME = 'sales'
  AND TABLE_SCHEMA = 'test_db'; 
```