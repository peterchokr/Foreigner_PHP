# Chapter 12. Advanced Board Features - User Management and Permissions

---

## 📚 Learning Objectives

After completing this chapter, you will be able to:

✅ Create user profile pages and display user information
✅ Manage posts and comments written by specific users
✅ Implement administrator permissions and admin-only features
✅ Design and implement role-based access control systems
✅ Differentiate between regular users and administrators
✅ Verify user permissions before performing sensitive operations
✅ Use SQL queries to aggregate user activity data
✅ Build comprehensive user management systems

이 장을 학습하고 나면 다음을 할 수 있습니다:

✅ 사용자 프로필 페이지를 만들고 사용자 정보를 표시할 수 있습니다
✅ 특정 사용자가 작성한 게시글과 댓글을 관리할 수 있습니다
✅ 관리자 권한을 설정하고 관리자만 수행 가능한 기능을 구현할 수 있습니다
✅ 역할 기반 액세스 제어 시스템을 설계하고 구현할 수 있습니다
✅ 일반 사용자와 관리자를 구분할 수 있습니다
✅ 민감한 작업을 수행하기 전에 사용자 권한을 확인할 수 있습니다
✅ SQL 쿼리를 사용하여 사용자 활동 데이터를 집계할 수 있습니다
✅ 포괄적인 사용자 관리 시스템을 구축할 수 있습니다

---

## 📌 Chapter Overview (이 장에서 배우는 것)

### **What Will We Build?** (무엇을 만드나요?)

👤 **User Management System** - User profiles, post management, administrator permissions

👤 **사용자 관리 시스템** - 사용자 프로필, 작성 글 관리, 관리자 권한

### **Why Do We Need It?** (왜 필요한가요?)

- 👥 Track user activity per user (사용자별 활동 추적)
- 🔐 Permission management (who can do what) (권한 관리 - 누가 뭘 할 수 있는가)
- 🗑️ Delete spam/abusive posts (administrators) (스팸/욕설 게시글 삭제 - 관리자)

### **How Will We Build It?** (어떻게 만드나요?)

```
Step 1: Design user permissions (regular users vs administrators)
Step 2: Implement profile pages
Step 3: Add permission verification logic

1단계: 사용자 권한 설계 (일반 사용자 vs 관리자)
2단계: 프로필 페이지 구현
3단계: 권한 확인 로직 추가
```

---

## 1️⃣ User Permission Database Design (사용자 권한 데이터베이스 설계)

### 1-1 Understanding Permission Systems (권한 시스템 이해)

Modern web applications require different permission levels to control user actions. This chapter implements a two-tier system: regular users and administrators.

현대 웹 애플리케이션은 사용자 작업을 제어하기 위해 다양한 권한 수준이 필요합니다. 이 장은 일반 사용자와 관리자라는 2단계 시스템을 구현합니다.

**Two User Permission Levels** (두 가지 사용자 등급):

```
【 User Permission Structure 】 (사용자 권한 구조)

┌─────────────────────────────────┐
│ Regular User (일반 사용자)       │
├─────────────────────────────────┤
│ ✅ Write own posts              │
│ ✅ Edit own posts               │
│ ✅ Delete own posts             │
│ ❌ Edit others' posts           │
│ ❌ Delete others' posts         │
│ ❌ Delete spam posts            │
└─────────────────────────────────┘

┌─────────────────────────────────┐
│ Administrator (관리자)           │
├─────────────────────────────────┤
│ ✅ Write own posts              │
│ ✅ Edit own posts               │
│ ✅ Delete own posts             │
│ ✅ Edit others' posts           │
│ ✅ Delete others' posts         │
│ ✅ Delete spam posts            │
│ ✅ System management            │
└─────────────────────────────────┘
```

### 1-2 Table Modification (테이블 수정)

Add the `is_admin` column to the users table to distinguish between regular users and administrators. This boolean field defaults to FALSE for new users.

users 테이블에 `is_admin` 컬럼을 추가하여 일반 사용자와 관리자를 구분합니다. 이 부울 필드는 새 사용자의 경우 기본값이 FALSE입니다.

**Add is_admin column to users table** (users 테이블에 is_admin 컬럼 추가):

```sql
-- Method 1: Modify existing table (방법 1: 기존 테이블 수정)
ALTER TABLE users ADD COLUMN is_admin BOOLEAN DEFAULT FALSE;

-- Method 2: Create new table (방법 2: 새로 생성하는 경우)
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    is_admin BOOLEAN DEFAULT FALSE,  -- Administrator status (관리자 여부)
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**Table Structure** (테이블 구조):

| Column       | Type      | Description                                    |
| ------------ | --------- | ---------------------------------------------- |
| `id`         | INT       | User ID                                        |
| `username`   | VARCHAR   | Username                                       |
| `password`   | VARCHAR   | Password (encrypted)                           |
| `is_admin`   | BOOLEAN   | Admin status (TRUE=admin, FALSE=regular user)  |
| `created_at` | TIMESTAMP | Registration date                              |

**Data Example** (데이터 예시):

```
id=1, username='john', is_admin=TRUE  → Administrator (관리자)
id=2, username='mary', is_admin=FALSE → Regular user (일반 사용자)
id=3, username='robert', is_admin=FALSE → Regular user (일반 사용자)
```

---

## 2️⃣ Update Login System (로그인 시스템 업데이트)

### 2-1 Store Admin Status in Session (세션에 관리자 정보 저장)

When users log in, store their admin status in the session. This allows quick permission checks without repeated database queries.

사용자가 로그인할 때 세션에 관리자 상태를 저장합니다. 이를 통해 반복적인 데이터베이스 쿼리 없이 빠른 권한 확인이 가능합니다.

**Updated login.php** (업데이트된 login.php):

**Filename: `login.php`**

**파일명: `login.php`**

```php
<?php

session_start();

if (isset($_SESSION['user_id'])) {
    header("Location: v4_list.php");
    exit;
}

$error = '';

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $username = $_POST['username'] ?? '';
    $password = $_POST['password'] ?? '';
  
    // For testing: john / password123 (admin) (테스트용: john / password123 - 관리자)
    // For testing: mary / password123 (regular user) (테스트용: mary / password123 - 일반 사용자)
    
    if ($username === 'john' && $password === 'password123') {
        // Administrator login (관리자 로그인)
        $_SESSION['user_id'] = 1;
        $_SESSION['username'] = 'john';
        $_SESSION['is_admin'] = true;  // Store admin status (관리자 상태 저장)
        header("Location: v4_list.php");
        exit;
    } elseif ($username === 'mary' && $password === 'password123') {
        // Regular user login (일반 사용자 로그인)
        $_SESSION['user_id'] = 2;
        $_SESSION['username'] = 'mary';
        $_SESSION['is_admin'] = false;  // Regular user (일반 사용자)
        header("Location: v4_list.php");
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
        <strong>📝 Test Accounts</strong>
        Admin: john / password123<br>
        User: mary / password123
    </div>
</div>

</body>
</html>
```

---

## 3️⃣ User Profile Page (사용자 프로필 페이지)

### 3-1 Profile Page Functionality (프로필 페이지 기능)

The profile page displays comprehensive user information including total posts, comments, and a list of all their contributions. This helps users track their activity.

프로필 페이지는 총 게시글 수, 댓글 수, 모든 기여 목록을 포함한 포괄적인 사용자 정보를 표시합니다. 이를 통해 사용자가 활동을 추적할 수 있습니다.

**Features** (기능):
- Display user information (사용자 정보 표시)
- Show total posts written (작성한 총 게시글 수)
- Show total comments written (작성한 총 댓글 수)
- List all posts by user (사용자의 모든 게시글 목록)

**Filename: `v4_profile.php`**

**파일명: `v4_profile.php`**

```php
<?php

require 'config.php';
requireLogin();

// Get username from URL or use current user (URL에서 username 받기 또는 현재 사용자 사용)
$username = $_GET['username'] ?? $_SESSION['username'];

try {
    // In production: fetch from database (실제로는 데이터베이스에서 조회)
    // For testing: use hardcoded data (테스트용: 하드코딩된 데이터 사용)
    
    // Simulate user data (사용자 데이터 시뮬레이션)
    if ($username === 'john') {
        $user_id = 1;
        $is_admin = true;
    } elseif ($username === 'mary') {
        $user_id = 2;
        $is_admin = false;
    } else {
        header("Location: v4_list.php");
        exit;
    }
  
    // Count user's posts (사용자의 게시글 개수)
    $post_count_sql = "SELECT COUNT(*) as total FROM posts WHERE user_id = ?";
    $post_count_stmt = $pdo->prepare($post_count_sql);
    $post_count_stmt->execute([$user_id]);
    $post_count = $post_count_stmt->fetch(PDO::FETCH_ASSOC)['total'];
  
    // Count user's comments (사용자의 댓글 개수)
    $comment_count_sql = "SELECT COUNT(*) as total FROM comments WHERE user_id = ?";
    $comment_count_stmt = $pdo->prepare($comment_count_sql);
    $comment_count_stmt->execute([$user_id]);
    $comment_count = $comment_count_stmt->fetch(PDO::FETCH_ASSOC)['total'];
  
    // Fetch user's posts (사용자의 게시글 조회)
    $posts_sql = "SELECT id, title, views, created_at FROM posts WHERE user_id = ? ORDER BY created_at DESC";
    $posts_stmt = $pdo->prepare($posts_sql);
    $posts_stmt->execute([$user_id]);
    $posts = $posts_stmt->fetchAll(PDO::FETCH_ASSOC);
  
} catch (Exception $e) {
    $error = "Error: " . $e->getMessage();
}

?>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>User Profile - <?php echo htmlspecialchars($username); ?></title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: 'Segoe UI', sans-serif;
            background: #f5f5f5;
            padding: 20px;
        }
        .container {
            max-width: 900px;
            margin: 0 auto;
            background: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        .profile-header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 30px;
            border-radius: 8px;
            margin-bottom: 30px;
        }
        .profile-header h1 {
            font-size: 32px;
            margin-bottom: 10px;
        }
        .admin-badge {
            background: #ffd700;
            color: #333;
            padding: 5px 15px;
            border-radius: 15px;
            font-size: 12px;
            font-weight: bold;
            display: inline-block;
        }
        .stats {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 20px;
            margin-bottom: 30px;
        }
        .stat-box {
            background: #f9f9f9;
            padding: 20px;
            border-radius: 8px;
            border-left: 4px solid navy;
        }
        .stat-box h3 {
            color: #666;
            font-size: 14px;
            margin-bottom: 10px;
        }
        .stat-box .number {
            font-size: 32px;
            font-weight: bold;
            color: navy;
        }
        .posts-section h2 {
            color: navy;
            margin-bottom: 20px;
            font-size: 24px;
        }
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
        a.post-link {
            color: #2196F3;
            text-decoration: none;
            font-weight: 500;
        }
        a.post-link:hover {
            text-decoration: underline;
        }
        .btn {
            padding: 10px 20px;
            background: #888;
            color: white;
            border: none;
            border-radius: 5px;
            text-decoration: none;
            display: inline-block;
            margin-top: 20px;
        }
        .btn:hover {
            background: #666;
        }
    </style>
</head>
<body>

<div class="container">
    <div class="profile-header">
        <h1>👤 <?php echo htmlspecialchars($username); ?></h1>
        <?php if ($is_admin): ?>
            <span class="admin-badge">⭐ Administrator</span>
        <?php endif; ?>
    </div>
  
    <div class="stats">
        <div class="stat-box">
            <h3>📝 Total Posts</h3>
            <div class="number"><?php echo $post_count; ?></div>
        </div>
        <div class="stat-box">
            <h3>💬 Total Comments</h3>
            <div class="number"><?php echo $comment_count; ?></div>
        </div>
    </div>
  
    <div class="posts-section">
        <h2>📋 Posts by <?php echo htmlspecialchars($username); ?></h2>
      
        <?php if (empty($posts)): ?>
            <p style="color: #999; padding: 20px; text-align: center;">No posts yet.</p>
        <?php else: ?>
            <table>
                <thead>
                    <tr>
                        <th>No.</th>
                        <th>Title</th>
                        <th>Views</th>
                        <th>Date</th>
                    </tr>
                </thead>
                <tbody>
                    <?php foreach ($posts as $post): ?>
                        <tr>
                            <td><?php echo $post['id']; ?></td>
                            <td>
                                <a href="v4_view.php?id=<?php echo $post['id']; ?>" class="post-link">
                                    <?php echo htmlspecialchars($post['title']); ?>
                                </a>
                            </td>
                            <td><?php echo $post['views']; ?></td>
                            <td><?php echo substr($post['created_at'], 0, 10); ?></td>
                        </tr>
                    <?php endforeach; ?>
                </tbody>
            </table>
        <?php endif; ?>
    </div>
  
    <a href="v4_list.php" class="btn">⬅️ Back to Board</a>
</div>

</body>
</html>
```

---

## 4️⃣ Permission Verification (권한 확인)

### 4-1 Permission Check Function (권한 확인 함수)

Create a reusable function to verify if the current user can modify or delete a post. This centralizes permission logic and prevents code duplication.

현재 사용자가 게시글을 수정하거나 삭제할 수 있는지 확인하는 재사용 가능한 함수를 만듭니다. 이것은 권한 로직을 중앙화하고 코드 중복을 방지합니다.

**Add to config.php** (config.php에 추가):

```php
// Permission verification function (권한 확인 함수)
function canModifyPost($post_user_id) {
    // Administrators can modify any post (관리자는 모든 게시글 수정 가능)
    if (isset($_SESSION['is_admin']) && $_SESSION['is_admin'] === true) {
        return true;
    }
  
    // Regular users can only modify their own posts (일반 사용자는 자신의 글만 수정 가능)
    if (isset($_SESSION['user_id']) && $_SESSION['user_id'] == $post_user_id) {
        return true;
    }
  
    // Otherwise, no permission (그 외에는 권한 없음)
    return false;
}
```

### 4-2 Apply Permission Check (권한 확인 적용)

Use the permission function in edit and delete operations to ensure only authorized users can modify posts.

편집 및 삭제 작업에서 권한 함수를 사용하여 승인된 사용자만 게시글을 수정할 수 있도록 합니다.

**Example in v4_edit.php** (v4_edit.php 예시):

```php
<?php

require 'config.php';
requireLogin();

$id = $_GET['id'] ?? null;

if (!$id) {
    header("Location: v4_list.php");
    exit;
}

try {
    // Fetch post (게시글 조회)
    $sql = "SELECT * FROM posts WHERE id = ?";
    $stmt = $pdo->prepare($sql);
    $stmt->execute([$id]);
    $post = $stmt->fetch(PDO::FETCH_ASSOC);
  
    if (!$post) {
        header("Location: v4_list.php");
        exit;
    }
  
    // Check permission (권한 확인)
    if (!canModifyPost($post['user_id'])) {
        die('You do not have permission to edit this post.');
    }
  
    // Process form submission (폼 제출 처리)
    if ($_SERVER['REQUEST_METHOD'] === 'POST') {
        $title = $_POST['title'] ?? '';
        $content = $_POST['content'] ?? '';
      
        // Update post (게시글 수정)
        $update_sql = "UPDATE posts SET title = ?, content = ? WHERE id = ?";
        $update_stmt = $pdo->prepare($update_sql);
        $update_stmt->execute([$title, $content, $id]);
      
        header("Location: v4_view.php?id=$id");
        exit;
    }
  
} catch (Exception $e) {
    die("Error: " . $e->getMessage());
}

?>
```

**Example in v4_delete.php** (v4_delete.php 예시):

```php
<?php

require 'config.php';
requireLogin();

$id = $_GET['id'] ?? null;

if (!$id) {
    header("Location: v4_list.php");
    exit;
}

try {
    // Fetch post to check permission (권한 확인을 위해 게시글 조회)
    $sql = "SELECT * FROM posts WHERE id = ?";
    $stmt = $pdo->prepare($sql);
    $stmt->execute([$id]);
    $post = $stmt->fetch(PDO::FETCH_ASSOC);
  
    if (!$post) {
        header("Location: v4_list.php");
        exit;
    }
  
    // Check permission (권한 확인)
    if (!canModifyPost($post['user_id'])) {
        die('You do not have permission to delete this post.');
    }
  
    // Delete post (CASCADE will delete comments and files)
    // (게시글 삭제 - CASCADE로 댓글과 파일도 삭제됨)
    $delete_sql = "DELETE FROM posts WHERE id = ?";
    $delete_stmt = $pdo->prepare($delete_sql);
    $delete_stmt->execute([$id]);
  
    header("Location: v4_list.php");
    exit;
  
} catch (Exception $e) {
    die("Error: " . $e->getMessage());
}

?>
```

---

## 5️⃣ Key Concepts Review (핵심 개념 정리)

### Permission Logic (권한 로직)

Understanding how permission checks work is crucial for secure application development. The logic follows a simple hierarchy.

권한 확인이 작동하는 방식을 이해하는 것은 안전한 애플리케이션 개발에 중요합니다. 로직은 간단한 계층 구조를 따릅니다.

```
Situation                        Allowed? (허용?)
─────────────────────────────────────────
Own post + Regular user          ✅ Yes (가능)
Others' post + Regular user      ❌ No (불가)
Own post + Administrator         ✅ Yes (가능)
Others' post + Administrator     ✅ Yes (가능)
```

### COUNT and GROUP BY (COUNT와 GROUP BY)

COUNT functions are essential for aggregating data and displaying statistics about user activity.

COUNT 함수는 데이터를 집계하고 사용자 활동에 대한 통계를 표시하는 데 필수적입니다.

**COUNT(*): Number of rows** (행의 개수):

```php
// Count all posts (모든 게시글 수)
SELECT COUNT(*) as total FROM posts;
// Result: total = 100

// Count posts by user 1 (사용자 1번의 게시글 수)
SELECT COUNT(*) as total FROM posts WHERE user_id = 1;
// Result: total = 5
```

### JOIN in Profile Pages (프로필 페이지의 JOIN)

JOIN operations combine user information with their posts for comprehensive profile displays.

JOIN 작업은 포괄적인 프로필 표시를 위해 사용자 정보와 게시글을 결합합니다.

```php
// Fetch user information and posts together (사용자 정보와 게시글을 함께 조회)
SELECT p.id, p.title, u.username
FROM posts p
JOIN users u ON p.user_id = u.id
WHERE u.username = 'john';

// Result (결과):
// id=1, title="First Post", username="john"
// id=2, title="Second Post", username="john"
// ...
```

---

## 6️⃣ Important Notes (주의사항)

**1. Store is_admin during login** (로그인 시 is_admin 저장)

Always store administrator information in the session when users log in. This enables quick permission checks throughout the application.

사용자가 로그인할 때 항상 세션에 관리자 정보를 저장하세요. 이를 통해 애플리케이션 전체에서 빠른 권한 확인이 가능합니다.

**2. Permission check location** (권한 확인 위치)

Always verify permissions before executing edit or delete operations. Never trust client-side data alone.

편집 또는 삭제 작업을 실행하기 전에 항상 권한을 확인하세요. 클라이언트 측 데이터만 신뢰하지 마세요.

**3. Session re-verification** (세션 재확인)

For critical operations, verify permissions directly from the database, not just from session variables.

중요한 작업의 경우 세션 변수만이 아닌 데이터베이스에서 직접 권한을 확인하세요.

**4. COUNT performance** (COUNT의 성능)

For large datasets, consider caching COUNT results to improve performance. Real-time counting can be slow with millions of records.

대량 데이터의 경우 성능 향상을 위해 COUNT 결과를 캐싱하는 것을 고려하세요. 수백만 개의 레코드가 있으면 실시간 계산이 느릴 수 있습니다.

---

## 1️⃣1️⃣ Assignments (과제)

### Assignment 1: Implement User Management Features (Required) (과제 1: 사용자 관리 기능 구현 - 필수)

Implement the complete user management system with profile pages and permission controls.

프로필 페이지와 권한 제어가 있는 완전한 사용자 관리 시스템을 구현하세요.

**Requirements** (요구사항):

1. **Update Database** (데이터베이스 업데이트)
   - Add `is_admin` column to users table (users 테이블에 `is_admin` 컬럼 추가)
   - Create test accounts: john (admin) and mary (regular user)
   - (테스트 계정 생성: john(관리자) 및 mary(일반 사용자))

2. **Update Login System** (로그인 시스템 업데이트)
   - Store `is_admin` in session during login (로그인 시 세션에 `is_admin` 저장)
   - Test both admin and regular user logins (관리자 및 일반 사용자 로그인 모두 테스트)

3. **Create Profile Page** (프로필 페이지 생성)
   - Display user information with admin badge if applicable
   - (해당되는 경우 관리자 배지와 함께 사용자 정보 표시)
   - Show total posts and comments count (총 게시글 및 댓글 수 표시)
   - List all posts by the user (사용자의 모든 게시글 목록)

4. **Implement Permission Checks** (권한 확인 구현)
   - Add `canModifyPost()` function to config.php
   - Apply permission checks in edit and delete operations
   - (편집 및 삭제 작업에 권한 확인 적용)
   - Test that regular users cannot modify others' posts
   - (일반 사용자가 다른 사람의 게시글을 수정할 수 없는지 테스트)
   - Verify that administrators can modify any post
   - (관리자가 모든 게시글을 수정할 수 있는지 확인)

---

### Assignment 2: Test Permission System (Optional) (과제 2: 권한 시스템 테스트 - 선택)

Thoroughly test the permission system to ensure security and proper functionality.

보안과 적절한 기능을 보장하기 위해 권한 시스템을 철저히 테스트하세요.

**Test Scenarios** (테스트 시나리오):

1. **Regular User Tests** (일반 사용자 테스트)
   - Login as mary (일반 사용자로 로그인)
   - Create a post and verify you can edit it (게시글 작성 및 편집 가능 확인)
   - Try to edit john's post → Should show "No permission" error
   - (john의 게시글 편집 시도 → "권한 없음" 오류 표시)
   - Try to delete john's post → Should show "No permission" error
   - (john의 게시글 삭제 시도 → "권한 없음" 오류 표시)

2. **Administrator Tests** (관리자 테스트)
   - Login as john (관리자로 로그인)
   - Create a post and verify you can edit it (게시글 작성 및 편집 가능 확인)
   - Edit mary's post → Should succeed (mary의 게시글 편집 → 성공)
   - Delete mary's post → Should succeed (mary의 게시글 삭제 → 성공)

3. **Profile Page Tests** (프로필 페이지 테스트)
   - View john's profile → Should show admin badge (관리자 배지 표시)
   - View mary's profile → Should NOT show admin badge (배지 없음)
   - Verify post counts are accurate (게시글 수가 정확한지 확인)
   - Verify comment counts are accurate (댓글 수가 정확한지 확인)

---

Thank you for your attention.

Professor Cho Jeonghyun (peterchokr@gmail.com)  
Yeungnam University College
