# Chapter 11. Board System Implementation - Part 3 - Add File Attachment Feature

---

## 📚 Learning Objectives

After completing this chapter, you will be able to:

✅ Implement file attachment functionality in a board system
✅ Validate uploaded files for security (size, extension, error checking)
✅ Store files safely with unique naming conventions
✅ Use multiple LEFT JOIN operations to aggregate related data
✅ Display and manage file attachments with posts
✅ Download attached files securely
✅ Delete files from both database and filesystem
✅ Build a complete board system with posts, comments, and file attachments

이 장을 학습하고 나면 다음을 할 수 있습니다:

✅ 게시판 시스템에 파일 첨부 기능을 구현할 수 있습니다
✅ 보안을 위해 업로드된 파일을 검증할 수 있습니다 (크기, 확장자, 오류 확인)
✅ 고유한 명명 규칙으로 파일을 안전하게 저장할 수 있습니다
✅ 여러 LEFT JOIN 작업을 사용하여 관련 데이터를 집계할 수 있습니다
✅ 게시글과 함께 파일 첨부를 표시하고 관리할 수 있습니다
✅ 첨부 파일을 안전하게 다운로드할 수 있습니다
✅ 데이터베이스와 파일 시스템 모두에서 파일을 삭제할 수 있습니다
✅ 게시글, 댓글, 파일 첨부가 있는 완전한 게시판 시스템을 구축할 수 있습니다

---

## 📌 Chapter Overview (이 장의 특징)

This chapter completes the board system by adding file attachment functionality. It builds upon Chapters 9 and 10, incorporating all previous features while adding secure file upload, storage, and download capabilities.

이 장은 파일 첨부 기능을 추가하여 게시판 시스템을 완성합니다. 9장과 10장을 기반으로 하며, 이전의 모든 기능을 통합하면서 안전한 파일 업로드, 저장 및 다운로드 기능을 추가합니다.

✅ **Complete Code**: All code from Chapters 9 + 10 + File attachment features

✅ **완전한 코드**: (9장 + 10장) 코드를 모두 포함 + 파일 첨부 기능 추가

---

## 🔑 Essential Files (필수 파일)

### 1-1 config.php - Database Connection and Session (데이터베이스 연결 및 세션)

This configuration file remains the same as previous chapters, providing centralized database connection and session management.

이 설정 파일은 이전 장과 동일하며, 중앙화된 데이터베이스 연결과 세션 관리를 제공합니다.

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

The login page redirects to v3_list.php (version 3) to reflect the file attachment functionality.

로그인 페이지는 파일 첨부 기능을 반영하기 위해 v3_list.php (버전 3)로 리다이렉트합니다.

**Filename: `login.php`**

**파일명: `login.php`**

```php
<?php

// Start session for login state (로그인 상태를 위한 세션 시작)
session_start();

// If already logged in, redirect to v3_list (이미 로그인했으면 v3_list로 이동)
if (isset($_SESSION['user_id'])) {
    header("Location: v3_list.php");  // Changed to v3_list.php (v3_list.php로 변경)
    exit;
}

$error = '';

// Handle POST request (POST 요청 처리)
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $username = $_POST['username'] ?? '';
    $password = $_POST['password'] ?? '';
  
    // For testing: john / password123 (테스트용: john / password123)
    if ($username === 'john' && $password === 'password123') {
        $_SESSION['user_id'] = 1;
        $_SESSION['username'] = 'john';
        header("Location: v3_list.php");  // Changed to v3_list.php (v3_list.php로 변경)
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

The database now includes three main tables: posts, comments, and attachments. The new attachments table stores file metadata with foreign key relationships.

데이터베이스는 이제 세 개의 주요 테이블을 포함합니다: posts, comments, attachments. 새로운 attachments 테이블은 외래 키 관계와 함께 파일 메타데이터를 저장합니다.

```sql
-- posts table (from Chapter 9) (posts 테이블 - 9장 기존)
CREATE TABLE posts (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    title VARCHAR(255) NOT NULL,
    content LONGTEXT NOT NULL,
    views INT DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- comments table (from Chapter 10) (comments 테이블 - 10장 기존)
CREATE TABLE comments (
    id INT AUTO_INCREMENT PRIMARY KEY,
    post_id INT NOT NULL,
    user_id INT NOT NULL,
    content LONGTEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (post_id) REFERENCES posts(id) ON DELETE CASCADE
);

-- attachments table (new in Chapter 11) (attachments 테이블 - 11장 추가)
CREATE TABLE attachments (
    id INT AUTO_INCREMENT PRIMARY KEY,
    post_id INT NOT NULL,
    user_id INT NOT NULL,
    original_name VARCHAR(255) NOT NULL,
    stored_name VARCHAR(255) NOT NULL,
    file_size INT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (post_id) REFERENCES posts(id) ON DELETE CASCADE
);
```

---

## ⚠️ Create uploads Directory (uploads 디렉토리 생성)

Before using file upload functionality, create the uploads directory with proper permissions. This directory will store all uploaded files.

파일 업로드 기능을 사용하기 전에 적절한 권한으로 uploads 디렉토리를 생성하세요. 이 디렉토리는 모든 업로드된 파일을 저장합니다.

```bash
mkdir -p boards/uploads/files
chmod 777 boards/uploads/files
```

**Important**: In production environments, use more restrictive permissions (755) and proper web server configuration.

**중요**: 프로덕션 환경에서는 더 제한적인 권한(755)과 적절한 웹 서버 구성을 사용하세요.

---

## 1️⃣ upload_file.php - File Upload Function (파일 업로드 함수)

This utility file provides a secure file upload function with comprehensive validation. It checks file size, extension, and errors before storing files with unique names.

이 유틸리티 파일은 포괄적인 검증과 함께 안전한 파일 업로드 함수를 제공합니다. 고유한 이름으로 파일을 저장하기 전에 파일 크기, 확장자 및 오류를 확인합니다.

**Filename: `upload_file.php`**

**파일명: `upload_file.php`**

```php
<?php

// upload_file.php - File upload processing (파일 업로드 처리)
// Purpose: Provide secure file upload function (역할: 안전한 파일 업로드 함수 제공)

function uploadFile($file, $post_id) {
    // File validation settings (파일 검증 설정)
    $max_size = 10 * 1024 * 1024;  // 10MB maximum (10MB 최대)
    $allowed_ext = ['jpg', 'jpeg', 'png', 'gif', 'pdf', 'doc', 'docx', 'txt'];
  
    // Check file size (파일 크기 확인)
    if ($file['size'] > $max_size) {
        throw new Exception("File size must be 10MB or less");
    }
  
    // Check upload errors (파일 업로드 오류 확인)
    if ($file['error'] !== UPLOAD_ERR_OK) {
        throw new Exception("File upload error: " . $file['error']);
    }
  
    // Check file extension (확장자 확인)
    $ext = strtolower(pathinfo($file['name'], PATHINFO_EXTENSION));
    if (!in_array($ext, $allowed_ext)) {
        throw new Exception("File type not allowed: " . $ext);
    }
  
    // Create upload directory if it doesn't exist (디렉토리 생성)
    $upload_dir = __DIR__ . '/uploads/files';
    if (!is_dir($upload_dir)) {
        mkdir($upload_dir, 0777, true);
    }
  
    // Generate safe filename with timestamp_postID_original (안전한 파일명 생성)
    $timestamp = time();
    $stored_name = "{$timestamp}_{$post_id}_" . basename($file['name']);
    $file_path = $upload_dir . '/' . $stored_name;
  
    // Move uploaded file to destination (파일 이동)
    if (!move_uploaded_file($file['tmp_name'], $file_path)) {
        throw new Exception("Failed to save file");
    }
  
    // Return file metadata (파일 메타데이터 반환)
    return [
        'original_name' => $file['name'],
        'stored_name' => $stored_name,
        'file_size' => $file['size']
    ];
}

?>
```

---

## 2️⃣ v3_list.php - Post List with Comments and Files (게시글 목록 - 댓글 + 파일 개수 표시)

**New Features in Chapter 11** (11장에서 새로 배운 것):

- Use LEFT JOIN to query comment count + file count together
- Display comment count + file count next to each post

- LEFT JOIN으로 댓글 + 첨부파일 개수 함께 조회
- 각 게시글 옆에 댓글 개수 + 파일 개수 표시

This page demonstrates advanced SQL with multiple LEFT JOIN operations to efficiently retrieve post information along with both comment and file attachment counts.

이 페이지는 댓글과 파일 첨부 수와 함께 게시글 정보를 효율적으로 검색하기 위해 여러 LEFT JOIN 작업이 있는 고급 SQL을 보여줍니다.

**Filename: `v3_list.php`**

**파일명: `v3_list.php`**

```php
<?php

require 'config.php';
requireLogin();

try {
    // Chapter 11: Use two LEFT JOINs to query comments + files together
    // (11장: 두 개의 LEFT JOIN으로 댓글 + 파일 개수 함께 조회)
    // COUNT(DISTINCT comments.id): Comment count (댓글 개수)
    // COUNT(DISTINCT attachments.id): File attachment count (첨부파일 개수)
    $sql = "
        SELECT 
            posts.id,
            posts.title,
            posts.views,
            posts.created_at,
            COUNT(DISTINCT comments.id) AS comment_count,
            COUNT(DISTINCT attachments.id) AS file_count
        FROM posts
        LEFT JOIN comments ON posts.id = comments.post_id
        LEFT JOIN attachments ON posts.id = attachments.post_id
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
        .badge {
            font-weight: bold;
            margin-left: 8px;
        }
        .comment-badge {
            color: #2196F3;
        }
        .file-badge {
            color: #4CAF50;
        }
    </style>
</head>
<body>

<div class="container">
    <div class="header">
        <h1>📋 Board List</h1>
        <div class="header-right">
            <a href="v3_write.php" class="btn btn-primary">✏️ Write</a>
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
                    <a href="v3_view.php?id=<?php echo $post['id']; ?>" class="post-title">
                        <?php echo htmlspecialchars($post['title']); ?>
                        <?php if ($post['comment_count'] > 0): ?>
                            <span class="badge comment-badge">💬[<?php echo $post['comment_count']; ?>]</span>
                        <?php endif; ?>
                        <?php if ($post['file_count'] > 0): ?>
                            <span class="badge file-badge">📎[<?php echo $post['file_count']; ?>]</span>
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

## 3️⃣ v3_write.php - Write Post with File Upload (게시글 작성 - 파일 업로드 포함)

**New Features in Chapter 11** (11장에서 새로 배운 것):

- Add file upload input to form
- Process uploaded files and save to database
- Support multiple file uploads

- 폼에 파일 업로드 input 추가
- 업로드된 파일 처리 및 데이터베이스 저장
- 여러 파일 업로드 지원

This page enables users to attach files when creating posts. It uses the uploadFile() function for secure file handling and stores metadata in the attachments table.

이 페이지는 사용자가 게시글을 작성할 때 파일을 첨부할 수 있도록 합니다. 안전한 파일 처리를 위해 uploadFile() 함수를 사용하고 attachments 테이블에 메타데이터를 저장합니다.

**Filename: `v3_write.php`**

**파일명: `v3_write.php`**

```php
<?php

require 'config.php';
require 'upload_file.php';
requireLogin();

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
          
            // Get inserted post ID (삽입된 게시글 ID 가져오기)
            $post_id = $pdo->lastInsertId();
          
            // Chapter 11: Process file uploads (11장: 파일 업로드 처리)
            if (!empty($_FILES['files']['name'][0])) {
                foreach ($_FILES['files']['name'] as $key => $name) {
                    if ($_FILES['files']['error'][$key] === UPLOAD_ERR_OK) {
                        // Prepare single file array (단일 파일 배열 준비)
                        $file = [
                            'name' => $_FILES['files']['name'][$key],
                            'tmp_name' => $_FILES['files']['tmp_name'][$key],
                            'size' => $_FILES['files']['size'][$key],
                            'error' => $_FILES['files']['error'][$key]
                        ];
                      
                        // Upload file (파일 업로드)
                        $file_info = uploadFile($file, $post_id);
                      
                        // Save file metadata to database (파일 메타데이터 DB 저장)
                        $file_sql = "INSERT INTO attachments 
                                    (post_id, user_id, original_name, stored_name, file_size) 
                                    VALUES (?, ?, ?, ?, ?)";
                        $file_stmt = $pdo->prepare($file_sql);
                        $file_stmt->execute([
                            $post_id,
                            $_SESSION['user_id'],
                            $file_info['original_name'],
                            $file_info['stored_name'],
                            $file_info['file_size']
                        ]);
                    }
                }
            }
          
            header("Location: v3_list.php");
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
        input[type="text"], textarea {
            width: 100%;
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-family: 'Segoe UI', sans-serif;
            font-size: 14px;
        }
        textarea { resize: vertical; min-height: 300px; }
        input[type="file"] {
            width: 100%;
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 5px;
            background: #f9f9f9;
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
        .info {
            color: #666;
            font-size: 12px;
            margin-top: 5px;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>✏️ Write New Post</h1>
  
    <?php if ($error): ?>
        <div class="error">❌ <?php echo $error; ?></div>
    <?php endif; ?>
  
    <form method="POST" enctype="multipart/form-data">
        <div class="form-group">
            <label for="title">Title:</label>
            <input type="text" id="title" name="title" placeholder="Enter title" required>
        </div>
  
        <div class="form-group">
            <label for="content">Content:</label>
            <textarea id="content" name="content" placeholder="Enter content" required></textarea>
        </div>
  
        <div class="form-group">
            <label for="files">Attach Files (Optional):</label>
            <input type="file" id="files" name="files[]" multiple>
            <div class="info">
                Allowed: jpg, jpeg, png, gif, pdf, doc, docx, txt (Max 10MB per file)
            </div>
        </div>
  
        <div class="button-group">
            <button type="submit" class="btn btn-primary">✅ Save</button>
            <a href="v3_list.php" class="btn btn-secondary">⬅️ Cancel</a>
        </div>
    </form>
</div>

</body>
</html>
```

---

## 4️⃣ v3_view.php - View Post with File Attachments (게시글 상세 - 파일 첨부 표시)

**New Features in Chapter 11** (11장에서 새로 배운 것):

- Display attached files below post content
- Show file name, size, and download link
- Support file deletion

- 게시글 내용 아래에 첨부 파일 표시
- 파일명, 크기, 다운로드 링크 표시
- 파일 삭제 지원

This page integrates file attachments with post viewing, displaying all files with download links and delete options.

이 페이지는 파일 첨부를 게시글 보기와 통합하여 다운로드 링크와 삭제 옵션이 있는 모든 파일을 표시합니다.

**Filename: `v3_view.php`**

**파일명: `v3_view.php`**

```php
<?php

require 'config.php';
requireLogin();

$id = $_GET['id'] ?? null;

if (!$id) {
    header("Location: v3_list.php");
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
        header("Location: v3_list.php");
        exit;
    }
  
    // Fetch comments (댓글 조회)
    $comment_sql = "SELECT * FROM comments WHERE post_id = ? ORDER BY created_at ASC";
    $comment_stmt = $pdo->prepare($comment_sql);
    $comment_stmt->execute([$id]);
    $comments = $comment_stmt->fetchAll(PDO::FETCH_ASSOC);
  
    // Chapter 11: Fetch attached files (11장: 첨부 파일 조회)
    $file_sql = "SELECT * FROM attachments WHERE post_id = ? ORDER BY created_at ASC";
    $file_stmt = $pdo->prepare($file_sql);
    $file_stmt->execute([$id]);
    $attachments = $file_stmt->fetchAll(PDO::FETCH_ASSOC);
  
} catch (Exception $e) {
    die("Error: " . $e->getMessage());
}

// Helper function to format file size (파일 크기 형식화 헬퍼 함수)
function formatFileSize($bytes) {
    if ($bytes >= 1048576) {
        return number_format($bytes / 1048576, 2) . ' MB';
    } elseif ($bytes >= 1024) {
        return number_format($bytes / 1024, 2) . ' KB';
    }
    return $bytes . ' bytes';
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
        .attachments-section {
            background: #f9f9f9;
            padding: 15px;
            margin-bottom: 30px;
            border-radius: 5px;
            border-left: 3px solid #4CAF50;
        }
        .attachments-section h3 {
            color: #4CAF50;
            margin-bottom: 15px;
            font-size: 16px;
        }
        .file-item {
            background: white;
            padding: 10px;
            margin-bottom: 10px;
            border-radius: 3px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .file-info {
            flex: 1;
        }
        .file-name {
            font-weight: bold;
            color: #333;
        }
        .file-size {
            color: #666;
            font-size: 12px;
            margin-left: 10px;
        }
        .file-actions {
            display: flex;
            gap: 5px;
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
        .btn-small {
            padding: 5px 10px;
            font-size: 12px;
        }
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
  
    <!-- Chapter 11: Attachments Section (11장: 첨부파일 섹션) -->
    <?php if (!empty($attachments)): ?>
        <div class="attachments-section">
            <h3>📎 Attached Files (<?php echo count($attachments); ?>)</h3>
            <?php foreach ($attachments as $file): ?>
                <div class="file-item">
                    <div class="file-info">
                        <span class="file-name">📄 <?php echo htmlspecialchars($file['original_name']); ?></span>
                        <span class="file-size">(<?php echo formatFileSize($file['file_size']); ?>)</span>
                    </div>
                    <div class="file-actions">
                        <a href="v3_download.php?id=<?php echo $file['id']; ?>" class="btn btn-primary btn-small">⬇️ Download</a>
                        <a href="v3_delete_file.php?id=<?php echo $file['id']; ?>&post_id=<?php echo $id; ?>" 
                           class="btn btn-danger btn-small"
                           onclick="return confirm('Delete this file?');">🗑️ Delete</a>
                    </div>
                </div>
            <?php endforeach; ?>
        </div>
    <?php endif; ?>
  
    <div class="button-group">
        <a href="v3_edit.php?id=<?php echo $post['id']; ?>" class="btn btn-primary">✏️ Edit</a>
        <a href="v3_delete.php?id=<?php echo $post['id']; ?>" 
           class="btn btn-danger"
           onclick="return confirm('Delete this post?');">🗑️ Delete</a>
        <a href="v3_list.php" class="btn btn-secondary">📋 Back to List</a>
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
                        <a href="v3_edit_comment.php?id=<?php echo $comment['id']; ?>&post_id=<?php echo $id; ?>" 
                           class="btn btn-primary btn-small">✏️ Edit</a>
                        <a href="v3_delete_comment.php?id=<?php echo $comment['id']; ?>&post_id=<?php echo $id; ?>" 
                           class="btn btn-danger btn-small"
                           onclick="return confirm('Delete this comment?');">🗑️ Delete</a>
                    </div>
                </div>
            <?php endforeach; ?>
        <?php endif; ?>
    </div>
  
    <!-- Comment Form (댓글 작성 폼) -->
    <div class="comment-form">
        <h3>✍️ Write a Comment</h3>
        <form action="v3_add_comment.php" method="POST">
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

## 5️⃣ v3_download.php - Download File (파일 다운로드)

**New in Chapter 11** (11장 새로 추가)

This script handles secure file downloads by verifying file existence and setting proper headers for browser download.

이 스크립트는 파일 존재를 확인하고 브라우저 다운로드를 위한 적절한 헤더를 설정하여 안전한 파일 다운로드를 처리합니다.

**Filename: `v3_download.php`**

**파일명: `v3_download.php`**

```php
<?php

require 'config.php';
requireLogin();

$id = $_GET['id'] ?? null;

if (!$id) {
    header("Location: v3_list.php");
    exit;
}

try {
    // Fetch file metadata (파일 메타데이터 조회)
    $sql = "SELECT * FROM attachments WHERE id = ?";
    $stmt = $pdo->prepare($sql);
    $stmt->execute([$id]);
    $file = $stmt->fetch(PDO::FETCH_ASSOC);
  
    if (!$file) {
        die("File not found");
    }
  
    // Build file path (파일 경로 구성)
    $file_path = __DIR__ . '/uploads/files/' . $file['stored_name'];
  
    // Check if file exists (파일 존재 확인)
    if (!file_exists($file_path)) {
        die("File does not exist on server");
    }
  
    // Set headers for download (다운로드를 위한 헤더 설정)
    header('Content-Description: File Transfer');
    header('Content-Type: application/octet-stream');
    header('Content-Disposition: attachment; filename="' . basename($file['original_name']) . '"');
    header('Content-Length: ' . filesize($file_path));
    header('Cache-Control: must-revalidate');
    header('Pragma: public');
  
    // Output file content (파일 내용 출력)
    readfile($file_path);
    exit;
  
} catch (Exception $e) {
    die("Error: " . $e->getMessage());
}

?>
```

---

## 6️⃣ v3_delete_file.php - Delete File (파일 삭제)

**New in Chapter 11** (11장 새로 추가)

This script deletes files from both the database and filesystem, ensuring complete cleanup.

이 스크립트는 데이터베이스와 파일 시스템 모두에서 파일을 삭제하여 완전한 정리를 보장합니다.

**Filename: `v3_delete_file.php`**

**파일명: `v3_delete_file.php`**

```php
<?php

require 'config.php';
requireLogin();

// Receive URL parameters (URL 매개변수 받기)
$id = $_GET['id'] ?? null;
$post_id = $_GET['post_id'] ?? null;

if (!$id || !$post_id) {
    header("Location: v3_list.php");
    exit;
}

try {
    // Fetch file metadata (파일 메타데이터 조회)
    $sql = "SELECT * FROM attachments WHERE id = ?";
    $stmt = $pdo->prepare($sql);
    $stmt->execute([$id]);
    $file = $stmt->fetch(PDO::FETCH_ASSOC);
  
    if ($file) {
        // Delete file from filesystem (파일 시스템에서 파일 삭제)
        $file_path = __DIR__ . '/uploads/files/' . $file['stored_name'];
        if (file_exists($file_path)) {
            unlink($file_path);
        }
      
        // Delete from database (데이터베이스에서 삭제)
        $delete_sql = "DELETE FROM attachments WHERE id = ?";
        $delete_stmt = $pdo->prepare($delete_sql);
        $delete_stmt->execute([$id]);
    }
  
    // Redirect back to post view (게시글 상세로 다시 이동)
    header("Location: v3_view.php?id=$post_id");
    exit;
  
} catch (Exception $e) {
    die("Error: " . $e->getMessage());
}

?>
```

---

## 🎯 Chapter Progression Summary (장별 진행 요약)

This table shows the evolution of the board system across all three chapters.

이 표는 세 장에 걸친 게시판 시스템의 진화를 보여줍니다.

| Feature        | Chapter 9 | Chapter 10            | Chapter 11                    |
| -------------- | --------- | --------------------- | ----------------------------- |
| Post List      | ✅        | ✅ + Comment count    | ✅ + Comment + File count     |
| View Post      | ✅        | ✅ + Comments         | ✅ + Comments + Files         |
| Write Post     | ✅        | ✅ + Login required   | ✅ + File upload              |
| Edit Post      | ✅        | ✅ + Login required   | ✅ + Login required           |
| Delete Post    | ✅        | ✅ + Login required   | ✅ + Cascade delete files     |
| Comment System | ❌        | ✅ New feature        | ✅ Included                   |
| Login System   | ❌        | ✅ Required           | ✅ Required                   |
| File Upload    | ❌        | ❌                    | ✅ New feature                |

| 기능        | 9장 | 10장                | 11장                      |
| ----------- | --- | ------------------- | ------------------------- |
| 게시글 목록 | ✅  | ✅ + 댓글 개수 표시 | ✅ + 댓글 + 파일 개수     |
| 게시글 보기 | ✅  | ✅ + 댓글 표시      | ✅ + 댓글 + 파일 표시     |
| 게시글 작성 | ✅  | ✅ + 로그인 필수    | ✅ + 파일 업로드          |
| 게시글 수정 | ✅  | ✅ + 로그인 필수    | ✅ + 로그인 필수          |
| 게시글 삭제 | ✅  | ✅ + 로그인 필수    | ✅ + 연쇄 파일 삭제       |
| 댓글 기능   | ❌  | ✅ 새로 추가        | ✅ 포함됨                 |
| 로그인      | ❌  | ✅ 필수             | ✅ 필수                   |
| 파일 업로드 | ❌  | ❌                  | ✅ 새로 추가              |

---

Thank you for your attention.

Professor Cho Jeonghyun (peterchokr@gmail.com)  
Yeungnam University College
