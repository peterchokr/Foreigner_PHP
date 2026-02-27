# 📝 Chapter 6 Practice Questions: Sessions, Cookies & Login System

---

## 🎯 Multiple Choice (10 Questions)

#### **Question 1: When should session_start() be called?**

① After HTML code output
② After all PHP code output
③ At the beginning of every PHP file
④ Just before using $_SESSION

---

#### **Question 2: What is the biggest difference between sessions and cookies?**

① Sessions are stored on the server, cookies are stored on the client
② Cookies are stored on the server, sessions are stored on the client
③ Both are stored on the server
④ Both are stored on the client

---

#### **Question 3: What is the result of the following code?**

```php
session_start();

$_SESSION['user'] = 'John Smith';

if (isset($_SESSION['user'])) {
    echo "Session exists";
} else {
    echo "Session not found";
}
```

① Session exists
② Session not found
③ Error occurs
④ Nothing is output

---

#### **Question 4: What is the role of session_destroy()?**

① Resets the entire $_SESSION array
② Deletes only specific session data
③ Deletes all session data
④ Restarts the server

---

#### **Question 5: Will the cookie be deleted 30 days later in the following code?**

```php
setcookie('user_theme', 'dark', time() + (86400 * 30));
```

① Yes
② No (deleted after 60 days)
③ No (deleted after 15 days)
④ Cookies are always stored permanently

---

#### **Question 6: What is the relationship between password_hash() and password_verify()?**

① password_hash(): receives password and stores it, password_verify(): compares input and stored values
② password_hash(): decrypts, password_verify(): encrypts
③ Both are password encryption functions
④ Their roles are opposite

---

#### **Question 7: What is the result of the following code?**

```php
$password = '12345678';
$hashed = password_hash($password, PASSWORD_DEFAULT);

if (password_verify('12345678', $hashed)) {
    echo "Match";
} else {
    echo "No match";
}
```

① Match
② No match
③ Error occurs
④ Unpredictable

---

#### **Question 8: What function should be executed after running header("Location: login.php")?**

① return;
② exit;
③ session_start();
④ session_destroy();

---

#### **Question 9: What is the safest way to store passwords in a database?**

① Store in plain text
② Use simple encryption function
③ Hash with password_hash() and store
④ Store after removing only special characters

---

#### **Question 10: What is the code to check login on a page that requires login?**

① `if (isset($_SESSION['user_id'])) { ... }`
② `if (!isset($_SESSION['user_id'])) { header("Location: login.php"); exit; }`
③ `if ($_SESSION['user_id']) { ... }`
④ `if ($SESSION['user_id'] !== null) { ... }`

---

## 💻 Practical Tasks (5 Questions)

### **Task 1: Session Start and Data Storage**

**Requirements:**

- Call session_start()
- Store user information in $_SESSION
  - 'user_id' = 1
  - 'username' = 'John Smith'
  - 'email' = 'john@example.com'
- Verify session data with isset()
- Output in format: "User: John Smith, Email: john@example.com"

**Filename**: `session_basic.php`

```php
<?php
// Write your code here

?>
```

---

### **Task 2: Cookie Setting and Usage**

**Requirements:**

- Set user theme preference with setcookie() (expires after 30 days)
- Read stored value with $_COOKIE
- Delete cookie (set expiration time to the past)
- Display current theme setting

**Filename**: `cookie_example.php`

```php
<?php
// Write your code here

?>
```

---

### **Task 3: password_hash and password_verify**

**Requirements:**

- User password input (POST form)
- Generate hash with password_hash() (simulate signup)
- Compare input password with hash using password_verify()
- Display match/no match result
- Include HTML form

**Filename**: `password_handler.php`

```php
<?php
// Write your code here

?>
```

---

### **Task 4: Basic Login System**

**Requirements:**

- POST form for username and password input
- Validate login with hardcoded user info
  - Username: "admin"
  - Password: "12345678" (stored as hash)
- Use password_verify() to confirm password
- On success: save user_id, username to $_SESSION
- On failure: display error message
- Include logout function

**Filename**: `login_system.php`

```php
<?php
// Write your code here

?>
```

---

### **Task 5: Login Verification and Page Protection**

**Requirements:**

- Call session_start()
- Verify login with isset($_SESSION['user_id'])
- Redirect non-logged-in users to login page
- Display personal information only to logged-in users
  - Username
  - User ID
- Include logout button

**Filename**: `protected_page.php`

```php
<?php
// Write your code here

?>
```

---

---

## ✅ Answers and Explanations

### **Multiple Choice Answers**

| Question | Answer                                                                                      | Explanation                                                               |
| -------- | ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| 1        | **③ At the beginning of every PHP file**                                             | session_start() must be executed before HTML output                       |
| 2        | **① Sessions stored on server, cookies stored on client**                            | Sessions are stored in server memory/files, cookies are stored in browser |
| 3        | **① Session exists**                                                                 | Since $_SESSION['user'] was stored, isset() returns true                  |
| 4        | **③ Deletes all session data**                                                       | session_destroy() removes all $_SESSION information from the server       |
| 5        | **① Yes**                                                                            | time() + (86400 * 30) means 30 days later (86400 = 1 day)                 |
| 6        | **① password_hash(): store password, password_verify(): compare password**           | password_hash creates hash, password_verify validates it                  |
| 7        | **① Match**                                                                          | password_verify('12345678', $hashed) returns true when matched            |
| 8        | **② exit;**                                                                          | Call exit; after header() to prevent further code execution               |
| 9        | **③ Hash with password_hash() and store**                                            | Storing in plain text is a security risk, must be hashed                  |
| 10       | **② `if (!isset($_SESSION['user_id'])) { header("Location: login.php"); exit; }`** | Block non-logged-in users and redirect to login page                      |

---

### **Practical Task Solutions**

#### **Task 1: Session Start and Data Storage - Answer**

```php
<?php
/**
 * session_basic.php - Session start and data storage
 */

// Step 1: Start session (first)
session_start();

// Step 2: Store user information in $_SESSION
$_SESSION['user_id'] = 1;
$_SESSION['username'] = 'John Smith';
$_SESSION['email'] = 'john@example.com';

// Step 3: Verify and output session data
if (isset($_SESSION['user_id']) && 
    isset($_SESSION['username']) && 
    isset($_SESSION['email'])) {
  
    $output = "User: " . $_SESSION['username'] . 
              ", Email: " . $_SESSION['email'];
    echo $output;
} else {
    echo "Session data not found";
}

?>
```

**Verification:**
✓ Call session_start() at the beginning
✓ Store user_id, username, email in $_SESSION
✓ Verify multiple data with isset()
✓ Output in specified format

---

#### **Task 2: Cookie Setting and Usage - Answer**

```php
<?php
/**
 * cookie_example.php - Cookie setting and usage
 */

// Step 1: Set cookie (expires after 30 days)
setcookie('user_theme', 'dark', time() + (86400 * 30));

// Step 2: Read cookie value
$theme = isset($_COOKIE['user_theme']) ? $_COOKIE['user_theme'] : 'default';

?>

<!DOCTYPE html>
<html>
<head>
    <title>Cookie Example</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 30px; }
        .container { max-width: 500px; padding: 20px; border: 1px solid #ddd; }
        button { padding: 10px 20px; background-color: navy; color: white; cursor: pointer; }
    </style>
</head>
<body>

<div class="container">
    <h1>🎨 Theme Settings</h1>
  
    <p>Current theme: <strong><?php echo htmlspecialchars($theme); ?></strong></p>
  
    <form action="cookie_set.php" method="POST" style="display:inline;">
        <button type="submit" name="theme" value="light">Light Theme</button>
        <button type="submit" name="theme" value="dark">Dark Theme</button>
    </form>
  
    <form action="cookie_delete.php" method="POST" style="display:inline;">
        <button type="submit">Delete Cookie</button>
    </form>
</div>

</body>
</html>
```

**Verification:**
✓ Set cookie with setcookie() expiring after 30 days
✓ Verify with isset($_COOKIE['user_theme'])
✓ Display cookie value
✓ Include cookie deletion form (set expiration to past)

---

#### **Task 3: password_hash and password_verify - Answer**

```php
<?php
/**
 * password_handler.php - password_hash and password_verify
 */

$message = '';
$password_input = '';

// Signup simulation (fixed password)
$original_password = '12345678';
$hashed_password = password_hash($original_password, PASSWORD_DEFAULT);

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $password_input = htmlspecialchars($_POST['password']);
  
    // Verify password with password_verify()
    if (password_verify($password_input, $hashed_password)) {
        $message = "✅ Password matches!";
    } else {
        $message = "❌ Password does not match";
    }
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>Password Verification</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 50px; }
        .container { max-width: 400px; padding: 20px; border: 1px solid #ddd; }
        input { width: 100%; padding: 8px; margin: 10px 0; box-sizing: border-box; }
        button { background-color: navy; color: white; padding: 10px; width: 100%; cursor: pointer; }
        .success { color: green; padding: 10px; }
        .error { color: red; padding: 10px; }
    </style>
</head>
<body>

<div class="container">
    <h1>🔐 Password Verification</h1>
  
    <p>Test password: <code>12345678</code></p>
  
    <form method="POST">
        <label>Enter password:</label>
        <input type="password" name="password" value="<?php echo $password_input; ?>" required>
      
        <button type="submit">Verify</button>
    </form>
  
    <?php if ($message): ?>
        <?php if (strpos($message, '✅') !== false): ?>
            <p class="success"><?php echo $message; ?></p>
        <?php else: ?>
            <p class="error"><?php echo $message; ?></p>
        <?php endif; ?>
    <?php endif; ?>
</div>

</body>
</html>
```

**Verification:**
✓ Generate hash with password_hash()
✓ Compare with password_verify()
✓ Receive input via POST form
✓ Display match/no match result

---

#### **Task 4: Basic Login System - Answer**

```php
<?php
/**
 * login_system.php - Basic login system
 */

session_start();

// Hardcoded user info (actually retrieved from database)
$admin_username = 'admin';
$admin_password_hash = password_hash('12345678', PASSWORD_DEFAULT);

$error = '';
$success = false;

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
  
    if (empty($_POST['username']) || empty($_POST['password'])) {
        $error = "Please enter username and password";
    } else {
      
        $username = htmlspecialchars($_POST['username']);
        $password = $_POST['password'];
      
        // Check username
        if ($username === $admin_username) {
          
            // Verify password with password_verify()
            if (password_verify($password, $admin_password_hash)) {
                // Login successful: save session
                $_SESSION['user_id'] = 1;
                $_SESSION['username'] = $username;
                $success = true;
            } else {
                $error = "Password is incorrect";
            }
        } else {
            $error = "Username does not exist";
        }
    }
}

// Logout processing
if (isset($_POST['logout'])) {
    session_destroy();
    header("Location: login_system.php");
    exit;
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>Login System</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 50px; }
        .container { max-width: 400px; padding: 20px; border: 1px solid #ddd; }
        input { width: 100%; padding: 8px; margin: 10px 0; box-sizing: border-box; }
        button { background-color: navy; color: white; padding: 10px; width: 100%; cursor: pointer; }
        .error { color: red; padding: 10px; }
        .success { color: green; padding: 10px; }
    </style>
</head>
<body>

<div class="container">
    <h1>Login</h1>
  
    <?php if ($success): ?>
        <div class="success">✅ Login successful!</div>
        <p>User: <?php echo htmlspecialchars($_SESSION['username']); ?></p>
        <form method="POST">
            <button type="submit" name="logout" value="1">Logout</button>
        </form>
    <?php else: ?>
        <?php if ($error): ?>
            <div class="error">❌ <?php echo $error; ?></div>
        <?php endif; ?>
      
        <form method="POST">
            <label>Username:</label>
            <input type="text" name="username" placeholder="admin" required>
          
            <label>Password:</label>
            <input type="password" name="password" placeholder="12345678" required>
          
            <button type="submit">Login</button>
        </form>
    <?php endif; ?>
</div>

</body>
</html>
```

**Verification:**
✓ Call session_start()
✓ Store hash with password_hash
✓ Verify with password_verify
✓ Save $_SESSION on successful login
✓ Display error message
✓ Logout function (session_destroy)

---

#### **Task 5: Login Verification and Page Protection - Answer**

```php
<?php
/**
 * protected_page.php - Login-required page
 */

session_start();

// Verify login: redirect to login page if not logged in
if (!isset($_SESSION['user_id'])) {
    header("Location: login_system.php");
    exit;  // Important: exit; prevents code below from executing
}

// Logout processing
if (isset($_POST['logout'])) {
    session_destroy();
    header("Location: login_system.php");
    exit;
}

// Content below is visible only to logged-in users

?>

<!DOCTYPE html>
<html>
<head>
    <title>My Page</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 50px; }
        .container { max-width: 600px; padding: 20px; border: 1px solid #ddd; }
        h1 { color: navy; }
        .user-info { 
            padding: 15px; 
            background-color: #f9f9f9; 
            border-left: 4px solid #2196F3; 
            margin: 20px 0;
        }
        button { background-color: navy; color: white; padding: 10px 20px; cursor: pointer; }
    </style>
</head>
<body>

<div class="container">
    <h1>My Page</h1>
  
    <div class="user-info">
        <p><strong>Username:</strong> <?php echo htmlspecialchars($_SESSION['username']); ?></p>
        <p><strong>User ID:</strong> <?php echo $_SESSION['user_id']; ?></p>
    </div>
  
    <p>This personal information is visible only to logged-in users.</p>
  
    <form method="POST">
        <button type="submit" name="logout" value="1">Logout</button>
    </form>
</div>

</body>
</html>
```

**Verification:**
✓ Call session_start()
✓ Verify login with !isset($_SESSION['user_id'])
✓ Redirect non-logged-in users + exit;
✓ Display personal information (username, user_id)
✓ Safe handling with htmlspecialchars()
✓ Include logout button

---

## 💡 Problem-Solving Tips

### **Multiple Choice Strategy**

1. **Session Location**: session_start() always first (before HTML output)
2. **Session vs Cookie**: Storage location difference (server vs client)
3. **session_destroy()**: Delete all session data
4. **Password**: password_hash (store) ↔ password_verify (verify)
5. **Redirect**: exit; required after header()
6. **Login Protection**: Check !isset($_SESSION['user_id'])

### **Practical Task Strategy**

1. **Session**: session_start() → Store in $_SESSION → Verify with isset()
2. **Cookie**: setcookie() → Use $_COOKIE → Delete with past time
3. **Password**: password_hash() store → password_verify() verify
4. **Login**: Check username → password_verify() → Save $_SESSION
5. **Page Protection**: Check !isset() → header() + exit → Display content

---

**Great effort! Keep it up! 💪**

---

Thank you for your attention.   
Cho Jeonghyun ([peterchokr@gmail.com](mailto:peterchokr@gmail.com)). Yeungnam University College

"Produced in collaboration with Claude and Gemini."
