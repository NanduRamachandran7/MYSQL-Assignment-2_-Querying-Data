# MySQL Assignment 2 – Querying Data

## Overview
This assignment continues **MySQL Assignment 1 – DDL Commands & Constraints**. It uses the `employee` database and the related `departments`, `location`, and `employees` tables.

The supplied data contains:
- 13 departments
- 4 locations
- 30 employees

## Repository Contents
Suggested repository structure:

```text
MySQL-Assignments/
├── Assignment-1/
│   └── MySQL Assignment 1 – DDL Commands & Constraints.pdf
└── Assignment-2/
    ├── MYSQL Assignment 2 - Querying Data.sql
    ├── MySQL Assignment 2 – Querying Data.pdf
    ├── MySQL_Assignment_2_Detailed_Documentation.docx
    └── README.md
```

The employee INSERT statements are included in the Assignment 2 SQL script. If you keep them in a separate file, include that file in this folder as well.

## Prerequisites
- MySQL Server 8.0 or a compatible MySQL version.
- MySQL Workbench or another MySQL client.
- The database and tables from Assignment 1 must already exist.

**Important:** The CREATE DATABASE and CREATE TABLE statements at the beginning of the Assignment 2 SQL script are commented out. Run the actual Assignment 1 schema creation script first.

## Execution Order
1. Execute the Assignment 1 database and table creation statements.
2. Select the database:

   ```sql
   USE employee;
   ```

3. Insert the department records.
4. Insert the location records.
5. Insert the employee records.
6. Execute the Assignment 2 queries.

Run the parent-table inserts before employee inserts because the Employees table has foreign keys referencing Departments and Location.

The INSERT statements are intended for a fresh database. Running them again without clearing/recreating the tables may cause duplicate primary-key or unique-key errors.

## Topics Covered
1. `DISTINCT` – retrieve unique salary values.
2. `AS` – assign output column aliases.
3. `WHERE` and `AND` – filter by salary and hire date.
4. `IS NULL` and `UPDATE` – find and fill a missing designation.
5. `ORDER BY` – sort by department and salary.
6. `LIMIT` – return the first five employees hired in 2018.
7. Aggregate functions – `SUM()` and `MIN()`.
8. `GROUP BY` – maximum salary by location and average salary by Analyst designation.
9. `HAVING` – filter grouped department counts and female-only location average ages.
10. `INNER JOIN`, `LEFT JOIN`, and `RIGHT JOIN` – combine related tables.

## Query Review Notes
The latest SQL script was checked against the Assignment 2 question sheet and the supplied data.

- All 15 assignment requirements have corresponding SQL queries.
- The department-count query now uses `LEFT JOIN`, so departments with zero employees are included when applying `HAVING COUNT(e.employee_id) < 3`.
- The female-age query now filters with `WHERE e.gender = 'F'`, so the average is calculated using female employees only. For a cleaner result, remove `e.gender` from the SELECT list because the question asks for location and average age only.
- The 2018 `LIKE '2018%'` filter works with the supplied DATE values. A date range is an alternative:

  ```sql
  WHERE hire_date >= '2018-01-01'
    AND hire_date < '2019-01-01'
  ```

- The missing designation belongs to employee ID `5004` (Kiara Malhotra) in the original supplied data. The UPDATE changes it to `Data Scientist`.
- The Finance department salary total from the supplied records is `170000`.
- The minimum age in the supplied records is `25`.

### Recommended female-only average-age query

```sql
SELECT l.location,
       AVG(e.age) AS average_age
FROM employees e
JOIN location l
    ON e.location_id = l.location_id
WHERE e.gender = 'F'
GROUP BY l.location_id, l.location
HAVING AVG(e.age) < 30;
```

### Department count including departments with zero employees

```sql
SELECT d.department_name,
       COUNT(e.employee_id) AS employee_count
FROM departments d
LEFT JOIN employees e
    ON d.department_id = e.department_id
GROUP BY d.department_id, d.department_name
HAVING COUNT(e.employee_id) < 3;
```

## Verification Queries

```sql
SELECT COUNT(*) AS department_count FROM departments; -- Expected: 13
SELECT COUNT(*) AS location_count FROM location;       -- Expected: 4
SELECT COUNT(*) AS employee_count FROM employees;       -- Expected: 30
```

Run the missing-designation SELECT before the UPDATE if you want to capture the original NULL value for evidence.

## Learning Outcomes
By completing this assignment, the learner practises data retrieval, filtering, sorting, aggregation, grouping, conditional group filtering, updating records, and joining relational tables.
