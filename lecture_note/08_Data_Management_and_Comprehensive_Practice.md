# Chapter 8. Data Management and Comprehensive Practice

---

## 📚 Learning Objectives

After completing this chapter, you will be able to:

✅ Design databases required for web applications
✅ Implement CRUD operations (Create, Read, Update, Delete)
✅ Manage data based on user authentication
✅ Develop basic web applications used in real-world scenarios
✅ Integrate multiple PHP concepts into a complete project

이 장을 학습하고 나면 다음을 할 수 있습니다:

✅ 웹 애플리케이션에 필요한 데이터베이스를 설계할 수 있습니다
✅ CRUD 작업(Create, Read, Update, Delete)을 구현할 수 있습니다
✅ 사용자 인증을 기반으로 데이터를 관리할 수 있습니다
✅ 실무에서 사용되는 기본적인 웹 애플리케이션을 개발할 수 있습니다
✅ 여러 PHP 개념을 완전한 프로젝트로 통합할 수 있습니다

---

## ⚡ Project Selection (프로젝트 선택)

In this chapter, you will choose **one of the following two projects** to complete. (이 장에서는 **다음 2개 프로젝트 중 1개**를 선택하여 진행합니다.)

**Option 1**: Simple TODO Management System (for personal use) (간단한 TODO 관리 시스템 (개인용))

**Option 2**: Simple Product Management System (for administrators) (간단한 상품 관리 시스템 (관리자용))

---

# Project 1️⃣: TODO Management System

## 1️⃣ Project Overview (프로젝트 개요)

This project allows logged-in users to add, update, delete, and mark their TODO items as complete. It demonstrates fundamental CRUD operations with user authentication and data ownership concepts.

이 프로젝트는 로그인한 사용자가 자신의 할 일(TODO)을 추가, 수정, 삭제, 완료 처리할 수 있는 시스템입니다. 사용자 인증과 데이터 소유권 개념을 포함한 기본적인 CRUD 작업을 보여줍니다.

**Purpose**: Build a personal TODO management system with user authentication

**목적**: 사용자 인증을 포함한 개인용 TODO 관리 시스템 구축

**Features**:

- User login verification
- Add TODO items (INSERT)
- View TODO list (SELECT)
- Update TODO items (UPDATE)
- Delete TODO items (DELETE) - Assignment
- Change TODO status (UPDATE) - Assignment

**기능**:

- 로그인 확인
- TODO 추가 (INSERT)
- TODO 목록 보기 (SELECT)
- TODO 수정 (UPDATE)
- TODO 삭제 (DELETE) - 과제
- TODO 상태 변경 (UPDATE) - 과제

---

## 2️⃣ Database Design (데이터베이스 설계)

### 2-1 Table Creation (테이블 생성)

```sql
-- Create database (데이터베이스 생성)
CREATE DATABASE IF NOT EXISTS todo_app;
USE todo_app;

-- Users table (users 테이블)
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Todos table (todos 테이블)
CREATE TABLE todos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    title VARCHAR(200) NOT NULL,
    status ENUM('incomplete', 'complete') DEFAULT 'incomplete',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Test data will be created using setup_users.php below
-- (To generate bcrypt hashes accurately)
-- (테스트 데이터는 아래의 setup_users.php로 생성합니다)
-- (bcrypt 해시를 정확하게 생성하기 위해)
```

### 2-2 Table Structure Explanation (테이블 설명)

The todos table stores TODO items with ownership tracking through foreign keys. Each TODO belongs to a specific user, and when a user is deleted, all their TODOs are automatically removed due to the CASCADE constraint.

todos 테이블은 외래 키를 통해 소유권 추적과 함께 TODO 항목을 저장합니다. 각 TODO는 특정 사용자에게 속하며, CASCADE 제약 조건으로 인해 사용자가 삭제되면 모든 TODO도 자동으로 제거됩니다.

```
todos table structure:

┌────┬─────────┬──────────────────┬──────────┬────────────┐
│ id │ user_id │ title            │ status   │ created_at │
├────┼─────────┼──────────────────┼──────────┼────────────┤
│ 1  │ 1       │ Buy groceries    │ incomplete│ 2024-01-11 │
│ 2  │ 1       │ Study PHP        │ complete  │ 2024-01-11 │
└────┴─────────┴──────────────────┴──────────┴────────────┘

- user_id: Owner of this TODO
- FOREIGN KEY: user_id must exist in users table

- user_id: 이 TODO의 소유자
- FOREIGN KEY: user_id는 users 테이블에 반드시 존재해야 함
```

---

### 2-3 Test Data Setup (setup_users.php) (테스트 데이터 생성)

After creating the tables, run this PHP script in your browser to generate test data. Execute this file only once to initialize the database with sample users and TODO items.

테이블을 생성한 후, 다음 PHP 스크립트를 브라우저에서 실행하여 테스트 데이터를 생성하세요. 이 파일은 샘플 사용자와 TODO 항목으로 데이터베이스를 초기화하기 위해 1회만 실행합니다.

**Filename: `setup_users.php`** (Execute only once)

```php
<?php

// Database connection parameters (데이터베이스 연결 매개변수)
$host = 'localhost';
$dbname = 'todo_app';
$username = 'root';
$password = '';

try {
    // PDO connection (PDO 연결)
    $pdo = new PDO(
        "mysql:host=$host;dbname=$dbname;charset=utf8",
        $username,
        $password
    );
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
  
    // User data array (사용자 데이터 배열)
    $users = array(
        array(
            'username' => 'john',
            'plain_password' => 'password123',
            'display_name' => 'John (Password: password123)'
        ),
        array(
            'username' => 'admin',
            'plain_password' => 'admin123',
            'display_name' => 'Admin (Password: admin123)'
        )
    );
  
    // Insert users (사용자 삽입)
    $sql = "INSERT INTO users (username, password) VALUES (?, ?)";
    $stmt = $pdo->prepare($sql);
  
    foreach ($users as $user) {
        // password_hash(): Securely hash password with bcrypt
        // (비밀번호를 bcrypt로 안전하게 해싱)
        $hashed = password_hash($user['plain_password'], PASSWORD_BCRYPT);
  
        $stmt->execute(array(
            $user['username'],
            $hashed
        ));
  
        echo "✅ " . $user['display_name'] . " user created<br>";
    }
  
    // Sample TODO data (TODO 샘플 데이터)
    // Add TODOs for first user (john, id=1)
    // (첫 번째 사용자(john, id=1)의 TODO 추가)
    $todo_sql = "INSERT INTO todos (user_id, title, status) VALUES (?, ?, ?)";
    $todo_stmt = $pdo->prepare($todo_sql);
  
    $todos = array(
        array('user_id' => 1, 'title' => 'Buy groceries', 'status' => 'incomplete'),
        array('user_id' => 1, 'title' => 'Study PHP', 'status' => 'complete')
    );
  
    foreach ($todos as $todo) {
        $todo_stmt->execute(array(
            $todo['user_id'],
            $todo['title'],
            $todo['status']
        ));
    }
  
    echo "<br>✅ All data has been created!<br><br>";
    echo "📝 Login test:<br>";
    echo "Username: john<br>";
    echo "Password: password123<br>";
  
} catch (PDOException $e) {
    echo "❌ Error: " . $e->getMessage();
}

?>
```

**Execution Steps** (실행 방법):

1. Create database and tables using the SQL above (위 SQL로 데이터베이스 및 테이블 생성)
2. Upload `setup_users.php` file to your server (서버에 업로드)
3. Access `http://localhost/setup_users.php` in your browser (브라우저에서 접속)
4. Verify the ✅ data creation success message (데이터 생성 완료 메시지 확인)

**Result** (결과):

- john / password123 user created (securely stored with bcrypt hash)
- admin / admin123 user created (securely stored with bcrypt hash)
- 2 sample TODOs created for john
- john / password123 사용자 생성 (bcrypt 해시로 안전하게 저장)
- admin / admin123 사용자 생성 (bcrypt 해시로 안전하게 저장)
- john 사용자의 TODO 샘플 2개 생성

---

## 3️⃣ Implementation Features (구현 기능)

### 3-1 Common Configuration (config.php) (공통 설정)

This file contains database connection and session management logic used by all pages. It ensures only authenticated users can access the application and provides a centralized database connection.

이 파일은 모든 페이지에서 사용하는 데이터베이스 연결과 세션 관리 로직을 포함합니다. 인증된 사용자만 애플리케이션에 접근할 수 있도록 하고 중앙화된 데이터베이스 연결을 제공합니다.

```php
<?php

// config.php - Include in all pages (모든 페이지에 포함)

// Database connection parameters (데이터베이스 연결 매개변수)
$host = 'localhost';
$dbname = 'todo_app';
$username = 'root';
$password = '';

try {
    // PDO = PHP Data Objects (PHP 데이터 객체)
    // = Class for secure database operations in PHP
    // = PHP에서 데이터베이스를 안전하게 다룰 수 있게 해주는 클래스
    $pdo = new PDO(
        "mysql:host=$host;dbname=$dbname;charset=utf8",
        $username,
        $password
    );
  
    // setAttribute(): Configure PDO behavior (PDO의 동작 방식 설정)
    // PDO::ATTR_ERRMODE = Error handling mode (에러 처리 방식)
    // PDO::ERRMODE_EXCEPTION = Throw exception on error (에러 발생 시 Exception 발생)
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
  
} catch (PDOException $e) {
    // PDOException = Database-related exception (데이터베이스 관련 예외)
    die("DB connection failed: " . $e->getMessage());
}

// Start session (세션 시작)
session_start();

// Check login status (로그인 확인)
// isset() = Does the variable exist? (변수가 존재하는가?)
// Redirect to login.php if not logged in (로그인하지 않으면 login.php로 이동)
if (!isset($_SESSION['user_id'])) {
    header("Location: login.php");
    exit;
}

// Store user_id from session (세션에서 user_id 저장)
$user_id = $_SESSION['user_id'];

?>
```

### 3-2 Login Page (login.php) 

The login page authenticates users by verifying their credentials against the database. It uses password_verify() to securely compare the input password with the stored bcrypt hash, protecting against unauthorized access.

로그인 페이지는 데이터베이스에 대해 사용자 자격 증명을 확인하여 사용자를 인증합니다. password_verify()를 사용하여 입력된 비밀번호를 저장된 bcrypt 해시와 안전하게 비교하여 무단 액세스를 방지합니다.

```php
<?php

// Database connection (데이터베이스 연결)
$host = 'localhost';
$dbname = 'todo_app';
$username = 'root';
$password = '';

try {
    $pdo = new PDO(
        "mysql:host=$host;dbname=$dbname;charset=utf8",
        $username,
        $password
    );
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
} catch (PDOException $e) {
    die("DB connection failed: " . $e->getMessage());
}

// Start session (세션 시작)
session_start();

// If already logged in, redirect to list (이미 로그인했으면 목록으로 이동)
if (isset($_SESSION['user_id'])) {
    header("Location: list_todos.php");
    exit;
}

$error = '';

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    try {
        // Sanitize input (입력값 정제)
        $username_input = htmlspecialchars($_POST['username'] ?? '');
        $password_input = htmlspecialchars($_POST['password'] ?? '');
  
        // Validate input (입력값 검증)
        if (empty($username_input) || empty($password_input)) {
            throw new Exception("Please enter username and password");
        }
  
        // Query user from database (데이터베이스에서 사용자 조회)
        $sql = "SELECT id, username, password FROM users WHERE username = ?";
        $stmt = $pdo->prepare($sql);
        $stmt->execute([$username_input]);
        $user = $stmt->fetch(PDO::FETCH_ASSOC);
  
        // password_verify(): Compare input password with bcrypt hash
        // (입력한 비밀번호와 bcrypt 해시 비교)
        if ($user && password_verify($password_input, $user['password'])) {
            // Login successful (로그인 성공)
            $_SESSION['user_id'] = $user['id'];
            $_SESSION['username'] = $user['username'];
            header("Location: list_todos.php");
            exit;
        } else {
            throw new Exception("Invalid username or password");
        }
  
    } catch (Exception $e) {
        $error = $e->getMessage();
    }
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>Login - TODO Management System</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
  
        body {
            font-family: 'Segoe UI', Tahoma, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }
  
        .login-container {
            background: white;
            padding: 40px;
            border-radius: 10px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 400px;
        }
  
        h1 {
            color: navy;
            text-align: center;
            margin-bottom: 30px;
            font-size: 24px;
        }
  
        .form-group {
            margin-bottom: 20px;
        }
  
        label {
            display: block;
            margin-bottom: 8px;
            color: #333;
            font-weight: bold;
        }
  
        input {
            width: 100%;
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 14px;
        }
  
        button {
            width: 100%;
            padding: 12px;
            background: navy;
            color: white;
            border: none;
            border-radius: 5px;
            font-size: 16px;
            cursor: pointer;
            margin-top: 10px;
        }
  
        button:hover {
            background: #000080;
        }
  
        .error {
            background: #ffe6e6;
            color: red;
            padding: 10px;
            border-radius: 5px;
            margin-bottom: 15px;
            border: 1px solid #ffcccc;
        }
  
        .test-info {
            margin-top: 20px;
            padding: 15px;
            background: #f0f8ff;
            border-radius: 5px;
            font-size: 13px;
            color: #333;
            border: 1px solid #cce5ff;
        }
    </style>
</head>
<body>

<div class="login-container">
    <h1>🔐 TODO Management System</h1>
  
    <?php if ($error): ?>
        <div class="error">❌ <?php echo htmlspecialchars($error); ?></div>
    <?php endif; ?>
  
    <form method="POST">
        <div class="form-group">
            <label for="username">Username:</label>
            <input type="text" id="username" name="username" placeholder="Enter username" required autofocus>
        </div>
  
        <div class="form-group">
            <label for="password">Password:</label>
            <input type="password" id="password" name="password" placeholder="Enter password" required>
        </div>
  
        <button type="submit">Login</button>
    </form>
  
    <div class="test-info">
        <strong>📝 Test Account</strong><br>
        Username: john<br>
        Password: password123<br>
        <br>
        <small>Passwords are securely encrypted with bcrypt</small>
    </div>
</div>

</body>
</html>
```

### 3-3 Logout (logout.php) 

This script destroys the user session and redirects to the login page. It ensures complete session cleanup by unsetting individual session variables before destroying the entire session.

이 스크립트는 사용자 세션을 파기하고 로그인 페이지로 리다이렉트합니다. 전체 세션을 파기하기 전에 개별 세션 변수를 해제하여 완전한 세션 정리를 보장합니다.

```php
<?php

// Start session (세션 시작)
session_start();

// Unset session variables (세션 변수 삭제)
unset($_SESSION['user_id']);
unset($_SESSION['username']);

// Destroy session completely (세션 완전 삭제)
session_destroy();

// Redirect to login page (로그인 페이지로 이동)
header("Location: login.php");
exit;

?>
```

### 3-4 Add TODO (add_todo.php) 

This page allows users to create new TODO items. It validates input, sanitizes data with htmlspecialchars(), and inserts the new TODO into the database with the current user's ID to establish ownership.

이 페이지는 사용자가 새로운 TODO 항목을 생성할 수 있도록 합니다. 입력을 검증하고 htmlspecialchars()로 데이터를 정제하며, 소유권을 설정하기 위해 현재 사용자의 ID와 함께 새 TODO를 데이터베이스에 삽입합니다.

```php
<?php

// Include common configuration (공통 설정 포함)
require 'config.php';

$error = '';
$success = '';

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    try {
        // Sanitize and validate input (입력값 정제 및 검증)
        $title = htmlspecialchars(trim($_POST['title'] ?? ''));
  
        // Validation (검증)
        if (empty($title)) throw new Exception("Please enter TODO title");
  
        // INSERT query (INSERT 쿼리)
        $sql = "INSERT INTO todos (user_id, title, status) VALUES (?, ?, 'incomplete')";
        $stmt = $pdo->prepare($sql);
        $stmt->execute([$user_id, $title]);
  
        $success = "TODO has been added!";
  
    } catch (Exception $e) {
        $error = $e->getMessage();
    }
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>Add TODO</title>
    <style>
        body { font-family: 'Segoe UI'; max-width: 600px; margin: 50px auto; padding: 20px; }
        h1 { color: navy; }
        form { background: #f5f5f5; padding: 15px; }
        input { width: 100%; padding: 8px; margin: 8px 0; border: 1px solid #ddd; }
        button { background: navy; color: white; padding: 8px 15px; border: none; cursor: pointer; }
        .error { color: red; padding: 8px; background: #ffe6e6; }
        .success { color: green; padding: 8px; background: #e6ffe6; }
        a { color: navy; text-decoration: none; }
    </style>
</head>
<body>

<h1>➕ Add New TODO</h1>

<?php if ($error): ?>
    <div class="error"><?php echo htmlspecialchars($error); ?></div>
<?php endif; ?>

<?php if ($success): ?>
    <div class="success"><?php echo htmlspecialchars($success); ?></div>
    <a href="list_todos.php">Back to List</a>
<?php else: ?>
    <form method="POST">
        <input type="text" name="title" placeholder="TODO title" required autofocus>
        <button type="submit">Add</button>
    </form>
    <a href="list_todos.php">Back to List</a>
<?php endif; ?>

</body>
</html>
```

### 3-5 TODO List (list_todos.php) 

This page displays all TODOs belonging to the current user. It retrieves data using a WHERE clause to filter by user_id, ensuring users only see their own TODOs. The list shows status, creation date, and action buttons.

이 페이지는 현재 사용자에게 속한 모든 TODO를 표시합니다. WHERE 절을 사용하여 user_id로 필터링하여 데이터를 검색하므로 사용자는 자신의 TODO만 볼 수 있습니다. 목록에는 상태, 생성 날짜 및 작업 버튼이 표시됩니다.

```php
<?php

// Include common configuration (공통 설정 포함)
require 'config.php';

try {
    // Query all TODOs for current user, sorted by newest first
    // (현재 사용자의 모든 TODO를 최신순으로 조회)
    $sql = "SELECT id, title, status, created_at FROM todos WHERE user_id = ? ORDER BY created_at DESC";
    $stmt = $pdo->prepare($sql);
    $stmt->execute([$user_id]);
  
    // fetchAll() = Return all results as array (모든 결과를 배열로 반환)
    $todos = $stmt->fetchAll(PDO::FETCH_ASSOC);
  
} catch (PDOException $e) {
    die("Query failed: " . $e->getMessage());
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>TODO List</title>
    <style>
        body { font-family: 'Segoe UI'; max-width: 1000px; margin: 50px auto; padding: 20px; }
        h1 { color: navy; }
        a { display: inline-block; margin: 10px 5px 10px 0; padding: 8px 12px; background: navy; color: white; text-decoration: none; }
        table { width: 100%; border-collapse: collapse; margin-top: 15px; }
        th, td { border: 1px solid #ddd; padding: 10px; text-align: left; }
        th { background: navy; color: white; }
        tr:nth-child(even) { background: #f9f9f9; }
        .complete { color: green; font-weight: bold; }
        .incomplete { color: orange; font-weight: bold; }
        .action a { margin: 0 3px; padding: 3px 8px; font-size: 12px; }
    </style>
</head>
<body>

<h1>📝 My TODO List</h1>

<a href="add_todo.php">➕ Add New TODO</a>
<a href="logout.php">🚪 Logout</a>

<?php if (empty($todos)): ?>
    <p style="color: #999;">No TODOs yet.</p>
<?php else: ?>
    <table>
        <tr>
            <th>Title</th>
            <th>Status</th>
            <th>Created</th>
            <th>Actions</th>
        </tr>
        <?php foreach ($todos as $todo): ?>
            <tr>
                <td><?php echo htmlspecialchars($todo['title']); ?></td>
                <td class="<?php echo $todo['status']; ?>">
                    <?php echo ucfirst($todo['status']); ?>
                </td>
                <td><?php echo substr($todo['created_at'], 0, 10); ?></td>
                <td class="action">
                    <a href="edit_todo.php?id=<?php echo $todo['id']; ?>">Edit</a>
                </td>
            </tr>
        <?php endforeach; ?>
    </table>
<?php endif; ?>

</body>
</html>
```

### 3-6 Edit TODO (edit_todo.php) 

This page allows users to update existing TODO items. It first retrieves the TODO data using the ID from the URL, validates ownership to ensure users can only edit their own TODOs, then updates the database when the form is submitted.

이 페이지는 사용자가 기존 TODO 항목을 업데이트할 수 있도록 합니다. 먼저 URL의 ID를 사용하여 TODO 데이터를 검색하고, 사용자가 자신의 TODO만 편집할 수 있도록 소유권을 검증한 다음, 폼이 제출되면 데이터베이스를 업데이트합니다.

```php
<?php

// Include common configuration (공통 설정 포함)
require 'config.php';

$error = '';
$todo = null;
$id = $_GET['id'] ?? null;

if (!$id) {
    die("Invalid request");
}

try {
    // Query TODO with ownership check (소유권 확인과 함께 TODO 조회)
    $sql = "SELECT id, title, status FROM todos WHERE id = ? AND user_id = ?";
    $stmt = $pdo->prepare($sql);
    $stmt->execute([$id, $user_id]);
  
    $todo = $stmt->fetch(PDO::FETCH_ASSOC);
  
    if (!$todo) {
        die("TODO not found or access denied");
    }
  
} catch (PDOException $e) {
    die("Query failed: " . $e->getMessage());
}

// Handle POST request (POST 요청 처리)
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    try {
        // Sanitize input (입력값 정제)
        $title = htmlspecialchars(trim($_POST['title'] ?? ''));
        $status = $_POST['status'] ?? '';
  
        // Validate input (입력값 검증)
        if (empty($title)) throw new Exception("Please enter TODO title");
        if (!in_array($status, ['incomplete', 'complete'])) throw new Exception("Invalid status");
  
        // UPDATE query (UPDATE 쿼리)
        $sql = "UPDATE todos SET title = ?, status = ? WHERE id = ? AND user_id = ?";
        $stmt = $pdo->prepare($sql);
        $stmt->execute([$title, $status, $id, $user_id]);
  
        header("Location: list_todos.php");
        exit;
  
    } catch (Exception $e) {
        $error = $e->getMessage();
    }
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>Edit TODO</title>
    <style>
        body { font-family: 'Segoe UI'; max-width: 600px; margin: 50px auto; padding: 20px; }
        h1 { color: navy; }
        form { background: #f5f5f5; padding: 15px; }
        input, select { width: 100%; padding: 8px; margin: 8px 0; border: 1px solid #ddd; }
        button { background: navy; color: white; padding: 8px 15px; border: none; cursor: pointer; }
        .error { color: red; padding: 8px; background: #ffe6e6; }
        a { color: navy; text-decoration: none; }
    </style>
</head>
<body>

<h1>✏️ Edit TODO</h1>

<?php if ($error): ?>
    <div class="error"><?php echo htmlspecialchars($error); ?></div>
<?php endif; ?>

<form method="POST">
    <input type="text" name="title" value="<?php echo htmlspecialchars($todo['title']); ?>" required>
  
    <select name="status">
        <option value="incomplete" <?php echo $todo['status'] === 'incomplete' ? 'selected' : ''; ?>>Incomplete</option>
        <option value="complete" <?php echo $todo['status'] === 'complete' ? 'selected' : ''; ?>>Complete</option>
    </select>
  
    <button type="submit">Update</button>
</form>

<a href="list_todos.php">Back to List</a>

</body>
</html>
```

---

# Project 2️⃣: Product Management System (상품 관리 시스템)

## 1️⃣ Project Overview (프로젝트 개요)

This project allows administrators to manage product inventory with full CRUD operations. It demonstrates database operations, form validation, and data presentation in a practical business context.

이 프로젝트는 관리자가 완전한 CRUD 작업으로 제품 재고를 관리할 수 있도록 합니다. 실제 비즈니스 컨텍스트에서 데이터베이스 작업, 폼 검증 및 데이터 표시를 보여줍니다.

**Purpose**: Build a product inventory management system

**목적**: 제품 재고 관리 시스템 구축

**Features**:

- Administrator login
- Add products (INSERT)
- View product list (SELECT)
- Update products (UPDATE)
- Delete products (DELETE) - Assignment
- Search products (SELECT with LIKE) - Assignment

**기능**:

- 관리자 로그인
- 상품 추가 (INSERT)
- 상품 목록 보기 (SELECT)
- 상품 수정 (UPDATE)
- 상품 삭제 (DELETE) - 과제
- 상품 검색 (LIKE를 사용한 SELECT) - 과제

---

## 2️⃣ Database Design (데이터베이스 설계)

### 2-1 Table Creation (테이블 생성)

```sql
-- Create database (데이터베이스 생성)
CREATE DATABASE IF NOT EXISTS product_app;
USE product_app;

-- Users table (사용자 테이블)
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Products table (상품 테이블)
CREATE TABLE products (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(200) NOT NULL,
    price INT NOT NULL,
    stock INT NOT NULL DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Test data will be created using setup_users.php
-- (테스트 데이터는 setup_users.php로 생성합니다)
```

### 2-2 Test Data Setup (setup_users.php)

Run this script once to create test users and sample products. It generates secure password hashes and initializes the database with realistic product data.

이 스크립트를 한 번 실행하여 테스트 사용자와 샘플 제품을 생성하세요. 안전한 비밀번호 해시를 생성하고 현실적인 제품 데이터로 데이터베이스를 초기화합니다.

```php
<?php

// Database connection (데이터베이스 연결)
$host = 'localhost';
$dbname = 'product_app';
$username = 'root';
$password = '';

try {
    $pdo = new PDO(
        "mysql:host=$host;dbname=$dbname;charset=utf8",
        $username,
        $password
    );
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
  
    // User data (사용자 데이터)
    $users = array(
        array(
            'username' => 'admin',
            'plain_password' => 'admin123',
            'display_name' => 'Admin (Password: admin123)'
        )
    );
  
    // Insert users (사용자 삽입)
    $sql = "INSERT INTO users (username, password) VALUES (?, ?)";
    $stmt = $pdo->prepare($sql);
  
    foreach ($users as $user) {
        $hashed = password_hash($user['plain_password'], PASSWORD_BCRYPT);
        $stmt->execute(array($user['username'], $hashed));
        echo "✅ " . $user['display_name'] . " created<br>";
    }
  
    // Sample products (샘플 상품)
    $products = array(
        array('name' => 'Laptop Computer', 'price' => 1200000, 'stock' => 10),
        array('name' => 'Wireless Mouse', 'price' => 35000, 'stock' => 50),
        array('name' => 'Mechanical Keyboard', 'price' => 150000, 'stock' => 20)
    );
  
    $product_sql = "INSERT INTO products (name, price, stock) VALUES (?, ?, ?)";
    $product_stmt = $pdo->prepare($product_sql);
  
    foreach ($products as $product) {
        $product_stmt->execute(array(
            $product['name'],
            $product['price'],
            $product['stock']
        ));
    }
  
    echo "<br>✅ All data created!<br><br>";
    echo "📝 Login test:<br>";
    echo "Username: admin<br>";
    echo "Password: admin123<br>";
  
} catch (PDOException $e) {
    echo "❌ Error: " . $e->getMessage();
}

?>
```

---

## 3️⃣ Implementation Features (구현 기능)

### 3-1 Common Configuration (config.php) (공통 설정)

```php
<?php

// config.php - Include in all pages (모든 페이지에 포함)

// Database connection (데이터베이스 연결)
$host = 'localhost';
$dbname = 'product_app';
$username = 'root';
$password = '';

try {
    $pdo = new PDO(
        "mysql:host=$host;dbname=$dbname;charset=utf8",
        $username,
        $password
    );
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
  
} catch (PDOException $e) {
    die("DB connection failed: " . $e->getMessage());
}

// Start session (세션 시작)
session_start();

// Check login (로그인 확인)
if (!isset($_SESSION['user_id'])) {
    header("Location: login.php");
    exit;
}

$user_id = $_SESSION['user_id'];

?>
```

### 3-2 Add Product (add_product.php) 

This page allows administrators to add new products to the inventory. It validates numeric input for price and stock, sanitizes the product name, and inserts the data into the database with proper error handling.

이 페이지는 관리자가 재고에 새 제품을 추가할 수 있도록 합니다. 가격과 재고에 대한 숫자 입력을 검증하고, 제품명을 정제하며, 적절한 오류 처리와 함께 데이터베이스에 데이터를 삽입합니다.

```php
<?php

// Include common configuration (공통 설정 포함)
require 'config.php';

$error = '';
$success = '';

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    try {
        // Sanitize and validate input (입력값 정제 및 검증)
        $name = htmlspecialchars(trim($_POST['name'] ?? ''));
        $price = $_POST['price'] ?? '';
        $stock = $_POST['stock'] ?? '';
  
        // Validation (검증)
        if (empty($name)) throw new Exception("Please enter product name");
        if (!is_numeric($price) || $price < 0) throw new Exception("Please enter valid price");
        if (!is_numeric($stock) || $stock < 0) throw new Exception("Please enter valid stock quantity");
  
        // INSERT query (INSERT 쿼리)
        $sql = "INSERT INTO products (name, price, stock) VALUES (?, ?, ?)";
        $stmt = $pdo->prepare($sql);
        $stmt->execute([$name, $price, $stock]);
  
        $success = "Product has been added!";
  
    } catch (Exception $e) {
        $error = $e->getMessage();
    }
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>Add Product</title>
    <style>
        body { font-family: 'Segoe UI'; max-width: 600px; margin: 50px auto; padding: 20px; }
        h1 { color: navy; }
        form { background: #f5f5f5; padding: 15px; }
        input { width: 100%; padding: 8px; margin: 8px 0; border: 1px solid #ddd; }
        button { background: navy; color: white; padding: 8px 15px; border: none; cursor: pointer; }
        .error { color: red; padding: 8px; background: #ffe6e6; }
        .success { color: green; padding: 8px; background: #e6ffe6; }
        a { color: navy; text-decoration: none; }
    </style>
</head>
<body>

<h1>➕ Add New Product</h1>

<?php if ($error): ?>
    <div class="error"><?php echo htmlspecialchars($error); ?></div>
<?php endif; ?>

<?php if ($success): ?>
    <div class="success"><?php echo htmlspecialchars($success); ?></div>
    <a href="list_products.php">Back to List</a>
<?php else: ?>
    <form method="POST">
        <input type="text" name="name" placeholder="Product Name" required>
        <input type="number" name="price" placeholder="Price" min="0" required>
        <input type="number" name="stock" placeholder="Stock Quantity" min="0" required>
        <button type="submit">Add Product</button>
    </form>
    <a href="list_products.php">Back to List</a>
<?php endif; ?>

</body>
</html>
```

### 3-3 Product List (list_products.php) 

This page displays all products in a table format with price formatting and action buttons. It retrieves products ordered by creation date and presents them in a professional layout with edit functionality.

이 페이지는 가격 형식과 작업 버튼이 있는 테이블 형식으로 모든 제품을 표시합니다. 생성 날짜별로 정렬된 제품을 검색하고 편집 기능이 있는 전문적인 레이아웃으로 표시합니다.

```php
<?php

// Include common configuration (공통 설정 포함)
require 'config.php';

try {
    // Query all products, sorted by newest first (모든 상품을 최신순으로 조회)
    $sql = "SELECT id, name, price, stock, created_at FROM products ORDER BY created_at DESC";
    $stmt = $pdo->prepare($sql);
    $stmt->execute();
  
    // fetchAll() = Return all results as array (모든 결과를 배열로 반환)
    $products = $stmt->fetchAll(PDO::FETCH_ASSOC);
  
} catch (PDOException $e) {
    die("Query failed: " . $e->getMessage());
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>Product List</title>
    <style>
        body { font-family: 'Segoe UI'; max-width: 1000px; margin: 50px auto; padding: 20px; }
        h1 { color: navy; }
        a { display: inline-block; margin: 10px 5px 10px 0; padding: 8px 12px; background: navy; color: white; text-decoration: none; }
        table { width: 100%; border-collapse: collapse; margin-top: 15px; }
        th, td { border: 1px solid #ddd; padding: 10px; text-align: left; }
        th { background: navy; color: white; }
        tr:nth-child(even) { background: #f9f9f9; }
        .action a { margin: 0 3px; padding: 3px 8px; font-size: 12px; }
    </style>
</head>
<body>

<h1>📦 Product List</h1>

<a href="add_product.php">➕ Add New Product</a>
<a href="logout.php">🚪 Logout</a>

<?php if (empty($products)): ?>
    <p style="color: #999;">No products registered.</p>
<?php else: ?>
    <table>
        <tr>
            <th>Product Name</th>
            <th>Price</th>
            <th>Stock</th>
            <th>Created Date</th>
            <th>Actions</th>
        </tr>
        <?php foreach ($products as $product): ?>
            <tr>
                <td><?php echo htmlspecialchars($product['name']); ?></td>
                <td>$<?php echo number_format($product['price']); ?></td>
                <td><?php echo $product['stock']; ?> units</td>
                <td><?php echo substr($product['created_at'], 0, 10); ?></td>
                <td class="action">
                    <a href="edit_product.php?id=<?php echo $product['id']; ?>">Edit</a>
                </td>
            </tr>
        <?php endforeach; ?>
    </table>
<?php endif; ?>

</body>
</html>
```

### 3-4 Edit Product (edit_product.php) 

This page enables editing existing product information. It retrieves the product data by ID, displays it in a pre-filled form, validates the updated input, and executes an UPDATE query to save changes.

이 페이지는 기존 제품 정보를 편집할 수 있도록 합니다. ID로 제품 데이터를 검색하고, 미리 채워진 폼에 표시하며, 업데이트된 입력을 검증하고, UPDATE 쿼리를 실행하여 변경사항을 저장합니다.

```php
<?php

// Include common configuration (공통 설정 포함)
require 'config.php';

$error = '';
$product = null;
$id = $_GET['id'] ?? null;

if (!$id) {
    die("Invalid request");
}

try {
    // Query product (상품 조회)
    $sql = "SELECT id, name, price, stock FROM products WHERE id = ?";
    $stmt = $pdo->prepare($sql);
    $stmt->execute([$id]);
  
    $product = $stmt->fetch(PDO::FETCH_ASSOC);
  
    if (!$product) {
        die("Product not found");
    }
  
} catch (PDOException $e) {
    die("Query failed: " . $e->getMessage());
}

// Handle POST request (POST 요청 처리)
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    try {
        // Sanitize input (입력값 정제)
        $name = htmlspecialchars(trim($_POST['name'] ?? ''));
        $price = $_POST['price'] ?? '';
        $stock = $_POST['stock'] ?? '';
  
        // Validate input (입력값 검증)
        if (empty($name)) throw new Exception("Please enter product name");
        if (!is_numeric($price) || $price < 0) throw new Exception("Please enter valid price");
        if (!is_numeric($stock) || $stock < 0) throw new Exception("Please enter valid stock quantity");
  
        // UPDATE query (UPDATE 쿼리)
        $sql = "UPDATE products SET name = ?, price = ?, stock = ? WHERE id = ?";
        $stmt = $pdo->prepare($sql);
        $stmt->execute([$name, $price, $stock, $id]);
  
        header("Location: list_products.php");
        exit;
  
    } catch (Exception $e) {
        $error = $e->getMessage();
    }
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>Edit Product</title>
    <style>
        body { font-family: 'Segoe UI'; max-width: 600px; margin: 50px auto; padding: 20px; }
        h1 { color: navy; }
        form { background: #f5f5f5; padding: 15px; }
        input { width: 100%; padding: 8px; margin: 8px 0; border: 1px solid #ddd; }
        button { background: navy; color: white; padding: 8px 15px; border: none; cursor: pointer; }
        .error { color: red; padding: 8px; background: #ffe6e6; }
        a { color: navy; text-decoration: none; }
    </style>
</head>
<body>

<h1>✏️ Edit Product</h1>

<?php if ($error): ?>
    <div class="error"><?php echo htmlspecialchars($error); ?></div>
<?php endif; ?>

<form method="POST">
    <input type="text" name="name" value="<?php echo htmlspecialchars($product['name']); ?>" required>
    <input type="number" name="price" value="<?php echo $product['price']; ?>" min="0" required>
    <input type="number" name="stock" value="<?php echo $product['stock']; ?>" min="0" required>
    <button type="submit">Update Product</button>
</form>

<a href="list_products.php">Back to List</a>

</body>
</html>
```

---

## 4️⃣ Assignment Extensions (과제로 확장할 기능)

### Assignment 1: Delete Product Feature (과제 1: 상품 삭제 기능)

**Objective**: Implement product deletion with confirmation dialog

**목표**: 확인 대화상자와 함께 상품 삭제 구현

**Requirements** (요구사항):

1. Create delete_product.php file (delete_product.php 파일 생성)
2. Add delete button to each product in list_products.php ("작업" column) (list_products.php의 각 상품 "작업" 열에 삭제 버튼 추가)
3. Use JavaScript confirm() function to show deletion confirmation (JavaScript confirm() 함수로 삭제 확인 메시지 표시)
4. Send product ID to delete_product.php via GET request (GET 요청으로 delete_product.php에 상품 ID 전송)
5. Execute DELETE query to remove product from database (DELETE 쿼리로 데이터베이스에서 해당 상품 삭제)
6. Redirect to list_products.php after deletion (삭제 완료 후 list_products.php로 리다이렉트)

**Implementation Tips** (구현 팁):

- Use `onclick="return confirm('Delete this product?')"` in HTML link
- Validate product ID exists before deletion
- Use prepared statements for security
- Handle errors with try-catch blocks
- HTML 링크에서 `onclick="return confirm('이 상품을 삭제하시겠습니까?')"` 사용
- 삭제 전 상품 ID 존재 여부 검증
- 보안을 위해 준비된 문장 사용
- try-catch 블록으로 오류 처리

---

### Assignment 2: Product Search Feature (과제 2: 상품 검색 기능)

**Objective**: Implement partial product name search functionality

**목표**: 상품명으로 부분 검색 기능 구현

**Requirements** (요구사항):

1. Create search_products.php file (search_products.php 파일 생성)
2. Add HTML form to accept search keyword (GET method) (검색어를 입력받는 HTML 폼 추가 (GET 방식))
3. Use LIKE operator to search: `name LIKE %keyword%` format (LIKE 연산자를 사용하여 검색: `name LIKE %keyword%` 형식)
4. Sanitize search keyword with htmlspecialchars() (htmlspecialchars()로 검색 키워드 안전 처리)
5. Display search results in table format (검색 결과를 테이블 형식으로 출력)
6. Add link to search page in list_products.php (list_products.php에 검색 페이지로 이동하는 링크 추가)

**Implementation Tips** (구현 팁):

- SQL example: `SELECT * FROM products WHERE name LIKE ?`
- Bind parameter: `'%' . $keyword . '%'`
- Show "No results found" message when empty
- Include back button to product list
- SQL 예시: `SELECT * FROM products WHERE name LIKE ?`
- 바인드 매개변수: `'%' . $keyword . '%'`
- 결과가 없을 때 "검색 결과가 없습니다" 메시지 표시
- 상품 목록으로 돌아가기 버튼 포함

---

### Assignment 3: Image Upload (Optional) (과제 3: 이미지 업로드 (선택사항))

**Objective**: Apply Chapter 7 file upload functionality to products

**목표**: 7장 파일 업로드 기능을 상품에 적용

**Requirements** (요구사항):

1. Add image column to products table (VARCHAR 200) (products 테이블에 image 컬럼 추가 (VARCHAR 200))
2. Add image file input in add_product.php and edit_product.php (add_product.php와 edit_product.php에서 이미지 파일 input 추가)
3. Apply file upload validation from Chapter 7 (file size, extension check) (7장의 파일 업로드 검증 로직 적용 (파일 크기, 확장자 확인))
4. Save image file to designated directory (이미지 파일을 지정된 디렉토리에 저장)
5. Display image thumbnails in list_products.php (list_products.php에서 상품 목록 보기 시 이미지 썸네일 표시)

**Implementation Tips** (구현 팁):

- Allowed extensions: jpg, jpeg, png, gif
- Maximum file size: 2MB
- Use unique filename: `time() . '_' . basename($_FILES['image']['name'])`
- Store uploaded files in `uploads/` directory
- Display with `<img src="uploads/<?php echo $image; ?>" width="100">`
- 허용 확장자: jpg, jpeg, png, gif
- 최대 파일 크기: 2MB
- 고유한 파일명 사용: `time() . '_' . basename($_FILES['image']['name'])`
- 업로드된 파일을 `uploads/` 디렉토리에 저장
- `<img src="uploads/<?php echo $image; ?>" width="100">`로 표시

---

Thank you for your attention.
Cho Jeonghyun ([peterchokr@gmail.com](mailto:peterchokr@gmail.com)). Yeungnam University College

"Produced in collaboration with Claude and Gemini."
