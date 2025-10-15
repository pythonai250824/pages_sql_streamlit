# 📘 Postgres Stored Procedures — Practice Exercises

Here are custom exercises based on the stored procedure and function examples. Each exercise explores a new twist on the examples so you can practice applying the concepts in different contexts.

### Exercise 1 — Greeting with Name and Timestamp

Create a function `greet_user(name TEXT)` that returns a greeting message including the name and current timestamp.

📥 **Example Call:**

```sql
SELECT greet_user('Alex');
-- Returns: Hello Alex! The time is 2025-07-31 15:22:01
```

### Exercise 2 — Create Table for Orders

Write a stored procedure `create_orders_table()` that creates a table called `orders` with:

* id (SERIAL primary key)
* customer\_name (TEXT, not null)
* amount (DOUBLE PRECISION, not null)

📥 **Example Call:**

```sql
CALL create_orders_table();
```

### Exercise 3 — Function to Multiply Three Numbers

Create a function `multiply_three(x DOUBLE PRECISION, y DOUBLE PRECISION, z DOUBLE PRECISION)` that returns the product.

📥 **Example Call:**

```sql
SELECT multiply_three(2, 3, 4); -- Returns 24
```

### Exercise 4 — Division and Modulo Function

Create a function `div_mod(a DOUBLE PRECISION, b DOUBLE PRECISION)` that returns:

* OUT quotient
* OUT remainder

📥 **Example Call:**

```sql
SELECT * FROM div_mod(17, 5);
-- Returns: 3.4 (quotient), 2 (remainder)
```

### Exercise 5 — Square Root and Power 4

Create a function `sp_math_roots(x DOUBLE PRECISION, y DOUBLE PRECISION)` that returns:

* OUT sum\_result
* OUT diff\_result
* OUT sqrt\_x
* OUT y\_power\_4

📥 **Example Call:**

```sql
SELECT * FROM sp_math_roots(16, 2);
-- Returns: 18, 14, 4, 16
```

### Exercise 6 — Insert Books and Authors + Get All Books with Publish Year

Create a procedure `prepare_books_db()` that creates and fills tables:

* `authors(id SERIAL, name TEXT NOT NULL)`
* `books(id SERIAL, title TEXT, price DOUBLE PRECISION, publish_date DATE, author_id INT REFERENCES authors)`

Then create a function `sp_get_books_with_year()` that returns:

* title, publish\_year, price

📥 **Book Data to Insert:**

```sql
-- Authors
INSERT INTO authors(name) VALUES ('Alice Munro'), ('George Orwell'), ('Haruki Murakami'), ('Chimamanda Ngozi Adichie');

-- Books
INSERT INTO books(title, price, publish_date, author_id) VALUES
('Lives of Girls and Women', 45.0, '1971-05-01', 1),
('1984', 30.0, '1949-06-08', 2),
('Norwegian Wood', 50.0, '1987-09-04', 3),
('Half of a Yellow Sun', 42.5, '2006-08-15', 4),
('Kafka on the Shore', 55.0, '2002-01-01', 3),
('Dear Life', 48.0, '2012-11-13', 1),
('The Thing Around Your Neck', 35.0, '2009-04-01', 4),
('Animal Farm', 28.0, '1945-08-17', 2),
('The Testaments', 60.0, '2019-09-10', 2),
('Colorless Tsukuru Tazaki', 47.5, '2013-04-12', 3);
```

📥 **Example Call:**

```sql
SELECT * FROM sp_get_books_with_year();
```

### Exercise 7 — Most Recently Published Book

Create a function `sp_latest_book()` that returns the most recent book's title and publish date.

📥 **Example Call:**

```sql
SELECT * FROM sp_latest_book();
```

### Exercise 8 — Books Summary Stats

Create a function `sp_books_summary()` that returns:

* OUT youngest\_book DATE
* OUT oldest\_book DATE
* OUT avg\_price NUMERIC(5,2)
* OUT total\_books INT

📥 **Example Call:**

```sql
SELECT * FROM sp_books_summary();
```

### Exercise 9 — Books by Year Range

Create a function `sp_books_by_year_range(from_year INT, to_year INT)` that returns:

* id, title, publish\_date, price

📥 **Example Call:**

```sql
SELECT * FROM sp_books_by_year_range(2000, 2015);
```
