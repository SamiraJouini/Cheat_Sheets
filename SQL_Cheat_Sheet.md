# Lesson #1: **SQL Relational Databases - Key Concepts**  

## 1. **Relational Databases (RDBMS)**  
- **DBMS**: Software managing database access (e.g., PostgreSQL, MySQL, Oracle).  
- **SQL**: Language for querying/manipulating relational databases.  
- **ACID Properties**:  
  - **Atomicity**: Transactions succeed entirely or fail.  
  - **Consistency**: Valid initial/final database states.  
  - **Isolation**: Concurrent transactions don’t interfere.  
  - **Durability**: Committed transactions persist.  
---
## 2. **Tables & Schemas**  
- **Table**: Collection of records (rows) with attributes (columns).  
- **Schema**: Defines table structure (attributes, types, constraints).  
  - Example:  
    ```sql  
    Films(FilmId INTEGER, Name VARCHAR, Director VARCHAR)  
    ```  
---
## 3. **Data Types**  
- Common types: `INTEGER`, `VARCHAR`, `DATE`, `FLOAT`, `BOOLEAN`.  
---
## 4. **Keys**  
- **Primary Key**: Uniquely identifies a row (e.g., `SaleId` in `Sales` table).  
  - Must be **unique** and **non-null**.  
- **Foreign Key**: References a primary key in another table.  
  - Example: `CountryId` in `Actors` references `Countries` table.  
- **Composite Key**: Primary key using multiple columns (e.g., `(StoryId, Language)` in `Books`).  
---
## 5. **Constraints**  
- **Domain**: Enforces data type/rules (e.g., `NOT NULL`).  
- **Referential Integrity**: Foreign keys must reference valid primary keys.  
- **Key/Table Constraints**: Ensure uniqueness and validity.  
---
## 6. **Relational Schemas**  
- **One-to-One**: One row in Table A → One row in Table B (e.g., `Films` ↔ `Synopsis`).  
- **One-to-Many**: One row in Table A → Multiple rows in Table B (e.g., `Films` → `Casting`).  
- **Many-to-Many**: Requires a junction table (e.g., `Casting` links `Films` and `Actors`).  
---
## 7. **Example Tables**  
### Sales Table Schema  
```sql  
Sales(SaleId INTEGER, Item VARCHAR, SaleDate DATE, Quantity INTEGER, UnitPrice DECIMAL)
```  
**Primary Key**: `SaleId`.
### Books Table
---
| StoryId | Language | Title                   |  
|---------|----------|-------------------------|  
| 30      | en       | Cyprien Overbeck-Wells  |  
| 30      | fr       | Cyprien Overbeck-Wells  |  
| 31      | en       | The Red-Headed League   |  
| 31      | fr       | La Ligue des rouquins   |  

- **Composite Primary Key**: `(StoryId, Language)`.
---
## 8. Key Takeaways
- **Primary keys**: uniquely identify rows.
- **Foreign keys**: enforce relationships between tables.
- **Composite keys**: use multiple columns for uniqueness.
- **ACID**: ensures reliable transactions in relational databases.**
- Relationships: **1:1, 1:Many, Many:Many**.

---
# Lesson #2: **SQL Queries - Key Concepts**  

## 1. **Basic SQL Operations**  
### Projection  
- Display columns from a table:  
  ```sql  
  SELECT column1, column2 FROM table;
  ```  
- Example (show all albums):
```sql
SELECT * FROM albums;
```  
### Distinct Values
- Remove duplicates:
```sql
SELECT DISTINCT ArtistId FROM albums;  
```
---
## 2. **Filtering (WHERE Clause)**
### Operators
- `=`, `<>`, `>`, `<`, `>=`, `<=`, `IN`, `BETWEEN`, `LIKE`, `IS NULL`, `IS NOT NULL`.
- Examples:
```sql
-- Employees in Calgary  
SELECT * FROM employees WHERE City = 'Calgary';  

-- Employees born before May 10, 1973  
SELECT * FROM employees WHERE BirthDate < '1973-05-10';  

-- Tracks with titles starting with 'P'  
SELECT * FROM tracks WHERE Name LIKE 'P%'; 
```
### Pattern Matching (LIKE)
- `%`: Any sequence of characters.
- `_`: Single character.
- Examples:
```sql
-- 3-character titles  
SELECT * FROM tracks WHERE Name LIKE '___';  

-- Tracks composed by Jimi Hendrix (any order)  
SELECT * FROM tracks WHERE Composer LIKE '%Jimi%' AND Composer LIKE '%Hendrix%';
```
---
## 3. Sorting & Limiting Results
### ORDER BY
- Sort ascending (`ASC`, default) or descending (`DESC`):
```sql
-- Alphabetical track titles (first 10)  
SELECT Name FROM tracks ORDER BY Name LIMIT 10;  

-- Longest tracks  
SELECT Name, Milliseconds FROM tracks ORDER BY Milliseconds DESC LIMIT 5;  
```
---
## 4. Example Tables
### Employees Table
---
| EmployeeId | LastName | FirstName | Title             | City      |  
|------------|----------|-----------|-------------------|-----------|  
| 1          | Adams    | Andrew    | General Manager   | Edmonton  |  
| 2          | Edwards  | Nancy     | Sales Manager     | Calgary   |  
### Tracks Table
---
| TrackId | Name                        | Composer                          | Milliseconds |  
|---------|-----------------------------|-----------------------------------|--------------|  
| 1       | For Those About To Rock...  | Angus Young, Malcolm Young, Brian | 343,719      |  
| 2       | Balls to the Wall           | None                              | 342,562      |  
---
## 5. Key Takeaways
- Use `SELECT` for projections, `WHERE` for filtering, and `ORDER` BY for sorting.
- `LIKE` with `%`/`_` enables flexible pattern matching.
- `LIMIT` restricts result count.
- Case-insensitive pattern matching in SQLite.

---
# Lesson #3: **Advanced SQL Queries**

**1. Joins**  
- **INNER JOIN**: Combines rows from two tables based on a common column (e.g., `Persons` and `Vehicles` on `Car`). Requires unambiguous column references (e.g., `tracks.GenreId`).  
- **LEFT/RIGHT JOIN**: Includes all rows from the left/right table, filling unmatched entries with `NULL`. `RIGHT JOIN` can be simulated using `LEFT JOIN` by reversing table order.  
- **OUTER JOIN**: Combines LEFT and RIGHT JOINs using `UNION` to include all rows from both tables.  
- **Self-Join**: Joins a table to itself using aliases (e.g., `employees` table to link employees and their managers).  
- **Nested Joins**: Joins involving subqueries (e.g., joining `tracks`, `albums`, and `artists` via subqueries).  
---
**2. Aggregate Functions**  
- Functions: `COUNT`, `MAX`, `MIN`, `AVG`, `SUM`.  
- **Examples**:  
  - Count tracks: `SELECT COUNT(TrackId) FROM tracks;`.  
  - Average track duration: `SELECT AVG(Milliseconds) FROM tracks;`.  
---
**3. GROUP BY & HAVING**  
- **GROUP BY**: Groups rows by columns for aggregate calculations (e.g., albums per artist: `SELECT ArtistId, COUNT(AlbumId) FROM albums GROUP BY ArtistId;`).  
- **HAVING**: Filters grouped results (e.g., albums with ≥2 tracks: `GROUP BY AlbumId HAVING COUNT(TrackId) >= 2;`).  
---
**4. Selection**  
- **Upstream (WHERE)**: Filters rows before grouping (e.g., tracks >400s: `WHERE Milliseconds > 400000`).  
- **Downstream (HAVING)**: Filters groups after aggregation (e.g., non-single albums).  
---
**5. Best Practices**  
- Use aliases (`AS`) for clarity.  
- Avoid ambiguity in column names with table prefixes.  
- Simulate `RIGHT JOIN` with `LEFT JOIN` if unsupported.  
---
**Conclusion**  
Key operations: `INNER JOIN`, `GROUP BY` with aggregates, and functions like `COUNT`/`SUM`. Use joins to combine tables, aggregates to summarize data, and `HAVING`/`WHERE` to filter results.

---
# Lesson #4: **SQL Tables Operations Guide**  

```
**Key Concepts & Commands**
```  
---
## 1. Table Creation  
**Syntax**:  
```sql  
CREATE TABLE table_name (  
    column1 DATATYPE CONSTRAINT,  
    column2 DATATYPE,  
    PRIMARY KEY (column1),  
    FOREIGN KEY (column2) REFERENCES other_table(column)  
);  
```  

**Example**:  
```sql  
CREATE TABLE courses (  
    id INTEGER NOT NULL,  
    name VARCHAR,  
    PRIMARY KEY (id)  
);  
```  
---
## 2. Temporary Tables  
- Exist only during the current session:  
```sql  
CREATE TEMPORARY TABLE temp_table (...);  
```  
---
## 3. Constraints  
- Add constraints later (SQLite workaround required):  
```sql  
ALTER TABLE students  
ADD CONSTRAINT FK_course_id  
FOREIGN KEY (course_id) REFERENCES courses(id);  
```  
---
## 4. Row Operations  
### Insert  
```sql  
INSERT INTO courses VALUES (1, 'Data Engineering');  
```  
### Delete  
```sql  
DELETE FROM students WHERE id = 1;  
```  
### Update  
```sql  
UPDATE students SET lastname = 'Durand' WHERE id = 2;  
```  
---
## 5. Foreign Key Constraints  
- Prevent orphaned records (e.g., deleting a course with enrolled students fails).  
---
## 6. Table Deletion  
- Order matters (delete dependent tables first):  
```sql  
DROP TABLE IF EXISTS students;  
DROP TABLE IF EXISTS courses;  
```  
---
## 7. Views  
- Virtual tables for simplified querying:  
```sql  
CREATE VIEW v_tracks AS  
SELECT tracks.name, albums.Title AS album  
FROM tracks  
JOIN albums ON tracks.AlbumId = albums.AlbumId;  
```  
---
## 8. Key Commands  
- Verify tables:  
```sql  
SELECT * FROM table;  
```  
- Enable foreign keys (SQLite):  
```sql  
PRAGMA foreign_keys = ON;  
```  

---
# Lesson #5: **SQL Alchemy (Bonus)**

**1. SQLAlchemy Setup & Connection**  
- **Engine Creation**: Connect to databases using `create_engine('sqlite:///database.db')`.  
- **Inspector**: Use `inspect(engine)` to retrieve table metadata (e.g., `get_table_names()`, `get_columns()`, `get_foreign_keys()`).  
---
**2. Executing Queries**  
- **Connection**: Instantiate with `conn = engine.connect()`.  
- **SQL Execution**: Use `text()` for safe query formatting (e.g., `SELECT Title FROM albums LIMIT 10`).  
- **Aggregates**: Apply `GROUP BY` and `ORDER BY` (e.g., sum invoices by customer).  
---
**3. Table Operations**  
- **Creation**: Define tables using `Table` class with `Column`, `Integer`, `String`, `ForeignKey`, and `MetaData`.  
  ```python
  courses = Table('courses', meta,
      Column('id', Integer, primary_key=True),
      Column('name', String))
  meta.create_all(engine)
  ```
- **Foreign Keys**: Specify with `Column('course_id', Integer, ForeignKey('courses.id'))`.
- **Deletion**: Use `DROP TABLE IF EXISTS table`; and handle dependencies (e.g., delete child tables first).
---
## 4. Transactions
- **Structure**: Use context managers for atomicity:
```python
with engine.connect() as connection:
    with connection.begin() as transaction:
        try:
            connection.execute(query)
        except:
            transaction.rollback()
```
- **Data Insertion**: Insert tuples via parameterized `INSERT` statements.
---
## 5. Data Integrity
- **Constraints**: Enforce primary/foreign keys and uniqueness.
- **ACID Compliance**: Use transactions to rollback on errors (e.g., duplicate primary keys).
---
## 6. Key Takeaways
- **SQLAlchemy Workflow**: Connect → Inspect → Query → Modify.
- **Syntax**: Use Pythonic syntax for SQL operations (e.g., `MetaData`, `Table`, `Column`).
- **Cheat Sheet**: Refer to provided SQLAlchemy documentation for quick reference.
### Conclusion
SQLAlchemy enables Python-driven database interactions, covering schema inspection, CRUD operations, and transaction management while ensuring data integrity.

---
``` 
**To use**:  
1. Save as `.md` file.  
2. Preview in any Markdown viewer (e.g., VS Code, GitHub, Obsidian).  
3. Export to PDF using tools like [Markdown to PDF converters](https://md2pdf.netlify.app/).
