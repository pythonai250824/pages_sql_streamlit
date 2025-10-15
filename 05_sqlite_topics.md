## SQLite Topics: AUTOINCREMENT, UNION, ON DELETE CASCADE, TRIGGER, VIEW

### 1. AUTOINCREMENT

מזהה ייחודי שמוגדר אוטומטית ומתקדם אוטומטית בכל הוספה חדשה

```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL
);

INSERT INTO users (name) VALUES ('Alice');  -- id = 1
INSERT INTO users (name) VALUES ('Bob');    -- id = 2
```

### 2. UNION

מאחד תוצאות משתי טבלאות עם אותו מבנה עמודות
ה- `UNION` מחזיר ערכים ייחודיים (כמו `DISTINCT`), ואילו `UNION ALL` כולל כפילויות

```sql
CREATE TABLE students (
    id INTEGER PRIMARY KEY,
    name TEXT
);

CREATE TABLE teachers (
    id INTEGER PRIMARY KEY,
    name TEXT
);

INSERT INTO students (name) VALUES ('Alice'), ('Bob');
INSERT INTO teachers (name) VALUES ('Bob'), ('Carol');

SELECT name FROM students
UNION
SELECT name FROM teachers;  -- מחזיר Alice, Bob, Carol
```

### 3. ON DELETE CASCADE

כאשר טבלה בת (child) תלויה בטבלת אב (parent), אפשר להגדיר `ON DELETE CASCADE`
כך שמחיקת שורה מההורה תגרור מחיקה אוטומטית מהטבלה הבת

```sql
PRAGMA foreign_keys = ON;

CREATE TABLE authors (
    author_id INTEGER PRIMARY KEY,
    name TEXT
);

CREATE TABLE books (
    book_id INTEGER PRIMARY KEY,
    title TEXT,
    author_id INTEGER,
    FOREIGN KEY (author_id) REFERENCES authors(author_id) ON DELETE CASCADE
);

INSERT INTO authors (name) VALUES ('Tolkien');
INSERT INTO books (title, author_id) VALUES ('The Hobbit', 1);

DELETE FROM authors WHERE author_id = 1;  -- מוחק גם את הספר The Hobbit
```

### 4. TRIGGER

ה- `TRIGGER` הוא מנגנון שמבצע פעולה אוטומטית בעקבות אירועים כמו `INSERT`, `DELETE` או `UPDATE`
בדוגמה הבאה, כל פעולה על טבלת `grades` תעדכן את טבלת הסטטיסטיקה `grade_stats`

```sql
CREATE TABLE grades (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    student_name TEXT NOT NULL,
    grade INT NOT NULL
);

CREATE TABLE grade_stats (
    id INTEGER PRIMARY KEY CHECK (id = 1),
    grade_count INTEGER DEFAULT 0,
    grade_avg REAL DEFAULT 0,
    grade_max REAL DEFAULT 0
);

INSERT INTO grade_stats (id) VALUES (1);

CREATE TRIGGER update_stats_after_insert
AFTER INSERT ON grades
BEGIN
    UPDATE grade_stats
    SET
        grade_count = (SELECT COUNT(*) FROM grades),
        grade_avg = (SELECT AVG(grade) FROM grades),
        grade_max = (SELECT MAX(grade) FROM grades)
    WHERE id = 1;
END;

CREATE TRIGGER update_stats_after_delete
AFTER DELETE ON grades
BEGIN
    UPDATE grade_stats
    SET
        grade_count = (SELECT COUNT(*) FROM grades),
        grade_avg = (SELECT AVG(grade) FROM grades),
        grade_max = (SELECT MAX(grade) FROM grades)
    WHERE id = 1;
END;

CREATE TRIGGER update_stats_after_update
AFTER UPDATE ON grades
BEGIN
    UPDATE grade_stats
    SET
        grade_count = (SELECT COUNT(*) FROM grades),
        grade_avg = (SELECT AVG(grade) FROM grades),
        grade_max = (SELECT MAX(grade) FROM grades)
    WHERE id = 1;
END;

-- דוגמאות שמפעילות את הטריגרים
INSERT INTO grades (student_name, grade) VALUES ('Alice', 95);  -- מפעיל טריגר insert
UPDATE grades SET grade = 100 WHERE id = 1;  -- מפעיל טריגר update
DELETE FROM grades WHERE id = 1;  -- מפעיל טריגר delete
```

### 5. VIEW

תצוגה (`VIEW`) היא שאילתה שמאוחסנת כמו טבלה וניתן לשלוף ממנה נתונים כמו מטבלה רגילה

```sql
CREATE TABLE orders (
    order_id INTEGER PRIMARY KEY,
    customer TEXT,
    total_price REAL
);

INSERT INTO orders (customer, total_price)
VALUES ('Alice', 100.50), ('Bob', 75.00);

CREATE VIEW orders_above_80 AS
SELECT customer, total_price
FROM orders
WHERE total_price > 80;

SELECT * FROM orders_above_80;  -- מציג רק את Alice
```
