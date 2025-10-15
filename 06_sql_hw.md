## שאלות על SQLite

### 1. AUTOINCREMENT

**שאלה:**
שנה את מבנה הטבלה כך שהמזהה יעבוד עם `AUTOINCREMENT`, והכנס עוד שלוש רשומות ללא ציון מזהה.

```sql
CREATE TABLE members (
    id INTEGER PRIMARY KEY,
    full_name TEXT NOT NULL
);

INSERT INTO members (id, full_name) VALUES
(101, 'Shira Levi'),
(102, 'Nadav Cohen'),
(103, 'Yael Azulay');
```

### 2. UNION

**שאלה:**
יש שתי טבלאות: `patients` ו־`nurses`, שתיהן כוללות עמודת `name`. כתוב שאילתה שתחזיר את רשימת כל השמות מתוך שתי הטבלאות – בלי כפילויות

```sql
CREATE TABLE patients (
    id INTEGER PRIMARY KEY,
    name TEXT
);

CREATE TABLE nurses (
    id INTEGER PRIMARY KEY,
    name TEXT
);

INSERT INTO patients (name) VALUES ('Yoni'), ('Dana'), ('Avi');
INSERT INTO nurses (name) VALUES ('Avi'), ('Tamar'), ('Lior');
```

### 3. ON DELETE CASCADE

**שאלה:**
נסה למחוק במאי שיש לו סרטים – וראה שהמחיקה נכשלת. לאחר מכן, שנה את הגדרת המפתח הזר כדי לאפשר מחיקה cascading

```sql
CREATE TABLE directors (
    director_id INTEGER PRIMARY KEY,
    name TEXT
);

CREATE TABLE movies (
    movie_id INTEGER PRIMARY KEY,
    title TEXT,
    director_id INTEGER,
    FOREIGN KEY(director_id) REFERENCES directors(director_id)
);

INSERT INTO directors (name) VALUES ('Spielberg');
INSERT INTO movies (title, director_id) VALUES ('Jaws', 1);
```

## שאלות על PostgreSQL

### 4. SERIAL

**שאלה:**
 שנה את מבנה הטבלה כך שהמזהה יעבוד עם SERIAL, והכנס שלוש רשומות נוספות ללא ציון מזהה

```sql
CREATE TABLE trainers (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL
);

INSERT INTO trainers (id, name) VALUES
(201, 'Erez Barak'),
(202, 'Liat Ben-Haim'),
(203, 'Gil Oren');
```

### 5. INSERT with RANDOM

**שאלה:**
כתוב שאילתה שמכניסה שלוש רשומות כאשר הערך הוא מספר אקראי בין 5 ל־15, עם שתי ספרות אחרי הנקודה

```sql
CREATE TABLE measurements (
    id SERIAL PRIMARY KEY,
    value NUMERIC(5,2)
);
```

### 6. JSONB במקום קשר M\:N

**שאלה:**
שנה את המבנה כך שהכול יישמר בטבלה אחת בשם `enrollments` עם שדה JSONB שמכיל את רשימת הקורסים של כל סטודנט

דוגמא לעמודת- JOSNB
```
{
  "math": "2024-11-01",
  "history": "2024-11-15"
}
```

```sql
CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL
);

CREATE TABLE courses (
    id SERIAL PRIMARY KEY,
    title TEXT NOT NULL,
    course_date DATE NOT NULL
);

CREATE TABLE student_courses (
    student_id INTEGER REFERENCES students(id),
    course_id INTEGER REFERENCES courses(id),
    PRIMARY KEY (student_id, course_id)
);

INSERT INTO students (name) VALUES
('Roni'),
('Alon');

INSERT INTO courses (title, course_date) VALUES
('math', '2024-11-01'),
('history', '2024-11-15');

INSERT INTO student_courses (student_id, course_id) VALUES
(1, 1),
(1, 2),
(2, 1);
```
