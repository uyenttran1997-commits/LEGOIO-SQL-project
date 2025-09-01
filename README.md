# 📚 Library Management System (SQL Project)

## 📌 Project Overview

This project demonstrates a simple **Library Management System** built with SQL.
It manages **members, books, and borrowing records**, and includes queries to answer common library-related questions.

The goal is to practice **SQL database design, data insertion, and querying**.

---

## 🏗️ Database Schema

We designed 3 main tables:

* **Members** → Stores library members.
* **Books** → Stores book information and available copies.
* **Borrowing** → Tracks borrowing and returning of books.

```sql
-- Members Table
CREATE TABLE Members (
    member_id INT PRIMARY KEY,
    name VARCHAR(100),
    join_date DATE
);

-- Books Table
CREATE TABLE Books (
    book_id INT PRIMARY KEY,
    title VARCHAR(200),
    author VARCHAR(100),
    available_copies INT
);

-- Borrowing Table
CREATE TABLE Borrowing (
    borrow_id INT PRIMARY KEY,
    member_id INT,
    book_id INT,
    borrow_date DATE,
    return_date DATE,
    FOREIGN KEY (member_id) REFERENCES Members(member_id),
    FOREIGN KEY (book_id) REFERENCES Books(book_id)
);
```

---

## 📥 Sample Data

```sql
INSERT INTO Members VALUES
(1, 'Alice', '2023-01-10'),
(2, 'Bob', '2023-02-15'),
(3, 'Charlie', '2023-03-20');

INSERT INTO Books VALUES
(101, 'The Great Gatsby', 'F. Scott Fitzgerald', 3),
(102, '1984', 'George Orwell', 2),
(103, 'To Kill a Mockingbird', 'Harper Lee', 4);

INSERT INTO Borrowing VALUES
(1001, 1, 101, '2023-04-01', '2023-04-10'),
(1002, 2, 102, '2023-04-05', NULL),
(1003, 3, 103, '2023-04-07', NULL);
```

---

## 🔍 Example Queries & Solutions

### 1️⃣ List all members who borrowed books

```sql
SELECT DISTINCT m.name
FROM Members m
JOIN Borrowing b ON m.member_id = b.member_id;
```

### 2️⃣ Show all books currently borrowed (not returned yet)

```sql
SELECT bk.title, m.name AS borrowed_by
FROM Books bk
JOIN Borrowing b ON bk.book_id = b.book_id
JOIN Members m ON m.member_id = b.member_id
WHERE b.return_date IS NULL;
```

### 3️⃣ Count how many books each member borrowed

```sql
SELECT m.name, COUNT(b.borrow_id) AS total_borrowed
FROM Members m
LEFT JOIN Borrowing b ON m.member_id = b.member_id
GROUP BY m.name;
```

### 4️⃣ Find the most borrowed book

```sql
SELECT bk.title, COUNT(b.borrow_id) AS borrow_count
FROM Books bk
JOIN Borrowing b ON bk.book_id = b.book_id
GROUP BY bk.title
ORDER BY borrow_count DESC
LIMIT 1;
```

### 5️⃣ Update book copies when returned

```sql
UPDATE Books
SET available_copies = available_copies + 1
WHERE book_id = 102;

UPDATE Borrowing
SET return_date = '2023-04-20'
WHERE borrow_id = 1002;
```

---

## 🚀 How to Run

1. Clone this repository
2. Run `schema.sql` to create tables
3. Run `data.sql` to insert sample data
4. Run queries from `queries.sql`


Do you also want me to structure the repo with **3 SQL files + README** (so I write the actual file contents for you), or do you just want to keep everything in README for simplicity?
