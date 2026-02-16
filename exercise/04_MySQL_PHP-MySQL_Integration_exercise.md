# 📝 Chapter 4 Practice Questions: MySQL Review & PHP-MySQL Integration

---

## 🎯 Multiple Choice (10 Questions)

#### **Question 1: What is the command to create a new table in MySQL?**

① INSERT  
② CREATE TABLE  
③ UPDATE  
④ SELECT

---

#### **Question 2: What is the result of the following SQL query?**

```sql
SELECT name, score FROM students 
WHERE score >= 90 
ORDER BY score DESC;
```

① Output students with 90 or higher scores in descending order  
② Output all students in name order  
③ Output only students with exactly 90 points  
④ Output students with less than 90 points

---

#### **Question 3: What is the role of ? in the following code?**

```php
$sql = "SELECT * FROM students WHERE score > ?";
$stmt = $pdo->prepare($sql);
$stmt->execute([85]);
```

① Wildcard character  
② Placeholder for Prepared Statement  
③ Optional parameter  
④ Error indicator symbol

---

#### **Question 4: What is SQL Injection?**

① Normal process of injecting data into the database  
② Attacking a database by entering malicious SQL code  
③ Database backup method  
④ Method of connecting to MySQL from PHP

---

#### **Question 5: What is the greatest advantage of Prepared Statement?**

① Faster execution speed  
② Safe from SQL Injection attacks  
③ Simpler syntax  
④ Automatic data type conversion

---

#### **Question 6: What should be filled in ? in the PDO connection code?**

```php
try {
    $pdo = new PDO(
        "mysql:host=localhost;dbname=test_db",
        "root",
        "password"
    );
} catch (?) {
    echo "Connection failed";
}
```

① SQLException  
② PDOException  
③ MySQLException  
④ DatabaseException

---

#### **Question 7: What is the difference between fetch() and fetchAll()?**

① fetch() retrieves one row, fetchAll() retrieves all rows  
② fetch() retrieves multiple rows, fetchAll() retrieves one row  
③ fetch() returns an array, fetchAll() returns an object  
④ There is no functional difference

---

#### **Question 8: What is the result of the following code?**

```php
$sql = "UPDATE students SET score = 95 WHERE id = 1";
$stmt = $pdo->prepare($sql);
$stmt->execute([]);
```

① Updates student with id 1's score to 95  
② Updates all students' scores to 95  
③ Deletes student with id 1  
④ Error occurs

---

#### **Question 9: What is the role of htmlspecialchars()?**

① Prevents HTML tags from being executed  
② Encrypts strings  
③ Removes special characters  
④ Database connection

---

#### **Question 10: What is the execution order of the following code?**

```php
$sql = "SELECT * FROM students WHERE age > ?";
$stmt = $pdo->prepare($sql);
$stmt->execute([20]);
$result = $stmt->fetch(PDO::FETCH_ASSOC);
```

① prepare → execute → fetch  
② execute → prepare → fetch  
③ prepare → fetch → execute  
④ fetch → prepare → execute

---

## 💻 Practical Tasks (5 Questions)

### **Task 1: SQL Query Writing (SELECT)**

**Requirements:**
- Query students with a score of 80 or higher from the student table
- Retrieve only name and score
- Sort in descending order (highest score first)
- Write the SQL query

**Filename**: `select_students.sql`

```sql
-- Write your SQL here

```

---

### **Task 2: PDO Database Connection**

**Requirements:**
- Connect to MySQL database using PDO
- Error handling with try-catch
- Output success/failure message

**Filename**: `db_connect.php`

```php
<?php
// Write your code here

?>
```

---

### **Task 3: Safe Search with Prepared Statement**

**Requirements:**
- Search for students with a score equal to or higher than user input
- Use Prepared Statement (? placeholder)
- Output search results with foreach
- Format: "John Smith: 85 points", "Sarah Johnson: 92 points"

**Filename**: `search_by_score.php`

```php
<?php
// Write your code here

?>
```

---

### **Task 4: Complex Search with Multiple Conditions**

**Requirements:**
- Search with two conditions: age and score
- Use Prepared Statement with format: WHERE age > ? AND score >= ?
- Conditions: age 20 or older, score 80 or higher
- Output search results in HTML table format

**Filename**: `complex_search.php`

```php
<?php
// Write your code here

?>
```

---

### **Task 5: Display Query Results and Error Handling**

**Requirements:**
- Retrieve all student information
- Handle database connection errors with try-catch
- Use htmlspecialchars() for safe output
- Display message when no results are found
- Distinguish between using fetch() and fetchAll()

**Filename**: `display_students.php`

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
| 1 | **② CREATE TABLE** | CREATE TABLE is the command to create a new table |
| 2 | **① Output students with 90 or higher scores in descending order** | WHERE score >= 90 filters scores 90 or higher, and ORDER BY score DESC sorts in descending order |
| 3 | **② Placeholder for Prepared Statement** | ? is a placeholder in Prepared Statement to safely substitute data |
| 4 | **② Attacking a database by entering malicious SQL code** | SQL Injection is an attack that exploits user input to manipulate SQL queries |
| 5 | **② Safe from SQL Injection attacks** | Prepared Statement separates queries from data to defend against SQL Injection |
| 6 | **② PDOException** | PDO connection errors are handled with PDOException |
| 7 | **① fetch() retrieves one row, fetchAll() retrieves all rows** | fetch() returns the first row of results, while fetchAll() returns all rows as an array |
| 8 | **① Updates student with id 1's score to 95** | UPDATE statement modifies data. WHERE id = 1 specifies the row with id 1 |
| 9 | **① Prevents HTML tags from being executed** | htmlspecialchars() converts <, >, &, " to HTML entities to prevent HTML tag injection |
| 10 | **① prepare → execute → fetch** | The correct order is to prepare the query first, execute it with data, then fetch results |

---

### **Practical Task Solutions**

#### **Task 1: SQL Query Writing (SELECT) - Answer**

```sql
SELECT name, score FROM students 
WHERE score >= 80 
ORDER BY score DESC;
```

**Query Verification:**
- SELECT name, score: Retrieves only name and score ✓
- WHERE score >= 80: Filters for 80 points or higher ✓
- ORDER BY score DESC: Sorts in descending order ✓

---

#### **Task 2: PDO Database Connection - Answer**

```php
<?php
/**
 * db_connect.php - PDO Database Connection
 */

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
    echo "✅ Database connection successful!";
} catch (PDOException $e) {
    echo "❌ Connection failed: " . $e->getMessage();
}
?>
```

**Verification:**
✓ PDO object creation
✓ Connection information (host, dbname, user, password)
✓ Exception handling with try-catch
✓ Success/failure message display

---

#### **Task 3: Safe Search with Prepared Statement - Answer**

```php
<?php
/**
 * search_by_score.php - Search students by score
 */

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
    die("Connection failed: " . $e->getMessage());
}

// Minimum score to search for
$min_score = isset($_GET['score']) ? intval($_GET['score']) : 80;

// Use Prepared Statement
$sql = "SELECT name, score FROM students WHERE score >= ? ORDER BY score DESC";
$stmt = $pdo->prepare($sql);
$stmt->execute([$min_score]);
$students = $stmt->fetchAll(PDO::FETCH_ASSOC);

// Output results
echo "<h3>Students with score " . $min_score . " or higher</h3>";
foreach ($students as $student) {
    echo htmlspecialchars($student['name']) . ": " . $student['score'] . " points<br>";
}
?>
```

**Verification:**
✓ Prepared Statement (? placeholder)
✓ Safe data substitution with $min_score variable
✓ Retrieve all results with fetchAll()
✓ Output results with foreach

---

#### **Task 4: Complex Search with Multiple Conditions - Answer**

```php
<?php
/**
 * complex_search.php - Complex search by age and score
 */

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
    die("Connection failed: " . $e->getMessage());
}

// Search conditions
$min_age = 20;
$min_score = 80;

// Prepared Statement with multiple conditions
$sql = "SELECT * FROM students 
        WHERE age > ? AND score >= ? 
        ORDER BY score DESC";

$stmt = $pdo->prepare($sql);
$stmt->execute([$min_age, $min_score]);
$students = $stmt->fetchAll(PDO::FETCH_ASSOC);

?>

<!DOCTYPE html>
<html>
<head>
    <title>Complex Search</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        table { border-collapse: collapse; width: 100%; margin-top: 20px; }
        th, td { border: 1px solid #ddd; padding: 10px; text-align: left; }
        th { background-color: navy; color: white; }
    </style>
</head>
<body>

<h1>Students 20 years old or older with score 80 or higher</h1>

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Age</th>
            <th>Score</th>
        </tr>
    </thead>
    <tbody>
        <?php foreach ($students as $student): ?>
        <tr>
            <td><?php echo htmlspecialchars($student['name']); ?></td>
            <td><?php echo $student['age']; ?></td>
            <td><?php echo $student['score']; ?></td>
        </tr>
        <?php endforeach; ?>
    </tbody>
</table>

</body>
</html>
```

**Verification:**
✓ Two placeholders (?, ?)
✓ execute([value1, value2]) format
✓ Multiple conditions combined with AND
✓ Results displayed in HTML table

---

#### **Task 5: Display Query Results and Error Handling - Answer**

```php
<?php
/**
 * display_students.php - Retrieve and display all students with error handling
 */

$host = 'localhost';
$dbname = 'test_db';
$user = 'root';
$password = '';

$pdo = null;

try {
    // Connect to database
    $pdo = new PDO(
        "mysql:host=$host;dbname=$dbname",
        $user,
        $password
    );
    
    // Retrieve all students
    $sql = "SELECT id, name, email, age, score FROM students ORDER BY score DESC";
    $stmt = $pdo->query($sql);
    $students = $stmt->fetchAll(PDO::FETCH_ASSOC);
    
} catch (PDOException $e) {
    // Handle connection error
    echo "❌ Database error: " . htmlspecialchars($e->getMessage());
    $students = array();
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>Student Information</title>
    <style>
        body { font-family: Arial, sans-serif; max-width: 900px; margin: 30px auto; }
        h1 { color: navy; }
        table { width: 100%; border-collapse: collapse; margin-top: 20px; }
        th { background-color: navy; color: white; padding: 12px; }
        td { padding: 10px; border-bottom: 1px solid #ddd; }
        tr:hover { background-color: #f5f5f5; }
        .no-data { color: red; padding: 20px; }
    </style>
</head>
<body>

<h1>📊 All Student Information</h1>

<?php if (count($students) > 0): ?>

<table>
    <thead>
        <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Email</th>
            <th>Age</th>
            <th>Score</th>
        </tr>
    </thead>
    <tbody>
        <?php foreach ($students as $student): ?>
        <tr>
            <td><?php echo $student['id']; ?></td>
            <td><?php echo htmlspecialchars($student['name']); ?></td>
            <td><?php echo htmlspecialchars($student['email']); ?></td>
            <td><?php echo $student['age']; ?></td>
            <td><?php echo $student['score']; ?></td>
        </tr>
        <?php endforeach; ?>
    </tbody>
</table>

<?php else: ?>

<p class="no-data">No results found.</p>

<?php endif; ?>

</body>
</html>
```

**Verification:**
✓ Handle connection errors with try-catch
✓ Retrieve all rows with fetchAll()
✓ Safe output with htmlspecialchars()
✓ Display message when no results
✓ Distinguish between information display and error handling

---

## 💡 Problem-Solving Tips

### **Multiple Choice Strategy**

1. **SQL Syntax**: Distinguish between SELECT, INSERT, UPDATE, DELETE roles
2. **Prepared Statement**: Relationship between ? placeholder and execute()
3. **Security**: Roles of SQL Injection and htmlspecialchars()
4. **PDO Methods**: fetch() vs fetchAll(), prepare() vs query()

### **Practical Task Strategy**

1. **SQL Writing**: Filter with WHERE, sort with ORDER BY
2. **PDO Connection**: Exception handling with try-catch
3. **Prepared Statement**: Safe data substitution with placeholders
4. **Result Processing**: fetchAll() for multiple rows, fetch() for one row
5. **Security**: Safe output handling with htmlspecialchars()

---

**Great effort! Keep it up! 💪**

---

Professor Cho Jeonghyun (peterchokr@gmail.com)
Yeungnam University College

