# 📝 Chapter 8 Practice Questions: Data Management & Comprehensive Practice

---

## 🎯 Multiple Choice (10 Questions)

#### **Question 1: What does "Update" mean in CRUD operations?**

① Adding new data  
② Retrieving existing data  
③ Modifying existing data  
④ Deleting data

---

#### **Question 2: What is the main advantage of Prepared Statement?**

① Increases query execution speed  
② Defends against SQL Injection attacks  
③ Reduces database capacity  
④ Auto-login feature

---

#### **Question 3: Which PDO function returns one row?**

① `fetchAll()`  
② `fetch()`  
③ `execute()`  
④ `prepare()`

---

#### **Question 4: What is essential when executing UPDATE and DELETE queries?**

① GROUP BY clause  
② WHERE clause  
③ JOIN clause  
④ ORDER BY clause

---

#### **Question 5: What is the role of FOREIGN KEY?**

① Sets the primary key of data  
② Sets relationship by referencing keys from other tables  
③ Sets auto-increment  
④ Sets default value

---

#### **Question 6: What does PDO::FETCH_ASSOC mean?**

① Return array with numeric index  
② Return associative array with column name as key  
③ Return as object  
④ Return as string

---

#### **Question 7: Why use htmlspecialchars() when outputting data?**

① Reduce database size  
② Convert HTML special characters to entities and enhance security  
③ Improve query speed  
④ Automatic encryption

---

#### **Question 8: To allow only logged-in users to access?**

① `if (isset($_SESSION['user_id']))`  
② `if (!isset($_SESSION['user_id'])) { header("Location: login.php"); exit; }`  
③ `if ($_SESSION['password'] !== null)`  
④ `if (isset($_COOKIE['username']))`

---

#### **Question 9: Example of using Prepared Statement in INSERT query?**

① `$sql = "INSERT INTO todos VALUES ($user_id, $title)"`  
② `$sql = "INSERT INTO todos VALUES (?, ?)"; $stmt->execute([$user_id, $title])`  
③ `INSERT INTO todos SET user_id = $user_id`  
④ `INSERT INTO todos VALUES ("' . $title . '")`

---

#### **Question 10: Which case implements security best?**

① Prepared Statement + htmlspecialchars() + WHERE user_id = ?  
② Input validation only  
③ htmlspecialchars() only  
④ No processing

---

## 💻 Practical Tasks (5 Questions)

### **Task 1: Database Design (Create Tables)**

**Requirements:**
- Create database (CREATE DATABASE)
- Create users table
  - id (PRIMARY KEY, AUTO_INCREMENT)
  - username (VARCHAR, UNIQUE)
  - password (VARCHAR)
  - created_at (TIMESTAMP)
- Create todos table
  - id (PRIMARY KEY, AUTO_INCREMENT)
  - user_id (INT, FOREIGN KEY)
  - title (VARCHAR)
  - status (ENUM: 'incomplete', 'complete')
  - created_at (TIMESTAMP)
- Set FOREIGN KEY relationship

**Filename**: `create_tables.sql`

```sql
-- Write your SQL here
```

---

### **Task 2: INSERT Query to Add Data**

**Requirements:**
- PDO database connection
- Use Prepared Statement (? placeholder)
- Validate input (title required, 200 characters or less)
- Use htmlspecialchars()
- Error handling with try-catch
- Execute INSERT query
- Pass values as array to execute()
- Display success/failure message
- Include HTML form

**Filename**: `add_todo.php`

```php
<?php
// Write your code here

?>
```

---

### **Task 3: SELECT Query to Retrieve Data**

**Requirements:**
- PDO database connection
- SELECT query to retrieve user's TODO list
- Include WHERE user_id = ? condition
- Sort with ORDER BY created_at DESC
- Use prepare() + execute()
- Use fetchAll(PDO::FETCH_ASSOC)
- Error handling with try-catch
- Output data with loop
- Safe handling with htmlspecialchars()
- Display empty list message

**Filename**: `list_todos.php`

```php
<?php
// Write your code here

?>
```

---

### **Task 4: UPDATE Query to Modify Data**

**Requirements:**
- Retrieve existing data (SELECT + WHERE id = ?)
- Get one row with fetch()
- Verify owner (WHERE id = ? AND user_id = ?)
- Process POST request
- Validate input
- Execute UPDATE query
- Include id AND user_id conditions in WHERE clause
- Pass [$title, $status, $id, $user_id] to execute()
- Error handling with try-catch
- Redirect after modification complete

**Filename**: `edit_todo.php`

```php
<?php
// Write your code here

?>
```

---

### **Task 5: DELETE Query to Delete Data**

**Requirements:**
- Write DELETE query
- Include id and user_id conditions in WHERE clause (MUST!)
- Use Prepared Statement
- Use execute([$id, $user_id]) format
- Confirm deletion success
- Error handling with try-catch
- Redirect to list_todos.php after deletion
- Use JavaScript confirm() for deletion confirmation
- Security: user_id verification essential

**Filename**: `delete_todo.php`

```php
<?php
// Write your code here

?>
```

---

---

## ✅ Answers and Explanations

### **Multiple Choice Answers**

| Question | Answer | Explanation |
|----------|--------|-------------|
| 1 | **③ Modifying existing data** | UPDATE is SQL command to modify existing data |
| 2 | **② Defends against SQL Injection attacks** | Prepared Statement safely handles user input |
| 3 | **② `fetch()`** | fetch() returns one row, fetchAll() returns all rows |
| 4 | **② WHERE clause** | Without WHERE clause, UPDATE and DELETE affect all data |
| 5 | **② Sets relationship by referencing keys from other tables** | FOREIGN KEY establishes relationship between tables |
| 6 | **② Return associative array with column name as key** | PDO::FETCH_ASSOC returns array with column names as keys |
| 7 | **② Convert HTML special characters to entities and enhance security** | htmlspecialchars() defends against XSS attacks |
| 8 | **② `if (!isset($_SESSION['user_id'])) { header("Location: login.php"); exit; }`** | Block non-logged-in users and redirect to login page |
| 9 | **② `$sql = "INSERT INTO todos VALUES (?, ?)"; $stmt->execute([$user_id, $title])`** | Prepared Statement uses ? and passes array to execute() |
| 10 | **① Prepared Statement + htmlspecialchars() + WHERE user_id = ?** | Applying all 3 security measures is most secure |

---

### **Practical Task Solutions**

#### **Task 1: Database Design - Answer**

```sql
-- Create database
CREATE DATABASE IF NOT EXISTS todo_app;
USE todo_app;

-- users table
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- todos table
CREATE TABLE todos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    title VARCHAR(200) NOT NULL,
    status ENUM('incomplete', 'complete') DEFAULT 'incomplete',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
```

**Verification:**
✓ CREATE DATABASE IF NOT EXISTS
✓ users table: id, username (UNIQUE), password, created_at
✓ todos table: id, user_id, title, status (ENUM)
✓ Set FOREIGN KEY relationship
✓ ON DELETE CASCADE (delete TODOs when user is deleted)

---

#### **Task 2: INSERT Query to Add Data - Answer**

```php
<?php
/**
 * add_todo.php - Add TODO
 */

require 'config.php';

$error = '';
$success = '';

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    try {
        // Step 1: Sanitize input
        $title = htmlspecialchars(trim($_POST['title'] ?? ''));
        
        // Step 2: Validate input
        if (empty($title)) {
            throw new Exception("Please enter a title");
        }
        if (strlen($title) > 200) {
            throw new Exception("Title must be 200 characters or less");
        }
        
        // Step 3: INSERT query (Prepared Statement)
        $sql = "INSERT INTO todos (user_id, title, status) VALUES (?, ?, 'incomplete')";
        $stmt = $pdo->prepare($sql);
        // Step 4: Pass values as array to execute()
        $stmt->execute([$user_id, $title]);
        
        $success = "✅ TODO has been added!";
        
    } catch (Exception $e) {
        // Step 5: Error handling
        $error = "❌ " . $e->getMessage();
    }
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>Add TODO</title>
    <style>
        body { font-family: Arial, sans-serif; max-width: 600px; margin: 50px auto; padding: 20px; }
        h1 { color: navy; }
        form { background: #f5f5f5; padding: 15px; }
        input { width: 100%; padding: 8px; margin: 8px 0; border: 1px solid #ddd; box-sizing: border-box; }
        button { background: navy; color: white; padding: 8px 15px; border: none; cursor: pointer; width: 100%; }
        .error { color: red; padding: 8px; background: #ffe6e6; }
        .success { color: green; padding: 8px; background: #e6ffe6; }
        a { color: navy; text-decoration: none; }
    </style>
</head>
<body>

<h1>📝 Add New TODO</h1>

<?php if ($error): ?>
    <div class="error"><?php echo htmlspecialchars($error); ?></div>
<?php endif; ?>

<?php if ($success): ?>
    <div class="success"><?php echo htmlspecialchars($success); ?></div>
    <a href="list_todos.php">← Back to list</a>
<?php else: ?>
    <form method="POST">
        <input type="text" name="title" placeholder="Enter what you need to do" autofocus required>
        <button type="submit">Add</button>
    </form>
    <a href="list_todos.php">← Back to list</a>
<?php endif; ?>

</body>
</html>
```

**Verification:**
✓ require 'config.php' (database connection)
✓ htmlspecialchars() sanitize input
✓ trim() remove whitespace
✓ Validate input (required, 200 chars)
✓ try-catch error handling
✓ Prepared Statement (?)
✓ execute([$user_id, $title])
✓ Include HTML form

---

#### **Task 3: SELECT Query to Retrieve Data - Answer**

```php
<?php
/**
 * list_todos.php - Retrieve TODO list
 */

require 'config.php';

$todos = [];

try {
    // SELECT query
    // WHERE user_id = ? : retrieve only current user's TODOs
    // ORDER BY created_at DESC : sort by newest first
    $sql = "SELECT id, title, status, created_at FROM todos 
            WHERE user_id = ? 
            ORDER BY created_at DESC";
    
    // Step 1: Prepare query with prepare()
    $stmt = $pdo->prepare($sql);
    
    // Step 2: Execute query with execute()
    $stmt->execute([$user_id]);
    
    // Step 3: Get all rows with fetchAll(PDO::FETCH_ASSOC)
    // PDO::FETCH_ASSOC = associative array with column names as keys
    $todos = $stmt->fetchAll(PDO::FETCH_ASSOC);
    
} catch (PDOException $e) {
    die("Retrieval failed: " . $e->getMessage());
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>TODO List</title>
    <style>
        body { font-family: Arial, sans-serif; max-width: 800px; margin: 50px auto; padding: 20px; }
        h1 { color: navy; }
        a { display: inline-block; margin: 10px 5px 10px 0; padding: 8px 12px; background: navy; color: white; text-decoration: none; }
        .todo-item { border: 1px solid #ddd; padding: 12px; margin: 10px 0; background: #f9f9f9; display: flex; justify-content: space-between; }
        .todo-title { font-weight: bold; }
        .status-complete { color: green; }
        .empty { color: #999; padding: 20px; }
    </style>
</head>
<body>

<h1>📋 My TODO List</h1>

<a href="add_todo.php">➕ Add New TODO</a>
<a href="logout.php">🚪 Logout</a>

<?php if (empty($todos)): ?>
    <div class="empty">No tasks to do.</div>
<?php else: ?>
    <?php foreach ($todos as $todo): ?>
        <div class="todo-item">
            <div>
                <div class="todo-title"><?php echo htmlspecialchars($todo['title']); ?></div>
                <div class="todo-meta">
                    <?php echo $todo['created_at']; ?>
                    <span class="<?php echo $todo['status'] === 'complete' ? 'status-complete' : ''; ?>">
                        (<?php echo $todo['status'] === 'complete' ? 'Complete' : 'Incomplete'; ?>)
                    </span>
                </div>
            </div>
            <a href="edit_todo.php?id=<?php echo $todo['id']; ?>">Edit</a>
        </div>
    <?php endforeach; ?>
<?php endif; ?>

</body>
</html>
```

**Verification:**
✓ require 'config.php'
✓ SELECT ... WHERE user_id = ? user filter
✓ ORDER BY created_at DESC sorting
✓ prepare() + execute([$user_id])
✓ fetchAll(PDO::FETCH_ASSOC)
✓ try-catch error handling
✓ foreach loop
✓ htmlspecialchars() safe handling
✓ empty() check

---

#### **Task 4: UPDATE Query to Modify Data - Answer**

```php
<?php
/**
 * edit_todo.php - Edit TODO
 */

require 'config.php';

$error = '';
$todo = null;
$id = $_GET['id'] ?? null;

if (!$id) {
    die("Invalid request");
}

// Retrieve existing data
try {
    // Step 1: WHERE id = ? AND user_id = ? (verify owner)
    $sql = "SELECT id, title, status FROM todos WHERE id = ? AND user_id = ?";
    $stmt = $pdo->prepare($sql);
    $stmt->execute([$id, $user_id]);
    
    // Step 2: Get one row with fetch()
    $todo = $stmt->fetch(PDO::FETCH_ASSOC);
    
    if (!$todo) {
        die("TODO not found");
    }
    
} catch (PDOException $e) {
    die("Retrieval failed: " . $e->getMessage());
}

// Process POST request (modify)
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    try {
        // Step 3: Sanitize and validate input
        $title = htmlspecialchars(trim($_POST['title'] ?? ''));
        $status = $_POST['status'] ?? 'incomplete';
        
        if (empty($title)) {
            throw new Exception("Please enter a title");
        }
        
        // Step 4: UPDATE query (WHERE id AND user_id)
        $sql = "UPDATE todos SET title = ?, status = ? WHERE id = ? AND user_id = ?";
        $stmt = $pdo->prepare($sql);
        // Step 5: Pass [$title, $status, $id, $user_id] to execute()
        $stmt->execute([$title, $status, $id, $user_id]);
        
        // Step 6: Redirect after modification complete
        header("Location: list_todos.php");
        exit;
        
    } catch (Exception $e) {
        // Error handling
        $error = $e->getMessage();
    }
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>Edit TODO</title>
    <style>
        body { font-family: Arial, sans-serif; max-width: 600px; margin: 50px auto; padding: 20px; }
        form { background: #f5f5f5; padding: 15px; }
        input, select { width: 100%; padding: 8px; margin: 8px 0; border: 1px solid #ddd; box-sizing: border-box; }
        button { background: navy; color: white; padding: 8px 15px; border: none; cursor: pointer; width: 100%; }
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

<a href="list_todos.php">← Back to list</a>

</body>
</html>
```

**Verification:**
✓ require 'config.php'
✓ SELECT WHERE id = ? AND user_id = ? (verify owner)
✓ fetch() single row retrieval
✓ Process POST request
✓ Validate input
✓ UPDATE ... WHERE id = ? AND user_id = ?
✓ execute([$title, $status, $id, $user_id])
✓ try-catch error handling
✓ Redirect after modification

---

#### **Task 5: DELETE Query to Delete Data - Answer**

```php
<?php
/**
 * delete_todo.php - Delete TODO
 */

require 'config.php';

// Get id passed via GET
$id = $_GET['id'] ?? null;

if (!$id) {
    die("Invalid request");
}

try {
    // Step 1: Confirm TODO exists before deleting
    $sql = "SELECT id FROM todos WHERE id = ? AND user_id = ?";
    $stmt = $pdo->prepare($sql);
    $stmt->execute([$id, $user_id]);
    
    if (!$stmt->fetch()) {
        die("TODO not found");
    }
    
    // Step 2: DELETE query (WHERE clause essential!)
    // WHERE id = ? AND user_id = ? : delete only own TODO
    $sql = "DELETE FROM todos WHERE id = ? AND user_id = ?";
    $stmt = $pdo->prepare($sql);
    
    // Step 3: Execute with [$id, $user_id]
    $stmt->execute([$id, $user_id]);
    
    // Step 4: Redirect after deletion complete
    header("Location: list_todos.php");
    exit;
    
} catch (PDOException $e) {
    die("Deletion failed: " . $e->getMessage());
}

?>
```

**Add delete button to list_todos.php:**

```php
<!-- Add <a> tag in list_todos.php -->
<a href="delete_todo.php?id=<?php echo $todo['id']; ?>" 
   onclick="return confirm('Are you sure you want to delete?');">Delete</a>
```

**Verification:**
✓ require 'config.php'
✓ Get id via GET
✓ SELECT to confirm existence before deleting
✓ WHERE id = ? AND user_id = ? (MUST!)
✓ Use Prepared Statement
✓ execute([$id, $user_id])
✓ try-catch error handling
✓ Redirect after deletion complete
✓ JavaScript confirm() for deletion confirmation

---

## 💡 Problem-Solving Tips

### **Multiple Choice Strategy**

1. **CRUD**: CREATE(INSERT), READ(SELECT), UPDATE, DELETE
2. **Prepared Statement**: Use ? placeholder (defend SQL Injection)
3. **fetch() vs fetchAll()**: Single row vs all rows
4. **WHERE clause**: Essential for UPDATE and DELETE (affects all without it!)
5. **FOREIGN KEY**: Establishes relationship between tables
6. **FETCH_ASSOC**: Array with column names as keys
7. **htmlspecialchars()**: Defend XSS attacks
8. **Login verification**: !isset() + header("Location: login.php")
9. **Prepared Statement**: Use ? + execute([])
10. **Comprehensive security**: Prepared + htmlspecialchars + WHERE user_id

### **Practical Task Strategy**

1. **Table Design**: PRIMARY KEY, FOREIGN KEY, ENUM, TIMESTAMP
2. **INSERT**: prepare() → execute([$user_id, $title])
3. **SELECT**: WHERE user_id = ? → fetchAll() → foreach
4. **UPDATE**: WHERE id AND user_id → execute([$title, $status, $id, $user_id])
5. **DELETE**: WHERE id AND user_id → execute([$id, $user_id])

---

**Great effort! Keep it up! 💪**

---

Professor Cho Jeonghyun (peterchokr@gmail.com)
Yeungnam University College

