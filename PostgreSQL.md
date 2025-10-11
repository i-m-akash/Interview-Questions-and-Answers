**PostgreSQL** (often called Postgres) is an open-source, object-relational database known for its reliability, feature richness, and standards compliance. It supports **ACID transactions**, **JSON**, **window functions**, and **CTEs** — making it a preferred choice for modern scalable systems.

---

## 🟢 Basic Questions

### 1. What is PostgreSQL?
> PostgreSQL is an open-source, object-relational database system (ORDBMS) that supports SQL and procedural languages, transactions, and advanced data types.

---

### 2. What are some key features of PostgreSQL?
- ACID compliance  
- MVCC (Multi-Version Concurrency Control)  
- Support for JSON, XML, HSTORE  
- Full-text search  
- Custom data types & functions  
- Table inheritance  
- Window functions  

---

### 3. What is the difference between PostgreSQL and MySQL?
| Feature | PostgreSQL | MySQL |
|----------|-------------|-------|
| Type | Object-relational | Relational |
| ACID Transactions | Fully supported | Partial (depends on engine) |
| JSON Support | Advanced | Basic |
| Concurrency | MVCC | Locks |
| Full-text Search | Native | Requires plugin |

---

### 4. What is a schema in PostgreSQL?
> A **schema** is a namespace that contains database objects like tables, views, functions, and indexes. It helps organize data logically.

```sql
CREATE SCHEMA company_data;
````

---

### 5. What is a tablespace?

> A **tablespace** defines the location on disk where PostgreSQL stores data files.

```sql
CREATE TABLESPACE fastspace LOCATION '/mnt/ssd1';
```

---

## 🟡 Intermediate Questions

### 6. Explain MVCC (Multi-Version Concurrency Control)

> MVCC allows concurrent transactions without locking by keeping multiple versions of a record.
> Readers never block writers, and writers never block readers.

---

### 7. What are CTEs (Common Table Expressions)?

> A CTE is a temporary result set defined within a query for better readability and reuse.

```sql
WITH high_salary AS (
  SELECT name, salary FROM employees WHERE salary > 100000
)
SELECT * FROM high_salary WHERE name LIKE 'A%';
```

---

### 8. What is a sequence in PostgreSQL?

> A **sequence** is a special object used to generate unique identifiers, often for primary keys.

```sql
CREATE SEQUENCE emp_id_seq;
SELECT nextval('emp_id_seq');
```

---

### 9. What is a view?

> A **view** is a virtual table based on a SQL query.

```sql
CREATE VIEW active_users AS
SELECT id, name FROM users WHERE is_active = TRUE;
```

---

### 10. What is the difference between `DELETE`, `TRUNCATE`, and `DROP`?

| Command  | Function                                       |
| -------- | ---------------------------------------------- |
| DELETE   | Removes rows conditionally; can be rolled back |
| TRUNCATE | Removes all rows; faster; cannot use WHERE     |
| DROP     | Removes the entire table structure             |

---

## 🔵 Advanced Questions

### 11. What is the difference between `INNER JOIN` and `LEFT JOIN`?

* **INNER JOIN:** Returns rows that have matching values in both tables
* **LEFT JOIN:** Returns all rows from the left table even if no match in right

```sql
SELECT u.name, o.id
FROM users u
LEFT JOIN orders o ON u.id = o.user_id;
```

---

### 12. What is a window function?

> Window functions perform calculations across a set of rows related to the current row.

```sql
SELECT name, salary,
       RANK() OVER (ORDER BY salary DESC) AS rank
FROM employees;
```

---

### 13. What are triggers?

> A **trigger** executes a function automatically in response to certain events on a table.

```sql
CREATE TRIGGER log_update
AFTER UPDATE ON employees
FOR EACH ROW
EXECUTE FUNCTION log_employee_changes();
```

---

### 14. How do you perform UPSERT in PostgreSQL?

```sql
INSERT INTO users (id, name)
VALUES (1, 'Akash')
ON CONFLICT (id) DO UPDATE
SET name = EXCLUDED.name;
```

---

### 15. What are JSON operators in PostgreSQL?

```sql
SELECT data->'name' AS name, data->>'age' AS age
FROM users_json;
```

* `->` returns JSON object
* `->>` returns text value

---

## ⚙️ Performance & Optimization

### 16. What are indexes in PostgreSQL?

> Indexes speed up data retrieval operations. PostgreSQL supports B-tree, Hash, GIN, and GiST indexes.

```sql
CREATE INDEX idx_users_email ON users(email);
```

---

### 17. How to check the query execution plan?

```sql
EXPLAIN ANALYZE SELECT * FROM orders WHERE amount > 5000;
```

---

### 18. What is VACUUM in PostgreSQL?

> VACUUM reclaims storage and removes dead tuples created by MVCC.

```sql
VACUUM FULL;
```

---

### 19. How to improve query performance?

* Create proper indexes
* Avoid `SELECT *`
* Use `EXPLAIN ANALYZE`
* Normalize schema
* Use connection pooling

---

### 20. What is a materialized view?

> Unlike regular views, **materialized views** store query results physically and can be refreshed.

```sql
CREATE MATERIALIZED VIEW sales_summary AS
SELECT region, SUM(amount) FROM sales GROUP BY region;
REFRESH MATERIALIZED VIEW sales_summary;
```

---

## 🧮 Joins, Indexes & Transactions

### 21. What is a transaction?

> A **transaction** is a group of SQL operations executed as a single unit.
> It ensures **ACID** properties: Atomicity, Consistency, Isolation, Durability.

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```

---

### 22. What is the difference between COMMIT and ROLLBACK?

| Command  | Description                                     |
| -------- | ----------------------------------------------- |
| COMMIT   | Saves all changes made during the transaction   |
| ROLLBACK | Cancels all changes made during the transaction |

---

### 23. Explain isolation levels in PostgreSQL.

| Level            | Description                                        |
| ---------------- | -------------------------------------------------- |
| Read Uncommitted | Reads uncommitted data (not supported in Postgres) |
| Read Committed   | Default level; only committed data visible         |
| Repeatable Read  | Prevents non-repeatable reads                      |
| Serializable     | Highest; ensures full isolation                    |

---

## 💡 Practical SQL Queries

### 24. Find second highest salary:

```sql
SELECT MAX(salary)
FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);
```

---

### 25. Count employees per department:

```sql
SELECT department_id, COUNT(*) 
FROM employees 
GROUP BY department_id;
```

---

### 26. Find duplicate emails:

```sql
SELECT email, COUNT(*) 
FROM users 
GROUP BY email 
HAVING COUNT(*) > 1;
```

---

### 27. Retrieve records created in last 7 days:

```sql
SELECT * FROM orders
WHERE created_at >= NOW() - INTERVAL '7 days';
```

---

### 28. Check running queries:

```sql
SELECT * FROM pg_stat_activity;
```

---

## 🧠 Bonus: System Design Concepts

### 29. How to scale PostgreSQL?

* Use **Read Replicas** for load distribution
* Use **Connection Pooling** (PgBouncer)
* Use **Sharding / Partitioning**
* Optimize **indexes and caching**

---

### 30. How to handle large datasets in PostgreSQL?

* Table partitioning
* Batch inserts
* Index-only scans
* Proper VACUUM & ANALYZE

---

## ✅ Conclusion

PostgreSQL combines powerful SQL features with enterprise-grade reliability. Mastering these questions will help you perform confidently in **backend, database, or system design interviews**.

---

