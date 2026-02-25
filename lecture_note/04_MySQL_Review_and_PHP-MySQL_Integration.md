# Chapter 4. MySQL Review and PHP-MySQL Integration

---

## 📚 Learning Objectives

After completing this chapter, you will be able to:

✅ Understand and use MySQL's main commands (CREATE, INSERT, SELECT, UPDATE, DELETE)
✅ Connect to MySQL databases from PHP
✅ Safely query databases using PDO
✅ Understand the risks of SQL Injection attacks and defend against them
✅ Write secure queries with Prepared Statements

이 장을 학습하면 다음을 할 수 있습니다:

✅ MySQL의 주요 명령어(CREATE, INSERT, SELECT, UPDATE, DELETE)를 이해할 수 있습니다.
✅ PHP에서 MySQL 데이터베이스에 연결할 수 있습니다.
✅ PDO를 사용하여 안전하게 데이터를 조회할 수 있습니다.
✅ SQL Injection 공격의 위험성을 이해하고 방어할 수 있습니다.
✅ Prepared Statement로 안전한 쿼리를 작성할 수 있습니다.

---

## 1️⃣ MySQL Basic Commands Review (MySQL 기본 명령어 복습)

### 1-1 Database and Table Concepts (데이터베이스와 테이블의 개념)

A database is an organized space for storing data systematically. A table is a data storage structure within a database that looks like a spreadsheet with rows and columns. Understanding this hierarchy is fundamental to working with relational databases.

데이터베이스는 데이터를 체계적으로 저장하는 공간입니다. 테이블은 데이터베이스 안의 표 형태의 데이터 저장소입니다. 이 계층 구조를 이해하는 것은 관계형 데이터베이스로 작업하기 위한 기초입니다.

```sql
-- Create database with UTF-8 support (UTF-8 지원으로 데이터베이스 생성)
CREATE DATABASE test_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

-- Select database for use (사용할 데이터베이스 선택)
USE test_db;
```

### 1-2 CREATE TABLE - Creating Tables (테이블 생성)

When you create a table, you define the structure of how data will be stored. Each column has a name, data type, and constraints that define what kind of data can be stored. The PRIMARY KEY uniquely identifies each row in the table.

테이블을 만들 때 데이터를 저장할 구조를 정의합니다. 각 열은 이름, 데이터 타입, 그리고 저장할 수 있는 데이터의 종류를 정의하는 제약 조건을 가집니다. PRIMARY KEY는 테이블의 각 행을 고유하게 식별합니다.

```sql
-- Create table to store student information (학생 정보를 저장하는 테이블)
CREATE TABLE students (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    email VARCHAR(100),
    age INT,
    score FLOAT
);

-- Field descriptions: (각 필드 설명:)
-- id: Student identification number, auto-incremented (학생 고유번호, 자동으로 증가)
-- name: Student name, required field (이름, 필수 입력)
-- email: Student email (이메일)
-- age: Student age (나이)
-- score: Student score (성적)
```

### 1-3 INSERT - Inserting Data (데이터 삽입)

The INSERT statement adds new rows to a table. You specify which columns you want to populate and provide the corresponding values. It's important to ensure that data matches the column's data type and constraints.

INSERT 문은 테이블에 새로운 행을 추가합니다. 채우고 싶은 열을 지정하고 해당 값을 제공합니다. 데이터가 열의 데이터 타입과 제약 조건과 일치하는지 확인하는 것이 중요합니다.

```sql
-- Insert single student record (단일 학생 기록 삽입)
INSERT INTO students (name, email, age, score)
VALUES ('John Smith', 'john@example.com', 20, 85);

-- Insert another record (다른 레코드 삽입)
INSERT INTO students (name, email, age, score)
VALUES ('Mary Johnson', 'mary@example.com', 21, 92);
```

### 1-4 SELECT - Querying Data (데이터 조회)

The SELECT statement retrieves data from a table. You can select all columns or specific ones, filter rows using WHERE clauses, and sort results using ORDER BY. SELECT is the most commonly used SQL command.

SELECT 문은 테이블에서 데이터를 검색합니다. 모든 열을 선택하거나 특정 열을 선택할 수 있으며, WHERE 절을 사용하여 행을 필터링하고, ORDER BY를 사용하여 결과를 정렬할 수 있습니다. SELECT는 가장 일반적으로 사용되는 SQL 명령입니다.

```sql
-- Retrieve all student information (모든 학생 정보 조회)
SELECT * FROM students;

-- Retrieve specific columns for specific students (특정 학생만 조회)
SELECT name, score FROM students WHERE age = 20;

-- Multiple conditions (조건 여러 개)
SELECT * FROM students WHERE age >= 20 AND score >= 85;

-- Sort by score in descending order (성적순으로 정렬)
SELECT * FROM students ORDER BY score DESC;
```

### 1-5 UPDATE - Modifying Data (데이터 수정)

The UPDATE statement modifies existing data in a table. You must specify which rows to update using a WHERE clause. Without a WHERE clause, all rows in the table will be updated, which is usually not what you want.

UPDATE 문은 테이블의 기존 데이터를 수정합니다. WHERE 절을 사용하여 어떤 행을 수정할지 지정해야 합니다. WHERE 절이 없으면 테이블의 모든 행이 업데이트되므로 주의해야 합니다.

```sql
-- Modify specific student's score (특정 학생의 성적 수정)
UPDATE students SET score = 90 WHERE name = 'John Smith';

-- Modify multiple fields (여러 필드 수정)
UPDATE students SET score = 95, age = 21 WHERE id = 1;
```

### 1-6 DELETE - Deleting Data (데이터 삭제)

The DELETE statement removes rows from a table. Like UPDATE, you should use a WHERE clause to specify which rows to delete. Deleting data is permanent, so be careful with DELETE statements.

DELETE 문은 테이블에서 행을 제거합니다. UPDATE와 마찬가지로 어떤 행을 삭제할지 지정하려면 WHERE 절을 사용해야 합니다. 데이터 삭제는 영구적이므로 DELETE 문에 주의해야 합니다.

```sql
-- Delete specific student record (특정 학생 정보 삭제)
DELETE FROM students WHERE id = 3;

-- Delete all records matching condition (조건에 맞는 모든 데이터 삭제)
DELETE FROM students WHERE score < 70;
```

---

## 2️⃣ PHP-MySQL Integration Methods (PHP-MySQL 연동 방식)

### 2-1 Comparison of Integration Methods (연동 방식 비교)

There are two main ways to access MySQL databases from PHP. Each approach has different advantages. PDO is the recommended modern approach because of its security features and flexibility.

PHP에서 MySQL 데이터베이스에 접근하는 방식은 2가지가 있습니다. 각 방식은 다른 장점을 가집니다. PDO는 보안 기능과 유연성 때문에 권장되는 현대적인 방식입니다.

#### **Method 1: PDO (PHP Data Objects)**

```php
<?php

// Advantages: (장점:)
// - Supports multiple databases (MySQL, PostgreSQL, etc.) (여러 데이터베이스 지원)
// - More modern and secure approach (더 현대적이고 안전한 방식)
// - Better exception handling (더 나은 예외 처리)

$pdo = new PDO(
    'mysql:host=localhost;dbname=test_db',
    'root',
    'password'
);

?>
```

#### **Method 2: MySQLi (MySQL Improved)** 

```php
<?php

// Characteristics: (특징:)
// - MySQL-only support (MySQL 전용)
// - Supports both procedural and object-oriented syntax (절차식/객체식 모두 지원)

$mysqli = new mysqli('localhost', 'root', 'password', 'test_db');

?>
```

### 2-2 Why Choose PDO? (왜 PDO를 선택하나?)

PDO offers several advantages that make it the better choice for modern PHP development. It provides better security through prepared statements, supports multiple databases, and includes robust error handling with exceptions. For these reasons, PDO is the standard in current PHP development.

PDO는 현대적인 PHP 개발을 위한 더 나은 선택지를 제공하는 여러 이점을 가지고 있습니다. 준비된 문장(prepared statements)을 통해 더 나은 보안을 제공하고, 여러 데이터베이스를 지원하며, 예외를 사용한 강력한 오류 처리를 포함합니다. 이러한 이유로 PDO는 현재 PHP 개발의 표준입니다.

```
✅ Multiple database support (여러 데이터베이스 지원)
✅ More secure Prepared Statements (더 안전한 Prepared Statement)
✅ Modern PHP standard (현대적인 PHP 표준)
✅ Exception handling (예외 처리)
```

---

## 3️⃣ Querying Data Using PDO (PDO를 사용한 데이터 조회)

### 3-1 Basic Connection Method (기본 연결 방법)

When connecting to a database, it's important to handle errors that may occur. The try-catch block catches database connection errors and prevents the application from crashing. This is essential for production applications.

데이터베이스에 연결할 때 발생할 수 있는 오류를 처리하는 것이 중요합니다. try-catch 블록은 데이터베이스 연결 오류를 포착하고 애플리케이션이 충돌하는 것을 방지합니다. 이는 프로덕션 애플리케이션에 필수적입니다.

```php
<?php

// Define connection parameters (연결 매개변수 정의)
$host = 'localhost';
$dbname = 'test_db';
$user = 'root';
$password = '';

try {
    // Attempt to create database connection (데이터베이스 연결 시도)
    $pdo = new PDO(
        "mysql:host=$host;dbname=$dbname",
        $user,
        $password
    );
    echo "Database connection successful!<br>";  // 데이터베이스 연결 성공!
} catch (PDOException $e) {
    // Handle connection error (연결 오류 처리)
    echo "Connection failed: " . $e->getMessage();  // 연결 실패:
}

?>
```

#### **Understanding try-catch (try-catch의 의미)**

The try block contains code that might throw an exception. If an error occurs, the catch block handles it. This prevents the application from crashing when an error occurs.

try 블록은 예외를 발생시킬 수 있는 코드를 포함합니다. 오류가 발생하면 catch 블록이 이를 처리합니다. 이렇게 하면 오류 발생 시 애플리케이션이 충돌하는 것을 방지합니다.

```
try: Code to attempt (시도할 코드)
catch: Code to execute if error occurs (오류 발생 시 처리할 코드)
```

### 3-2 SELECT - Querying Data (SELECT 데이터 조회)

The query() method executes a SQL statement directly. This is useful for simple queries without parameters. The fetchAll() method retrieves all rows as an associative array, which is convenient for displaying multiple results.

query() 메서드는 SQL 문을 직접 실행합니다. 이는 매개변수가 없는 간단한 쿼리에 유용합니다. fetchAll() 메서드는 모든 행을 연관 배열로 검색하므로 여러 결과를 표시하기에 편리합니다.

```php
<?php

// Retrieve all student information (모든 학생 정보 조회)
$sql = "SELECT * FROM students";
$stmt = $pdo->query($sql);
$students = $stmt->fetchAll(PDO::FETCH_ASSOC);

// Loop through results and display (결과를 반복하고 표시)
foreach ($students as $student) {
    echo $student['name'] . ": " . $student['score'] . "<br>";
}

?>
```

### 3-3 Specific Data Query - Search (특정 데이터 조회 (검색))

When user input is involved in a query, you must use prepared statements. The ? is a placeholder that will be replaced with the actual value safely. This prevents SQL injection attacks.

사용자 입력이 쿼리에 포함될 때는 준비된 문장을 사용해야 합니다. ?는 실제 값으로 안전하게 대체될 자리 표시자입니다. 이렇게 하면 SQL 주입 공격을 방지합니다.

```php
<?php

// Search student by name (이름으로 학생 검색)
$search_name = "John Smith";

// Prepare statement with placeholder (자리 표시자를 사용하여 문장 준비)
$sql = "SELECT * FROM students WHERE name = ?";
$stmt = $pdo->prepare($sql);

// Execute with parameter (매개변수로 실행)
$stmt->execute([$search_name]);

// Fetch result (결과 가져오기)
$student = $stmt->fetch(PDO::FETCH_ASSOC);

if ($student) {
    echo "Found: " . $student['name'];  // 찾음:
}

?>
```

### 3-4 fetch() vs fetchAll() 

Understanding the difference between fetch() and fetchAll() is important for efficient database access. fetch() retrieves a single row and is useful for queries expected to return one result. fetchAll() retrieves all matching rows and is useful for displaying lists.

fetch()와 fetchAll()의 차이를 이해하는 것은 효율적인 데이터베이스 접근을 위해 중요합니다. fetch()는 단일 행을 검색하며 하나의 결과를 반환할 것으로 예상되는 쿼리에 유용합니다. fetchAll()은 일치하는 모든 행을 검색하며 목록을 표시하는 데 유용합니다.

```php
<?php

// fetch(): Retrieve only one row (한 행만 조회)
$sql = "SELECT * FROM students WHERE id = 1";
$stmt = $pdo->query($sql);
$student = $stmt->fetch(PDO::FETCH_ASSOC);
echo $student['name'] . ": " . $student['score'] . "<br>";

// fetchAll(): Retrieve all rows (모든 행 조회)
$sql = "SELECT * FROM students";
$stmt = $pdo->query($sql);
$students = $stmt->fetchAll(PDO::FETCH_ASSOC);
foreach ($students as $student) {
    echo $student['name'] . ": " . $student['score'] . "<br>";
}

?>
```

---

## 4️⃣ SQL Injection Attacks and Defense (SQL Injection 공격과 방어)

### 4-1 What is SQL Injection? 

SQL Injection is a serious security vulnerability where attackers manipulate SQL queries by injecting malicious SQL code. This can lead to unauthorized data access, modification, or deletion. Understanding this attack is critical for writing secure database code.

SQL Injection은 공격자가 악의적인 SQL 코드를 주입하여 SQL 쿼리를 조작하는 심각한 보안 취약점입니다. 이는 권한이 없는 데이터 접근, 수정, 삭제로 이어질 수 있습니다. 이 공격을 이해하는 것은 안전한 데이터베이스 코드를 작성하는 데 중요합니다.

#### **Dangerous Example - NEVER USE! (위험한 예제 - 절대 사용 금지!)**

```php
<?php

// ⚠️ DANGEROUS CODE - vulnerable to SQL Injection (위험한 코드 - SQL Injection에 취약)
$username = $_GET['username'];

// If user enters: ' OR '1'='1
// The query becomes: SELECT * FROM students WHERE name = '' OR '1'='1'
// Result: ALL student information is retrieved! (모든 학생 정보가 조회됨!)

$sql = "SELECT * FROM students WHERE name = '$username'";
$result = $pdo->query($sql);

$students = $result->fetchAll(PDO::FETCH_ASSOC);

foreach ($students as $student) {
    echo $student['name'] . ": " . $student['score'] . "<br>";
}

?>
```

### 4-2 Defense with Prepared Statements (Prepared Statement로 방어)

Prepared statements separate the SQL query structure from the data values. The query is compiled first, then data is safely inserted. This makes it impossible for malicious input to alter the query structure.

준비된 문장은 SQL 쿼리 구조를 데이터 값과 분리합니다. 쿼리는 먼저 컴파일되고 데이터가 안전하게 삽입됩니다. 이렇게 하면 악의적인 입력이 쿼리 구조를 변경할 수 없게 됩니다.

```php
<?php

// ✅ SECURE CODE - using Prepared Statements (안전한 코드 - Prepared Statement 사용)

$username = "' OR '1'='1";

// Step 1: Write query with placeholders (? = placeholder) (쿼리 작성 (? = 플레이스홀더))
$sql = "SELECT * FROM students WHERE name = ?";

// Step 2: Prepare the query (쿼리 준비)
$stmt = $pdo->prepare($sql);

// Step 3: Safely insert data (데이터 안전하게 대입)
$stmt->execute([$username]);

// Step 4: Retrieve results (결과 조회)
$students = $stmt->fetchAll(PDO::FETCH_ASSOC);

foreach ($students as $student) {
    echo $student['name'] . ": " . $student['score'] . "<br>";
}

?>
```

### 4-3 Prepared Statement Formats (Prepared Statement 형식들)

There are different ways to use prepared statements. The placeholder approach (?) is the simplest and most common. Named placeholders (:name) offer more readability, especially with many parameters.

준비된 문장을 사용하는 방법은 여러 가지가 있습니다. 자리 표시자 방식(?)이 가장 간단하고 일반적입니다. 명명된 자리 표시자(:name)는 특히 많은 매개변수가 있을 때 더 나은 가독성을 제공합니다.

```php
<?php

// Basic pattern with question mark placeholders (기본 패턴)
$sql = "SELECT * FROM students WHERE score > ?";
$stmt = $pdo->prepare($sql);
$stmt->execute([85]);
$result = $stmt->fetch(PDO::FETCH_ASSOC);

// Multiple placeholders (여러 개의 플레이스홀더)
$sql = "SELECT * FROM students 
        WHERE age > ? AND score < ?";
$stmt = $pdo->prepare($sql);
$stmt->execute([20, 90]);

// Named placeholders - optional but more readable (이름을 사용한 플레이스홀더 (선택사항))
$sql = "SELECT * FROM students 
        WHERE age > :age AND score < :score";
$stmt = $pdo->prepare($sql);
$stmt->execute([':age' => 20, ':score' => 90]);

?>
```

#### **Advantages of Prepared Statements (Prepared Statement의 장점)**

```
✅ Defense against SQL Injection attacks (SQL Injection 공격 방어)
✅ Automatic special character handling (특수문자 자동 처리)
✅ Improved readability (가독성 향상)
✅ Query reusability (재사용 가능)
```

### 4-4 Multiple Condition Search (여러 조건 검색)

When searching with multiple conditions, prepared statements are even more important. This example demonstrates how to safely combine multiple search criteria without risk of SQL injection.

여러 조건으로 검색할 때 준비된 문장이 더욱 중요합니다. 이 예제는 SQL 주입 위험 없이 여러 검색 기준을 안전하게 결합하는 방법을 보여줍니다.

```php
<?php

// Search by age and score (나이와 성적으로 검색)
$age = 21;
$min_score = 94;

$sql = "SELECT * FROM students 
        WHERE age = ? AND score >= ?";

$stmt = $pdo->prepare($sql);
$stmt->execute([$age, $min_score]);

$results = $stmt->fetchAll(PDO::FETCH_ASSOC);

foreach ($results as $row) {
    echo $row['name'] . " (" . $row['score'] . " points)<br>";  // 점
}

?>
```

---

## 5️⃣ Practice Example (실습 예제)

### 5-1 Practice Example: Student Information Query System (실습 예제: 학생 정보 조회 시스템)

This practical example demonstrates how to create a complete student management system. It includes database connection, prepared statements for safe queries, error handling, and user-friendly output in an HTML table format.

이 실제 예제는 완전한 학생 관리 시스템을 만드는 방법을 보여줍니다. 데이터베이스 연결, 안전한 쿼리를 위한 준비된 문장, 오류 처리, 그리고 HTML 테이블 형식의 사용자 친화적 출력을 포함합니다.

**File name: student_list.php**

```php
<?php

// Database connection (데이터베이스 연결)
$host = 'localhost';
$dbname = 'test_db';
$user = 'root';
$password = '';

try {
    $pdo = new PDO(
        "mysql:host=$host;dbname=$dbname",
        $user,
        $password
    );
} catch (PDOException $e) {
    die("Connection failed: " . $e->getMessage());  // 연결 실패:
}

// Process search condition (검색 조건 처리)
$min_score = isset($_GET['score']) ? intval($_GET['score']) : 0;

// Safe query with prepared statement (준비된 문장으로 안전한 쿼리)
$sql = "SELECT * FROM students WHERE score >= ? ORDER BY score DESC";
$stmt = $pdo->prepare($sql);
$stmt->execute([$min_score]);
$students = $stmt->fetchAll(PDO::FETCH_ASSOC);

?>

<!DOCTYPE html>
<html>
<head>
    <title>Student Information Query</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 30px auto;
            padding: 20px;
        }
  
        .container {
            background: white;
            padding: 30px;
            border: 1px solid #ddd;
        }
  
        h1 {
            color: navy;
        }
  
        .search-box {
            margin: 20px 0;
            padding: 15px;
            background-color: #f9f9f9;
            border-left: 4px solid #2196F3;
        }
  
        input, button {
            padding: 8px;
            border: 1px solid #ddd;
        }
  
        button {
            background-color: navy;
            color: white;
            cursor: pointer;
        }
  
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
  
        th {
            background-color: navy;
            color: white;
            padding: 10px;
        }
  
        td {
            padding: 10px;
            border-bottom: 1px solid #ddd;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>📊 Student Information Query</h1>
  
    <!-- Search form (검색 폼) -->
    <div class="search-box">
        <form method="GET">
            <label>Minimum score:</label>
            <input type="number" name="score" value="<?php echo $min_score; ?>" min="0" max="100">
            <button type="submit">Search</button>
        </form>
    </div>
  
    <!-- Results table (결과 테이블) -->
    <?php if (count($students) > 0): ?>
    <table>
        <thead>
            <tr>
                <th>Name</th>
                <th>Email</th>
                <th>Age</th>
                <th>Score</th>
            </tr>
        </thead>
        <tbody>
            <?php foreach ($students as $student): ?>
            <tr>
                <td><?php echo htmlspecialchars($student['name']); ?></td>
                <td><?php echo htmlspecialchars($student['email']); ?></td>
                <td><?php echo $student['age']; ?></td>
                <td><?php echo $student['score']; ?></td>
            </tr>
            <?php endforeach; ?>
        </tbody>
    </table>
    <?php else: ?>
    <p>No search results found.</p>
    <?php endif; ?>
</div>

</body>
</html>
```

---

## ✅ Practice Assignments (연습과제)

### Assignment 1: Writing Basic SQL Queries (과제 1: 기본 SQL 쿼리 작성)

Write SQL statements for the following scenarios:

다음 상황에 맞는 SQL을 작성하세요:

1. **Query all students with score 90 or above (성적이 90점 이상인 모든 학생 조회)**
2. **Query students whose name starts with 'John' (이름이 'John'으로 시작하는 학생 조회)**
3. **Query all students sorted by age (나이 순으로 정렬하여 모든 학생 조회)**
4. **Update specific student's score to 95 (특정 학생의 성적을 95점으로 수정)**
5. **Delete students with score below 70 (성적이 70점 미만인 학생 삭제)**

### Assignment 2: Building PHP-MySQL Integration System (과제 2: PHP-MySQL 연동 시스템 구축)

Implement the following components:

다음을 구현하세요:

1. **Database Design (데이터베이스 설계)**

   - Create students table (students 테이블 생성)
   - Fields: id, name, email, age, score (필드: id, name, email, age, score)
2. **PHP Query Program (PHP 조회 프로그램)**

   - Search students using Prepared Statements (Prepared Statement로 학생 검색)
   - Display results in HTML table (HTML 테이블로 결과 표시)
3. **Implement Search Function (검색 기능 구현)**

   - Filter by score (성적으로 필터링)
   - Filter by age (나이로 필터링)
   - Combined condition search (복합 조건 검색)
4. **Error Handling (오류 처리)**

   - Handle connection errors with try-catch (try-catch로 연결 오류 처리)
   - Display friendly message when no results found (검색 결과가 없을 때 메시지 표시)

---

## 🔑 Important Points (중요 포인트)

### Always Remember (항상 기억하기)

✅ **Prepared Statements are Essential (Prepared Statement 필수)**

All user input must use Prepared Statements. Use ? or :name placeholders to ensure data safety.

모든 사용자 입력은 Prepared Statement를 사용해야 합니다. ? 또는 :name 플레이스홀더를 사용하여 데이터 안전성을 보장합니다.

✅ **Use htmlspecialchars() (htmlspecialchars() 사용)**

Always use htmlspecialchars() when outputting data from the database. This prevents HTML tag injection attacks.

데이터베이스에서 가져온 데이터를 출력할 때는 항상 htmlspecialchars()를 사용합니다. 이는 HTML 태그 주입 공격을 방지합니다.

✅ **Error Handling (오류 처리)**

Use try-catch blocks to handle exceptions gracefully. Display user-friendly error messages instead of exposing system details.

예외를 우아하게 처리하기 위해 try-catch 블록을 사용합니다. 시스템 세부 사항을 노출하는 대신 사용자 친화적인 오류 메시지를 표시합니다.

---

Thank you for your attention.
Cho Jeonghyun ([peterchokr@gmail.com](mailto:peterchokr@gmail.com)). Yeungnam University College

"Produced in collaboration with Claude and Gemini."
