# PHP Web Development Course - 13 Chapter Curriculum

## 📋 Course Overview

- **Grade Level**: 2nd Year (Prerequisites: HTML, CSS, JavaScript, MySQL fundamentals)
- **Total Instruction Time**: 13 chapters × 3 hours = 39 hours
- **Time Allocation**: 70% Lecture, 30% Hands-on Practice
- **Course Objectives**:
  - Foundation Phase (60%, Chapters 1-8): Master PHP basics and MySQL integration for service development
  - Professional Phase (40%, Chapters 9-13): Develop practical skills through board system implementation
  - Final Project: Deepen development capabilities through real-world implementation

---

## 📅 Detailed Chapter Curriculum

### **Chapter 1: Web Development Review & PHP Environment Setup**

**Learning Objectives**: Review web development fundamentals and set up PHP development environment

**Lecture Content**

- HTML/CSS/JavaScript Core Review
  - Key HTML tags (form, input, table)
  - CSS selectors and basic styling
  - JavaScript DOM manipulation (getElementById, addEventListener)
- MySQL Fundamentals Review
  - CREATE TABLE, INSERT, SELECT, WHERE syntax
  - Basic JOIN concepts
  - Data types (INT, VARCHAR, DATE)
- PHP Environment Setup
  - Verify Apache, PHP, MySQL installation
  - Basic php.ini configuration
  - Execute and verify first PHP file output

**Practice Activities**

- Execute simple PHP file in local environment (Apache + PHP + MySQL)
- Output "Hello, PHP World!"
- Verify database access through PHPMyAdmin

---

### **Chapter 2: PHP Basic Syntax & Variables, Data Types**

**Learning Objectives**: Understand PHP syntax fundamentals and utilize variables and data types

**Lecture Content**

- PHP Basic Syntax
  - PHP tags (`<?php ?>`)
  - Difference between echo and print statements
  - Variable inspection with var_dump()
  - Comment writing (// /* */)
- Variables and Constants
  - Variable declaration rules ($variable_name)
  - Dynamic type system
  - Constant definition (define, const)
- Data Types
  - String, Integer, Float
  - Boolean, Array, Object, NULL
  - gettype(), settype() functions
- Operators
  - Arithmetic operators (+, -, *, /, %)
  - Logical operators (&&, ||, !)
  - Comparison operators (==, ===, !=, !==)
  - String concatenation (.)

**Practice Activities**

- Variable declaration and output
- Declare various data types and verify with var_dump()
- Receive user input (GET/POST), store in variables, and output
- Simple calculator: Accept two numbers and output sum

---

### **Chapter 3: Conditionals, Loops & Functions**

**Learning Objectives**: Understand control flow and define and utilize functions

**Lecture Content**

- Conditional Statements
  - if / else / elseif statements
  - switch statement and case structure
  - Ternary operator (? :)
- Loops
  - for loop (initialization, condition, increment)
  - foreach loop (array traversal)
  - while / do-while loops
  - break, continue control
- Functions
  - Function definition (function keyword)
  - Parameters and return values
  - Function scope (global keyword)
  - Recursive function concepts
- Built-in Function Utilization
  - String functions: strlen(), substr(), strpos(), strtoupper()
  - Array functions: count(), array_push(), array_pop(), in_array()
  - Math functions: abs(), round(), max(), min()

**Practice Activities**

- Write registration validation logic
  - Check username length (5+ characters)
  - Verify password match
  - Validate email format
- Output multiplication table (nested for loops)
- Find specific value in array (foreach)
- Use functions: temperature conversion, average calculation, etc.

---

### **Chapter 4: MySQL Review & PHP-MySQL Integration**

**Learning Objectives**: Access MySQL database from PHP and process data securely

**Lecture Content**

- MySQL Review
  - CREATE TABLE (table creation, field definition)
  - INSERT (data insertion)
  - SELECT (data retrieval)
  - UPDATE (data modification)
  - DELETE (data deletion)
  - Conditional specification with WHERE clause
- PHP-MySQL Connection Method Comparison
  - PDO (PHP Data Objects) introduction and examples
    ```
    $pdo = new PDO('mysql:host=localhost;dbname=test', 'user', 'password');
    ```
  - MySQLi (MySQL Improved) introduction and examples
    ```
    $mysqli = new mysqli('localhost', 'user', 'password', 'test');
    ```
  - Pros and cons comparison (demonstrated with actual code)
    - PDO: Multiple DB support, more modern syntax
    - MySQLi: MySQL-specific, supports both procedural and object-oriented styles
  - **Conclusion**: This course will use PDO
- SQL Injection Attacks and Defense
  - What is SQL Injection? (malicious users manipulating queries)
  - Necessity and operation principle of Prepared Statements (parameterized queries)
  - Using placeholders (?) method (concrete practice)

**Practice Activities**

- Create database and tables
  - students table (id, name, email, age, score)
- Retrieve student information from PHP (using PDO)
  - Query and display all students in HTML table
  - Query using Prepared Statement (using placeholder ?)
- Secure search functionality
  - Compare cases with and without Prepared Statement
  - Understand why Prepared Statement is necessary

---

### **Chapter 5: Form Processing & GET/POST Requests**

**Learning Objectives**: Receive user input, perform basic validation, and process it securely

**Lecture Content**

- Form Basics Review
  - HTML form tag
  - Understanding method="GET" vs method="POST" difference
  - input, textarea, select elements
- PHP Form Processing (Basics)
  - Receive form data with $_POST variable array
  - Understand $_GET variable array (focus on POST usage)
  - Receive and verify form data
- Basic Data Validation
  - Check required fields (empty, isset functions)
  - Verify string length (strlen function)
  - Simple email format check (characters + @ + characters)
  - Number verification (is_numeric function)
- Data Security Processing (Basics)
  - htmlspecialchars function (HTML special character conversion) - brief introduction
  - Remaining security concepts will be covered together in Chapter 6

**Practice Activities**

- Create simple search form
  - Search term input field (single field only)
  - Receive search term via $_POST
  - Check required input (if empty, display "Please enter search term")
  - If search term exists, search student table by name and display results
- Display basic error messages
  - If no search results, output "No search results found"

---

### **Chapter 6: Session & Cookie & Login System**

**Learning Objectives**: Understand sessions and cookies, and implement basic login/logout functionality

**Lecture Content**

- Session Concept and Operation Principle
  - What is a session? (storing user information on server)
  - Using session_start() function
  - Storing data in $_SESSION array
  - Session termination (session_destroy)
- Cookie Concept (Basics)
  - What is a cookie? (storing information on client)
  - Using setcookie() function (brief introduction)
  - Understanding cookies vs sessions
- Login/Logout System (Basics)
  - User authentication process (verify ID, password)
  - Password verification: using password_verify function
  - Login success → store user_id, username in $_SESSION
  - Logout → delete session
- Security Considerations (explain 2 main points)
  - Importance of password hashing (why password_hash, password_verify are needed)
  - Login verification: check session before page access (if(isset($_SESSION['user_id'])))
  - Remaining security (CSRF, XSS, session hijacking) will be learned in assignments

**Practice Activities**

- Basic login system (simple)
  - login.php: login form
    - Username, password input fields (2 fields only)
  - Search for user by ID in users table
  - Verify password with password_verify()
  - Authentication success → store $_SESSION['user_id'], $_SESSION['username']
  - Authentication failure → "Incorrect ID or password"
- Logout functionality
  - logout.php: delete session, redirect to login page
- Simple my page
  - Only logged-in users can access
  - Display user's name
  - Logout button

---

### **Chapter 7: File Upload & Error Handling**

**Learning Objectives**: Understand file upload principles and perform basic validation and error handling

**Lecture Content**

- File Upload Basics
  - Set enctype="multipart/form-data" in HTML form
  - Understanding $_FILES array structure
    - $_FILES['filename']['name']: original filename
    - $_FILES['filename']['type']: MIME type
    - $_FILES['filename']['size']: file size
    - $_FILES['filename']['tmp_name']: temporary path
    - $_FILES['filename']['error']: error code
  - Save file with move_uploaded_file() function
- Basic File Validation
  - Verify file was actually uploaded
  - Check file size limit
  - Check file extension (simple: allow only .jpg, .png)
  - Safe filename processing (timestamp + original name)
- Error Handling (Basics)
  - Handle upload exceptions with try-catch block
  - Provide user-friendly error messages
  - Stop script with die(), exit() functions
- Debugging Techniques (brief introduction)
  - Check variables with var_dump(), print_r()
  - error_log() function (optional)

**Practice Activities**

- Simple image upload (focus on understanding principles)
  - Create file selection form (single file only)
  - Check and output $_FILES array
  - Allow only image files (.jpg, .png)
  - Limit file size to 2MB or less
  - Save with safe filename in uploads/ folder
  - Display upload result (success/failure message)

---

### **Chapter 8: Data Management & Comprehensive Practice (First Project)**

**Learning Objectives**: Integrate content learned over the past 7 weeks to develop basic web application

**Lecture Content - Choose one of two projects to proceed**

**Option 1: Simple TODO Management System**

*Parts to implement together in class:*

- Database design: todos table (id, user_id, title, status, created_at)
- TODO add functionality
  - Title input form
  - INSERT with logged-in user's user_id
  - Redirect to list page after completion
- TODO list retrieval and output
  - Display only logged-in user's TODOs
  - Sort by latest first
  - Display title, status (complete/incomplete), creation date
- TODO edit functionality
  - TODO title edit form
  - Pre-fill form with existing data (SELECT)
  - Modify data with UPDATE query
  - Return to list after editing

*Parts to expand as assignments:*

- Stage 1: Change TODO status (complete/incomplete)
  - UPDATE status column (simple toggle button)
- Stage 2: Delete TODO
  - Confirmation message on delete button click
  - Delete with DELETE query
- Optional: Add pagination (display 5 per page)

---

**Option 2: Simple Product Management System (For Administrators)**

*Parts to implement together in class:*

- Database design: products table (id, name, price, stock, created_at)
- Product add functionality
  - Product name, price, stock input form
  - Login verification (administrators only)
  - Save to database with INSERT
  - Redirect to list page after completion
- Product list retrieval and output
  - Display all products
  - Display product name, price, stock, creation date
  - Sort by latest first
  - Display edit, delete buttons for each product
- Product edit functionality
  - Product edit form
  - Pre-fill form with existing data (SELECT)
  - Modify data with UPDATE query
  - Return to list after editing

*Parts to expand as assignments:*

- Stage 1: Delete products
  - Confirmation message on delete button click
  - Delete with DELETE query
- Stage 2: Search products
  - Add search form to search by product name
  - Partial search with LIKE operator
- Optional: Image upload (apply file upload functionality learned in Chapter 7)

---

**Class Procedure**

1. Select one project (instructor decides based on student learning level)
2. During class time, implement everything from database design to edit functionality
3. In assignments, expand capabilities by implementing additional features like delete, status change, search

---

### **Chapter 9: Board System Implementation (1/3) - Basic Board Features**

**Learning Objectives**: Understand board basics by implementing CRUD functionality from database design

**Lecture Approach**

- 📌 **What are we building?** Board system (functionality for people to post and share content)
- 📌 **Why is it needed?** Information sharing, user interaction, community formation
- 📌 **How do we build it?** DB design → CRUD implementation → add search/pagination

**Lecture Content**

- Board Database Design
  - posts table: id, user_id, title, content, views, created_at, updated_at
  - Field explanation and data types (INT, VARCHAR, TEXT, TIMESTAMP)
  - FOREIGN KEY (user_id) foreign key concept
  - 1:N relationship visualization (user 1 ↔ posts N)
- Post CRUD Implementation
  - Create (write): write.php - write new post
  - Read (view): list.php (list), view.php (detail) - view posts
  - Update (edit): edit.php - edit own post (verify author)
  - Delete (delete): delete.php - delete own post
- Pagination
  - Using LIMIT and OFFSET
  - Display 10 per page (LIMIT 10 OFFSET 0)
  - Page navigation UI
  - Calculate total post count
- View Count Management
  - Increase view count each time post is opened
  - UPDATE posts SET views = views + 1
- Basic Search Functionality
  - Search by title (LIKE %search_term%)
  - Search by author (JOIN users)

**Practice Activities**

- Create database and tables
  - Create board_db database
  - Create posts table (fields, constraints)
  - Insert sample data
- Post write functionality
  - write.php: input form
  - Receive title, content
  - Save data (INSERT)
- Post list and view
  - list.php: display post list (with pagination)
  - view.php: post detail information + increase view count
  - Display author information (JOIN)
- Post edit and delete
  - edit.php: retrieve existing content then edit (author only)
  - delete.php: delete (confirmation message)
- Basic search
  - Title/author search form
  - Search results pagination

---

### **Chapter 10: Board System Implementation (2/3) - Add Comment Feature**

**Learning Objectives**: Understand 1:N relationships and implement comment system

**Lecture Approach**

- 📌 **What are we building?** Comment system (functionality to leave opinions on posts)
- 📌 **Why is it needed?** Promote user interaction, collect feedback, activate community
- 📌 **How do we build it?** DB design → comment CRUD implementation → display on view.php

**Lecture Content**

- Comment Database Design
  - comments table: id, post_id, user_id, content, created_at, updated_at
  - posts ↔ comments (1:N relationship) visualization
  - FOREIGN KEY CASCADE concept
- Comment CRUD Implementation
  - Create (write): add_comment.php - add comment
  - Read (view): modify view.php - display comments
  - Update (edit): edit_comment.php - edit comment (author only)
  - Delete (delete): delete_comment.php - delete comment (author only)
- Security: Verify author
  - WHERE id = ? AND user_id = ? pattern
  - Can only edit/delete own comments
- Retrieve author information using JOIN

**Practice Activities**

- Create comments table
- add_comment.php: write comment
  - Comment input form
  - Security processing with htmlspecialchars
  - Login verification
- Modify view.php: display comments
  - Retrieve author information together with JOIN
  - Sort by latest first
  - Display edit/delete buttons only for own comments
- edit_comment.php: edit comment (author only)
- delete_comment.php: delete comment (author only)

---

### **Chapter 11: Board System Implementation (3/3) - Add File Attachment Feature**

**Learning Objectives**: Process file uploads securely and implement file attachment system

**Lecture Approach**

- 📌 **What are we building?** File attachment system (functionality to upload files to posts)
- 📌 **Why is it needed?** Share images and documents, store materials
- 📌 **How do we build it?** File validation → secure storage → download functionality

**Lecture Content**

- File Attachment Database Design
  - attachments table: id, post_id, original_name, stored_name, file_size, created_at
  - Original filename vs stored filename (security, prevent conflicts)
- File Validation
  - File size limit (10MB)
  - Extension validation (jpg, png, pdf, doc, docx, txt)
  - Extract extension with pathinfo()
- Safe Filename Generation
  - time() + post_id + basename
  - Remove special characters/Korean characters
- File Download
  - Provide original filename to user
  - Transfer file with readfile()

**Practice Activities**

- Create attachments table
- Define upload_file.php function
  - Validate file size
  - Validate extension
  - Generate safe filename
  - Save file
- Modify write.php
  - enctype="multipart/form-data"
  - Upload multiple files
  - Call uploadFile() function
- Modify view.php
  - Display attachment list
  - Download links
- download.php: file download
  - Set HTTP headers
  - Transfer with readfile()
- delete_attachment.php: delete file

---

### **Chapter 12: Advanced Board Features - User Features & Permissions**

**Learning Objectives**: Implement user profiles and permission management

**Lecture Approach**

- 📌 **What are we building?** User management system (profile, permission management)
- 📌 **Why is it needed?** Track users, prevent spam, management features
- 📌 **How do we build it?** Add is_admin → verify permissions → profile page

**Lecture Content**

- User Permission Design
  - Add is_admin column to users table
  - Regular users: manage only their own posts
  - Administrators: manage all posts
- Permission Verification Logic
  - WHERE id = ? AND user_id = ? (verify author)
  - Check is_admin (administrator permission)
  - Permission flowchart
- Profile Page
  - Display user information
  - List of written posts
  - Display statistics with COUNT()
- Administrator Dashboard
  - Overall statistics (posts, users, comments)
  - Recent post list

**Practice Activities**

- Modify users table
  - ALTER TABLE users ADD COLUMN is_admin BOOLEAN DEFAULT FALSE
  - Set administrator account
- Implement profile.php
  - Retrieve profile by username
  - Post/comment count with COUNT()
  - Display 10 recent posts
- Modify edit.php, delete.php
  - Verify WHERE id = ? AND user_id = ? or is_admin
  - Error message if no permission
- admin_dashboard.php
  - Aggregate functions like COUNT(), SUM()
  - Recent posts, users, comment statistics

---

Thank you for your attention.

Professor Cho Jeonghyun (peterchokr@gmail.com)  
Yeungnam University College
