# Chapter 10. Board System Implementation - Part 2 - Add Comment Feature

---

## 📚 Learning Objectives

After completing this chapter, you will be able to:

✅ Implement complete comment functionality in a board system
✅ Use LEFT JOIN to count and display comments for each post
✅ Create, read, update, and delete comments (CRUD for comments)
✅ Apply user authentication to all board operations
✅ Handle cascading deletes with FOREIGN KEY constraints
✅ Build a multi-table relational database system
✅ Integrate comment features seamlessly with existing board functionality

이 장을 학습하고 나면 다음을 할 수 있습니다:

✅ 게시판 시스템에 완전한 댓글 기능을 구현할 수 있습니다
✅ LEFT JOIN을 사용하여 각 게시글의 댓글을 계산하고 표시할 수 있습니다
✅ 댓글을 생성, 읽기, 수정, 삭제할 수 있습니다 (댓글 CRUD)
✅ 모든 게시판 작업에 사용자 인증을 적용할 수 있습니다
✅ FOREIGN KEY 제약 조건으로 연쇄 삭제를 처리할 수 있습니다
✅ 다중 테이블 관계형 데이터베이스 시스템을 구축할 수 있습니다
✅ 기존 게시판 기능과 댓글 기능을 완벽하게 통합할 수 있습니다

---

## 📌 Chapter Overview (이 장의 특징)

This chapter builds upon Chapter 9 by adding complete comment functionality. It includes all code from Chapter 9 plus new comment features, creating a fully functional board system with user interactions.

이 장은 9장을 기반으로 완전한 댓글 기능을 추가합니다. 9장의 모든 코드를 포함하고 새로운 댓글 기능을 추가하여 사용자 상호 작용이 있는 완전한 기능의 게시판 시스템을 만듭니다.

✅ **Complete Code**: All Chapter 9 code + Comment features

✅ **완전한 코드**: 9장 코드를 모두 포함 + 댓글 기능 추가

---

## 🔑 Essential Files (필수 파일)

### 1-1 config.php - Database Connection and Session (데이터베이스 연결 및 세션)

This file provides centralized database connection and session management. The new `requireLogin()` function enforces authentication across all protected pages.

이 파일은 중앙화된 데이터베이스 연결과 세션 관리를 제공합니다. 새로운 `requireLogin()` 함수는 모든 보호된 페이지에서 인증을 강제합니다.

**Filename: `config.php`**

**파일명: `config.php`**

```php
<?php

// config.php - Include in all pages (모든 페이지에서 include)
// Purpose: DB connection + Session start (역할: DB 연결 + 세션 시작)

// Database credentials (데이터베이스 접속 정보)
$host = 'localhost';
$dbname = 'board_db';
$username = 'root';
$password = '';

try {
    // Establish PDO connection (PDO 연결 설정)
    $pdo = new PDO(
        "mysql:host=$host;dbname=$dbname;charset=utf8",
        $username,
        $password
    );
    // Set error mode to exception (에러 모드를 예외로 설정)
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
} catch (PDOException $e) {
    die("DB connection failed: " . $e->getMessage());
}

// Start session for user tracking (사용자 추적을 위한 세션 시작)
session_start();

// Login verification function (로그인 확인 함수)
function requireLogin() {
    if (!isset($_SESSION['user_id'])) {
        header("Location: login.php");
        exit;
    }
}

?>
```

### 1-2 login.php - User Authentication (사용자 인증)

The login page authenticates users and creates sessions. For testing purposes, it uses hardcoded credentials, but in production, this should verify against database records.

로그인 페이지는 사용자를 인증하고 세션을 생성합니다. 테스트 목적으로 하드코딩된 자격 증명을 사용하지만, 프로덕션에서는 데이터베이스 레코드에 대해 확인해야 합니다.

**Filename: `login.php`**

**파일명: `login.php`**

```php
<?php

// Start session for login state (로그인 상태를 위한 세션 시작)
session_start();

// If already logged in, redirect to list (이미 로그인했으면 목록으로 이동)
if (isset($_SESSION['user_id'])) {
    header("Location: v2_list.php");
    exit;
}

$error = '';

// Handle POST request (POST 요청 처리)
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $username = $_POST['username'] ?? '';
    $password = $_POST['password'] ?? '';
  
    // For testing: john / password123 (테스트용: john / password123)
    // In production: query database for verification (실제로는 데이터베이스에서 조회)
    if ($username === 'john' && $password === 'password123') {
        $_SESSION['user_id'] = 1;
        $_SESSION['username'] = 'john';
        header("Location: v2_list.php");
        exit;
    } else {
        $error = 'Invalid username or password';
    }
}

?>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Login</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: 'Segoe UI', sans-serif;
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
        input:focus {
            outline: none;
            border-color: navy;
            box-shadow: 0 0 5px rgba(0, 0, 139, 0.3);
        }
        button {
            width: 100%;
            padding: 12px;
            background-color: navy;
            color: white;
            border: none;
            border-radius: 5px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
        }
        button:hover {
            background-color: #000080;
        }
        .error {
            color: #d32f2f;
            background-color: #ffebee;
            padding: 12px;
            border-radius: 5px;
            margin-bottom: 20px;
            border-left: 4px solid #d32f2f;
        }
        .test-info {
            background-color: #e8f5e9;
            padding: 12px;
            border-radius: 5px;
            margin-top: 20px;
            border-left: 4px solid #4caf50;
            font-size: 12px;
            color: #333;
        }
        .test-info strong {
            display: block;
            margin-bottom: 5px;
            color: #2e7d32;
        }
    </style>
</head>
<body>

<div class="login-container">
    <h1>📝 Board Login</h1>
  
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
        <strong>📝 Test Account</strong>
        Username: john<br>
        Password: password123
    </div>
</div>

</body>
</html>
```

### 1-3 logout.php - Session Termination (세션 종료)

This simple script destroys the user session and redirects to the login page, effectively logging the user out.

이 간단한 스크립트는 사용자 세션을 파기하고 로그인 페이지로 리다이렉트하여 사용자를 로그아웃시킵니다.

**Filename: `logout.php`**

**파일명: `logout.php`**

```php
<?php

// Start session to access session variables (세션 변수 액세스를 위한 세션 시작)
session_start();
// Destroy all session data (모든 세션 데이터 파기)
session_destroy();
// Redirect to login page (로그인 페이지로 리다이렉트)
header("Location: login.php");
exit;

?>
```

---

## 📊 Required Tables (SQL) (필요한 테이블)

The database now includes two main tables: posts for board content and comments for user responses. The FOREIGN KEY with CASCADE ensures referential integrity.

데이터베이스는 이제 두 개의 주요 테이블을 포함합니다: 게시판 콘텐츠용 posts와 사용자 응답용 comments. CASCADE가 있는 FOREIGN KEY는 참조 무결성을 보장합니다.

```sql
-- posts table (existing from Chapter 9) (posts 테이블 - 9장 기존)
CREATE TABLE posts (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    title VARCHAR(255) NOT NULL,
    content LONGTEXT NOT NULL,
    views INT DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Sample post data (샘플 게시글 데이터)
INSERT INTO posts (user_id, title, content, views) VALUES
(1, 'Started Learning PHP!', 'PHP is really interesting. Making a board is amazing!', 5),
(2, 'Database Query Tips', 'Using WHERE clause effectively in SELECT queries helps find data easily.', 12),
(1, 'What is Web Development?', 'Learn HTML, CSS, JavaScript, PHP, MySQL to build websites!', 8),
(3, 'Starting Board Creation', 'Let\'s build a board in this chapter. Starting with database.', 3);

-- comments table (new in Chapter 10) (comments 테이블 - 10장 추가)
CREATE TABLE comments (
    id INT AUTO_INCREMENT PRIMARY KEY,
    post_id INT NOT NULL,
    user_id INT NOT NULL,
    content LONGTEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (post_id) REFERENCES posts(id) ON DELETE CASCADE
);
```

---

## 1️⃣ v2_list.php - Post List with Comment Count (게시글 목록 - 댓글 개수 표시)

**New Features in Chapter 10** (10장에서 새로 배운 것):

- Use LEFT JOIN to query comment count together
- Display comment count next to each post
- Add login verification
- LEFT JOIN으로 댓글 개수 함께 조회
- 각 게시글 옆에 댓글 개수 표시
- 로그인 확인 추가

This page demonstrates advanced SQL with JOIN operations to efficiently retrieve post information along with comment counts in a single query.

이 페이지는 단일 쿼리에서 댓글 수와 함께 게시글 정보를 효율적으로 검색하기 위해 JOIN 작업이 있는 고급 SQL을 보여줍니다.

**Filename: `v2_list.php`**

**파일명: `v2_list.php`**

```php
<?php

require 'config.php';
requireLogin();  // Chapter 10: Login verification required (10장: 로그인 확인 필수)

try {
    // Chapter 10: Use LEFT JOIN to query comment count together
    // (10장: LEFT JOIN으로 댓글 개수도 함께 조회)
    // COUNT(comments.id): Count comments for each post (각 게시글의 댓글 개수)
    $sql = "
        SELECT 
            posts.id,
            posts.title,
            posts.views,
            posts.created_at,
            COUNT(comments.id) AS comment_count
        FROM posts
        LEFT JOIN comments ON posts.id = comments.post_id
        GROUP BY posts.id
        ORDER BY posts.id DESC
    ";
  
    $stmt = $pdo->prepare($sql);
    $stmt->execute();
    $posts = $stmt->fetchAll(PDO::FETCH_ASSOC);
  
} catch (Exception $e) {
    $error = "Error: " . $e->getMessage();
}

?>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Board List</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: 'Segoe UI', sans-serif;
            background: #f5f5f5;
            padding: 20px;
        }
        .container {
            max-width: 800px;
            margin: 0 auto;
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 30px;
            border-bottom: 2px solid navy;
            padding-bottom: 15px;
        }
        h1 { color: navy; font-size: 28px; }
        .header-right {
            display: flex;
            gap: 10px;
        }
        .btn {
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
            text-decoration: none;
            display: inline-block;
        }
        .btn-primary { background: navy; color: white; }
        .btn-primary:hover { background: #000080; }
        .btn-secondary { background: #888; color: white; }
        .btn-secondary:hover { background: #666; }
        table {
            width: 100%;
            border-collapse: collapse;
        }
        th {
            background: #f0f0f0;
            padding: 12px;
            text-align: left;
            border-bottom: 2px solid navy;
        }
        td {
            padding: 12px;
            border-bottom: 1px solid #eee;
        }
        tr:hover {
            background: #f9f9f9;
        }
        a.post-title {
            color: #333;
            text-decoration: none;
            font-weight: 500;
        }
        a.post-title:hover {
            color: navy;
            text-decoration: underline;
        }
        .comment-badge {
            color: #2196F3;
            font-weight: bold;
            margin-left: 8px;
        }
    </style>
</head>
<body>

<div class="container">
    <div class="header">
        <h1>📋 Board List</h1>
        <div class="header-right">
            <a href="v2_write.php" class="btn btn-primary">✏️ Write</a>
            <a href="logout.php" class="btn btn-secondary">🚪 Logout</a>
        </div>
    </div>

    <table>
        <tr>
            <th>No.</th>
            <th>Title</th>
            <th>Views</th>
            <th>Date</th>
        </tr>
        <?php foreach ($posts as $post): ?>
            <tr>
                <td><?php echo $post['id']; ?></td>
                <td>
                    <a href="v2_view.php?id=<?php echo $post['id']; ?>" class="post-title">
                        <?php echo htmlspecialchars($post['title']); ?>
                        <?php if ($post['comment_count'] > 0): ?>
                            <span class="comment-badge">[<?php echo $post['comment_count']; ?>]</span>
                        <?php endif; ?>
                    </a>
                </td>
                <td><?php echo $post['views']; ?></td>
                <td><?php echo substr($post['created_at'], 0, 10); ?></td>
            </tr>
        <?php endforeach; ?>
    </table>
</div>

</body>
</html>
```

---

## 2️⃣ v2_view.php - Post Detail with Comments (게시글 상세 - 댓글 표시)

**New Features in Chapter 10** (10장에서 새로 배운 것):

- Display all comments below post content
- Add comment form at bottom
- Show edit/delete buttons for each comment
- 게시글 내용 아래에 모든 댓글 표시
- 하단에 댓글 작성 폼 추가
- 각 댓글에 수정/삭제 버튼 표시

This page integrates post viewing with complete comment functionality, creating a full discussion thread interface.

이 페이지는 게시글 보기와 완전한 댓글 기능을 통합하여 전체 토론 스레드 인터페이스를 만듭니다.

**Filename: `v2_view.php`**

**파일명: `v2_view.php`**

```php
<?php

require 'config.php';
requireLogin();  // Chapter 10: Login required (10장: 로그인 필수)

$id = $_GET['id'] ?? null;

if (!$id) {
    header("Location: v2_list.php");
    exit;
}

try {
    // Increment view count (조회수 증가)
    $update_sql = "UPDATE posts SET views = views + 1 WHERE id = ?";
    $update_stmt = $pdo->prepare($update_sql);
    $update_stmt->execute([$id]);
  
    // Fetch post details (게시글 상세 조회)
    $sql = "SELECT * FROM posts WHERE id = ?";
    $stmt = $pdo->prepare($sql);
    $stmt->execute([$id]);
    $post = $stmt->fetch(PDO::FETCH_ASSOC);
  
    if (!$post) {
        header("Location: v2_list.php");
        exit;
    }
  
    // Chapter 10: Fetch all comments for this post (10장: 이 게시글의 모든 댓글 조회)
    $comment_sql = "SELECT * FROM comments WHERE post_id = ? ORDER BY created_at ASC";
    $comment_stmt = $pdo->prepare($comment_sql);
    $comment_stmt->execute([$id]);
    $comments = $comment_stmt->fetchAll(PDO::FETCH_ASSOC);
  
} catch (Exception $e) {
    die("Error: " . $e->getMessage());
}

?>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title><?php echo htmlspecialchars($post['title']); ?></title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: 'Segoe UI', sans-serif;
            background: #f5f5f5;
            padding: 20px;
        }
        .container {
            max-width: 800px;
            margin: 0 auto;
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        h1 { 
            color: navy; 
            margin-bottom: 20px; 
            border-bottom: 2px solid navy;
            padding-bottom: 10px;
        }
        .post-meta {
            color: #666;
            margin-bottom: 30px;
            font-size: 14px;
        }
        .post-content {
            line-height: 1.8;
            margin-bottom: 30px;
            white-space: pre-wrap;
        }
        .button-group {
            display: flex;
            gap: 10px;
            margin-bottom: 40px;
            padding-bottom: 20px;
            border-bottom: 2px solid #eee;
        }
        .btn {
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
            text-decoration: none;
            display: inline-block;
        }
        .btn-primary { background: navy; color: white; }
        .btn-primary:hover { background: #000080; }
        .btn-secondary { background: #888; color: white; }
        .btn-secondary:hover { background: #666; }
        .btn-danger { background: #d32f2f; color: white; }
        .btn-danger:hover { background: #b71c1c; }
        .comments-section {
            margin-top: 30px;
        }
        .comments-section h2 {
            color: navy;
            margin-bottom: 20px;
            font-size: 20px;
        }
        .comment {
            background: #f9f9f9;
            padding: 15px;
            margin-bottom: 15px;
            border-radius: 5px;
            border-left: 3px solid navy;
        }
        .comment-meta {
            color: #666;
            font-size: 12px;
            margin-bottom: 10px;
        }
        .comment-content {
            line-height: 1.6;
            margin-bottom: 10px;
        }
        .comment-actions {
            display: flex;
            gap: 10px;
        }
        .comment-actions a {
            font-size: 12px;
            padding: 5px 10px;
        }
        .comment-form {
            margin-top: 30px;
            padding-top: 30px;
            border-top: 2px solid #eee;
        }
        .comment-form h3 {
            color: navy;
            margin-bottom: 15px;
        }
        textarea {
            width: 100%;
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 5px;
            resize: vertical;
            min-height: 100px;
            font-family: 'Segoe UI', sans-serif;
        }
    </style>
</head>
<body>

<div class="container">
    <h1><?php echo htmlspecialchars($post['title']); ?></h1>
  
    <div class="post-meta">
        👁️ Views: <?php echo $post['views']; ?> | 
        📅 Date: <?php echo substr($post['created_at'], 0, 16); ?>
    </div>
  
    <div class="post-content">
        <?php echo nl2br(htmlspecialchars($post['content'])); ?>
    </div>
  
    <div class="button-group">
        <a href="v2_edit.php?id=<?php echo $post['id']; ?>" class="btn btn-primary">✏️ Edit</a>
        <a href="v2_delete.php?id=<?php echo $post['id']; ?>" 
           class="btn btn-danger"
           onclick="return confirm('Are you sure you want to delete this post?');">🗑️ Delete</a>
        <a href="v2_list.php" class="btn btn-secondary">📋 Back to List</a>
    </div>
  
    <!-- Comments Section (댓글 섹션) -->
    <div class="comments-section">
        <h2>💬 Comments (<?php echo count($comments); ?>)</h2>
    
        <?php if (empty($comments)): ?>
            <p style="color: #999;">No comments yet. Be the first to comment!</p>
        <?php else: ?>
            <?php foreach ($comments as $comment): ?>
                <div class="comment">
                    <div class="comment-meta">
                        User ID: <?php echo $comment['user_id']; ?> | 
                        <?php echo substr($comment['created_at'], 0, 16); ?>
                    </div>
                    <div class="comment-content">
                        <?php echo nl2br(htmlspecialchars($comment['content'])); ?>
                    </div>
                    <div class="comment-actions">
                        <a href="v2_edit_comment.php?id=<?php echo $comment['id']; ?>&post_id=<?php echo $id; ?>" 
                           class="btn btn-primary">✏️ Edit</a>
                        <a href="v2_delete_comment.php?id=<?php echo $comment['id']; ?>&post_id=<?php echo $id; ?>" 
                           class="btn btn-danger"
                           onclick="return confirm('Are you sure you want to delete this comment?');">🗑️ Delete</a>
                    </div>
                </div>
            <?php endforeach; ?>
        <?php endif; ?>
    </div>
  
    <!-- Comment Form (댓글 작성 폼) -->
    <div class="comment-form">
        <h3>✍️ Write a Comment</h3>
        <form action="v2_add_comment.php" method="POST">
            <input type="hidden" name="post_id" value="<?php echo $post['id']; ?>">
            <textarea name="content" placeholder="Enter your comment..." required></textarea>
            <br><br>
            <button type="submit" class="btn btn-primary">💬 Add Comment</button>
        </form>
    </div>
</div>

</body>
</html>
```

---

## 3️⃣ v2_write.php - Write New Post (새 게시글 작성)

**Same as Chapter 9, with login required** (9장과 동일, 로그인 필수)

This page allows authenticated users to create new posts. The login requirement ensures all posts are associated with valid user accounts.

이 페이지는 인증된 사용자가 새 게시글을 만들 수 있도록 합니다. 로그인 요구 사항은 모든 게시글이 유효한 사용자 계정과 연결되도록 합니다.

**Filename: `v2_write.php`**

**파일명: `v2_write.php`**

```php
<?php

require 'config.php';
requireLogin();  // Chapter 10: Login required (10장: 로그인 필수)

$error = '';

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $title = $_POST['title'] ?? '';
    $content = $_POST['content'] ?? '';
  
    // Validation (검증)
    if (empty($title) || empty($content)) {
        $error = 'Please enter both title and content';
    } else {
        try {
            // Insert new post (새 게시글 삽입)
            $sql = "INSERT INTO posts (user_id, title, content) VALUES (?, ?, ?)";
            $stmt = $pdo->prepare($sql);
            $stmt->execute([$_SESSION['user_id'], $title, $content]);
        
            header("Location: v2_list.php");
            exit;
        } catch (Exception $e) {
            $error = "Error: " . $e->getMessage();
        }
    }
}

?>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Write Post</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: 'Segoe UI', sans-serif;
            background: #f5f5f5;
            padding: 20px;
        }
        .container {
            max-width: 800px;
            margin: 0 auto;
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        h1 { color: navy; margin-bottom: 30px; }
        .form-group {
            margin-bottom: 20px;
        }
        label {
            display: block;
            margin-bottom: 8px;
            color: #333;
            font-weight: bold;
        }
        input, textarea {
            width: 100%;
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-family: 'Segoe UI', sans-serif;
            font-size: 14px;
        }
        textarea { resize: vertical; min-height: 300px; }
        .button-group {
            display: flex;
            gap: 10px;
        }
        .btn {
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
            text-decoration: none;
            display: inline-block;
        }
        .btn-primary { background: navy; color: white; }
        .btn-primary:hover { background: #000080; }
        .btn-secondary { background: #888; color: white; }
        .btn-secondary:hover { background: #666; }
        .error { color: red; background: #ffebee; padding: 12px; border-radius: 5px; margin-bottom: 20px; }
    </style>
</head>
<body>

<div class="container">
    <h1>✏️ Write New Post</h1>
  
    <?php if ($error): ?>
        <div class="error">❌ <?php echo $error; ?></div>
    <?php endif; ?>
  
    <form method="POST">
        <div class="form-group">
            <label for="title">Title:</label>
            <input type="text" id="title" name="title" placeholder="Enter title" required>
        </div>
  
        <div class="form-group">
            <label for="content">Content:</label>
            <textarea id="content" name="content" placeholder="Enter content" required></textarea>
        </div>
  
        <div class="button-group">
            <button type="submit" class="btn btn-primary">✅ Save</button>
            <a href="v2_list.php" class="btn btn-secondary">⬅️ Cancel</a>
        </div>
    </form>
</div>

</body>
</html>
```

---

## 4️⃣ v2_edit.php - Edit Post (게시글 수정)

**Same as Chapter 9, with login required** (9장과 동일, 로그인 필수)

**Filename: `v2_edit.php`**

**파일명: `v2_edit.php`**

```php
<?php

require 'config.php';
requireLogin();  // Chapter 10: Login required (10장: 로그인 필수)

$id = $_GET['id'] ?? null;
$error = '';

if (!$id) {
    header("Location: v2_list.php");
    exit;
}

try {
    // Fetch existing post (기존 게시글 조회)
    $sql = "SELECT * FROM posts WHERE id = ?";
    $stmt = $pdo->prepare($sql);
    $stmt->execute([$id]);
    $post = $stmt->fetch(PDO::FETCH_ASSOC);
  
    if (!$post) {
        header("Location: v2_list.php");
        exit;
    }
  
    // Handle POST request (POST 요청 처리)
    if ($_SERVER['REQUEST_METHOD'] === 'POST') {
        $title = $_POST['title'] ?? '';
        $content = $_POST['content'] ?? '';
  
        if (empty($title) || empty($content)) {
            $error = 'Please enter both title and content';
        } else {
            // Update post (게시글 수정)
            $update_sql = "UPDATE posts SET title = ?, content = ? WHERE id = ?";
            $update_stmt = $pdo->prepare($update_sql);
            $update_stmt->execute([$title, $content, $id]);
        
            header("Location: v2_view.php?id=$id");
            exit;
        }
    }
} catch (Exception $e) {
    $error = "Error: " . $e->getMessage();
}

?>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Edit Post</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: 'Segoe UI', sans-serif;
            background: #f5f5f5;
            padding: 20px;
        }
        .container {
            max-width: 800px;
            margin: 0 auto;
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        h1 { color: navy; margin-bottom: 30px; }
        .form-group {
            margin-bottom: 20px;
        }
        label {
            display: block;
            margin-bottom: 8px;
            color: #333;
            font-weight: bold;
        }
        input, textarea {
            width: 100%;
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-family: 'Segoe UI', sans-serif;
            font-size: 14px;
        }
        textarea { resize: vertical; min-height: 300px; }
        .button-group {
            display: flex;
            gap: 10px;
        }
        .btn {
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
            text-decoration: none;
            display: inline-block;
        }
        .btn-primary { background: navy; color: white; }
        .btn-primary:hover { background: #000080; }
        .btn-secondary { background: #888; color: white; }
        .btn-secondary:hover { background: #666; }
        .error { color: red; background: #ffebee; padding: 12px; border-radius: 5px; margin-bottom: 20px; }
    </style>
</head>
<body>

<div class="container">
    <h1>✏️ Edit Post</h1>
  
    <?php if ($error): ?>
        <div class="error">❌ <?php echo $error; ?></div>
    <?php endif; ?>
  
    <form method="POST">
        <div class="form-group">
            <label for="title">Title:</label>
            <input type="text" id="title" name="title" value="<?php echo htmlspecialchars($post['title']); ?>" required>
        </div>
  
        <div class="form-group">
            <label for="content">Content:</label>
            <textarea id="content" name="content" required><?php echo htmlspecialchars($post['content']); ?></textarea>
        </div>
  
        <div class="button-group">
            <button type="submit" class="btn btn-primary">✅ Update</button>
            <a href="v2_view.php?id=<?php echo $post['id']; ?>" class="btn btn-secondary">⬅️ Cancel</a>
        </div>
    </form>
</div>

</body>
</html>
```

---

## 5️⃣ v2_delete.php - Delete Post (게시글 삭제)

**Same as Chapter 9** (9장과 동일)

When a post is deleted, all associated comments are automatically removed due to the CASCADE constraint.

게시글이 삭제되면 CASCADE 제약 조건으로 인해 모든 관련 댓글이 자동으로 제거됩니다.

**Filename: `v2_delete.php`**

**파일명: `v2_delete.php`**

```php
<?php

require 'config.php';
requireLogin();  // Chapter 10: Login required (10장: 로그인 필수)

$id = $_GET['id'] ?? null;

if (!$id) {
    header("Location: v2_list.php");
    exit;
}

try {
    // Delete post (comments are automatically deleted via CASCADE)
    // (게시글 삭제 - comments는 CASCADE로 자동 삭제)
    $sql = "DELETE FROM posts WHERE id = ?";
    $stmt = $pdo->prepare($sql);
    $stmt->execute([$id]);
  
    header("Location: v2_list.php");
    exit;
} catch (Exception $e) {
    die("Error: " . $e->getMessage());
}

?>
```

---

## 6️⃣ v2_add_comment.php - Add Comment (댓글 작성)

**New in Chapter 10** (10장 새로 추가)

This page handles comment submission. It receives comment data via POST, inserts it into the database, and redirects back to the post view.

이 페이지는 댓글 제출을 처리합니다. POST를 통해 댓글 데이터를 받고, 데이터베이스에 삽입하고, 게시글 보기로 다시 리다이렉트합니다.

- Receive comment via POST and save to database
- Return to post after saving
- 댓글을 POST로 받아서 DB에 저장
- 저장 후 다시 게시글로 돌아감

**Filename: `v2_add_comment.php`**

**파일명: `v2_add_comment.php`**

```php
<?php

require 'config.php';
requireLogin();  // Chapter 10: Login required (10장: 로그인 필수)

// Receive POST data (POST 데이터 받기)
$post_id = $_POST['post_id'] ?? null;
$content = $_POST['content'] ?? '';

// Validate input (입력 검증)
if (!$post_id || empty($content)) {
    header("Location: v2_list.php");
    exit;
}

try {
    // Insert new comment (새 댓글 삽입)
    $sql = "INSERT INTO comments (post_id, user_id, content) VALUES (?, ?, ?)";
    $stmt = $pdo->prepare($sql);
    $stmt->execute([$post_id, $_SESSION['user_id'], $content]);
  
    // Redirect back to post view (게시글 상세로 다시 이동)
    header("Location: v2_view.php?id=$post_id");
    exit;
} catch (Exception $e) {
    die("Error: " . $e->getMessage());
}

?>
```

---

## 7️⃣ v2_edit_comment.php - Edit Comment (댓글 수정)

**New in Chapter 10** (10장 새로 추가)

This page allows users to modify their existing comments. It pre-fills the form with current content and updates the database on submission.

이 페이지는 사용자가 기존 댓글을 수정할 수 있도록 합니다. 현재 내용으로 폼을 미리 채우고 제출 시 데이터베이스를 업데이트합니다.

**Filename: `v2_edit_comment.php`**

**파일명: `v2_edit_comment.php`**

```php
<?php

require 'config.php';
requireLogin();  // Chapter 10: Login required (10장: 로그인 필수)

// Receive URL parameters (URL 매개변수 받기)
$id = $_GET['id'] ?? null;
$post_id = $_GET['post_id'] ?? null;
$error = '';

if (!$id || !$post_id) {
    header("Location: v2_list.php");
    exit;
}

try {
    // Fetch existing comment (기존 댓글 조회)
    $sql = "SELECT * FROM comments WHERE id = ?";
    $stmt = $pdo->prepare($sql);
    $stmt->execute([$id]);
    $comment = $stmt->fetch(PDO::FETCH_ASSOC);
  
    if (!$comment) {
        header("Location: v2_view.php?id=$post_id");
        exit;
    }
  
    // Handle POST request (POST 요청 처리)
    if ($_SERVER['REQUEST_METHOD'] === 'POST') {
        $content = $_POST['content'] ?? '';
  
        if (empty($content)) {
            $error = 'Please enter comment content';
        } else {
            // Update comment (댓글 수정)
            $update_sql = "UPDATE comments SET content = ? WHERE id = ?";
            $update_stmt = $pdo->prepare($update_sql);
            $update_stmt->execute([$content, $id]);
  
            header("Location: v2_view.php?id=$post_id");
            exit;
        }
    }
} catch (Exception $e) {
    $error = "Error: " . $e->getMessage();
}

?>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Edit Comment</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: 'Segoe UI', sans-serif;
            background: #f5f5f5;
            padding: 20px;
        }
        .container {
            max-width: 600px;
            margin: 0 auto;
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        h1 { color: navy; margin-bottom: 30px; font-size: 22px; }
        .form-group {
            margin-bottom: 20px;
        }
        label {
            display: block;
            margin-bottom: 8px;
            color: #333;
            font-weight: bold;
        }
        textarea {
            width: 100%;
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-family: 'Segoe UI', sans-serif;
            font-size: 14px;
            resize: vertical;
            min-height: 150px;
        }
        .button-group {
            display: flex;
            gap: 10px;
        }
        .btn {
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
            text-decoration: none;
            display: inline-block;
        }
        .btn-primary { background: navy; color: white; }
        .btn-primary:hover { background: #000080; }
        .btn-secondary { background: #888; color: white; }
        .btn-secondary:hover { background: #666; }
        .error { color: red; background: #ffebee; padding: 12px; border-radius: 5px; margin-bottom: 20px; }
    </style>
</head>
<body>

<div class="container">
    <h1>✏️ Edit Comment</h1>
  
    <?php if ($error): ?>
        <div class="error">❌ <?php echo $error; ?></div>
    <?php endif; ?>
  
    <form method="POST">
        <div class="form-group">
            <label for="content">Comment:</label>
            <textarea id="content" name="content" required><?php echo htmlspecialchars($comment['content']); ?></textarea>
        </div>
  
        <div class="button-group">
            <button type="submit" class="btn btn-primary">✅ Update</button>
            <a href="v2_view.php?id=<?php echo $post_id; ?>" class="btn btn-secondary">⬅️ Cancel</a>
        </div>
    </form>
</div>

</body>
</html>
```

---

## 8️⃣ v2_delete_comment.php - Delete Comment (댓글 삭제)

**New in Chapter 10** (10장 새로 추가)

This simple script removes a comment from the database and redirects back to the post view.

이 간단한 스크립트는 데이터베이스에서 댓글을 제거하고 게시글 보기로 다시 리다이렉트합니다.

**Filename: `v2_delete_comment.php`**

**파일명: `v2_delete_comment.php`**

```php
<?php

require 'config.php';
requireLogin();  // Chapter 10: Login required (10장: 로그인 필수)

// Receive URL parameters (URL 매개변수 받기)
$id = $_GET['id'] ?? null;
$post_id = $_GET['post_id'] ?? null;

if (!$id || !$post_id) {
    header("Location: v2_list.php");
    exit;
}

try {
    // Delete comment (댓글 삭제)
    $sql = "DELETE FROM comments WHERE id = ?";
    $stmt = $pdo->prepare($sql);
    $stmt->execute([$id]);
  
    // Redirect back to post view (게시글 상세로 다시 이동)
    header("Location: v2_view.php?id=$post_id");
    exit;
} catch (Exception $e) {
    die("Error: " . $e->getMessage());
}

?>
```

---

## 🎯 Chapter 9 vs Chapter 10 Comparison (9장 vs 10장 비교)

This table summarizes the key differences between the two chapters, highlighting the new comment functionality.

이 표는 두 장 사이의 주요 차이점을 요약하고 새로운 댓글 기능을 강조합니다.

| Feature        | Chapter 9 | Chapter 10            |
| -------------- | --------- | --------------------- |
| Post List      | ✅        | ✅ + Comment count    |
| View Post      | ✅        | ✅ + Display comments |
| Write Post     | ✅        | ✅ + Login required   |
| Edit Post      | ✅        | ✅ + Login required   |
| Delete Post    | ✅        | ✅ + Login required   |
| Comment System | ❌        | ✅ Newly added        |
| Login System   | ❌        | ✅ Required           |

| 기능        | 9장 | 10장                |
| ----------- | --- | ------------------- |
| 게시글 목록 | ✅  | ✅ + 댓글 개수 표시 |
| 게시글 보기 | ✅  | ✅ + 댓글 표시      |
| 게시글 작성 | ✅  | ✅ + 로그인 필수    |
| 게시글 수정 | ✅  | ✅ + 로그인 필수    |
| 게시글 삭제 | ✅  | ✅ + 로그인 필수    |
| 댓글 기능   | ❌  | ✅ 새로 추가        |
| 로그인      | ❌  | ✅ 필수             |

---

Thank you for your attention.  
Cho Jeonghyun ([peterchokr@gmail.com](mailto:peterchokr@gmail.com)). Yeungnam University College

"Produced in collaboration with Claude and Gemini."
