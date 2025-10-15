# PostgreSQL Stored Procedures and Functions

## ✨ Benefits of Using Stored Procedures & Functions in PostgreSQL

PostgreSQL stored procedures and functions allow you to encapsulate logic, optimize performance, and maintain better control over your database layer. Whether you're creating tables, doing math, or wrapping business logic, PL/pgSQL gives you the power to make your SQL smart, modular, and efficient

## ✨ Basic Example: Function that Returns a Greeting

```sql
DROP FUNCTION IF EXISTS hello_world();

CREATE OR REPLACE FUNCTION hello_world() 
RETURNS VARCHAR
LANGUAGE plpgsql AS
$$
BEGIN
    RETURN CONCAT('hello', ' world', ' !! ', current_timestamp);
END;
$$;

-- Usage:
SELECT * FROM hello_world();
```

**Explanation:**

* This function returns a greeting string with the current timestamp
* No input parameters, and it returns a single VARCHAR

## 🔧 Example: Stored Procedure to Create a Table

```sql
DROP PROCEDURE IF EXISTS create_demo_table();

CREATE OR REPLACE PROCEDURE create_demo_table()
LANGUAGE plpgsql AS
$$
BEGIN
    CREATE TABLE IF NOT EXISTS demo (
        id SERIAL PRIMARY KEY,
        name TEXT NOT NULL,
        email TEXT UNIQUE NOT NULL
    );
END;
$$;

-- Usage:
CALL create_demo_table();
```

**Explanation:**

* This procedure creates a table called `demo` with `id`, `name`, and `email`
* `CALL` is used to invoke stored procedures

## ➕ Example: Function to Add Two Numbers

```sql
CREATE OR REPLACE FUNCTION sp_sum(m DOUBLE PRECISION, n DOUBLE PRECISION)
RETURNS DOUBLE PRECISION
LANGUAGE plpgsql AS
$$
DECLARE
    x INTEGER := 0;
BEGIN
    RETURN m + n + x;
END;
$$;

-- Usage:
SELECT sp_sum(2.2, 3.5);
```

**Explanation:**

* Adds `m` and `n`, plus a default integer `x = 0`
* Returns a single double precision result


## ❎ Example: Function Returning Multiple Output

```sql
CREATE OR REPLACE FUNCTION sp_product(
    x DOUBLE PRECISION, 
    y DOUBLE PRECISION,
    OUT prod DOUBLE PRECISION,
    OUT div_res DOUBLE PRECISION
)
LANGUAGE plpgsql AS
$$
DECLARE
    z DOUBLE PRECISION := 1.0;
BEGIN
    prod = x * y * z;
    div_res = x / y;
END;
$$;

-- Usage:
SELECT * FROM sp_product(8, 2);
```

**Explanation:**

* Returns both the product and division result
* Uses `OUT` parameters to return multiple values as columns

## 🧠 Final Challenge: Function with Multiple Calculations

```sql
CREATE OR REPLACE FUNCTION sp_math_summary(
    x DOUBLE PRECISION,
    y DOUBLE PRECISION,
    OUT sum_result DOUBLE PRECISION,
    OUT diff_result DOUBLE PRECISION,
    OUT x_squared DOUBLE PRECISION,
    OUT y_cubed DOUBLE PRECISION
)
LANGUAGE plpgsql AS
$$
BEGIN
    sum_result := x + y;
    diff_result := x - y;
    x_squared := POWER(x, 2);
    y_cubed := POWER(y, 3);
END;
$$;

-- Usage:
SELECT * FROM sp_math_summary(8, 4);
-- Returns: 12, 4, 64, 64
```

**Explanation:**

* This function takes two numbers and returns their sum, difference, square of the first, and cube of the second
* Demonstrates the use of mathematical functions and multiple `OUT` parameters

---

## 📘 Key Concepts

### 🔹 What is PL/pgSQL?

**PL/pgSQL (Procedural Language/PostgreSQL Structured Query Language)** is PostgreSQL's procedural extension of SQL. It adds:

* Control structures like loops and conditionals (`IF`, `WHILE`, `FOR`)
* Variables and custom logic
* Error handling (`EXCEPTION` blocks)

It allows you to write **procedures and functions** that are much more powerful than plain SQL.

### 🔹 Why use `$$` to wrap function/procedure bodies?

PostgreSQL uses `$$` as a **quote delimiter** for blocks of code (also known as dollar-quoting). It allows you to include single quotes and other symbols inside the body without escaping them.

You can also use `$_$`, `$$custom$$`, etc., but `$$` is the most common.

### 🔹 Function vs Procedure

| Feature                 | FUNCTION                      | PROCEDURE                         |
| ----------------------- | ----------------------------- | --------------------------------- |
| Call Syntax             | `SELECT my_function()`        | `CALL my_procedure()`             |
| Can Return Value        | ✅ Yes                         | ❌ No (only OUT parameters)        |
| Use in SELECT queries   | ✅ Yes                         | ❌ No                              |
| Can manage transactions | ❌ No                          | ✅ Yes (`COMMIT`, `ROLLBACK`)      |
| Preferred for...        | Returning data / calculations | Performing actions (side effects) |

