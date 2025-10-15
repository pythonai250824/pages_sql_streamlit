## Beginner's Guide to PostgreSQL

<img src="pg.jpg" align="left" />

- why postgresql?
- SERIAL
- INSERT with Random  
- FK Inline
- TIMESTAMP
- JSONB
  

### Why Use PostgreSQL?

ה- PostgreSQL היא מערכת ניהול מסדי נתונים רלציונית בקוד פתוח, הנחשבת לחזקה, מאובטחת וגמישה. לעומת SQLite – שמתאימה יותר לפרויקטים קטנים, אפליקציות מוטמעות או שלב הפיתוח – PostgreSQL מתאימה למערכות מורכבות עם ריבוי משתמשים, נפחים גדולים ודרישות ביצועים גבוהות. היא תומכת בשאילתות מקביליות, אינדוקסים מתקדמים, סוגי נתונים מותאמים אישית, טריגרים, תהליכונים שמורים (stored procedures), וטיפוסי JSON מתקדמים. כמו כן, PostgreSQL מתמודדת טוב יותר עם כתיבה מקבילה ומציעה יציבות ויכולות ניהול מתקדמות לפרויקטים בקנה מידה תעשייתי

### 1. PostgreSQL vs SQLite: CREATE TABLE and AUTOINCREMENT

SQLite uses `AUTOINCREMENT` with `INTEGER PRIMARY KEY`, while PostgreSQL uses `SERIAL` types.

**SQLite:**

```sql
CREATE TABLE my_users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    username TEXT NOT NULL,
    email TEXT UNIQUE,
    age INTEGER
);
```

**PostgreSQL:**

```sql
CREATE TABLE my_users (
    id SERIAL PRIMARY KEY,
    username TEXT NOT NULL,
    email TEXT UNIQUE,
    age INTEGER
);
```

* **SERIAL** = `INTEGER` + auto-increment (4 bytes)
* **BIGSERIAL** = `BIGINT` + auto-increment (8 bytes)
* **SMALLSERIAL** = `SMALLINT` + auto-increment (2 bytes)

In PostgreSQL, the SERIAL keyword is a shorthand for creating an auto-incrementing column. When you define a column as SERIAL, PostgreSQL automatically creates a sequence behind the scenes and sets the column’s default value to the next value from that sequence. This eliminates the need to manually create and maintain sequences for primary keys.

### 2. Inline FOREIGN KEY

In PostgreSQL, you can define a foreign key inline, directly in the column definition:

```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES my_users(id),
    amount REAL NOT NULL,
    order_datetime TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### 3. TIMESTAMP and DEFAULT CURRENT\_TIMESTAMP

You can use `TIMESTAMP` with a default value to automatically store the current time:

```sql
order_datetime TIMESTAMP DEFAULT CURRENT_TIMESTAMP
```

This ensures that every new row gets the insertion timestamp.

### 4. INSERT with RANDOM

In PostgreSQL, you can use the `random()` function and cast the result:

```sql
INSERT INTO orders (user_id, amount) VALUES
(1, round((random() * 500 + 20)::numeric, 2)),
(2, round((random() * 500 + 20)::numeric, 2)),
(3, round((random() * 500 + 20)::numeric, 2));
```

This inserts random values between 20 and 520, rounded to 2 decimal places.

### 5. JSONB: Table, Insert, Query

PostgreSQL supports a powerful JSON data type called `JSONB`.

#### Create Table with JSONB:

```sql
CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    grades JSONB
);
```

#### Insert Data:

```sql
INSERT INTO students (name, grades) VALUES
('danny', '{"math": 99, "english": 80, "history": 75}'),
('arya', '{"math": 88, "english": 95, "history": 70}'),
('jon',  '{"math": 76, "english": 67, "history": 82}');
```

#### Select Specific Field:

```sql
SELECT name, grades->>'math' AS math_grade FROM students;
```

#### Filter by JSONB Field:

```sql
SELECT * FROM students
WHERE (grades->>'math')::INT > 90;
```

