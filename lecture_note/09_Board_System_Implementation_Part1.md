# Chapter 9. Board System Implementation - Part 1 - Basic Board Features

---

## 📚 Learning Objectives

After completing this chapter, you will be able to:

✅ Create posts using INSERT operations
✅ Read and display post lists with pagination
✅ View post details and increment view counts
✅ Update existing posts using UPDATE operations
✅ Delete posts using DELETE operations
✅ Display large amounts of posts efficiently with pagination
✅ Implement complete CRUD operations for a board system
✅ Apply security measures against SQL Injection and XSS attacks

이 장을 학습하고 나면 다음을 할 수 있습니다:

✅ INSERT 작업을 사용하여 게시글을 작성할 수 있습니다
✅ 페이지네이션으로 게시글 목록을 조회하고 표시할 수 있습니다
✅ 게시글 상세를 보고 조회수를 증가시킬 수 있습니다
✅ UPDATE 작업을 사용하여 게시글을 수정할 수 있습니다
✅ DELETE 작업을 사용하여 게시글을 삭제할 수 있습니다
✅ 페이지네이션으로 대량의 게시글을 효율적으로 표시할 수 있습니다
✅ 게시판 시스템의 완전한 CRUD 작업을 구현할 수 있습니다
✅ SQL Injection과 XSS 공격에 대한 보안 조치를 적용할 수 있습니다

---

## 1️⃣ Board Database Design (게시판 데이터베이스 설계)

### 1-1 Understanding Board System Structure (게시판 시스템의 구조 이해하기)

A board system requires the following core functionalities based on CRUD operations. Each operation represents a fundamental interaction between users and the database, enabling the creation, retrieval, modification, and removal of posts.

게시판 시스템은 CRUD 작업을 기반으로 다음과 같은 핵심 기능이 필요합니다. 각 작업은 사용자와 데이터베이스 간의 기본적인 상호 작용을 나타내며, 게시글의 생성, 검색, 수정 및 제거를 가능하게 합니다.

```
📝 Core Board Features (CRUD) (게시판의 핵심 기능)

┌──────────────────────────────┐
│ C - CREATE: Write posts       │
│ R - READ:   View posts        │
│ U - UPDATE: Edit posts        │
│ D - DELETE: Remove posts      │
└──────────────────────────────┘
```

Each post stores the following information to maintain complete data about user contributions and activity tracking.

각 게시글은 사용자 기여 및 활동 추적에 대한 완전한 데이터를 유지하기 위해 다음 정보를 저장합니다.

```
📋 Post Information (게시글 정보)
├─ ID (post number)
├─ Author (who wrote it)
├─ Title (what it's about)
├─ Content (detailed description)
├─ Views (how many people viewed it)
├─ Created date
└─ Updated date
```

### 1-2 Table Design (테이블 설계)

#### **users table** (사용자 정보 테이블)

The users table stores authentication information for registered users. The password field uses VARCHAR(255) to accommodate bcrypt hashes which are typically 60 characters but may vary.

users 테이블은 등록된 사용자의 인증 정보를 저장합니다. password 필드는 일반적으로 60자이지만 가변적일 수 있는 bcrypt 해시를 수용하기 위해 VARCHAR(255)를 사용합니다.

| Column     | Type         | Description                  |
| ---------- | ------------ | ---------------------------- |
| id         | INT          | User ID (auto increment)     |
| username   | VARCHAR(50)  | Username                     |
| password   | VARCHAR(255) | Password (encrypted)         |
| created_at | TIMESTAMP    | Registration date            |

#### **posts table** (게시글 테이블)

The posts table maintains all board content with foreign key relationships to users. The LONGTEXT type for content allows for extensive post bodies without limitation.

posts 테이블은 사용자에 대한 외래 키 관계와 함께 모든 게시판 콘텐츠를 유지합니다. content에 대한 LONGTEXT 타입은 제한 없이 광범위한 게시글 본문을 허용합니다.

| Column     | Type         | Description                  |
| ---------- | ------------ | ---------------------------- |
| id         | INT          | Post ID (auto increment)     |
| user_id    | INT          | Author ID (users.id)         |
| title      | VARCHAR(255) | Title                        |
| content    | LONGTEXT     | Content                      |
| views      | INT          | View count                   |
| created_at | TIMESTAMP    | Created date                 |
| updated_at | TIMESTAMP    | Updated date                 |

### 1-3 Database Creation (데이터베이스 생성)

Execute the following SQL in PHPMyAdmin to create the database structure. The CASCADE option ensures that when a user is deleted, all their posts are automatically removed to maintain referential integrity.

다음 SQL을 PHPMyAdmin에서 실행하여 데이터베이스 구조를 생성하세요. CASCADE 옵션은 사용자가 삭제될 때 모든 게시글이 자동으로 제거되어 참조 무결성을 유지하도록 합니다.

```sql
-- Create database (데이터베이스 생성)
CREATE DATABASE board_db;
USE board_db;

-- users table (사용자 테이블)
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- posts table (게시글 테이블)
CREATE TABLE posts (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    title VARCHAR(255) NOT NULL,
    content LONGTEXT NOT NULL,
    views INT DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Sample user data (샘플 사용자 데이터)
-- Password is 'password123' hashed with bcrypt (비밀번호는 bcrypt로 해시된 'password123')
INSERT INTO users (username, password) VALUES
('john', '$2y$10$z8d8AjYwJHVH7zIqceIpceJGmcRbD1vxAazUkxaNm/s99By8/rGT.'),
('mary', '$2y$10$z8d8AjYwJHVH7zIqceIpceJGmcRbD1vxAazUkxaNm/s99By8/rGT.'),
('robert', '$2y$10$z8d8AjYwJHVH7zIqceIpceJGmcRbD1vxAazUkxaNm/s99By8/rGT.');

-- Sample post data (샘플 게시글 데이터)
INSERT INTO posts (user_id, title, content, views) VALUES
(1, 'Started Learning PHP!', 'PHP is really interesting. Making a board is amazing!', 5),
(2, 'Database Query Tips', 'Using WHERE clause effectively in SELECT queries helps find data easily.', 12),
(1, 'What is Web Development?', 'Learn HTML, CSS, JavaScript, PHP, MySQL to build websites!', 8),
(3, 'Starting Board Creation', 'Let\'s build a board in this chapter. Starting with database.', 3);
```

---

## 2️⃣ Essential File Preparation (필수 파일 준비)

The board system consists of multiple PHP files working together. Each file has a specific responsibility in the overall architecture, following the principle of separation of concerns for maintainability.

게시판 시스템은 함께 작동하는 여러 PHP 파일로 구성됩니다. 각 파일은 유지 관리를 위한 관심사 분리 원칙에 따라 전체 아키텍처에서 특정 책임을 가집니다.

```
📁 Project Folder (프로젝트 폴더)
├── 📄 config.php         (Database connection - 데이터베이스 연결)
├── 📄 functions.php      (Common functions - 공통 함수)
├── 📄 list.php          (Post list - 게시글 목록)
├── 📄 write.php         (Write post - 게시글 작성)
├── 📄 view.php          (Post detail - 게시글 상세)
├── 📄 edit.php          (Edit post - 게시글 수정)
└── 📄 delete.php        (Delete post - 게시글 삭제)
```

### 2-1 config.php - Database Connection (데이터베이스 연결)

This is the common connection file used by all PHP files. It establishes the PDO connection and starts the session for user authentication across all pages.

이것은 모든 PHP 파일에서 사용하는 공통 연결 파일입니다. PDO 연결을 설정하고 모든 페이지에서 사용자 인증을 위한 세션을 시작합니다.

The purpose of this file is to connect to the database and start the session. All other PHP files include this file using `require` or `include`.

이 파일의 목적은 데이터베이스에 연결하고 세션을 시작하는 것입니다. 다른 모든 PHP 파일은 `require` 또는 `include`를 사용하여 이 파일을 포함합니다.

**Filename: `config.php`**

**파일명: `config.php`**

```php
<?php
// 📄 config.php - Database connection configuration
// (데이터베이스 연결 설정)

// Database connection information (데이터베이스 접속 정보)
$db_host = 'localhost';
$db_user = 'root';
$db_password = '';
$db_name = 'board_db';

// Connect to database with PDO (PDO로 데이터베이스 연결)
try {
    $pdo = new PDO(
        'mysql:host=' . $db_host . ';dbname=' . $db_name . ';charset=utf8',
        $db_user,
        $db_password
    );
    // Set error mode to exception (에러 모드를 예외로 설정)
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
} catch (PDOException $e) {
    die('Database connection failed: ' . $e->getMessage());
}

// Start session for user information storage (사용자 정보 저장용 세션 시작)
session_start();
?>
```

### 2-2 functions.php - Common Function Collection (공통 함수 모음)

We create functions for repetitive code to promote reusability and maintainability. These utility functions handle common tasks like sanitization, formatting, and pagination calculations.

반복되는 코드를 함수로 만들어 재사용성과 유지 관리를 촉진합니다. 이러한 유틸리티 함수는 정제, 형식 지정 및 페이지네이션 계산과 같은 일반적인 작업을 처리합니다.

Frequently used functionalities are made into functions. For example, the `safe_html()` function converts special characters to display HTML tags safely, preventing XSS attacks.

자주 사용하는 기능들을 함수로 만들었습니다. 예를 들어 `safe_html()` 함수는 XSS 공격을 방지하기 위해 HTML 태그를 안전하게 표시하도록 특수 문자를 변환합니다.

**Filename: `functions.php`**

**파일명: `functions.php`**

```php
<?php
// 📄 functions.php - Common function collection (공통 함수 모음)

// Display HTML tags safely for security (HTML 태그 안전하게 표시 - 보안)
// ENT_QUOTES: Convert both single and double quotes (작은따옴표와 큰따옴표 모두 변환)
function safe_html($text) {
    return htmlspecialchars($text, ENT_QUOTES, 'UTF-8');
}

// Convert date format (Example: 2024-01-15 14:30) (날짜 형식 변환)
function format_date($datetime) {
    return date('Y-m-d H:i', strtotime($datetime));
}

// Limit string length for long titles (문자열 길이 제한 - 긴 제목 줄이기)
function truncate($text, $limit = 50) {
    if (strlen($text) > $limit) {
        return substr($text, 0, $limit) . '...';
    }
    return $text;
}

// Pagination calculation function (페이지네이션 계산 함수)
function get_pagination($total_count, $current_page, $per_page = 10) {
    // Calculate total pages (총 페이지 수 계산)
    $total_pages = ceil($total_count / $per_page);
    // Calculate offset for database query (데이터베이스 쿼리용 오프셋 계산)
    $offset = ($current_page - 1) * $per_page;
  
    return [
        'offset' => $offset,
        'per_page' => $per_page,
        'total_pages' => $total_pages,
        'current_page' => $current_page
    ];
}
?>
```

### 2-3 Why Pagination is Necessary (페이지네이션이 필요한 이유)

Loading all posts at once creates performance issues and poor user experience. Pagination divides content into manageable chunks, reducing load times and improving navigation.

모든 게시글을 한 번에 로드하면 성능 문제와 나쁜 사용자 경험이 발생합니다. 페이지네이션은 콘텐츠를 관리 가능한 청크로 나누어 로드 시간을 줄이고 탐색을 개선합니다.

```
When you have 1000 posts: (게시글이 1000개 있을 때)
❌ Display all on one page → Slow, excessive scrolling
✅ Divide into 10 per page → Fast, clean

❌ 모두 한 페이지에 표시하면 → 느림, 스크롤 많음
✅ 10개씩 페이지 나누면 → 빠름, 깔끔함
```

### 2-4 Pagination Calculation Principle (페이지네이션 계산 원리)

Understanding the mathematical relationship between total items, page size, and offset is crucial for implementing pagination correctly. The OFFSET value determines which records to skip.

총 항목, 페이지 크기 및 오프셋 간의 수학적 관계를 이해하는 것은 페이지네이션을 올바르게 구현하는 데 중요합니다. OFFSET 값은 건너뛸 레코드를 결정합니다.

```
Total posts: 47 (전체 게시글: 47개)
10 posts per page (1페이지에 10개씩 표시)

Required pages = ceil(47 / 10) = 5 pages (필요한 페이지 수 = 5페이지)

Page 1: Records 0-9   (OFFSET 0, LIMIT 10)
Page 2: Records 10-19 (OFFSET 10, LIMIT 10)
Page 3: Records 20-29 (OFFSET 20, LIMIT 10)
Page 4: Records 30-39 (OFFSET 30, LIMIT 10)
Page 5: Records 40-47 (OFFSET 40, LIMIT 10)
```

The offset calculation follows a simple formula: `offset = (page_number - 1) * items_per_page`. This ensures the correct subset of records is retrieved for each page.

오프셋 계산은 간단한 공식을 따릅니다: `offset = (page_number - 1) * items_per_page`. 이것은 각 페이지에 대해 올바른 레코드 하위 집합이 검색되도록 합니다.

```php
// Get current page number (현재 페이지 번호 받기)
$page = 2;

// Calculate pagination (페이지네이션 계산)
$per_page = 10;
$offset = ($page - 1) * $per_page;  // (2-1)*10 = 10

// Execute query (쿼리 실행)
SELECT * FROM posts 
LIMIT $per_page OFFSET $offset;
// → LIMIT 10 OFFSET 10
// → Fetch 10 records starting from 11th (11번째부터 20번째까지 10개 가져오기)
```

---

## 3️⃣ Overall Board Flow (게시판의 전체 흐름)

The board system follows a logical flow where each page connects to others through links and form submissions. Understanding this flow helps visualize how users navigate through the application.

게시판 시스템은 각 페이지가 링크와 폼 제출을 통해 다른 페이지에 연결되는 논리적 흐름을 따릅니다. 이 흐름을 이해하면 사용자가 애플리케이션을 탐색하는 방법을 시각화하는 데 도움이 됩니다.

```
🏠 list.php (List - 목록)
   ├─ Display all posts (모든 게시글 표시)
   ├─ Pagination (페이지네이션)
   └─ "Write" button → Move to write.php ("글쓰기" 버튼 → write.php로 이동)

📝 write.php (Write - 작성)
   ├─ Post creation form (게시글 작성 폼)
   ├─ Input validation (유효성 검사)
   └─ Database INSERT (데이터베이스 INSERT)

📖 view.php (Detail - 상세)
   ├─ Display post details (게시글 상세 표시)
   ├─ Increment view count (조회수 증가)
   ├─ "Edit" button → Move to edit.php ("수정" 버튼 → edit.php로 이동)
   └─ "Delete" button → Move to delete.php ("삭제" 버튼 → delete.php로 이동)

✏️ edit.php (Edit - 수정)
   ├─ Pre-fill form with existing content (기존 내용을 폼에 미리 채우기)
   ├─ Save after editing (수정 후 저장)
   └─ Permission check (user_id 1 only) (권한 확인 - user_id 1만 수정 가능)

🗑️ delete.php (Delete - 삭제)
   ├─ Delete post (게시글 삭제)
   ├─ Permission check (권한 확인)
   └─ Redirect to list (목록으로 리다이렉트)
```

---

## 4️⃣ Creating Post List Page (게시글 목록 페이지 만들기)

### 4-1 Understanding the Principle (원리 이해하기)

The post list page performs several key operations to display paginated content. It retrieves the current page from the URL, counts total posts, calculates pagination values, and fetches only the posts needed for the current page.

게시글 목록 페이지는 페이지네이션된 콘텐츠를 표시하기 위해 여러 주요 작업을 수행합니다. URL에서 현재 페이지를 검색하고, 총 게시글을 계산하고, 페이지네이션 값을 계산하고, 현재 페이지에 필요한 게시글만 가져옵니다.

Tasks for the post list page are:

게시글 목록 페이지에서 해야 할 일은 다음과 같습니다:

```
1️⃣ Receive current page number from URL (현재 페이지 번호를 URL에서 받기)
2️⃣ Count total posts (전체 게시글 개수 세기)
3️⃣ Select only posts to display on current page using LIMIT and OFFSET
   (현재 페이지에 표시할 게시글만 선택하기 - LIMIT, OFFSET 사용)
4️⃣ Display posts in table format in HTML (게시글들을 표의 형태로 HTML에 표시하기)
5️⃣ Create page number buttons (1, 2, 3, ...) (페이지 번호 버튼 만들기)
```

**SQL Query Explanation** (SQL 쿼리 설명):

We must count total posts to determine how many pages are needed. This COUNT query returns a single value representing all posts in the database.

페이지가 몇 개인지 알기 위해 전체 게시글 개수를 구해야 합니다. 이 COUNT 쿼리는 데이터베이스의 모든 게시글을 나타내는 단일 값을 반환합니다.

```sql
-- Count total posts (전체 게시글 개수)
SELECT COUNT(*) as total FROM posts;
```

Fetch posts for the current page. Page 1 uses OFFSET 0, page 2 uses OFFSET 10, page 3 uses OFFSET 20. The JOIN operation combines user information with post data.

현재 페이지의 게시글을 가져옵니다. 1페이지는 OFFSET 0, 2페이지는 OFFSET 10, 3페이지는 OFFSET 20을 사용합니다. JOIN 작업은 사용자 정보와 게시글 데이터를 결합합니다.

```sql
-- Fetch posts with pagination (페이지네이션과 함께 게시글 가져오기)
SELECT 
    p.id, p.title, u.username, p.created_at, p.views
FROM posts p
JOIN users u ON p.user_id = u.id
ORDER BY p.created_at DESC
LIMIT 10 OFFSET 0;  -- Page 1: First 10 posts (1페이지: 처음부터 10개)
```

### 4-2 Creating list.php File (list.php 파일 만들기)

This file displays all posts in a list format. It implements pagination to show 10 posts per page, improving performance and user experience for large datasets.

이 파일은 모든 게시글을 목록 형식으로 표시합니다. 페이지당 10개의 게시글을 표시하기 위해 페이지네이션을 구현하여 대용량 데이터셋에 대한 성능과 사용자 경험을 개선합니다.

**Filename: `list.php`**

**파일명: `list.php`**

```php
<?php
// Include configuration and functions (설정과 함수 포함)
include 'config.php';
include 'functions.php';

// Get current page number from URL, default is 1 (현재 페이지 번호 받기, 기본값은 1)
$current_page = isset($_GET['page']) ? intval($_GET['page']) : 1;
if ($current_page < 1) $current_page = 1;

// Pagination settings (페이지네이션 설정)
$per_page = 10;  // 10 posts per page (1페이지에 10개 게시글)

// Get total post count (전체 게시글 개수 구하기)
try {
    $count_query = "SELECT COUNT(*) as total FROM posts";
    $count_stmt = $pdo->prepare($count_query);
    $count_stmt->execute();
    $total_count = $count_stmt->fetch(PDO::FETCH_ASSOC)['total'];
  
    // Calculate pagination (페이지네이션 계산)
    $pagination = get_pagination($total_count, $current_page, $per_page);
  
    // Fetch posts for current page (현재 페이지의 게시글 가져오기)
    $query = "SELECT 
                p.id,
                p.title,
                u.username,
                p.created_at,
                p.views
            FROM posts p
            JOIN users u ON p.user_id = u.id
            ORDER BY p.created_at DESC
            LIMIT ? OFFSET ?";
  
    $stmt = $pdo->prepare($query);

    // Bind per_page value to first placeholder as integer
    // (첫 번째 물음표에 per_page 값을 정수로 바인딩)
    $stmt->bindValue(1, (int)$pagination['per_page'], PDO::PARAM_INT);
    // Bind offset value to second placeholder as integer
    // (두 번째 물음표에 offset 값을 정수로 바인딩)
    $stmt->bindValue(2, (int)$pagination['offset'], PDO::PARAM_INT);

    $stmt->execute();
    $posts = $stmt->fetchAll(PDO::FETCH_ASSOC);
  
} catch (PDOException $e) {
    die('Error: ' . $e->getMessage());
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Board - List</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
  
        body {
            font-family: 'Segoe UI', Arial, sans-serif;
            background-color: #f5f5f5;
            padding: 20px;
        }
  
        .container {
            max-width: 1200px;
            margin: 0 auto;
            background-color: white;
            padding: 30px;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        }
  
        h1 {
            color: #333;
            margin-bottom: 30px;
        }
  
        .top-bar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }
  
        .btn {
            display: inline-block;
            padding: 10px 20px;
            background-color: #4CAF50;
            color: white;
            text-decoration: none;
            border-radius: 4px;
            transition: background-color 0.3s;
        }
  
        .btn:hover {
            background-color: #45a049;
        }
  
        table {
            width: 100%;
            border-collapse: collapse;
        }
  
        th, td {
            padding: 12px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }
  
        th {
            background-color: #f8f9fa;
            font-weight: bold;
            color: #333;
        }
  
        tr:hover {
            background-color: #f5f5f5;
        }
  
        .post-title {
            color: #2196F3;
            text-decoration: none;
        }
  
        .post-title:hover {
            text-decoration: underline;
        }
  
        .pagination {
            margin-top: 30px;
            text-align: center;
        }
  
        .pagination a {
            display: inline-block;
            padding: 8px 12px;
            margin: 0 2px;
            background-color: #f8f9fa;
            color: #333;
            text-decoration: none;
            border-radius: 4px;
        }
  
        .pagination a:hover {
            background-color: #e9ecef;
        }
  
        .pagination a.active {
            background-color: #2196F3;
            color: white;
        }
  
        .no-posts {
            text-align: center;
            padding: 50px;
            color: #999;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>📋 Board - Post List</h1>
  
        <div class="top-bar">
            <div>Total: <?php echo $total_count; ?> posts</div>
            <a href="write.php" class="btn">✏️ Write Post</a>
        </div>
  
        <?php if (empty($posts)): ?>
            <div class="no-posts">No posts yet. Be the first to write!</div>
        <?php else: ?>
            <table>
                <thead>
                    <tr>
                        <th style="width: 10%;">No.</th>
                        <th style="width: 50%;">Title</th>
                        <th style="width: 15%;">Author</th>
                        <th style="width: 15%;">Date</th>
                        <th style="width: 10%;">Views</th>
                    </tr>
                </thead>
                <tbody>
                    <?php foreach ($posts as $post): ?>
                        <tr>
                            <td><?php echo $post['id']; ?></td>
                            <td>
                                <a href="view.php?id=<?php echo $post['id']; ?>" class="post-title">
                                    <?php echo safe_html($post['title']); ?>
                                </a>
                            </td>
                            <td><?php echo safe_html($post['username']); ?></td>
                            <td><?php echo format_date($post['created_at']); ?></td>
                            <td><?php echo $post['views']; ?></td>
                        </tr>
                    <?php endforeach; ?>
                </tbody>
            </table>
  
            <!-- Pagination (페이지네이션) -->
            <div class="pagination">
                <?php for ($i = 1; $i <= $pagination['total_pages']; $i++): ?>
                    <a href="?page=<?php echo $i; ?>" 
                       class="<?php echo ($i === $current_page) ? 'active' : ''; ?>">
                        <?php echo $i; ?>
                    </a>
                <?php endfor; ?>
            </div>
        <?php endif; ?>
    </div>
</body>
</html>
```

---

## 5️⃣ Creating Post Write Page (게시글 작성 페이지 만들기)

### 5-1 Understanding the Principle (원리 이해하기)

The write page collects user input through a form and validates it before inserting into the database. Proper validation prevents empty or malicious data from being stored.

작성 페이지는 폼을 통해 사용자 입력을 수집하고 데이터베이스에 삽입하기 전에 검증합니다. 적절한 검증은 빈 데이터나 악의적인 데이터가 저장되는 것을 방지합니다.

Tasks for the write page:

작성 페이지에서 해야 할 일:

```
1️⃣ Display form with title and content fields (제목과 내용 필드가 있는 폼 표시)
2️⃣ Receive form data when user clicks submit (사용자가 제출을 클릭하면 폼 데이터 받기)
3️⃣ Validate input (check if empty) (입력 검증 - 비어 있는지 확인)
4️⃣ Insert into database using INSERT query (INSERT 쿼리로 데이터베이스에 삽입)
5️⃣ Redirect to list page after success (성공 후 목록 페이지로 리다이렉트)
```

**SQL Query**:

```sql
-- Insert new post (새 게시글 삽입)
INSERT INTO posts (user_id, title, content) 
VALUES (1, 'Post Title', 'Post Content');
```

### 5-2 Creating write.php File (write.php 파일 만들기)

This file provides a form for creating new posts and handles the submission process. It validates input, sanitizes data, and executes the INSERT operation.

이 파일은 새 게시글을 만들기 위한 폼을 제공하고 제출 프로세스를 처리합니다. 입력을 검증하고, 데이터를 정제하고, INSERT 작업을 실행합니다.

**Filename: `write.php`**

**파일명: `write.php`**

```php
<?php
include 'config.php';
include 'functions.php';

// Initialize error array (에러 배열 초기화)
$errors = [];

// Handle POST request (POST 요청 처리)
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    // Receive and trim form data (폼 데이터 받기 및 공백 제거)
    $title = trim($_POST['title'] ?? '');
    $content = trim($_POST['content'] ?? '');
  
    // Validation (검증)
    if (empty($title)) {
        $errors[] = 'Please enter a title.';
    }
  
    if (empty($content)) {
        $errors[] = 'Please enter content.';
    }
  
    // If no errors, insert into database (에러가 없으면 데이터베이스에 삽입)
    if (empty($errors)) {
        try {
            // INSERT query (게시글 삽입)
            $query = "INSERT INTO posts (user_id, title, content) VALUES (?, ?, ?)";
            $stmt = $pdo->prepare($query);
            // user_id = 1 for testing (테스트용으로 user_id = 1 사용)
            $stmt->execute([1, $title, $content]);
          
            // Redirect to list page after success (성공 후 목록으로 이동)
            header('Location: list.php');
            exit;
          
        } catch (PDOException $e) {
            $errors[] = 'Error occurred while saving: ' . $e->getMessage();
        }
    }
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Write Post</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
  
        body {
            font-family: 'Segoe UI', Arial, sans-serif;
            background-color: #f5f5f5;
            padding: 20px;
        }
  
        .container {
            max-width: 800px;
            margin: 0 auto;
            background-color: white;
            padding: 30px;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        }
  
        h1 {
            color: #333;
            margin-bottom: 30px;
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
  
        input[type="text"],
        textarea {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-family: Arial, sans-serif;
            font-size: 14px;
        }
  
        input[type="text"]:focus,
        textarea:focus {
            outline: none;
            border-color: #4CAF50;
            box-shadow: 0 0 5px rgba(76, 175, 80, 0.3);
        }
  
        textarea {
            resize: vertical;
            min-height: 300px;
        }
  
        .error {
            background-color: #ffebee;
            color: #c62828;
            padding: 15px;
            border-radius: 4px;
            margin-bottom: 20px;
        }
  
        .error ul {
            margin-left: 20px;
        }
  
        .error li {
            margin-bottom: 5px;
        }
  
        .button-group {
            display: flex;
            gap: 10px;
        }
  
        .btn {
            display: inline-block;
            padding: 10px 20px;
            border-radius: 4px;
            text-decoration: none;
            border: none;
            cursor: pointer;
            font-size: 14px;
            transition: background-color 0.3s;
        }
  
        .btn-primary {
            background-color: #4CAF50;
            color: white;
        }
  
        .btn-primary:hover {
            background-color: #45a049;
        }
  
        .btn-secondary {
            background-color: #2196F3;
            color: white;
        }
  
        .btn-secondary:hover {
            background-color: #0b7dda;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>✏️ Write New Post</h1>
  
        <!-- Display error messages (에러 메시지 표시) -->
        <?php if (!empty($errors)): ?>
            <div class="error">
                <ul>
                    <?php foreach ($errors as $error): ?>
                        <li><?php echo safe_html($error); ?></li>
                    <?php endforeach; ?>
                </ul>
            </div>
        <?php endif; ?>
  
        <!-- Write form (작성 폼) -->
        <form method="POST">
            <div class="form-group">
                <label for="title">Title</label>
                <input type="text" id="title" name="title" placeholder="Enter title" required>
            </div>
  
            <div class="form-group">
                <label for="content">Content</label>
                <textarea id="content" name="content" placeholder="Enter content" required></textarea>
            </div>
  
            <div class="button-group">
                <button type="submit" class="btn btn-primary">Save</button>
                <a href="list.php" class="btn btn-secondary">Cancel</a>
            </div>
        </form>
    </div>
</body>
</html>
```

---

## 6️⃣ Creating Post Detail View Page (게시글 상세 페이지 만들기)

### 6-1 Understanding the Principle (원리 이해하기)

The detail page displays a single post's complete information and increments the view count each time it's accessed. This requires two database operations in sequence.

상세 페이지는 단일 게시글의 완전한 정보를 표시하고 액세스할 때마다 조회수를 증가시킵니다. 이것은 순차적으로 두 개의 데이터베이스 작업이 필요합니다.

Tasks for the detail view page:

상세 페이지에서 해야 할 일:

```
1️⃣ Receive post ID from URL (URL에서 게시글 ID 받기)
2️⃣ Fetch post details from database (데이터베이스에서 게시글 상세 가져오기)
3️⃣ Increment view count by 1 (조회수 1 증가)
4️⃣ Display title, content, author, date, views (제목, 내용, 작성자, 날짜, 조회수 표시)
5️⃣ Show edit and delete buttons (수정, 삭제 버튼 표시)
```

**SQL Queries**:

```sql
-- Fetch post details (게시글 상세 가져오기)
SELECT p.*, u.username 
FROM posts p
JOIN users u ON p.user_id = u.id
WHERE p.id = 1;

-- Increment view count (조회수 증가)
UPDATE posts 
SET views = views + 1 
WHERE id = 1;
```

### 6-2 Creating view.php File (view.php 파일 만들기)

This file displays the full content of a selected post and tracks how many times it has been viewed. The view count is automatically incremented on each page load.

이 파일은 선택한 게시글의 전체 콘텐츠를 표시하고 몇 번 조회되었는지 추적합니다. 조회수는 각 페이지 로드 시 자동으로 증가합니다.

**Filename: `view.php`**

**파일명: `view.php`**

```php
<?php
include 'config.php';
include 'functions.php';

// Receive post ID from URL (URL에서 게시글 ID 받기)
$post_id = isset($_GET['id']) ? intval($_GET['id']) : 0;

if ($post_id === 0) {
    // If ID is invalid, redirect to list (ID가 유효하지 않으면 목록으로)
    header('Location: list.php');
    exit;
}

try {
    // Increment view count (조회수 증가)
    $update_query = "UPDATE posts SET views = views + 1 WHERE id = ?";
    $update_stmt = $pdo->prepare($update_query);
    $update_stmt->execute([$post_id]);
  
    // Fetch post details (게시글 상세 조회)
    $query = "SELECT p.*, u.username 
              FROM posts p
              JOIN users u ON p.user_id = u.id
              WHERE p.id = ?";
    $stmt = $pdo->prepare($query);
    $stmt->execute([$post_id]);
    $post = $stmt->fetch(PDO::FETCH_ASSOC);
  
    if (!$post) {
        // If post not found, redirect to list (게시글이 없으면 목록으로)
        header('Location: list.php');
        exit;
    }
  
} catch (PDOException $e) {
    die('Error: ' . $e->getMessage());
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title><?php echo safe_html($post['title']); ?></title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
  
        body {
            font-family: 'Segoe UI', Arial, sans-serif;
            background-color: #f5f5f5;
            padding: 20px;
        }
  
        .container {
            max-width: 800px;
            margin: 0 auto;
            background-color: white;
            padding: 30px;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        }
  
        .post-header {
            border-bottom: 2px solid #e9ecef;
            padding-bottom: 20px;
            margin-bottom: 30px;
        }
  
        .post-title {
            font-size: 28px;
            color: #333;
            margin-bottom: 15px;
        }
  
        .post-meta {
            color: #666;
            font-size: 14px;
        }
  
        .post-meta span {
            margin-right: 20px;
        }
  
        .post-content {
            line-height: 1.8;
            color: #333;
            margin-bottom: 30px;
            min-height: 200px;
            white-space: pre-wrap;
        }
  
        .button-group {
            display: flex;
            gap: 10px;
            justify-content: space-between;
        }
  
        .btn {
            display: inline-block;
            padding: 10px 20px;
            border-radius: 4px;
            text-decoration: none;
            border: none;
            cursor: pointer;
            font-size: 14px;
            transition: background-color 0.3s;
        }
  
        .btn-primary {
            background-color: #4CAF50;
            color: white;
        }
  
        .btn-primary:hover {
            background-color: #45a049;
        }
  
        .btn-secondary {
            background-color: #2196F3;
            color: white;
        }
  
        .btn-secondary:hover {
            background-color: #0b7dda;
        }
  
        .btn-danger {
            background-color: #f44336;
            color: white;
        }
  
        .btn-danger:hover {
            background-color: #da190b;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="post-header">
            <h1 class="post-title"><?php echo safe_html($post['title']); ?></h1>
            <div class="post-meta">
                <span>👤 Author: <?php echo safe_html($post['username']); ?></span>
                <span>📅 Date: <?php echo format_date($post['created_at']); ?></span>
                <span>👁️ Views: <?php echo $post['views']; ?></span>
            </div>
        </div>
  
        <div class="post-content">
            <?php echo nl2br(safe_html($post['content'])); ?>
        </div>
  
        <div class="button-group">
            <div>
                <a href="edit.php?id=<?php echo $post['id']; ?>" class="btn btn-primary">✏️ Edit</a>
                <a href="delete.php?id=<?php echo $post['id']; ?>" 
                   class="btn btn-danger"
                   onclick="return confirm('Are you sure you want to delete this post?');">🗑️ Delete</a>
            </div>
            <a href="list.php" class="btn btn-secondary">📋 Back to List</a>
        </div>
    </div>
</body>
</html>
```

---

## 7️⃣ Creating Post Edit Page (게시글 수정 페이지 만들기)

### 7-1 Understanding the Principle (원리 이해하기)

The edit page pre-fills a form with existing post data, allowing users to modify and save changes. Permission checking ensures only the author can edit their posts.

수정 페이지는 기존 게시글 데이터로 폼을 미리 채워서 사용자가 수정하고 변경사항을 저장할 수 있도록 합니다. 권한 확인은 작성자만 게시글을 편집할 수 있도록 합니다.

Tasks for the edit page:

수정 페이지에서 해야 할 일:

```
1️⃣ Receive post ID from URL (URL에서 게시글 ID 받기)
2️⃣ Fetch existing post data (기존 게시글 데이터 가져오기)
3️⃣ Pre-fill form with current title and content (현재 제목과 내용으로 폼 미리 채우기)
4️⃣ Receive modified data when user submits (사용자가 제출하면 수정된 데이터 받기)
5️⃣ Update database using UPDATE query (UPDATE 쿼리로 데이터베이스 업데이트)
```

**SQL Query**:

```sql
-- Update post (게시글 수정)
UPDATE posts 
SET title = 'New Title', content = 'New Content', updated_at = NOW()
WHERE id = 1 AND user_id = 1;
```

### 7-2 Creating edit.php File (edit.php 파일 만들기)

This file enables users to modify their existing posts. It retrieves the current post data, displays it in an editable form, and processes the update when submitted.

이 파일은 사용자가 기존 게시글을 수정할 수 있도록 합니다. 현재 게시글 데이터를 검색하고, 편집 가능한 폼에 표시하고, 제출 시 업데이트를 처리합니다.

**Filename: `edit.php`**

**파일명: `edit.php`**

```php
<?php
include 'config.php';
include 'functions.php';

// Receive post ID from URL (URL에서 게시글 ID 받기)
$post_id = isset($_GET['id']) ? intval($_GET['id']) : 0;
$errors = [];

if ($post_id === 0) {
    header('Location: list.php');
    exit;
}

try {
    // Fetch existing post (기존 게시글 조회)
    $query = "SELECT * FROM posts WHERE id = ?";
    $stmt = $pdo->prepare($query);
    $stmt->execute([$post_id]);
    $post = $stmt->fetch(PDO::FETCH_ASSOC);
  
    if (!$post) {
        header('Location: list.php');
        exit;
    }
  
    // Permission check (testing: only user_id 1 can edit)
    // (권한 확인 - 테스트용: user_id 1만 수정 가능)
    if ($post['user_id'] != 1) {
        die('You do not have permission to edit this post.');
    }
  
} catch (PDOException $e) {
    die('Error: ' . $e->getMessage());
}

// Handle POST request (POST 요청 처리)
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $title = trim($_POST['title'] ?? '');
    $content = trim($_POST['content'] ?? '');
  
    // Validation (검증)
    if (empty($title)) {
        $errors[] = 'Please enter a title.';
    }
  
    if (empty($content)) {
        $errors[] = 'Please enter content.';
    }
  
    // If no errors, update database (에러가 없으면 데이터베이스 업데이트)
    if (empty($errors)) {
        try {
            // UPDATE query (게시글 수정)
            $update_query = "UPDATE posts 
                            SET title = ?, content = ?, updated_at = NOW()
                            WHERE id = ? AND user_id = 1";
            $update_stmt = $pdo->prepare($update_query);
            $update_stmt->execute([$title, $content, $post_id]);
          
            // Redirect to detail page after success (성공 후 상세 페이지로)
            header('Location: view.php?id=' . $post_id);
            exit;
          
        } catch (PDOException $e) {
            $errors[] = 'Error occurred while updating: ' . $e->getMessage();
        }
    }
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Edit Post</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
  
        body {
            font-family: 'Segoe UI', Arial, sans-serif;
            background-color: #f5f5f5;
            padding: 20px;
        }
  
        .container {
            max-width: 800px;
            margin: 0 auto;
            background-color: white;
            padding: 30px;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        }
  
        h1 {
            color: #333;
            margin-bottom: 30px;
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
  
        input[type="text"],
        textarea {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-family: Arial, sans-serif;
            font-size: 14px;
        }
  
        input[type="text"]:focus,
        textarea:focus {
            outline: none;
            border-color: #4CAF50;
            box-shadow: 0 0 5px rgba(76, 175, 80, 0.3);
        }
  
        textarea {
            resize: vertical;
            min-height: 300px;
        }
  
        .error {
            background-color: #ffebee;
            color: #c62828;
            padding: 15px;
            border-radius: 4px;
            margin-bottom: 20px;
        }
  
        .error ul {
            margin-left: 20px;
        }
  
        .error li {
            margin-bottom: 5px;
        }
  
        .button-group {
            display: flex;
            gap: 10px;
            justify-content: space-between;
        }
  
        .btn {
            display: inline-block;
            padding: 10px 20px;
            border-radius: 4px;
            text-decoration: none;
            border: none;
            cursor: pointer;
            font-size: 14px;
            transition: background-color 0.3s;
        }
  
        .btn-primary {
            background-color: #4CAF50;
            color: white;
        }
  
        .btn-primary:hover {
            background-color: #45a049;
        }
  
        .btn-secondary {
            background-color: #2196F3;
            color: white;
        }
  
        .btn-secondary:hover {
            background-color: #0b7dda;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>✏️ Edit Post</h1>
  
        <!-- Display error messages (에러 메시지 표시) -->
        <?php if (!empty($errors)): ?>
            <div class="error">
                <ul>
                    <?php foreach ($errors as $error): ?>
                        <li><?php echo safe_html($error); ?></li>
                    <?php endforeach; ?>
                </ul>
            </div>
        <?php endif; ?>
  
        <!-- Edit form (수정 폼) -->
        <form method="POST">
            <div class="form-group">
                <label for="title">Title</label>
                <input type="text" id="title" name="title" 
                       value="<?php echo safe_html($post['title']); ?>" required>
            </div>
  
            <div class="form-group">
                <label for="content">Content</label>
                <textarea id="content" name="content" required><?php echo safe_html($post['content']); ?></textarea>
            </div>
  
            <div class="button-group">
                <div>
                    <button type="submit" class="btn btn-primary">Save</button>
                    <a href="view.php?id=<?php echo $post_id; ?>" class="btn btn-secondary">Cancel</a>
                </div>
            </div>
        </form>
    </div>
</body>
</html>
```

### 7-3 Core Code Explanation (핵심 코드 설명)

**Pre-filling Form with Existing Data** (기존 정보를 폼에 미리 채우기)

When opening the edit form, display current title and content as the value of input and textarea. This allows users to see and modify existing content easily.

수정 폼을 열 때 현재 제목과 내용을 input과 textarea의 값으로 표시합니다. 이를 통해 사용자가 기존 콘텐츠를 쉽게 보고 수정할 수 있습니다.

```php
<input type="text" id="title" name="title" 
       value="<?php echo safe_html($post['title']); ?>" required>
<textarea id="content" name="content" required>
<?php echo safe_html($post['content']); ?>
</textarea>
```

**Updating with UPDATE Query** (UPDATE 쿼리로 수정)

Check both ID and author in WHERE clause to ensure only the author can modify their own posts. The NOW() function updates the timestamp automatically.

WHERE 절에서 ID와 작성자를 확인하여 본인의 글만 수정하도록 합니다. NOW() 함수는 타임스탬프를 자동으로 업데이트합니다.

```php
$update_query = "UPDATE posts 
                SET title = ?, content = ?, updated_at = NOW()
                WHERE id = ? AND user_id = 1";
// WHERE id = ? AND user_id = 1: Only if both ID and author match
// (WHERE id = ? AND user_id = 1: ID가 맞고 작성자도 맞은 경우만)
```

---

## 8️⃣ Creating Post Delete Page (게시글 삭제 페이지 만들기)

### 8-1 Understanding the Principle (원리 이해하기)

The delete page removes posts from the database after verifying the user has permission. It's a destructive operation that should include confirmation dialogs.

삭제 페이지는 사용자에게 권한이 있는지 확인한 후 데이터베이스에서 게시글을 제거합니다. 이것은 확인 대화 상자를 포함해야 하는 파괴적인 작업입니다.

To delete a post:

게시글을 삭제하려면:

```
1️⃣ Receive post ID from URL (URL에서 게시글 ID 받기)
2️⃣ Check permission (only author can delete) (권한 확인 - 작성자만 삭제 가능)
3️⃣ Execute DELETE in database (데이터베이스에서 DELETE 실행)
4️⃣ Return to list page (목록 페이지로 돌아가기)
```

**SQL Query**:

The DELETE statement permanently removes the record. The WHERE clause with both ID and user_id ensures authorization.

DELETE 문은 레코드를 영구적으로 제거합니다. ID와 user_id가 모두 있는 WHERE 절은 권한을 보장합니다.

```sql
-- Delete post (게시글 삭제)
DELETE FROM posts 
WHERE id = 1 AND user_id = 1;
```

### 8-2 Creating delete.php File (delete.php 파일 만들기)

This file handles post deletion after verifying ownership. It performs minimal UI rendering since it immediately redirects after the operation.

이 파일은 소유권을 확인한 후 게시글 삭제를 처리합니다. 작업 후 즉시 리다이렉트하므로 최소한의 UI 렌더링을 수행합니다.

**Filename: `delete.php`**

**파일명: `delete.php`**

```php
<?php
include 'config.php';
include 'functions.php';

// Receive post ID from URL (URL에서 게시글 ID 받기)
$post_id = isset($_GET['id']) ? intval($_GET['id']) : 0;

if ($post_id === 0) {
    header('Location: list.php');
    exit;
}

try {
    // Verify post to delete (삭제할 게시글 확인)
    $query = "SELECT * FROM posts WHERE id = ?";
    $stmt = $pdo->prepare($query);
    $stmt->execute([$post_id]);
    $post = $stmt->fetch(PDO::FETCH_ASSOC);
  
    if (!$post) {
        header('Location: list.php');
        exit;
    }
  
    // Check author permission (testing: user_id 1 can delete)
    // (작성자 권한 확인 - 테스트용: user_id 1이면 삭제 가능)
    if ($post['user_id'] != 1) {
        die('You do not have permission to delete this post.');
    }
  
    // Execute deletion (삭제 실행)
    $delete_query = "DELETE FROM posts WHERE id = ? AND user_id = 1";
    $delete_stmt = $pdo->prepare($delete_query);
    $delete_stmt->execute([$post_id]);
  
    // Redirect to list after deletion (삭제 후 목록으로 이동)
    header('Location: list.php');
    exit;
  
} catch (PDOException $e) {
    die('Error occurred during deletion: ' . $e->getMessage());
}
?>
```

### 8-3 Core Code Explanation (핵심 코드 설명)

**Deleting Post with DELETE** (DELETE로 게시글 삭제)

Check both ID and author in WHERE clause to ensure only the author can delete their own posts. This prevents unauthorized deletion attempts.

WHERE 절에서 ID와 작성자를 확인하여 본인의 글만 삭제하도록 합니다. 이것은 무단 삭제 시도를 방지합니다.

```php
$delete_query = "DELETE FROM posts WHERE id = ? AND user_id = 1";
$delete_stmt = $pdo->prepare($delete_query);
$delete_stmt->execute([$post_id]);
// DELETE: Completely removes the row from the table
// (DELETE: 해당 행을 완전히 삭제합니다)
```

---

## 9️⃣ Verifying Functionality (작동 확인)

### 9-1 File Save Verification (파일 저장 확인)

Before testing, ensure all files are properly saved in the correct location with proper encoding to prevent character display issues.

테스트하기 전에 문자 표시 문제를 방지하기 위해 모든 파일이 올바른 인코딩으로 올바른 위치에 제대로 저장되었는지 확인하세요.

1. Verify all files are saved in `C:\xampp\htdocs` (or web server root)
2. Confirm file encoding is **UTF-8** (use VS Code, not Notepad)

1. 모든 파일이 `C:\xampp\htdocs` (또는 웹서버 루트)에 저장되었는지 확인
2. 파일 인코딩이 **UTF-8**인지 확인 (메모장이 아닌 VS Code 사용 권장)

### 9-2 Basic Functionality Testing (기본 동작 테스트)

Follow this test sequence to verify all CRUD operations work correctly. Each step builds on the previous one to ensure complete functionality.

모든 CRUD 작업이 올바르게 작동하는지 확인하기 위해 이 테스트 시퀀스를 따르세요. 각 단계는 완전한 기능을 보장하기 위해 이전 단계를 기반으로 합니다.

```
1️⃣  http://localhost/list.php
    → Post list should be displayed (게시글 목록이 보여야 함)
    → "Write Post" button should exist ("글쓰기" 버튼이 있어야 함)

2️⃣  Click "Write Post" ("글쓰기" 클릭)
    → write.php page should open (write.php 페이지가 열려야 함)
    → Enter title and content, then save (제목, 내용 입력 후 저장)

3️⃣  Return to list.php (list.php로 돌아가기)
    → New post should appear at the top (새 글이 맨 위에 보여야 함)

4️⃣  Click post title (게시글 제목 클릭)
    → Navigate to view.php (view.php로 이동)
    → Full content should be displayed (전체 내용이 보여야 함)
    → View count should increment (조회수가 증가해야 함)

5️⃣  Click "Edit" button ("수정" 버튼 클릭)
    → Navigate to edit.php (edit.php로 이동)
    → Existing content should be pre-filled in form (기존 내용이 폼에 미리 채워져 있어야 함)
    → Save after editing (수정 후 저장)

6️⃣  Click "Delete" button ("삭제" 버튼 클릭)
    → Confirmation message appears (삭제 확인 메시지)
    → Return to list.php after deletion (삭제 후 list.php로 돌아가기)
```

---

## 🔟 Security Explanation (보안 설명)

### 10-1 SQL Injection Prevention (SQL Injection 방지)

When malicious users try to manipulate SQL queries, using **Prepared Statements** keeps the application safe. This separates SQL logic from user data.

악의적인 사용자가 SQL 쿼리를 조작하려고 할 때, **Prepared Statement**를 사용하면 안전합니다. 이것은 SQL 로직과 사용자 데이터를 분리합니다.

```php
// ❌ Dangerous (Never do this!) (위험 - 절대 하지 마세요!)
$id = $_GET['id'];
$query = "SELECT * FROM posts WHERE id = " . $id;
// If user inputs "1 OR 1=1", all posts are retrieved!
// (사용자가 "1 OR 1=1"을 입력하면 모든 게시글이 조회됨!)

// ✅ Safe (Always do this!) (안전 - 항상 이렇게 하세요!)
$query = "SELECT * FROM posts WHERE id = ?";
$stmt = $pdo->prepare($query);
$stmt->execute([$_GET['id']]);
// User input is treated only as a string, SQL query manipulation impossible
// (사용자 입력은 문자열로만 취급되어 SQL 쿼리 조작 불가능)
```

### 10-2 XSS Prevention (XSS 방지)

User-entered content may contain HTML tags. The `htmlspecialchars()` function safely converts these to prevent script execution.

사용자가 입력한 내용에 HTML 태그가 포함되어 있을 수 있습니다. `htmlspecialchars()` 함수는 스크립트 실행을 방지하기 위해 이를 안전하게 변환합니다.

```php
// ❌ Dangerous (Malicious JavaScript executes!) (위험 - 악성 JavaScript 실행!)
echo $_POST['title'];  // If user enters <script>alert('Hacked')</script>, it executes!
                       // (사용자가 <script>alert('해킹')</script> 입력 시 실행됨!)

// ✅ Safe (Converts special characters) (안전 - 특수 문자를 변환)
echo htmlspecialchars($_POST['title']);  // <script> → &lt;script&gt; (displayed as text)
                                          // (<script> → &lt;script&gt;로 변환)
```

---

## 1️⃣1️⃣ Assignments (과제)

### Assignment 1: Complete Functionality Testing (Required) (과제 1: 전체 기능 테스트 - 필수)

Access `http://localhost/list.php` and test the following in order. Document any errors or unexpected behavior you encounter.

`http://localhost/list.php`에 접속하여 다음을 순서대로 테스트하세요. 발생한 오류나 예상치 못한 동작을 문서화하세요.

**1️⃣ Verify List Page** (목록 페이지 확인)

- Confirm 4 posts are displayed in table format (4개의 게시글이 표 형태로 표시되는지 확인)
- Verify "✏️ Write Post" button is visible ("✏️ 글쓰기" 버튼이 보이는지 확인)

**2️⃣ Write New Post** (새 게시글 작성)

- Click "Write Post" button ("글쓰기" 버튼 클릭)
- Title: "Test Post" / Content: "This is a board test."
- Click "Save" and verify it appears in the list (저장 후 목록에 추가되는지 확인)

**3️⃣ View Post and View Count** (게시글 조회 및 조회수)

- Click the newly created post title (방금 작성한 게시글 제목 클릭)
- Verify detailed content is visible and view count is displayed (상세 내용이 보이고 조회수가 표시되는지 확인)
- Refresh page and confirm view count increments (페이지 새로고침 후 조회수가 증가하는지 확인)

**4️⃣ Edit Post** (게시글 수정)

- Click "✏️ Edit" button ("✏️ 수정" 버튼 클릭)
- Change title to "(Edited) Test Post" ("(수정) 테스트 게시글"로 변경)
- Verify edited content displays after saving (저장 후 수정된 내용이 표시되는지 확인)

**5️⃣ Delete Post** (게시글 삭제)

- Click "🗑️ Delete" button ("🗑️ 삭제" 버튼 클릭)
- Click "OK" in confirmation dialog (확인 메시지에서 "확인" 클릭)
- Verify post disappears from list (게시글이 목록에서 사라지는지 확인)

---

### Assignment 2: Error Handling Testing (Optional) (과제 2: 에러 처리 테스트 - 선택)

Test edge cases and error conditions to ensure the application handles them gracefully without crashing or exposing sensitive information.

애플리케이션이 충돌하거나 민감한 정보를 노출하지 않고 우아하게 처리하는지 확인하기 위해 엣지 케이스와 오류 조건을 테스트하세요.

**Test 1: Non-existent Post** (테스트 1: 존재하지 않는 게시글)

- Enter `http://localhost/view.php?id=99999` in address bar (주소창에 입력)
- → Verify automatic redirect to list.php (list.php로 자동 이동하는지 확인)

**Test 2: Save Without Title** (테스트 2: 제목 없이 저장)

- Leave title empty in write.php and save (write.php에서 제목을 비운 후 저장)
- → Verify "Please enter a title." error message displays ("제목을 입력해주세요." 에러 메시지 표시 확인)

**Test 3: Save Without Content** (테스트 3: 내용 없이 저장)

- Leave content empty in write.php and save (write.php에서 내용을 비운 후 저장)
- → Verify "Please enter content." error message displays ("내용을 입력해주세요." 에러 메시지 표시 확인)

---

Thank you for your attention.

Professor Cho Jeonghyun (peterchokr@gmail.com)  
Yeungnam University College
