# 🧠 שיעורי בית – עבודה עם SQLite בפייתון

## 📋 הקדמה

בתרגיל זה נתרגל שימוש בסיסי ב־SQLite דרך שפת Python
נעבור שלב אחר שלב ביצירת מסד נתונים, יצירת טבלה, הכנסת נתונים, שליפה, עדכון ומחיקה
המטרה היא להבין את הזרימה של עבודה עם sqlite3 כולל עבודה עם קלט מהמשתמש וטיפול בשגיאות

## ✅ שלבים לתרגול

### 1️⃣ מחיקת קובץ מסד נתונים קודם אם קיים

```python
import os
if os.path.exists('students.db'):
    os.remove('students.db')
```

### 2️⃣ יצירת חיבור וקובץ חדש בשם `students.db`

```python
import sqlite3
conn = sqlite3.connect('students.db')
conn.row_factory = sqlite3.Row
cursor = conn.cursor()
```

### 3️⃣ יצירת טבלה בשם STUDENTS

```python
cursor.execute('''
CREATE TABLE IF NOT EXISTS STUDENTS(
  ID INTEGER PRIMARY KEY,
  NAME TEXT NOT NULL,
  GRADE INTEGER NOT NULL,
  BIRTHYEAR INTEGER
);
''')
```

### 4️⃣ הוספת נתונים בקבוצה

```python
data = [
  (1, 'Noa', 85, 2010),
  (2, 'Lior', 90, 2011),
  (3, 'Dana', 78, 2009)
]
```

הוסף רשומות אלה בשאילתא אחת

### 5️⃣ עדכון נתון

עדכן את הציון של Dana ל־88
(השתמש בפרמטרים `?` בשאילתה)

### 6️⃣ מחיקת תלמיד

מחק את התלמיד עם ID = 2

### 7️⃣ הצגת כל השורות

כתוב קוד שמציג את כל התלמידים שנמצאים בטבלה באמצעות `fetchall`

### 8️⃣ קבלת קלט מהמשתמש

כתוב קוד שמבקש מהמשתמש להזין ID, שם, ציון ושנת לידה ומכניס תלמיד חדש לטבלה
הוסף לולאה עם `while True` וטיפול בשגיאות עם `try-except` כדי לוודא שהקלט תקין
הצג את השורה שהוזנה לאחר ההכנסה

