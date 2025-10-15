## דף עבודה לאימון SQL - JOINs

### מבנה:

התרגיל כולל 2 טבלאות:

#### נוסעים (passengers)

* id INTEGER PRIMARY KEY
* name TEXT NOT NULL
* destination TEXT
* taxi\_id INTEGER (FOREIGN KEY)

#### מוניות (taxis)

* id INTEGER PRIMARY KEY
* driver\_name TEXT NOT NULL
* car\_type TEXT NOT NULL

### שלבים לביצוע:

#### 1. יצירת הטבלאות

```sql
CREATE TABLE taxis (
    id INTEGER PRIMARY KEY,
    driver_name TEXT NOT NULL,
    car_type TEXT NOT NULL
);

CREATE TABLE passengers (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    destination TEXT,
    taxi_id INTEGER,
    FOREIGN KEY(taxi_id) REFERENCES taxis(id)
);
```

#### 2. הוספת נתונים

```sql
INSERT INTO taxis (id, driver_name, car_type) VALUES
(1, 'Moshe Levi', 'Van'),
(2, 'Rina Cohen', 'Sedan'),
(3, 'David Azulay', 'Minibus'),
(4, 'Maya Bar', 'Electric'),
(5, 'Yossi Peretz', 'SUV');

INSERT INTO passengers (id, name, destination, taxi_id) VALUES
(1, 'Tamar', 'Jerusalem', 1),
(2, 'Eitan', 'Haifa', 2),
(3, 'Noa', 'Tel Aviv', NULL),
(4, 'Lior', 'Eilat', 1),
(5, 'Dana', 'Beer Sheva', NULL),
(6, 'Gil', 'Ashdod', 3),
(7, 'Moran', 'Netanya', NULL);
```

#### 3. שאילתות JOIN

🔸 הצג את כל הנוסעים שהשיגו מונית יחד עם פרטי המונית

🔸 הצג את כל הנוסעים כולל כאלה שמצאו מונית וכאלה שלא

🔸 הצג רק את הנוסעים שלא מצאו מונית

🔸 הצג את כל הנוסעים וכל המוניות — נוסעים בלי מונית + נוסעים עם מונית + מונית בלי נוסעים

🔸 הצג את כל הצירופים האפשריים בין נוסעים למוניות

#### 4. כתיבה והרצה בפייתון

* כתוב סקריפט פייתון המשתמש בספריית `sqlite3`  
* הסקריפט צריך ליצור את הטבלאות, להכניס את הנתונים, להריץ את השאילתות מכל הסעיפים הקודמים (INNER, LEFT וכו')  
* הדפס למסך את התוצאות של כל שאילתה עם כותרת מתאימה  

דוגמה להדפסת שאילתה:

```python
print("
--- INNER JOIN: נוסעים עם מונית ---")
cursor.execute("""
SELECT p.name, p.destination, t.driver_name, t.car_type
FROM passengers p
INNER JOIN taxis t ON p.taxi_id = t.id
""")
for row in cursor.fetchall():
    print(row)
```

🔸 הצג את כל הנוסעים שהשיגו מונית יחד עם פרטי המונית

🔸 הצג את כל הנוסעים כולל כאלה שמצאו מונית וכאלה שלא

🔸 הצג רק את הנוסעים שאין להם taxi\_id תואם

🔸 הצג את כל הנוסעים וכל המוניות — גם אם אין התאמה ביניהם

🔸 הצג את כל הצירופים האפשריים בין נוסעים למוניות




