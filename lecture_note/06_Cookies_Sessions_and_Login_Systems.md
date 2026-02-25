# Chapter 6. Cookies, Sessions, and Login Systems

---

## 📚 Learning Objectives

After completing this chapter, you will be able to:

✅ Understand the concept of cookies
✅ Understand the concept of sessions and how to use them
✅ Know the differences between sessions and cookies
✅ Implement basic login and logout functionality
✅ Safely store and verify passwords using password hashing

이 장을 학습하면 다음을 할 수 있습니다:

✅ 쿠키의 개념을 이해할 수 있습니다.
✅ 세션의 개념을 이해하고 사용할 수 있습니다.
✅ 세션과 쿠키의 차이를 알 수 있습니다.
✅ 기본 로그인/로그아웃 기능을 구현할 수 있습니다.
✅ 비밀번호를 안전하게 저장하고 확인할 수 있습니다.

---

## 1️⃣ Cookies: Basics 

### 1-1 What is a Cookie?

A cookie is a way to store information on the client (browser). Unlike sessions which store data on the server, cookies store data directly on the user's computer.

쿠키(Cookie)는 클라이언트(브라우저)에 정보를 저장하는 방식입니다. 서버에 데이터를 저장하는 세션과 달리 쿠키는 사용자의 컴퓨터에 직접 데이터를 저장합니다.

### 1-2 setcookie() Function 

**Purpose:** Store a cookie in the browser. (브라우저에 쿠키를 저장합니다.)

**Function Description:**

```
setcookie(name, value, expire, path, domain);

- name: Cookie name (identifier)
  (쿠키 이름 - 구분하기 위한 이름)

- value: Cookie value (data to store)
  (쿠키 값 - 저장할 데이터)

- expire: Cookie expiration time (unixtime)
  (쿠키 만료 시간 - unixtime)
  time() + (86400 * 30) = Delete after 30 days
  (time() + (86400 * 30) = 30일 후 삭제)

- path: Directory where cookie is valid (usually '/')
  (쿠키가 유효한 디렉토리 - 보통 '/')

Note: Must be executed before any HTML output!
(주의: HTML 출력 전에 실행해야 함!)
```

```php
<?php

// Set cookie (before session_start())
// (쿠키 설정 - session_start() 전에)
// Cookie expires after 30 days (30일 후 삭제)
setcookie('user_theme', 'dark', time() + (86400 * 30));
setcookie('language', 'en');

// Use cookie (쿠키 사용)
echo $_COOKIE['user_theme'];

// Delete cookie (set expiration to past time)
// (쿠키 삭제 - 만료 시간을 과거로 설정)
setcookie('user_theme', '', time() - 3600);

?>
```

---

## 2️⃣ Sessions: Concepts and How They Work (세션의 개념과 작동 원리)

### 2-1 What is a Session? 

A session is a way for the server to store user information. When a user logs in, the server stores their information and tracks their activities across multiple page visits.

세션(Session)은 서버에서 사용자 정보를 저장하는 방식입니다. 사용자가 로그인하면 서버가 사용자 정보를 저장하고 여러 페이지 방문에 걸쳐 활동을 추적합니다.

**How sessions work:**
(세션 작동 방식:)

```
1. User logs in → server stores user information
   (사용자가 로그인하면 → 서버가 사용자 정보를 저장)

2. Server assigns a unique session ID
   (서버가 사용자마다 고유한 세션 ID를 할당)

3. User visits pages → server checks session ID
   (사용자가 페이지를 방문할 때마다 → 서버가 세션 ID 확인)

4. Server provides stored information
   (서버가 저장된 정보 제공)

Characteristics: (특징:)

- Stored on server (safe)
  (서버에 저장됨 - 안전)

- Session timeout can be set
  (세션 시간 설정 가능)

- Can be deleted on logout
  (로그아웃 시 삭제 가능)
```

**Sessions vs Cookies:**

(세션 vs 쿠키)

```
┌─────────────┬──────────────────┬──────────────────┐
│ Item        │ Session          │ Cookie           │
│ 항목        │ 세션             │ 쿠키             │
├─────────────┼──────────────────┼──────────────────┤
│ Storage Location (저장 위치)
│             │ Server           │ Client (Browser) │
│             │ 서버             │ 클라이언트(브라우저)
│
│ Security (보안)
│             │ High             │ Low              │
│             │ 높음             │ 낮음             │
│
│ Purpose (용도)
│             │ Sensitive data   │ Remember info    │
│             │ 민감한 정보      │ 기억 정보        │
│
│ Capacity (용량)
│             │ Unlimited        │ ~4KB             │
│             │ 제한 없음        │ 약 4KB           │
│
│ Expiration (만료)
│             │ Configurable     │ Specified time   │
│             │ 설정 가능        │ 지정된 시간      │
└─────────────┴──────────────────┴──────────────────┘
```


### 2-2 session_start() Function

**Purpose:** Start a session. Write this at the top of every PHP file.

목적: 세션을 시작합니다. 모든 PHP 파일의 최상단에 작성합니다.

**Function Description:**

함수 설명:

The session_start() function:
(session_start() 함수는:)

```
- Creates space on the server to store session data
  (서버에서 세션 데이터를 저장할 공간을 만들어줌)

- Generates a unique session ID for each user
  (사용자마다 고유한 세션 ID를 생성)

- Stores this session ID in a cookie on the client
  (이 세션 ID를 쿠키로 클라이언트에 저장)

- Enables the use of the $_SESSION array
  (이후 $_SESSION 배열을 사용할 수 있게 해줌)
```

```php
<?php

// Start session (at the very beginning of every PHP file)
// (세션 시작 - 모든 PHP 파일의 맨 처음)
session_start();

// Now $_SESSION can be used
// (이제 $_SESSION을 사용 가능)
echo "Session has started";

?>
```

**Important Note:** session_start() must be executed before any HTML output.

중요: session_start()는 반드시 HTML 코드 출력 전에 실행해야 합니다.

```php
<?php
session_start();  // ✅ Correct position (올바른 위치)
?>

<!DOCTYPE html>
<!-- HTML code (HTML 코드) -->

<?php
// session_start();  // ❌ Too late (여기는 너무 늦음)
?>
```

### 2-3 Storing Data in $_SESSION ($_SESSION에 데이터 저장)

**Purpose:** Store user information in the session array.

목적: 세션 배열에 사용자 정보를 저장합니다.

**What is an Array?**

배열이란?

```
$_SESSION is an associative array (key-value pairs).
($_SESSION은 연관 배열(Key-Value 쌍)입니다.)

You can store and retrieve values using keys (names).
(Key(이름)을 통해 Value(값)를 저장하고 불러올 수 있습니다.)

Example: (예시:)
$_SESSION['user_id'] = 1;        // Key: 'user_id', Value: 1
$_SESSION['username'] = 'John Smith'; // Key: 'username', Value: 'John Smith'

To retrieve later: (이후 불러올 때:)
echo $_SESSION['user_id'];      // Outputs 1 (1 출력)
echo $_SESSION['username'];     // Outputs John Smith (John Smith 출력)
```

```php
<?php

session_start();

// Store session data
// (세션 데이터 저장)
$_SESSION['user_id'] = 1;
$_SESSION['username'] = 'John Smith';
$_SESSION['email'] = 'john@example.com';

// Use session data
// (세션 데이터 사용)
echo "User: " . $_SESSION['username'];

// Check session data
// (세션 데이터 확인)
if (isset($_SESSION['user_id'])) {
    echo "Logged in";
}

?>
```

### 2-4 End Session (세션 종료)

**Purpose:** Delete session data. Mainly used when logging out.

목적: 세션 데이터를 삭제합니다. 주로 로그아웃할 때 사용합니다.

**Function Description:**

```
unset() - Delete specific session data
(unset() - 특정 세션 데이터 삭제)

  When $_SESSION['user_id'] is deleted,
  (($_SESSION['user_id']를 삭제하면)

  isset($_SESSION['user_id']) returns false on next access
  (다시 접근할 때 isset($_SESSION['user_id'])는 false 반환)

session_destroy() - Delete all session data
(session_destroy() - 모든 세션 데이터 삭제)

  Deletes all session information stored on the server
  (서버에 저장된 모든 세션 정보를 삭제)

  $_SESSION cannot be used after this function
  (이 함수 이후로는 $_SESSION 사용 불가)

  Mainly used when logging out
  (주로 로그아웃 시 사용)
```

```php
<?php

session_start();

// Delete specific session data
// (특정 세션 데이터 삭제)
unset($_SESSION['user_id']);

// Delete all session data
// (모든 세션 데이터 삭제)
session_destroy();

// $_SESSION can no longer be used
// (이제 $_SESSION 사용 불가)

?>
```

### 2-5 When to Use Sessions vs Cookies (세션과 쿠키 사용 시점)

```
Use sessions when: (세션을 사용할 때:)

- Storing login information
  (로그인 정보 저장)

- Storing sensitive user data
  (민감한 사용자 정보 저장)

- Security is important
  (보안이 중요할 때)


Use cookies when: (쿠키를 사용할 때:)

- Storing user preferences (theme, language)
  (사용자 설정 저장 - 테마, 언어)

- Auto-login option
  (자동 로그인 옵션)

- Storing user preferences
  (사용자 선호도 저장)
```

---

## 3️⃣ Login and Logout Systems (로그인/로그아웃 시스템)

### 3-1 User Authentication Process (사용자 인증 프로세스)

**Login Process:**

로그인 과정:

```
1. User enters username and password
   (사용자가 아이디, 비밀번호 입력)

2. Server searches database for username
   (서버가 데이터베이스에서 아이디 검색)

3. If found, verify password with password_verify
   (아이디 있으면 비밀번호 확인)

4. If password matches, save to session
   (비밀번호 일치하면 세션 저장)

5. Login complete
   (로그인 완료)
```

### 3-2 Password Verification (password_verify) (비밀번호 검증)

**Purpose:** Verify that the entered password matches the stored hash.

목적: 입력한 비밀번호가 저장된 해시와 일치하는지 확인합니다.

**Function Description:**

```
password_verify($password, $hash);

- $password: Plaintext password entered by user
  (사용자가 입력한 평문 비밀번호)

- $hash: Hashed password stored in database
  (데이터베이스에 저장된 해시된 비밀번호)

Return value: (반환값:)

- true: Password matches
  (비밀번호가 일치함)

- false: Password does not match
  (비밀번호가 일치하지 않음)

How it works: (내부 작동:)

1. Convert entered password using hash function
   (입력한 비밀번호를 해시 함수로 변환)

2. Compare converted hash with stored hash
   (변환된 해시와 저장된 해시를 비교)

3. Return true if same, false if different
   (같으면 true, 다르면 false 반환)

Important: (중요:)

- Stored hash cannot be viewed, so it's safe
  (저장된 해시를 볼 수 없으므로 안전함)

- Never store plaintext passwords!
  (원본 비밀번호를 절대 평문으로 저장하지 말 것!)
```

```php
<?php

// Password hash stored in database
// (데이터베이스에 저장된 비밀번호 - 해시)
$hashed_password = '$2y$10$eImiTXuWVxfaHNYY0iNAeuK2kRabMmwqbF/LewY5D...';

// Password entered by user
// (사용자가 입력한 비밀번호)
$user_input = 'password123';

// Verify with password_verify()
// (password_verify()로 확인)
if (password_verify($user_input, $hashed_password)) {
    echo "Password matches! Login successful";
} else {
    echo "Password mismatch";
}

?>
```

### 3-3 Basic Login Logic (기본 로그인 로직)

```php
<?php

session_start();

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
  
    // Step 1: Check input values
    // (단계 1: 입력값 확인)
    if (empty($_POST['username']) || empty($_POST['password'])) {
        $error = "Please enter username and password";
    } else {
  
        // Step 2: Search database for user
        // (단계 2: 데이터베이스에서 사용자 검색)
        $username = htmlspecialchars($_POST['username']);
  
        // (After database connection)
        // Use Prepared Statement to prevent SQL Injection
        // (준비된 문장으로 SQL Injection 방어)
        $sql = "SELECT id, password FROM users WHERE username = ?";
        $stmt = $pdo->prepare($sql);
        $stmt->execute([$username]);
        $user = $stmt->fetch(PDO::FETCH_ASSOC);
        // $user = ['id' => 1, 'password' => 'hashed_value...']
  
        // Step 3: Verify password
        // (단계 3: 비밀번호 확인)
        // password_verify(): Check if entered password matches stored hash
        // (입력한 비밀번호가 저장된 해시와 일치하는지 확인)
        // Return value: true(match) or false(no match)
        // (반환값: true(일치) 또는 false(불일치))
        if ($user && password_verify($_POST['password'], $user['password'])) {
            // ✅ Password matches: Login successful
            // (비밀번호 일치: 로그인 성공)
            $_SESSION['user_id'] = $user['id'];
            $_SESSION['username'] = $username;
            header("Location: mypage.php");
            exit;
        } else {
            // ❌ Password mismatch or user not found
            // (비밀번호 불일치 또는 사용자 없음)
            $error = "Username or password is incorrect";
        }
    }
}

?>
```

### 3-4 Logout Function (로그아웃 기능)

```php
<?php

session_start();

// session_destroy(): Delete all session data
// (모든 세션 데이터 삭제)

// Removes all $_SESSION information stored on server
// (서버에 저장된 모든 $_SESSION 정보 제거)

// Invalidates the session ID connected to user
// (사용자와 연결된 세션 ID도 무효화됨)

// Security: If hacker steals session, it can no longer be used
// (보안: 해커가 세션을 탈취해도 더 이상 사용 불가)
session_destroy();

// Redirect to login page
// (로그인 페이지로 리다이렉트)

// User returns to login screen
// (사용자가 로그인 화면으로 돌아감)

// Must enter username and password again
// (다시 아이디, 비밀번호를 입력해야 함)
header("Location: login.php");
exit;

?>
```

---

## 4️⃣ Security Considerations (보안 고려사항)

### 4-1 Password Hashing (비밀번호 해싱)

**Why is it necessary?**(왜 필요한가?)

```
❌ Dangerous method: Store password as plaintext
   (위험한 방법: 비밀번호를 그대로 저장)

   If database is hacked, all passwords are exposed
   (데이터베이스가 해킹되면 모든 비밀번호 노출됨)

   User information completely compromised
   (사용자 정보가 완전히 탈취됨)

✅ Safe method: Store password as hash
   (안전한 방법: 비밀번호를 해시로 저장)

   If database is hacked, original password cannot be discovered
   (데이터베이스가 해킹되어도 원본 비밀번호 알 수 없음)

   Can only verify if password matches
   (비밀번호 일치 여부만 확인 가능)
```

**password_hash() Function Description:**

```
password_hash($password, $algo);

- $password: Plaintext password entered by user
  (사용자가 입력한 평문 비밀번호)

- $algo: Hashing algorithm (PASSWORD_DEFAULT recommended)
  (해싱 알고리즘 - PASSWORD_DEFAULT 권장)

Return value: (반환값:)

- Encrypted hash string
  (암호화된 해시 문자열)

Example: (예시:)

Input: "password123"
(입력: "password123")

Output: "$2y$10$eImiTXuWVxfaHNYY0iNAeuK2kRabMmwqbF/LewY5D..."
(출력: "$2y$10$eImiTXuWVxfaHNYY0iNAeuK2kRabMmwqbF/LewY5D...")

Different hash generated each time - for security
(매번 다른 해시 생성됨 - 보안 때문)
```

```php
<?php

// Password entered by user
// (사용자가 입력한 비밀번호)
$password = 'password123';

// Generate hash (to store in database)
// (해시 생성 - 데이터베이스에 저장)
$hashed = password_hash($password, PASSWORD_DEFAULT);
echo $hashed;

// Save: INSERT INTO users (password) VALUES ('$hashed')

?>
```

### 4-2 Login Verification (로그인 확인)

**Check session before page access:**

페이지 접근 전 세션 확인:

```php
<?php

session_start();

// isset($_SESSION['user_id']): Does session exist?
// (세션이 존재하는가?)

// - true: Logged-in user
//   (로그인된 사용자)

// - false: Not logged in
//   (로그인하지 않은 사용자)

// If not logged in, redirect to login page
// (로그인하지 않으면 로그인 페이지로 리다이렉트)
if (!isset($_SESSION['user_id'])) {
    header("Location: login.php");
    exit;
    // exit; prevents code below from executing
    // (exit;를 통해 이 아래 코드 실행 방지)
}

// Code below is executed only by logged-in users
// (여기 아래는 로그인한 사용자만 실행됨)
echo "Welcome, " . $_SESSION['username'];

?>
```

---

## 5️⃣ Practice Example (실습 예제)

### 5-1 Preparation: Create Database and Users Table (준비: 데이터베이스 및 users 테이블 생성)

Before running the practice example, create a users table and add sample data.

실습 예제를 실행하기 전에 먼저 users 테이블을 생성하고 샘플 데이터를 넣어야 합니다.

**Create Table:**

Execute the following SQL in MySQL Workbench:

```sql
CREATE TABLE IF NOT EXISTS users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    email VARCHAR(100) NOT NULL,
    name VARCHAR(50) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    last_login TIMESTAMP NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

**Generate Test Data in PHP:**

If you want to create the table with the SQL above and hash passwords with PHP:

테이블은 위 SQL로 만들고, 비밀번호만 PHP로 해싱하고 싶다면:

**File: `setup_users.php`** (Run once only)

```php
<?php

// Database connection
// (데이터베이스 연결)
$host = 'localhost';
$dbname = 'test_db';
$user = 'root';
$password = '';

try {
    // Create PDO connection
    // (PDO 연결 생성)
    $pdo = new PDO(
        "mysql:host=$host;dbname=$dbname",
        $user,
        $password
    );
} catch (PDOException $e) {
    die("Connection failed: " . $e->getMessage());
}

// Sample user data
// (샘플 사용자 데이터)
$users = array(
    array('username' => 'john', 'password' => 'password123', 'email' => 'john@example.com', 'name' => 'John Smith'),
    array('username' => 'mary', 'password' => 'mary12345', 'email' => 'mary@example.com', 'name' => 'Mary Johnson'),
    array('username' => 'sarah', 'password' => 'sarah@123456', 'email' => 'sarah@example.com', 'name' => 'Sarah Davis'),
);

try {
    // Insert each user
    // (각 사용자 삽입)
    foreach ($users as $user) {
        // Hash password using password_hash()
        // (password_hash()로 비밀번호 해싱)
        $hashed_password = password_hash($user['password'], PASSWORD_BCRYPT);
  
        // Prepare SQL statement with placeholders
        // (준비된 문장 사용)
        $sql = "INSERT INTO users (username, password, email, name) 
                VALUES (?, ?, ?, ?)";
        $stmt = $pdo->prepare($sql);
  
        // Execute with actual values
        // (실제 값으로 실행)
        $stmt->execute(array(
            $user['username'],
            $hashed_password,
            $user['email'],
            $user['name']
        ));
  
        echo "✅ User " . $user['username'] . " created successfully<br>";
    }
  
    echo "<br>All users have been created!";
  
} catch (PDOException $e) {
    echo "❌ Error: " . $e->getMessage();
}

?>
```

**Key Points:**

주요 포인트:

✅ **Table Structure:**

- `id`: Unique user ID (PRIMARY KEY, AUTO_INCREMENT)
  (사용자 고유 ID)
- `username`: Login ID (UNIQUE: no duplicates)
  (로그인 아이디 - UNIQUE: 중복 불가)
- `password`: Hashed password (255 chars: hash length)
  (해싱된 비밀번호)
- `email`: Email address (이메일 주소)
- `name`: User name (사용자 이름)
- `created_at`: Registration date/time (auto-generated)
  (가입 일시 - 자동 생성)
- `last_login`: Last login time
  (마지막 로그인 시간)

✅ **Security:**

- `password_hash()`: Hash password safely
  (비밀번호 안전하게 해싱)
- `PASSWORD_BCRYPT`: Bcrypt algorithm (recommended)
  (bcrypt 알고리즘 - 권장)
- `password_verify()`: Compare input password with hash
  (입력한 비밀번호와 해시 비교)

✅ **Login Test:**

```
Username: john
Password: password123

Username: mary
Password: mary12345

Username: sarah
Password: sarah@123456
```

### 5-2 Practice Example: Basic Login System (실습 예제: 기본 로그인 시스템)

**File 1: login.php (Login Form)**

```php
<?php

session_start();

// If already logged in, go to mypage
// (이미 로그인했으면 마이페이지로 이동)
if (isset($_SESSION['user_id'])) {
    header("Location: mypage.php");
    exit;
}

// Database connection
// (데이터베이스 연결)
$host = 'localhost';
$dbname = 'test_db';
$user = 'root';
$password = '';

try {
    // Create PDO connection
    // (PDO 연결 생성)
    $pdo = new PDO(
        "mysql:host=$host;dbname=$dbname",
        $user,
        $password
    );
} catch (PDOException $e) {
    die("Connection failed: " . $e->getMessage());
}

$error = '';

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
  
    // Check if username and password are provided
    // (사용자명과 비밀번호 확인)
    if (empty($_POST['username']) || empty($_POST['password'])) {
        $error = "Please enter username and password";
    } else {
  
        $username = htmlspecialchars($_POST['username']);
  
        // Search for user in database
        // (데이터베이스에서 사용자 검색)
        $sql = "SELECT id, password FROM users WHERE username = ?";
        $stmt = $pdo->prepare($sql);
        $stmt->execute([$username]);
        $user_data = $stmt->fetch(PDO::FETCH_ASSOC);
  
        // password_verify(): Compare entered password with hash
        // (입력한 비밀번호와 해시 비교)
        // $user_data: User information found in database
        // (데이터베이스에서 찾은 사용자 정보)
        // password_verify(input, stored_hash): Returns true or false
        // (true 또는 false 반환)
        if ($user_data && password_verify($_POST['password'], $user_data['password'])) {
            // Authentication successful: Save to session
            // (인증 성공: 세션에 저장)
            $_SESSION['user_id'] = $user_data['id'];
            $_SESSION['username'] = $username;
            header("Location: mypage.php");
            exit;
        } else {
            // Authentication failed: Display error
            // (인증 실패: 오류 메시지 표시)
            // Username doesn't exist or password is wrong
            // (아이디가 없거나 비밀번호가 틀렸음)
            $error = "Username or password is incorrect";
        }
    }
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>Login</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 400px;
            margin: 50px auto;
            padding: 20px;
        }
  
        .container {
            background: white;
            padding: 30px;
            border: 1px solid #ddd;
        }
  
        h1 {
            color: navy;
            text-align: center;
        }
  
        .form-group {
            margin: 20px 0;
        }
  
        label {
            display: block;
            margin-bottom: 5px;
        }
  
        input {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            box-sizing: border-box;
        }
  
        button {
            width: 100%;
            padding: 10px;
            background-color: navy;
            color: white;
            border: none;
            cursor: pointer;
            margin-top: 10px;
        }
  
        .error {
            color: red;
            padding: 10px;
            margin: 10px 0;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>Login</h1>
  
    <?php if ($error): ?>
    <div class="error"><?php echo $error; ?></div>
    <?php endif; ?>
  
    <form method="POST">
        <div class="form-group">
            <label>Username:</label>
            <input type="text" name="username" placeholder="Enter username" required>
        </div>
  
        <div class="form-group">
            <label>Password:</label>
            <input type="password" name="password" placeholder="Enter password" required>
        </div>
  
        <button type="submit">Login</button>
    </form>
</div>

</body>
</html>
```

**File 2: mypage.php (My Page)**

```php
<?php

session_start();

// Login verification: Does $_SESSION['user_id'] exist?
// (로그인 확인: $_SESSION['user_id']가 존재하는가?)

// - isset(): Check if variable exists and is not NULL
//   (변수가 존재하고 NULL이 아닌가?)

// - !isset(): Variable does not exist or is NULL
//   (변수가 존재하지 않거나 NULL인가?)
if (!isset($_SESSION['user_id'])) {
    // Not logged in: Force redirect to login page
    // (로그인하지 않은 사용자: 로그인 페이지로 강제 이동)
    header("Location: login.php");
    exit;  // Prevent code below from executing
         // (이 아래 코드 실행 방지)
}

// Content below is only visible to logged-in users
// (여기부터는 로그인한 사용자만 볼 수 있는 콘텐츠)

?>

<!DOCTYPE html>
<html>
<head>
    <title>My Page</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 600px;
            margin: 50px auto;
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
  
        .user-info {
            padding: 15px;
            background-color: #f9f9f9;
            border-left: 4px solid #2196F3;
            margin: 20px 0;
        }
  
        button {
            background-color: navy;
            color: white;
            padding: 10px 20px;
            border: none;
            cursor: pointer;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>My Page</h1>
  
    <div class="user-info">
        <p><strong>User:</strong> <?php echo htmlspecialchars($_SESSION['username']); ?></p>
        <p><strong>User ID:</strong> <?php echo $_SESSION['user_id']; ?></p>
    </div>
  
    <form action="logout.php" style="display: inline;">
        <button type="submit">Logout</button>
    </form>
</div>

</body>
</html>
```

**File 3: logout.php (Logout)**

```php
<?php

session_start();

// session_destroy(): Delete all session data
// (모든 세션 데이터 삭제)

// Removes all $_SESSION information stored on server
// (서버에 저장된 모든 $_SESSION 정보 제거)

// Invalidates session ID connected to user
// (사용자와 연결된 세션 ID도 무효화됨)

// Security: Even if hacker steals session, it can no longer be used
// (보안: 만약 해커가 세션을 탈취했어도 더 이상 사용 불가)
session_destroy();

// Redirect to login page
// (로그인 페이지로 리다이렉트)

// Force user to login screen
// (로그인 페이지로 강제 이동)

// User must enter username and password again
// (사용자는 다시 아이디와 비밀번호를 입력해야 함)
header("Location: login.php");

// exit; is required!
// (exit; 필수!)

// Prevents code below from executing after header()
// (header()로 페이지 이동 후 이 아래 코드 실행 방지)

// Without exit; the current page will continue to execute
// (만약 exit;이 없으면 현재 페이지 실행 계속됨)
exit;

?>
```

---

## 6️⃣ Assignments (과제)

### Assignment 1: Understanding Sessions and Cookies (과제 1: 세션과 쿠키 차이 이해)

Choose whether to use sessions or cookies in the following scenarios:

다음 상황에서 세션과 쿠키 중 어떤 것을 사용할지 선택하세요:

1. Store login information
   (로그인 정보 저장)
2. Store user theme preference
   (사용자의 테마 설정 저장)
3. Store shopping cart information
   (장바구니 정보 저장)
4. Remember me (auto-login) option
   (자동 로그인 옵션)
5. Store language preference
   (사용 언어 설정 저장)

---

### Assignment 2: Extend Login System (과제 2: 로그인 시스템 확장)

Extend the basic login system you learned in class as follows:

강의에서 학습한 기본 로그인 시스템을 다음과 같이 확장하세요:

1. **Step 1: Basic Login/Logout**
   (기본 로그인/로그아웃)

   - Implement login.php, mypage.php, logout.php (as learned in class)
     (강의에서 학습)
2. **Step 2: Add Registration**
   (회원가입 추가)

   - Create signup.php file (signup.php 파일 생성)
   - Enter username and password (아이디, 비밀번호 입력)
   - Store password using password_hash() (비밀번호는 password_hash()로 저장)
3. **Step 3: Improve My Page**
   (마이페이지 개선)

   - Display logged-in user information (로그인된 사용자 정보 표시)
   - Add logout button (로그아웃 버튼 추가)

---

## 7️⃣ Important Points (중요 포인트)

### Always Remember (항상 기억하기)

✅ **Session Management:**(세션 관리:)

- Start session at the beginning of every PHP file
  (모든 PHP 파일의 맨 처음에 세션 시작)
- Always verify session before accessing protected pages
  (보호된 페이지 접근 전 반드시 세션 확인)
- Use session_destroy() when logging out
  (로그아웃할 때 session_destroy() 사용)

✅ **Password Security:**(비밀번호 보안:)

- Always hash passwords before storing
  (저장하기 전에 항상 비밀번호를 해싱)
- Use password_verify() to check passwords
  (비밀번호 확인할 때 password_verify() 사용)
- Never store plaintext passwords
  (절대 평문 비밀번호를 저장하지 말 것)

✅ **Secure Coding:**(안전한 코딩:)

- Use htmlspecialchars() for user input
  (사용자 입력에 htmlspecialchars() 사용)
- Use Prepared Statements for database queries
  (데이터베이스 쿼리에 준비된 문장 사용)
- Always use exit; after header() redirect
  (header() 리다이렉트 후 반드시 exit; 사용)

---

Thank you for your attention.
Cho Jeonghyun ([peterchokr@gmail.com](mailto:peterchokr@gmail.com)). Yeungnam University College

"Produced in collaboration with Claude and Gemini."
