
## 1. Data Retrieval Queries

- **Fetch all data:**
    
    ```sql
    SELECT * FROM table_name;
    ```
    
- **Select specific columns:**
    
    ```sql
    SELECT column1, column2 FROM table_name;
    ```
    
- **Remove duplicate values:**
    
    ```sql
    SELECT DISTINCT column_name FROM table_name;
    ```
    
- **Filter data using conditions:**
    
    ```sql
    SELECT * FROM table_name WHERE column_name = 'value';
    ```
    
- **Use `BETWEEN` for range filtering:**
    
    ```sql
    SELECT * FROM table_name WHERE column_name BETWEEN value1 AND value2;
    ```
    
- **Use `IN` for multiple values:**
    
    ```sql
    SELECT * FROM table_name WHERE column_name IN ('value1', 'value2', 'value3');
    ```
[[Data Retrival Excerise]]


## 2. Aggregation Queries

- **Count total rows:**
    
    ```sql
    SELECT COUNT(*) FROM table_name;
    ```
    
- **Sum values of a column:**
    
    ```sql
    SELECT SUM(column_name) FROM table_name;
    ```
    
- **Get average of values:**
    
    ```sql
    SELECT AVG(column_name) FROM table_name;
    ```
    
- **Find minimum and maximum values:**
    
    ```sql
    SELECT MIN(column_name), MAX(column_name) FROM table_name;
    ```
    
- **Grouping results:**
    
    ```sql
    SELECT column_name, COUNT(*) FROM table_name GROUP BY column_name;
    ```
    
- **Use `HAVING` to filter grouped results:**
    
    ```sql
    SELECT column_name, COUNT(*) FROM table_name GROUP BY column_name HAVING COUNT(*) > 10;
    ```
    
 [[Aggrecastion Excerise]]
 
## 3. Joining Tables

- **`INNER JOIN` (matching records from both tables):**
    
    ```sql
    SELECT t1.column, t2.column
    FROM table1 t1
    INNER JOIN table2 t2 ON t1.common_column = t2.common_column;
    ```
    
- **`LEFT JOIN` (all records from left table, matching records from right table):**
    
    ```sql
    SELECT t1.column, t2.column
    FROM table1 t1
    LEFT JOIN table2 t2 ON t1.common_column = t2.common_column;
    ```
    
- **`RIGHT JOIN` (all records from right table, matching records from left table):**
    
    ```sql
    SELECT t1.column, t2.column
    FROM table1 t1
    RIGHT JOIN table2 t2 ON t1.common_column = t2.common_column;
    ```
    
- **`FULL OUTER JOIN` (all records from both tables):**
    
    ```sql
    SELECT t1.column, t2.column
    FROM table1 t1
    FULL OUTER JOIN table2 t2 ON t1.common_column = t2.common_column;
    ```
    

## 4. Subqueries

- **Subquery in `WHERE` clause:**
    
    ```sql
    SELECT * FROM table_name WHERE column_name = (SELECT column_name FROM another_table WHERE condition);
    ```
    
- **Using `EXISTS` to check existence:**
    
    ```sql
    SELECT * FROM table1 WHERE EXISTS (SELECT * FROM table2 WHERE table1.column = table2.column);
    ```
    

## 5. Data Modification Queries

- **Insert new record:**
    
    ```sql
    INSERT INTO table_name (column1, column2) VALUES ('value1', 'value2');
    ```
    
- **Update existing record:**
    
    ```sql
    UPDATE table_name SET column1 = 'new_value' WHERE column2 = 'condition';
    ```
    
- **Delete record:**
    
    ```sql
    DELETE FROM table_name WHERE column_name = 'value';
    ```
    

## 6. Indexing and Performance Optimization

- **Create an index:**
    
    ```sql
    CREATE INDEX index_name ON table_name (column_name);
    ```
    
- **Analyze query performance:**
    
    ```sql
    EXPLAIN SELECT * FROM table_name WHERE column_name = 'value';
    ```
    

These are some of the most important SQL queries every developer should know. Let me know if you need more details! 🚀