# 📘 לימוד SQLite עם טבלת COMPANY

<div dir="rtl" style="text-align: right">

## יצירת טבלה – CREATE TABLE

```sql
-- Create a table with fields: ID, name, age, address, and salary
CREATE TABLE COMPANY(
  ID INT PRIMARY KEY NOT NULL,
  NAME TEXT NOT NULL,
  AGE INT NOT NULL,
  ADDRESS CHAR(50),
  SALARY REAL
);
```

## הכנסת נתונים – INSERT INTO

```sql
-- Insert employees with age, address, and salary
INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (1, 'Paul', 32, 'California', 20000.00 );

INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (2, 'Allen', 25, 'Texas', 15000.00 );

INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (3, 'Teddy', 23, 'Norway', 20000.00 );

INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (4, 'Mark', 25, 'Rich-Mond ', 65000.00 );

INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (5, 'David', 27, 'Texas', 85000.00 );

INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (6, 'Kim', 22, 'South-Hall', 45000.00 );

INSERT INTO COMPANY VALUES (7, 'James', 24, 'Houston', 10000.00 );
```

## ה- SELECT הבסיסי

```sql
-- Show all rows and columns from the table
SELECT * FROM COMPANY;
```

```sql
-- Return the highest age in the table
SELECT MAX(AGE) FROM COMPANY;
-- Output: 32
```

```sql
-- Return the lowest age in the table
SELECT MIN(AGE) FROM COMPANY;
-- Output: 22
```

```sql
-- Calculate the average age
SELECT AVG(AGE) FROM COMPANY;
-- Output: 25.43
```

## ביטויים מתמטיים ופונקציות

```sql
SELECT 1;                    -- Output: 1
SELECT 5 + 3;                -- Output: 8
SELECT 1.0 / 100;            -- Output: 0.01
SELECT pow(2, 5);            -- Output: 32
SELECT sqrt(49);             -- Output: 7
SELECT ABS(-15);             -- Output: 15
SELECT ROUND(3.14159, 2);    -- Output: 3.14
```

## תאריכים ושעה

```sql
SELECT CURRENT_DATE;                      -- Output: 2025-06-27
SELECT CURRENT_TIME;                      -- Output: 14:52:03
SELECT CURRENT_TIMESTAMP;                 -- Output: 2025-06-27 14:52:03
SELECT datetime('now', 'localtime');      -- Output: 2025-06-27 17:52:03
SELECT strftime('%Y', 'now');             -- Output: 2025
```

## סינון עם WHERE, IN, LIKE, BETWEEN

```sql
-- Employees with salary greater than 4500
SELECT * FROM COMPANY WHERE SALARY > 4500;

-- Employees older than 25
SELECT * FROM COMPANY WHERE AGE > 25;

-- Employees whose address is exactly 'Texas'
SELECT * FROM COMPANY WHERE ADDRESS = 'Texas';

-- Employees named 'Kim'
SELECT * FROM COMPANY WHERE NAME = 'Kim';

-- Employees from Texas who earn more than 50,000
SELECT * FROM COMPANY WHERE ADDRESS = 'Texas' AND SALARY > 50000;

-- Employees whose address is either Texas or Norway
SELECT * FROM COMPANY WHERE ADDRESS IN ('Texas', 'Norway');

-- Employees whose name contains the letter 'a'
SELECT * FROM COMPANY WHERE NAME LIKE '%a%';

-- Employees whose age is between 25 and 27 (inclusive)
SELECT * FROM COMPANY WHERE AGE BETWEEN 25 AND 27;
```

## הגבלת כמות שורות – LIMIT ו־OFFSET

```sql
-- Show the first 3 rows in the table
SELECT * FROM COMPANY LIMIT 3;

-- Skip the first 2 rows and return the next 3
SELECT * FROM COMPANY LIMIT 3 OFFSET 2;
```

## מיון שורות עם ORDER BY

```sql
-- Sort employees by age in ascending order
SELECT * FROM COMPANY ORDER BY AGE ASC;

-- Sort employees by salary in descending order
SELECT * FROM COMPANY ORDER BY SALARY DESC;

-- Return the names of the top 2 earners
SELECT NAME FROM COMPANY ORDER BY SALARY DESC LIMIT 2;

-- Skip top 2 earners and show the next 2 by salary
SELECT NAME FROM COMPANY ORDER BY SALARY DESC LIMIT 2 OFFSET 2;
```

## עדכון נתונים – UPDATE

```sql
-- Update the address to 'Texas' and increase age by 1 for the employee with ID = 7
UPDATE COMPANY 
SET ADDRESS = 'Texas', AGE = AGE + 1
WHERE ID = 7;
```

## מחיקת רשומות – DELETE

```sql
-- Delete the employee with ID = 7
DELETE FROM COMPANY WHERE ID = 7;
```

</div>
