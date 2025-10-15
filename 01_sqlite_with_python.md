# 📘 הסבר קוד Python לעבודה עם SQLite וטבלת COMPANY

<div dir="rtl" style="text-align: right">

## פתיחה והכנה

</div>

```python
import sqlite3
import os
```

Importing `sqlite3` for database operations and `os` to check if the database file exists and remove it

```python
if os.path.exists('db1.db'):
    os.remove('db1.db')
```

If the database file already exists, it's deleted to start fresh

```python
conn = sqlite3.connect('db1.db')
conn.row_factory = sqlite3.Row
```

Connecting to the database and setting `row_factory` to `sqlite3.Row` allows us to access columns by name (e.g., `row['name']`) instead of only by index

### What is a cursor?

A **cursor** is an object that allows us to execute SQL commands and retrieve results from the database
It manages the context of a fetch operation and provides methods like `.execute()`, `.fetchone()`, `.fetchall()`, and `.executemany()`
It also ensures that results can be processed row by row, which is memory-efficient when working with large datasets

## יצירת טבלה

```python
cursor = conn.cursor()
cursor.execute('''
CREATE TABLE IF NOT EXISTS COMPANY(
    ID INT PRIMARY KEY NOT NULL,
    NAME TEXT NOT NULL,
    AGE INT NOT NULL,
    ADDRESS CHAR(50),
    SALARY REAL
);
''')
```

We create a `cursor` object and use it to execute a SQL command to create a `COMPANY` table with fields for ID, name, age, address, and salary

## הכנסת נתונים בבטחה (batch)

```python
data = [
    (1, 'Paul', 32, 'California', 20000.00),
    (2, 'Allen', 25, 'Texas', 15000.00),
    (3, 'Teddy', 23, 'Norway', 20000.00),
    (4, 'Mark', 25, 'Rich-Mond ', 65000.00),
    (5, 'David', 27, 'Texas', 85000.00),
    (6, 'Kim', 22, 'South-Hall', 45000.00),
]
cursor.executemany('''
INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (?, ?, ?, ?, ?);
''', data)
```

We insert a list of employee records using `executemany`, which is efficient for bulk inserts

### Why use `?` in SQL queries?

Using `?` is a **parameterized query**
Instead of inserting values directly into the SQL string, placeholders are used and values are provided as a tuple
This avoids SQL injection and ensures that values are properly escaped
It is safer and more reliable than string concatenation, especially when dealing with user input

## עדכון כתובת לעובד עם ID = 6 (כולל עדכון עם שעה נוכחית)

```python
cursor.execute('''
UPDATE COMPANY SET ADDRESS = ? WHERE ID = ?;
''', ('Texas', 6))

import datetime
time_now = datetime.datetime.now().strftime('%H:%M:%S')
address_with_time = "Texas " + time_now
cursor.execute('''
UPDATE COMPANY SET ADDRESS = ? WHERE ID = ?;
''', (address_with_time, 6))
```

Updating the address of employee ID 6 first to "Texas", then adding the current time to the address, e.g., "Texas 19:07:00"

## מחיקת עובד לפי ID

```python
cursor.execute('''
DELETE FROM COMPANY WHERE ID = ?
''', (5,))
```

Deleting the employee with ID 5 from the table

## הצגת כל העובדים

```python
cursor.execute('SELECT * FROM COMPANY;')
result = cursor.fetchall()
for row in result:
    print(row['name'])
    print(dict(row))
```

Fetching all employee records and printing each name and row as a dictionary

## הצגת עובד ראשון בלבד

```python
cursor.execute('SELECT * FROM COMPANY;')
result = cursor.fetchone()
print(result['name'])
```

Fetching and printing only the first row

## קבלת נתונים מהמשתמש והכנסה עם טיפול בשגיאות

```python
while True:
    try:
        new_id = int(input('enter id:'))
        new_name = input('enter name:')
        new_age = int(input('enter age:'))
        new_address = input('enter address:')
        new_salary = float(input('enter salary:'))
        cursor.execute('''
        INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
        VALUES (?, ?, ?, ?, ?);
        ''', (new_id, new_name, new_age, new_address, new_salary))
        last_inserted = cursor.lastrowid
        break
    except (sqlite3.IntegrityError, sqlite3.OperationalError) as e:
        print('db error ', e)
    except:
        print('=== cannot insert this row. try again ===')
```

This block uses a `while True` loop to keep asking the user for input until the insert is successful
The `try` block attempts to get input, convert it to the correct types, and insert it into the database
If there is a problem like duplicate ID or invalid data type, the `except` block catches the error and asks the user to try again
This approach improves robustness and avoids program crashes due to bad input

## הצגת כלל העובדים (כולל החדש)

```python
cursor.execute('SELECT * FROM COMPANY;')
result = cursor.fetchall()
for row in result:
    print(dict(row))
```

## הדפסת העובד האחרון ברשימה

```python
cursor.execute('SELECT * FROM COMPANY;')
result = cursor.fetchall()
print(dict(result[-1]))
```

## שליפת העובד לפי ID שהוזן

```python
cursor.execute('SELECT * FROM COMPANY WHERE ID = ?;', (new_id,))
result = cursor.fetchone()
print(dict(result))
```

## סיום

```python
conn.commit()
conn.close()
```

Saving all changes to the database and closing the connection
