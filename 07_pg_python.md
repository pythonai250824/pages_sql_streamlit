## Using PostgreSQL in Python with psycopg2

<img src="pg.jpg" align="left" />

This guide demonstrates how to connect to a PostgreSQL database using Python and `psycopg2`, run parameterized queries using `%s`, and handle connections with `try-except-finally`.

### Why Use a Cursor?

A `cursor` is a control structure that allows traversal over the records in a database. You use it to execute queries and fetch data. It represents the context of a query. Without it, you can't interact with the database.

### Why Close in `finally`?

The `finally` block ensures that your cursor and connection are closed even if an exception occurs. This prevents open connections from lingering and consuming resources, and avoids potential data corruption or memory leaks.

### 1. Installing `psycopg2`

Install the binary version to avoid compiler errors:

```bash
pip install psycopg2-binary
```

This version comes with precompiled dependencies and is perfect for most use cases.

### 2. Connect to PostgreSQL

```python
import psycopg2
import psycopg2.extras

try:
    conn = psycopg2.connect(
        dbname='postgres',
        user='postgres',
        password='admin',
        host='localhost',
        port='5432',
        cursor_factory=psycopg2.extras.RealDictCursor
    )
    print("Connected successfully.")
except psycopg2.Error as e:
    print("Connection error:", e)
```

#### What is `cursor_factory=psycopg2.extras.RealDictCursor`?

By default, `fetchall()` returns results as tuples. When using `RealDictCursor`, results are returned as dictionaries where column names become keys. This makes the data easier to work with:

```python
# Without RealDictCursor:
# [('Alice', 'alice@example.com')]

# With RealDictCursor:
# [{'name': 'Alice', 'email': 'alice@example.com'}]
```

**Example:**

```python
cur = conn.cursor()
cur.execute("SELECT name, email FROM users")
rows = cur.fetchall()
for row in rows:
    print(f"Name: {row['name']}, Email: {row['email']}")
```

### 3. Create Table

```python
try:
    cur = conn.cursor()
    cur.execute("""
        CREATE TABLE IF NOT EXISTS users (
            id SERIAL PRIMARY KEY,
            name TEXT NOT NULL,
            email TEXT UNIQUE NOT NULL
        );
    """)
    conn.commit()
    print("Table created.")
except psycopg2.Error as e:
    print("Error creating table:", e)
finally:
    cur.close()
```

### 4. Insert with Parameterized Query (`%s`)

```python
try:
    cur = conn.cursor()
    cur.execute(
        "INSERT INTO users (name, email) VALUES (%s, %s)",
        ("Alice", "alice@example.com")
    )
    conn.commit()
    print("Data inserted.")
except psycopg2.Error as e:
    print("Insert error:", e)
finally:
    cur.close()
```

### 5. Select Data

```python
try:
    cur = conn.cursor()
    cur.execute("SELECT * FROM users")
    rows = cur.fetchall()
    for row in rows:
        print(row)
except psycopg2.Error as e:
    print("Select error:", e)
finally:
    cur.close()
```

### 6. Close Connection

```python
conn.close()
print("Connection closed.")
```

### Notes

* Always use `%s` and pass parameters as a tuple to prevent SQL injection.
* Always close cursor and connection to avoid memory leaks.
* Use `RealDictCursor` if you want results as dictionaries instead of tuples.
* Wrap queries in `try-except-finally` blocks for safety.
