## Creating a table from a SELECT query :
```sql
CREATE TABLE TempCustomer AS
SELECT first_name, last_name
FROM ExternalCustomer AS c
WHERE c.create_date >= '8/1/2015';
```

## Inserting Data Using Queries :
```sql
INSERT INTO Customer (first_name, last_name) VALUES ('John', 'Smith');
INSERT INTO Customer (first_name, last_name, address) SELECT first_name, last_name, address FROM TempCustomer;
```

## Updating Tables from a SELECT :
```sql
UPDATE Customer AS c
LEFT JOIN TempCustomer AS tc ON c.customer_id = tc.customer_id
SET c.first_name = tc.first_name, c.last_name = tc.last_name, c.address = tc.address
WHERE c.customer_id > 5;

UPDATE Customer
SET first_name = (SELECT first_name FROM TempCustomer where TempCustomer.customer_id = Customer.customer_id);
```

## Deleting Data with Constraints :
```sql
ALTER TABLE Order
ADD CONSTRAINT fk_order1
FOREIGN KEY (customer_id)
REFERENCES Customer (customer_id)
ON DELETE CASCADE;
```
The foreign key is on the `customer_id` column located in the `Order` table. The `Order` table is linked to the main `Customer` table on its `customer_id`.

The `ON DELETE CASCADE` statement tells the MySQL database engine to delete any records linked to the record you're deleting.

## Importing Data from a CSV File :
```sql
LOAD DATA INFILE 'path/customer.csv'
INTO TABLE Customer
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;
(customer_id, @firstname, last_name, address)
SET first_name = @firstname;
```
The SET value then tells the MySQL server to use the field name contained in the` @firstname` variable as the value for the first_name column.

## Exporting Data to a CSV:
```sql
SELECT customer_id, first_name, last_name
FROM Customer
WHERE create_date >= ‘8/1/2015'
INTO OUTFILE 'path/customers.csv'
FIELDS ENCLOSED BY '"' ESCAPED BY '"'
LINES TERMINATED BY ';';
```

## Write the MySQL query to find the 5 highest-paid salaries of employees for each department in descending order :

```sql
WITH RankedEmployees AS ( 
    SELECT employee_id, department_id, salary 
    ROW_NUMBER() OVER (PARTITION BY department_id ORDER BY salary DESC) AS rank 
    FROM employees 
)

SELECT employee_id, department_id, salary 
FROM RankedEmployees 
WHERE rank <= 5 
ORDER BY department_id, rank; 
```

## Write a MySQL query to find out the Nth highest donation paid by the student without using TOP and LIMIT Operator :

```sql
SELECT Donation 
FROM StudentDonationTable F1 
WHERE N-1 = ( 
    SELECT COUNT( DISTINCT ( F2.Donation ) ) 
    FROM StudentDonationTable F2 
    WHERE F2.Donation > F1.Donation 
);
```

## Write a MySQL query to find Fetch even rows :

`SELECT * FROM ( SELECT *, @rowNumber := @rowNumber+ 1 rn FROM users JOIN (SELECT @rowNumber:= 0) r ) s WHERE rn % 2 = 0;`

## Write a MySQL query to find Fetch odd rows : 

`SELECT * FROM ( SELECT *, @rowNumber := @rowNumber+ 1 rn FROM users JOIN (SELECT @rowNumber:= 0) r ) s WHERE rn % 2 = 1;`

## You want to calculate the total revenue for each product over the past 30 days, as well as the percentage change in revenue compared to the previous 30-day period

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
    (s.total_revenue - p.total_revenue) / p.total_revenue * 100 AS revenue_change
FROM sales_last_30_days s
JOIN sales_previous_30_days p ON s.product_name = p.product_name;
```

## Creating an Event in MySQL:
```sql
CREATE TABLE event.timestamp_diff (
    id INT AUTO_INCREMENT PRIMARY KEY,
    created_at TIMESTAMP,
    time_difference INT
);

DELIMITER //
CREATE PROCEDURE event.insert_timestamp_row()
BEGIN
    DECLARE last_timestamp TIMESTAMP;
    SELECT MAX(created_at) INTO last_timestamp FROM timestamp_diff;
    INSERT INTO event.timestamp_diff (created_at, time_difference)
    VALUES (CURRENT_TIMESTAMP, TIMESTAMPDIFF(SECOND, last_timestamp, CURRENT_TIMESTAMP));
END //
DELIMITER ;

DELIMITER //
CREATE EVENT insert_timestamp_event
ON SCHEDULE EVERY 10 SECOND
DO
    CALL insert_timestamp_row();
```