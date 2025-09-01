# LEGOIO-SQL-project
Library Management System: Design a simple SQL database for a library that keeps track of books, members, and borrowing activities. Write queries to answer common business questions.
1. Create Tables
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
2. Insert Sample Data
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
3. Practice Queries with Solutions
Q1: List all members who borrowed books.
SELECT DISTINCT m.name
FROM Members m
JOIN Borrowing b ON m.member_id = b.member_id;
Q2: Show all books currently borrowed (not yet returned).
SELECT bk.title, m.name AS borrowed_by
FROM Books bk
JOIN Borrowing b ON bk.book_id = b.book_id
JOIN Members m ON m.member_id = b.member_id
WHERE b.return_date IS NULL;
Q3: Count how many books each member borrowed.
SELECT m.name, COUNT(b.borrow_id) AS total_borrowed
FROM Members m
LEFT JOIN Borrowing b ON m.member_id = b.member_id
GROUP BY m.name;
Q4: Find the most borrowed book.
SELECT bk.title, COUNT(b.borrow_id) AS borrow_count
FROM Books bk
JOIN Borrowing b ON bk.book_id = b.book_id
GROUP BY bk.title
ORDER BY borrow_count DESC
LIMIT 1;
Q5: Update book copies when borrowed (example: member 2 returns book 102).
UPDATE Books
SET available_copies = available_copies + 1
WHERE book_id = 102;

UPDATE Borrowing
SET return_date = '2023-04-20'
WHERE borrow_id = 1002;
