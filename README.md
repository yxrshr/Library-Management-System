#  Library Management System (MySQL)

##  Project Overview
The **Library Management System (LMS)** is a DBMS project implemented in **MySQL** to efficiently manage the operations of a library.  
It covers functionalities such as maintaining book records, member details, issuing and returning of books, dues calculation, and reporting.  

The project uses **tables, triggers, stored procedures, and functions** to simulate a real-world library workflow.  
It also includes **error handling, audit trail via history table, and business rules enforcement**.

---

##  Purpose of the Project
- To automate and simplify library operations.  
- To track book availability, issued/returned books, and overdue fines.  
- To maintain member details, their borrowing history, and dues.  
- To provide **reports** on books, members, and publishers.  
- To enforce **business rules** (borrowing limits, dues, Sunday restriction).  

---

## Contributors 
- Yash Saxena
- Arnav Chaudhary
- Rachit Jain


##  Database Schema & Tables

### **Publisher Table**
| publisher_id | name                  | phone_no   | email                    | address                           |
|--------------|-----------------------|------------|--------------------------|-----------------------------------|
| 101          | Penguin Random House  | 9876543210 | contact@penguin.com      | 123 Publisher Street, New Delhi   |
| 102          | HarperCollins         | 9876543211 | info@harpercollins.com   | 456 Collins Avenue, Mumbai        |
| 103          | Pearson Education     | 9876543212 | support@pearson.com      | 789 Education Plaza, Bangalore    |

### **Books Table**
| book_id | name                | author            | genre      | copies_available | publisher_id |
|---------|---------------------|-------------------|------------|------------------|--------------|
| 1001    | The Alchemist       | Paulo Coelho      | Fiction    | 5                | 101          |
| 1002    | To Kill a Mockingbird | Harper Lee      | Classic    | 3                | 102          |
| 1003    | Database Systems    | Elmasri & Navathe | Technical  | 4                | 103          |
| 1004    | 1984                | George Orwell     | Dystopian  | 2                | 101          |
| 1005    | Harry Potter        | J.K. Rowling      | Fantasy    | 6                | 102          |

### **Members Table**
| member_id | name           | books_issued | dues | date_of_joining | phone_no   | email                |
|-----------|----------------|--------------|------|-----------------|------------|----------------------|
| 2001      | Yash Saxena    | 0            | 0.00 | 2024-01-15      | 9876543201 | saxenayash@gmail.com |
| 2002      | Arnav Chaudhary| 0            | 0.00 | 2024-02-20      | 9876543202 | ac@email.com         |
| 2003      | Rachit Jain    | 0            | 0.00 | 2024-03-10      | 9876543203 |jainrachit56@email.com|
| 2004      | Tom            | 0            | 0.00 | 2024-04-05      | 9876543204 | tom@email.com        |

### **Copies Table**
| copy_id | book_id | member_id | issue_date | due_date |
|---------|---------|-----------|------------|----------|
| 3001    | 1001    | NULL      | NULL       | NULL     |
| 3002    | 1001    | NULL      | NULL       | NULL     |
| 3003    | 1002    | NULL      | NULL       | NULL     |
| 3004    | 1003    | NULL      | NULL       | NULL     |
| 3005    | 1004    | NULL      | NULL       | NULL     |
| 3006    | 1005    | NULL      | NULL       | NULL     |

### **History Table**
Maintains records of all issued/returned books:  
| copy_id | book_id | member_id | issue_date | return_date |

---

##  Features Implemented

### **Triggers**
- **no_operations_on_sunday** → Prevents book issue on Sundays.  
- **book_returned** → Updates inventory and history table when a book is returned.  
- **book_issued_trigger** → Updates availability & member counters when a book is issued.  

### 🔹 **Stored Procedures**
- `display_books(id, name)` → Shows details of a book.  
- `display_members(id, name)` → Shows member details.  
- `display_publisher(id, name)` → Shows publisher details & their books.  
- `book_issued(copy_id, member_id)` → Issues a book to a member.  
- `book_returned(copy_id)` → Returns a book.  
- `calculate_dues()` → Calculates dues for all members.  
- `dues_paid(member_id, amount)` → Updates dues after payment.  

### 🔹 **Function**
- `dues_calc(member_id)` → Calculates overdue fines (₹5/day).  

---

##  Business Rules
-  No book operations allowed on **Sundays**.  
-  Maximum **5 books per member**.  
-  Fine of **₹5 per day** for overdue books.  
-  Books cannot be issued if dues exceed **₹200**.  
-  Automatic inventory and dues update.  

---

##  Usage Instructions
1. Run the script in **MySQL Workbench / CLI**:
   ```sql
   SOURCE Library_Management_System_Saxena.sql;


## Common operations : 
- CALL display_books(1001, NULL);       -- Show book details
- CALL display_members(2001, NULL);    -- Show member details
- CALL book_issued(3001, 2001);        -- Issue book copy 3001 to member 2001
- CALL book_returned(3001);            -- Return issued book
- CALL calculate_dues();               -- Update dues for all members
- CALL dues_paid(2001, 100);           -- Member pays Rs. 100


## Compatibility

 MySQL 5.7+ and MySQL 8.0+

Works in MySQL Workbench, CLI, and phpMyAdmin.

## Reports Available 

Issued books → SELECT * FROM copies WHERE member_id IS NOT NULL;

Transaction history → SELECT * FROM history;

Books by Genre / Publisher / Member Activity

## Final Notes
This project demonstrates a realistic library workflow using SQL features like triggers, functions, procedures, and constraints.
It can be extended into a full-stack Library Management application by integrating with Java, Python, or a Web Framework.