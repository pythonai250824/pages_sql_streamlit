## 🔁 What is UPSERT?

**UPSERT** is a combination of **INSERT** and **UPDATE**.
It solves the problem of inserting a new row into a table **if it doesn't exist**, or **updating** the existing row if it **already exists** (based on a constraint like a primary key or a unique column).

This is especially useful when you want to keep your data up-to-date without writing extra queries to first check if a record exists.

### 🧨 Problem
Imagine you want to insert a new book into the `books` table. But if a book with the same `book_id` already exists, you want to update its information instead of getting a conflict error.

Before PostgreSQL supported `ON CONFLICT`, you had to write complicated logic using `IF EXISTS` or perform a transaction with multiple steps.

### ✅ Solution
PostgreSQL now supports the `ON CONFLICT DO UPDATE` syntax, which makes UPSERT operations clean and simple. We can also wrap this logic in a **stored procedure** for reuse and readability.

---

## 🔁 Example: UPSERT with Stored Procedure

```sql
-- Create table
CREATE TABLE books (
    book_id INT PRIMARY KEY,
    title TEXT NOT NULL,
    author TEXT NOT NULL,
    year_published INT
);

-- Insert some initial data
INSERT INTO books (book_id, title, author, year_published) VALUES
(1, 'The Hobbit', 'J.R.R. Tolkien', 1937),
(2, '1984', 'George Orwell', 1949),
(3, 'To Kill a Mockingbird', 'Harper Lee', 1960);

-- Drop the procedure if it exists
DROP PROCEDURE IF EXISTS upsert_book;

-- Create procedure for upsert
CREATE OR REPLACE PROCEDURE upsert_book(
    p_book_id INT,
    p_title TEXT,
    p_author TEXT,
    p_year INT
)
LANGUAGE plpgsql
AS $$
BEGIN
    INSERT INTO books (book_id, title, author, year_published)
    VALUES (p_book_id, p_title, p_author, p_year)
    ON CONFLICT (book_id)
    DO UPDATE SET
        title = EXCLUDED.title,
        author = EXCLUDED.author,
        year_published = EXCLUDED.year_published;
END;
$$;

-- Usage examples
CALL upsert_book(2, 'Nineteen Eighty-Four', 'George Orwell', 1949);
CALL upsert_book(5, 'The Silent Patient', 'Alex Michaelides', 2019);
```

**Explanation:**

* This procedure performs an "upsert" — update if the record exists (based on `book_id`), or insert if it doesn’t
* Uses `ON CONFLICT (book_id) DO UPDATE` to handle duplicates by updating the existing row
* `EXCLUDED` refers to the proposed row that would have been inserted

---

## 🧠 Exercise: UPSERT for Users

```sql
-- Create user table
CREATE TABLE public_users (
    user_id SERIAL PRIMARY KEY,
    email TEXT UNIQUE NOT NULL,
    name TEXT NOT NULL,
    age INT
);

-- Sample data
INSERT INTO public_users (email, name, age) VALUES
('alice@example.com', 'Alice', 30),
('bob@example.com', 'Bob', 25);

-- Your Task:
-- Write a procedure: upsert_public_user(email, name, age)
-- It should INSERT if the email doesn't exist, or UPDATE name and age if it does

-- DROP PROCEDURE IF EXISTS upsert_public_user;
-- CREATE OR REPLACE PROCEDURE upsert_public_user(
--     p_email TEXT,
--     p_name TEXT,
--     p_age INT
-- )
-- LANGUAGE plpgsql AS $$
-- BEGIN
--     ...
-- END;
-- $$;

-- Usage Examples
-- CALL upsert_public_user('bob@example.com', 'Robert', 26);  -- Updates Bob
-- CALL upsert_public_user('charlie@example.com', 'Charlie', 40);  -- Inserts new user
```

**Tips:**

* Use `ON CONFLICT (email)` to detect duplicates
* Use `EXCLUDED.column_name` to access new values
* Make sure `email` is `UNIQUE` so `ON CONFLICT` works

Good luck 💪
