# 📝 Chapter 5 Practice Questions: Form Processing & GET/POST Requests

---

## 🎯 Multiple Choice (10 Questions)

#### **Question 1: What is the most significant difference between POST and GET methods in a form?**

① POST displays data in the URL, GET hides it
② GET displays data in the URL, POST hides it
③ POST has a data size limit, GET does not
④ GET has higher security, POST has lower security

---

#### **Question 2: Which method should be used for a signup form, POST or GET?**

① GET (better to display in URL for confirmation)
② POST (safer for security)
③ Use both POST and GET simultaneously
④ It doesn't matter (both have the same function)

---

#### **Question 3: What should be filled in ? in the following code?**

```php
if ($_SERVER['REQUEST_METHOD'] === ?) {
    $username = $_POST['username'];
    echo "Login attempt";
}
```

① 'GET'
② 'POST'
③ 'SUBMIT'
④ 'FORM'

---

#### **Question 4: What is the difference between isset() and empty()?**

① isset() checks if variable exists, empty() checks if value is empty
② isset() checks if value is empty, empty() checks if variable exists
③ Both have the same role
④ isset() only checks numbers, empty() only checks strings

---

#### **Question 5: What is the result of the following code?**

```php
$value = 0;

if (isset($value)) {
    echo "A";
}

if (!empty($value)) {
    echo "B";
}
```

① Only A is output
② Only B is output
③ Both A and B are output
④ Nothing is output

---

#### **Question 6: What is the PHP function to validate email?**

① is_numeric()
② strlen()
③ filter_var($email, FILTER_VALIDATE_EMAIL)
④ htmlspecialchars()

---

#### **Question 7: What is the role of htmlspecialchars()?**

① Encryption
② Converts HTML special characters to HTML entities (prevents HTML tag injection)
③ Converts string to number
④ Saves data to database

---

#### **Question 8: What is the result of the following code?**

```php
$name = "<script>alert('attack')</script>";

echo htmlspecialchars($name);
```

① &lt;script&gt;alert('attack')&lt;/script&gt; is output
② `<script>`alert('attack')`</script>` is output
③ Script is executed
④ Error occurs

---

#### **Question 9: What is the safe method to search a database with user-entered search terms?**

① `$sql = "SELECT * FROM students WHERE name = '" . $_GET['keyword'] . "'";`
② `$sql = "SELECT * FROM students WHERE name LIKE ?";` (Prepared Statement)
③ Use directly without validation
④ Remove all special characters

---

#### **Question 10: What should be filled in ? in the following HTML form code?**

```html
<form ? action="login.php">
    <input type="text" name="username">
    <input type="password" name="password">
    <button type="submit">Login</button>
</form>
```

① method="GET"
② method="POST"
③ method="SUBMIT"
④ type="form"

---

## 💻 Practical Tasks (5 Questions)

### **Task 1: Simple Login Form (Using POST)**

**Requirements:**

- Create HTML form (method="POST", action="login_process.php")
- Username input field (name="username")
- Password input field (name="password")
- Login button
- Process and output POST data received

**Filename**: `login_form.php`

```php
<?php
// Write your code here

?>
```

---

### **Task 2: Search Form (Using GET)**

**Requirements:**

- Create HTML form (method="GET", action="search.php")
- Search term input field (name="keyword")
- Search button
- Output the search term received via GET to the screen
- Format: "Search term: John Smith"

**Filename**: `search_form.php`

```php
<?php
// Write your code here

?>
```

---

### **Task 3: Signup Form with Email Validation**

**Requirements:**

- Create HTML form (method="POST", name="username", name="email")
- Username input field
- Email input field
- Signup button
- POST data reception and validation:
  - Check if username and email are required
  - Check if email format is correct
  - If validation passes: "Signup completed"
  - If validation fails: Output error message

**Filename**: `signup_form.php`

```php
<?php
// Write your code here

?>
```

---

### **Task 4: Input Data Validation (Length Limit)**

**Requirements:**

- Create HTML form (username, password input)
- Username: 5 to 20 characters
- Password: 6 characters or more
- Validation logic:
  - Use strlen() function to check length
  - Store error message in array if validation fails
  - Display all error messages on screen

**Filename**: `validation_form.php`

```php
<?php
// Write your code here

?>
```

---

### **Task 5: Form Data Processing and Safe Output**

**Requirements:**

- HTML form (POST): User information input (name, major, introduction)
- Check form submission ($_SERVER['REQUEST_METHOD'] === 'POST')
- Use htmlspecialchars() for safe processing of all inputs
- Display input data in HTML table format
- Do not display table before form submission
- Retain previous input values in form (using value attribute)

**Filename**: `profile_form.php`

```php
<?php
// Write your code here

?>
```

---

---

## ✅ Answers and Explanations

### **Multiple Choice Answers**

| Question | Answer                                                                                       | Explanation                                                                                       |
| -------- | -------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| 1        | **② GET displays data in the URL, POST hides it**                                     | POST method sends data in the HTTP request body, so it is not displayed in the URL                |
| 2        | **② POST (safer for security)**                                                       | Signup involves sensitive information (password), so POST should be used                          |
| 3        | **② 'POST'**                                                                          | When REQUEST_METHOD value is 'POST', $_POST data is used                                          |
| 4        | **① isset() checks if variable exists, empty() checks if value is empty**             | isset() checks if not NULL, empty() checks if value is empty, 0, false, etc.                      |
| 5        | **① Only A is output**                                                                | isset(0) is true (variable exists), empty(0) is true (0 is considered empty), so only A is output |
| 6        | **③ filter_var($email, FILTER_VALIDATE_EMAIL)**                                       | This is a built-in PHP function to validate email format                                          |
| 7        | **② Converts HTML special characters to HTML entities (prevents HTML tag injection)** | htmlspecialchars() converts <, >, &, " to HTML entities                                           |
| 8        | **① &lt;script&gt;alert('attack')&lt;/script&gt; is output**                          | Converted by htmlspecialchars(), so the script is not executed                                    |
| 9        | **② `$sql = "SELECT * FROM students WHERE name LIKE ?";` (Prepared Statement)**     | Using Prepared Statement protects against SQL Injection attacks                                   |
| 10       | **② method="POST"**                                                                   | Login with sensitive information should use POST method                                           |

---

### **Practical Task Solutions**

#### **Task 1: Simple Login Form (Using POST) - Answer**

```php
<?php
/**
 * login_form.php - Login form using POST method
 */

$username = '';
$password = '';
$message = '';

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $username = htmlspecialchars($_POST['username']);
    $password = htmlspecialchars($_POST['password']);
  
    if (!empty($username) && !empty($password)) {
        $message = "Login attempt: " . $username;
    }
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>Login</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 50px; }
        .form { max-width: 400px; padding: 20px; border: 1px solid #ddd; }
        input { width: 100%; padding: 8px; margin: 10px 0; box-sizing: border-box; }
        button { background-color: navy; color: white; padding: 10px; width: 100%; cursor: pointer; }
        .message { color: green; padding: 10px; margin: 10px 0; }
    </style>
</head>
<body>

<div class="form">
    <h2>Login</h2>
  
    <form method="POST">
        <label>Username:</label>
        <input type="text" name="username" value="<?php echo $username; ?>" required>
      
        <label>Password:</label>
        <input type="password" name="password" required>
      
        <button type="submit">Login</button>
    </form>
  
    <?php if ($message): ?>
    <p class="message"><?php echo $message; ?></p>
    <?php endif; ?>
</div>

</body>
</html>
```

**Verification:**
✓ Uses method="POST"
✓ Checks $_SERVER['REQUEST_METHOD'] === 'POST'
✓ Safe data handling with htmlspecialchars()
✓ Retains previous values in form (value attribute)

---

#### **Task 2: Search Form (Using GET) - Answer**

```php
<?php
/**
 * search_form.php - Search form using GET method
 */

$keyword = '';

if (isset($_GET['keyword'])) {
    $keyword = htmlspecialchars($_GET['keyword']);
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>Search</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 30px; }
        .search-box { padding: 20px; background-color: #f9f9f9; border-left: 4px solid #2196F3; }
        input { padding: 8px; width: 200px; }
        button { padding: 8px 20px; background-color: navy; color: white; cursor: pointer; }
        .result { padding: 20px; margin-top: 20px; background-color: #f0f0f0; }
    </style>
</head>
<body>

<div class="search-box">
    <h1>🔍 Search</h1>
    <form method="GET">
        <input type="text" name="keyword" value="<?php echo $keyword; ?>" placeholder="Enter search term">
        <button type="submit">Search</button>
    </form>
</div>

<?php if ($keyword): ?>
<div class="result">
    <h3>Search term: <?php echo $keyword; ?></h3>
    <p>Search results are displayed here</p>
</div>
<?php endif; ?>

</body>
</html>
```

**Verification:**
✓ Uses method="GET"
✓ Checks isset($_GET['keyword'])
✓ Safe data handling with htmlspecialchars()
✓ Outputs search term to screen

---

#### **Task 3: Signup Form with Email Validation - Answer**

```php
<?php
/**
 * signup_form.php - Signup form with email validation
 */

$username = '';
$email = '';
$errors = array();
$success = false;

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $username = htmlspecialchars($_POST['username']);
    $email = htmlspecialchars($_POST['email']);
  
    // Check required fields
    if (empty($username)) {
        $errors[] = "Please enter your username";
    }
  
    if (empty($email)) {
        $errors[] = "Please enter your email";
    }
  
    // Check email format
    if (!empty($email) && !filter_var($email, FILTER_VALIDATE_EMAIL)) {
        $errors[] = "Invalid email format";
    }
  
    // Validation passed
    if (count($errors) === 0) {
        $success = true;
    }
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>Sign Up</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 50px; }
        .container { max-width: 500px; padding: 20px; border: 1px solid #ddd; }
        input { width: 100%; padding: 8px; margin: 10px 0; box-sizing: border-box; }
        button { background-color: navy; color: white; padding: 10px; width: 100%; cursor: pointer; }
        .error { color: red; padding: 5px; }
        .success { color: green; padding: 10px; background-color: #f0f0f0; }
    </style>
</head>
<body>

<div class="container">
    <h2>Sign Up</h2>
  
    <?php if ($success): ?>
    <div class="success">✅ Signup completed!</div>
    <?php endif; ?>
  
    <?php foreach ($errors as $error): ?>
    <div class="error">❌ <?php echo $error; ?></div>
    <?php endforeach; ?>
  
    <form method="POST">
        <label>Username:</label>
        <input type="text" name="username" value="<?php echo $username; ?>" required>
      
        <label>Email:</label>
        <input type="email" name="email" value="<?php echo $email; ?>" required>
      
        <button type="submit">Sign Up</button>
    </form>
</div>

</body>
</html>
```

**Verification:**
✓ POST method form
✓ Checks isset() and empty()
✓ Validates email with filter_var(FILTER_VALIDATE_EMAIL)
✓ Stores error messages in array
✓ Retains previous values in form

---

#### **Task 4: Input Data Validation (Length Limit) - Answer**

```php
<?php
/**
 * validation_form.php - Validate length with strlen()
 */

$username = '';
$password = '';
$errors = array();

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $username = htmlspecialchars($_POST['username']);
    $password = htmlspecialchars($_POST['password']);
  
    // Check username length
    if (strlen($username) < 5) {
        $errors[] = "Username must be at least 5 characters";
    } elseif (strlen($username) > 20) {
        $errors[] = "Username must be 20 characters or less";
    }
  
    // Check password length
    if (strlen($password) < 6) {
        $errors[] = "Password must be at least 6 characters";
    }
  
    // Validation success
    if (count($errors) === 0) {
        $errors[] = "✅ All validations passed!";
    }
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>Validation Form</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 50px; }
        .container { max-width: 400px; padding: 20px; border: 1px solid #ddd; }
        input { width: 100%; padding: 8px; margin: 10px 0; box-sizing: border-box; }
        button { background-color: navy; color: white; padding: 10px; width: 100%; cursor: pointer; }
        .error { color: red; padding: 5px; margin: 5px 0; }
        .success { color: green; padding: 5px; margin: 5px 0; }
    </style>
</head>
<body>

<div class="container">
    <h2>Create Account</h2>
  
    <?php foreach ($errors as $error): ?>
        <?php if (strpos($error, '✅') !== false): ?>
            <div class="success"><?php echo $error; ?></div>
        <?php else: ?>
            <div class="error"><?php echo $error; ?></div>
        <?php endif; ?>
    <?php endforeach; ?>
  
    <form method="POST">
        <label>Username (5-20 characters):</label>
        <input type="text" name="username" value="<?php echo $username; ?>" required>
      
        <label>Password (6 characters or more):</label>
        <input type="password" name="password" required>
      
        <button type="submit">Validate</button>
    </form>
</div>

</body>
</html>
```

**Verification:**
✓ Checks string length with strlen()
✓ Validates minimum and maximum length
✓ Stores error messages in array
✓ Distinguishes results by color (red: error, green: success)

---

#### **Task 5: Form Data Processing and Safe Output - Answer**

```php
<?php
/**
 * profile_form.php - Safe output with htmlspecialchars()
 */

$name = '';
$major = '';
$introduction = '';
$submitted = false;

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $name = htmlspecialchars($_POST['name']);
    $major = htmlspecialchars($_POST['major']);
    $introduction = htmlspecialchars($_POST['introduction']);
    $submitted = true;
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>User Profile</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 30px; }
        .container { max-width: 700px; padding: 20px; border: 1px solid #ddd; }
        input, textarea { width: 100%; padding: 8px; margin: 10px 0; box-sizing: border-box; }
        button { background-color: navy; color: white; padding: 10px; cursor: pointer; }
        table { width: 100%; border-collapse: collapse; margin-top: 30px; }
        th { background-color: navy; color: white; padding: 10px; }
        td { padding: 10px; border-bottom: 1px solid #ddd; }
    </style>
</head>
<body>

<div class="container">
    <h1>📋 User Profile</h1>
  
    <!-- Input form -->
    <h2>Enter Profile Information</h2>
    <form method="POST">
        <label>Name:</label>
        <input type="text" name="name" value="<?php echo $name; ?>" placeholder="John Smith" required>
      
        <label>Major:</label>
        <input type="text" name="major" value="<?php echo $major; ?>" placeholder="Software Engineering" required>
      
        <label>Introduction:</label>
        <textarea name="introduction" rows="5" placeholder="Enter your introduction"><?php echo $introduction; ?></textarea>
      
        <button type="submit">Save</button>
    </form>
  
    <!-- Display results -->
    <?php if ($submitted && !empty($name) && !empty($major) && !empty($introduction)): ?>
    <h2>Profile Information</h2>
    <table>
        <thead>
            <tr>
                <th>Item</th>
                <th>Content</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>Name</td>
                <td><?php echo $name; ?></td>
            </tr>
            <tr>
                <td>Major</td>
                <td><?php echo $major; ?></td>
            </tr>
            <tr>
                <td>Introduction</td>
                <td><?php echo nl2br($introduction); ?></td>
            </tr>
        </tbody>
    </table>
    <?php endif; ?>
</div>

</body>
</html>
```

**Verification:**
✓ Checks REQUEST_METHOD (POST)
✓ Safe handling of all inputs with htmlspecialchars()
✓ Retains previous input values in form (value attribute)
✓ Retains previous value in textarea
✓ Displays result table only after submission
✓ Handles line breaks with nl2br()

---

## 💡 Problem-Solving Tips

### **Multiple Choice Strategy**

1. **GET vs POST**: GET (displays in URL) vs POST (hidden), security differences
2. **Validation Functions**: isset (existence check) vs empty (empty value check)
3. **Filter Functions**: filter_var($email, FILTER_VALIDATE_EMAIL)
4. **Safety**: Always use htmlspecialchars()
5. **Form Submission Check**: $_SERVER['REQUEST_METHOD'] === 'POST'

### **Practical Task Strategy**

1. **Form Writing**: Accurately specify method, action, name attributes
2. **Form Submission Check**: Check REQUEST_METHOD
3. **Data Reception**: Use $_POST or $_GET
4. **Validation**: isset(), empty(), strlen(), filter_var()
5. **Safe Processing**: htmlspecialchars() and retain previous values
6. **Error Handling**: Collect in array and output with foreach
7. **Conditional Display**: Display results only after submission

---

**Great effort! Keep it up! 💪**

---

Thank you for your attention.   
Cho Jeonghyun ([peterchokr@gmail.com](mailto:peterchokr@gmail.com)). Yeungnam University College

"Produced in collaboration with Claude and Gemini."
