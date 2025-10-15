### SQLite Commands: ALTER TABLE and GROUP BY

#### Sample Table: learners

| id | name  | course  | birth\_year |
| -- | ----- | ------- | ----------- |
| 1  | Alice | Math    | 2001        |
| 2  | Bob   | Science | 1999        |
| 3  | Carol | Math    | 2003        |
| 4  | David | History | 2002        |
| 5  | Eva   | Math    | 1998        |
| 6  | Frank | Science | 2001        |

#### 1. ALTER TABLE RENAME

Rename an existing table in the database

```sql
ALTER TABLE students RENAME TO learners
```

#### 2. ALTER TABLE ADD COLUMN

Add a new column to an existing table

```sql
ALTER TABLE learners ADD COLUMN birth_year INTEGER
```

#### 3. GROUP BY

Group rows that have the same values in specified columns

```sql
SELECT course, COUNT(*)
FROM learners
GROUP BY course
```

**Result:**

| course  | COUNT(\*) |
| ------- | --------- |
| History | 1         |
| Math    | 3         |
| Science | 2         |

#### 4. GROUP BY with WHERE

Use `WHERE` to filter rows before grouping

```sql
SELECT course, COUNT(*)
FROM learners
WHERE birth_year > 2000
GROUP BY course
```

**Result:**

| course  | COUNT(\*) |
| ------- | --------- |
| History | 1         |
| Math    | 1         |
| Science | 1         |

#### 5. GROUP BY with HAVING

Use `HAVING` to filter groups after aggregation

```sql
SELECT course, COUNT(*) AS total
FROM learners
GROUP BY course
HAVING total > 1
```

**Result:**

| course  | total |
| ------- | ----- |
| Math    | 3     |
| Science | 2     |
