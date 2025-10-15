## 🔐 What is a Transaction in SQL?

A **transaction** is a sequence of one or more SQL operations executed as a single unit. If **all** operations succeed, the changes are saved to the database. If **any** operation fails, all changes are rolled back, leaving the database unchanged.

### 💡 Why Use Transactions?

- Prevent data corruption or inconsistency
- Ensure all-or-nothing logic (like bank transfers)
- Protect data from partial updates or system failures

### ✅ `COMMIT`
Finalizes a transaction and saves all changes made during the transaction to the database.

### ❌ `ROLLBACK`
Cancels the entire transaction and undoes all changes made during it.

## 💳 Example: Transferring Money Between Accounts

#### 📘 Explanation of Key Parts

* `try:` block starts the transaction manually
* `UPDATE` queries simulate money transfer
* `commit()` applies the changes permanently
* `rollback()` cancels all operations if something goes wrong

```python
import psycopg2
import psycopg2.extras

# Connect to PostgreSQL
db_connection = psycopg2.connect(
    dbname='postgres',
    user='postgres',
    password='admin',
    host='localhost',
    port='5432',
    cursor_factory=psycopg2.extras.RealDictCursor
)

cursor = db_connection.cursor()

def create_table():
    # 1. Create a table for bank accounts
    cursor.execute("""
        CREATE TABLE IF NOT EXISTS bank_account (
            id SERIAL PRIMARY KEY,
            name TEXT NOT NULL,
            balance NUMERIC CHECK (balance >= 0)
        )
    """)

def insert_new_account():
    # 2. Insert initial test data
    cursor.execute("DELETE FROM bank_account")  # Clean the table
    cursor.execute("INSERT INTO bank_account (name, balance) VALUES (%s, %s)", ("Alice", 1000))
    cursor.execute("INSERT INTO bank_account (name, balance) VALUES (%s, %s)", ("Bob", 100))
    print("Initial data inserted.\n")

create_table()
insert_new_account()

# 🎯 Transaction: Transfer 200 from Bob to Alice
try:
    print("Starting transaction to transfer money...")

    # Step 1: Add 200 to Alice (id=1)
    cursor.execute("UPDATE bank_account SET balance = balance + 200 WHERE id = 1")

    # Step 2: Subtract 200 from Bob (id=2)
    cursor.execute("UPDATE bank_account SET balance = balance - 200 WHERE id = 2")

    db_connection.commit()  # ✅ Commit changes if both succeed
    print("Transaction successful. Changes committed.")

except Exception as e:
    db_connection.rollback()  # ❌ Roll back if there's an error
    print("Transaction failed. All changes rolled back.")
    print("Error:", e)

# 🧾 Show final state
cursor.execute("SELECT id, name, balance FROM bank_account ORDER BY id")
rows = cursor.fetchall()
print("Final state of bank_account:")
for row in rows:
    print(row)

db_connection.close()
```


