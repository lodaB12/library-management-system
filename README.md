# Library Management System

A MySQL-based database project for managing library operations, including books, members, loans, and more.

---

## Project Overview

The Library Management System is designed to help librarians and staff efficiently manage core operations of a library. It provides a database to store and organize information about books, authors, categories, and members. The system supports issuing and returning books, tracks due dates, and enables monitoring of overdue items.

### Core Features

- Add, update, and delete records for books, authors, and categories  
- Register new library members and store their contact details  
- Issue and return books, update availability automatically  
- Track overdue loans and return statuses  
- Generate useful reports: most borrowed books, member borrowing history, overdue items  
- (Optional) Add fine calculation system for late returns  

---

## Entities and Attributes

| Entity     | Description                         | Key Attributes |
|------------|-------------------------------------|----------------|
| **Books**    | Library book data                  | `book_id`, `title`, `author_id`, `category_id`, `total_copies`, `available_copies` |
| **Authors**  | Info about book authors            | `author_id`, `name`, `bio` |
| **Categories** | Book categories or genres        | `category_id`, `name` |
| **Members**  | Registered library users           | `member_id`, `name`, `email`, `phone`, `join_date` |
| **Loans**    | Book borrow and return tracking    | `loan_id`, `member_id`, `book_id`, `issue_date`, `due_date`, `return_date` |
| **Admins**   | System admins (optional)           | `admin_id`, `username`, `password_hash` |

---

## ER Diagram

Library Management System Diagram.png

---

## SQL Schema

```sql
CREATE TABLE Authors (
    author_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    bio TEXT
);

CREATE TABLE Categories (
    category_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) NOT NULL
);

CREATE TABLE Books (
    book_id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(150) NOT NULL,
    author_id INT,
    category_id INT,
    total_copies INT DEFAULT 1,
    available_copies INT DEFAULT 1,
    FOREIGN KEY (author_id) REFERENCES Authors(author_id),
    FOREIGN KEY (category_id) REFERENCES Categories(category_id)
);

CREATE TABLE Members (
    member_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE,
    phone VARCHAR(15),
    join_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE Loans (
    loan_id INT AUTO_INCREMENT PRIMARY KEY,
    member_id INT,
    book_id INT,
    issue_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    due_date DATE,
    return_date DATE,
    FOREIGN KEY (member_id) REFERENCES Members(member_id),
    FOREIGN KEY (book_id) REFERENCES Books(book_id)
);

CREATE TABLE Admins (
    admin_id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL
);


EXAMPLE QUERIES
1. List All Available Books
SELECT book_id, title, total_copies, available_copies
FROM Books
WHERE available_copies > 0;
Fetches all books currently available for borrowing.

2. List All Registered Members
SELECT member_id, name, email, phone, join_date
FROM Members;
Retrieves all members registered in the system.

3. Find All Books Currently Loaned Out
SELECT Loans.loan_id, Members.name AS member_name, Books.title AS book_title, Loans.issue_date, Loans.due_date
FROM Loans
JOIN Members ON Loans.member_id = Members.member_id
JOIN Books ON Loans.book_id = Books.book_id
WHERE Loans.return_date IS NULL;
Shows loans where books have been issued but not yet returned.

4. List Overdue Loans
SELECT Members.name AS member_name, Books.title AS book_title, Loans.due_date
FROM Loans
JOIN Members ON Loans.member_id = Members.member_id
JOIN Books ON Loans.book_id = Books.book_id
WHERE Loans.return_date IS NULL AND Loans.due_date < CURDATE();
Lists loans overdue (not returned and past due date).

5. Borrowing History of a Specific Member (Example: Member ID 1)
SELECT Books.title, Loans.issue_date, Loans.due_date, Loans.return_date
FROM Loans
JOIN Books ON Loans.book_id = Books.book_id
WHERE Loans.member_id = 1;
Shows all loan history for a particular member.

6. Count of How Many Times Each Book Has Been Borrowed
SELECT Books.title, COUNT(Loans.loan_id) AS times_borrowed
FROM Books
LEFT JOIN Loans ON Books.book_id = Loans.book_id
GROUP BY Books.book_id
ORDER BY times_borrowed DESC;
Counts total loans per book to find popular titles.

7. List Authors and Number of Books Written
SELECT Authors.name, COUNT(Books.book_id) AS book_count
FROM Authors
LEFT JOIN Books ON Authors.author_id = Books.author_id
GROUP BY Authors.author_id;
Shows authors and how many books they have in the library.
