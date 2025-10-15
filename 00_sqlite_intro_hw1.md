# 🧠 שיעורי בית – טבלת STUDENTS

## 📋 הקדמה

בתרגול זה נעבוד על טבלה חדשה בשם `STUDENTS`, המדמה רשימת תלמידים עם ציונים, כיתות ושנת לידה  
המטרה היא לתרגל שאילתות בסיסיות ב־SQL תוך שימוש בפקודות  
SELECT, WHERE, ORDER BY, LIMIT, OFFSET, UPDATE, DELETE, GROUP BY ועוד  

תחילה ניצור את הטבלה ונטעין לתוכה נתונים, ולאחר מכן נבצע עליהם פעולות שונות  
כל שאלה בתרגול מתבססת על מה שלמדנו יחד בשיעור

## ✅ יצירת הטבלה

```sql
CREATE TABLE STUDENTS (
  ID INTEGER PRIMARY KEY,
  NAME TEXT NOT NULL,
  GRADE INTEGER,
  CLASS TEXT,
  BIRTHYEAR INTEGER
)
```

## ✅ הכנסת נתונים לדוגמה

```sql
INSERT INTO STUDENTS (ID, NAME, GRADE, CLASS, BIRTHYEAR) VALUES (1, 'Liam', 95, 'H1', 2010)
INSERT INTO STUDENTS (ID, NAME, GRADE, CLASS, BIRTHYEAR) VALUES (2, 'Emma', 88, 'H2', 2011)
INSERT INTO STUDENTS (ID, NAME, GRADE, CLASS, BIRTHYEAR) VALUES (3, 'Noah', 76, 'W1', 2009)
INSERT INTO STUDENTS (ID, NAME, GRADE, CLASS, BIRTHYEAR) VALUES (4, 'Olivia', 83, 'W2', 2008)
INSERT INTO STUDENTS (ID, NAME, GRADE, CLASS, BIRTHYEAR) VALUES (5, 'Ava', 91, 'H1', 2010)
INSERT INTO STUDENTS (ID, NAME, GRADE, CLASS, BIRTHYEAR) VALUES (6, 'Ethan', 67, 'Z1', 2007)
INSERT INTO STUDENTS (ID, NAME, GRADE, CLASS, BIRTHYEAR) VALUES (7, 'Mia', 72, 'W1', 2009)
INSERT INTO STUDENTS (ID, NAME, GRADE, CLASS, BIRTHYEAR) VALUES (8, 'Tom', 55, 'H2', 2012)
INSERT INTO STUDENTS (ID, NAME, GRADE, CLASS, BIRTHYEAR) VALUES (9, 'Dana', 89, 'W2', 2010)
INSERT INTO STUDENTS (ID, NAME, GRADE, CLASS, BIRTHYEAR) VALUES (10, 'Isaac', 78, 'Z1', 2006)
```

## ✍️ תרגיל 1: שאילתות SELECT

1 הצג את כל התלמידים בטבלה  
2 הצג רק את השם והציון של כל תלמיד  
3 הצג את ממוצע הציונים  
4 מצא את שנת הלידה הכי מוקדמת  

## ✍️ תרגיל 2: סינון עם WHERE

5 הצג את כל התלמידים שקיבלו ציון מעל 80  
6 הצג את כל התלמידים שנולדו אחרי 2005  
7 הצג את כל התלמידים בכיתה H1  
8 הצג תלמידים ששמם מכיל את האות i  

## ✍️ תרגיל 3: מיון והגבלה

9 הצג את שלושת התלמידים עם הציון הגבוה ביותר  
10 הצג את שמות התלמידים לפי שנת לידה מהישן לחדש  
11 הצג את שלושת התלמידים הראשונים בטבלה  
12 דלג על 2 שורות והצג את 3 הבאות  

## ✍️ תרגיל 4: עדכון ומחיקה

13 עדכן את הציון של תלמיד בשם Dana ל־100  
14 שנה את הכיתה ל־GRADUATED לכל תלמיד שנולד לפני 2010  
15 מחק את התלמיד ששמו Tom  

## ⭐ תרגיל בונוס - רשות**

16 הצג את מספר התלמידים בכל כיתה  
17 הצג את גיל כל תלמיד לפי שנת לידה  
18 הצג את התלמיד עם הציון הכי נמוך  

## 📤 הגשה
הפתרון יכלול קובץ טקסט של השאילתות - יש לציין מספר שאלה ליד כל פתרון!  
יש לעלות את הפתרונות ל- REPO משלכם ב- GITHUB ושלוח לינק לקובץ   
(מי שמסתבך יכול הפעם לשלוח קובץ טקסט)  
שמרו על סדר השאלות וכתיבה מדויקת של SQL  

