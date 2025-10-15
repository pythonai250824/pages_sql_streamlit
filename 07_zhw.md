## Homework: PostgreSQL + Streamlit in Python

This assignment will guide you through basic usage of PostgreSQL and Streamlit in Python

## Part 1: PostgreSQL + Python

### Step 1: Create a new table

Create a table called `products`:

```sql
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    price NUMERIC(6, 2) NOT NULL,
    in_stock BOOLEAN DEFAULT TRUE
);
```

### Step 2: Insert Data

Insert a few example rows into the table:

```sql
INSERT INTO products (name, price, in_stock) VALUES
('Laptop', 3200.50, TRUE),
('Mouse', 99.99, TRUE),
('Keyboard', 250.00, FALSE),
('Monitor', 1190.95, TRUE);
```

### Step 3: Select With Condition

Select only products that are in stock:

```sql
SELECT * FROM products WHERE in_stock = TRUE;
```

---

### ❗ Your Task — Python Code

1. Write Python code that connects to the database
2. Runs the 3 SQL statements: CREATE, INSERT, SELECT
3. Prints the result of the final SELECT query
4. Use `psycopg2` and `RealDictCursor`
5. Handle exceptions using `try-except-finally`

---

## Part 2: Streamlit App

Create a simple Streamlit program with **two separate sections**:

### Section A: Calculator UI

1. Show your name using `st.title()`
2. Add two number input fields using `st.text_input()`
3. Add a button labeled "Add"
4. When clicked, it should:

   * Add the two numbers
   * Display the result using `st.success()`

### Section B: Show Available Products

1. Add a subheader like "Available Products"
2. When clicking a button like "Show Products":

   * Connect to the PostgreSQL database
   * Run the query:

   ```sql
   SELECT * FROM products WHERE in_stock = TRUE
   ```

   * Show the result using `st.write()`

---

### Example Output:

```
👤 Name: Alice Cohen
Number 1: 5
Number 2: 8
✅ Sum: 13

📦 Available Products:
{'product_id': 1, 'name': 'Laptop', 'price': 3200.50, 'in_stock': True}
...
```

---

### 📌 Submission

Submit both files:

* `pg_script.py` with your psycopg2 code
* `streamlit_app.py` with the Streamlit code

## 📤 הגשה

יש לשלוח את הפתרון עם כל שלבי התרגול והמימושים למייל:
📧 [pythonai200425+sql7@gmail.com](mailto:pythonai200425+sql7@gmail.com)
