# library-management-system
Library Management System MySQL project
1. Project Description
The Library Management System is designed to help librarians and staff efficiently manage the core operations of a library. It provides a database to store and organize information about books, authors, categories, and members. The system supports issuing and returning books, tracks due dates, and enables monitoring of overdue items. It also provides administrative capabilities to generate reports such as the most borrowed books and current loans.

2. Core Features
Add, update, and delete records for books, authors, and categories.

Register new library members and maintain their contact information.

Issue books to members with an automatic due date calculation (e.g., 14 days).

Record the return of books and update availability counts accordingly.

Identify and list overdue loans where books have not been returned by their due date.

Generate reports, such as the most borrowed books and overdue items.

(Optional) Implement fine calculations for late returns.

3. Entities and Attributes
Entity	Description	Key Attributes (Fields)
Books	Stores information about library books	book_id (PK), title, author_id (FK), category_id (FK), total_copies, available_copies
Authors	Information about book authors	author_id (PK), name, bio
Categories	Book categories or genres	category_id (PK), name
Members	Registered library users	member_id (PK), name, email, phone, join_date
Loans	Records of book borrowings	loan_id (PK), member_id (FK), book_id (FK), issue_date, due_date, return_date
Admins	System administrators (optional)	admin_id (PK), username, password_hash

4. Relationships
Each Book is linked to one Author and one Category.

A Member can borrow multiple Books, recorded as separate Loans.

Each Loan associates exactly one Member with one Book and includes issue, due, and return dates.

Admins manage the system’s data and users but are not involved in the borrowing process directly.
