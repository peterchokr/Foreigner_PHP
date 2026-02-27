# Chapter 5. Form Processing and GET/POST Requests

---

## 📚 Learning Objectives

After completing this chapter, you will be able to:

✅ Create user input forms using HTML form tags
✅ Understand and correctly apply GET and POST methods
✅ Receive form data in PHP using $_POST and $_GET
✅ Validate user input to ensure data integrity
✅ Process and display data safely using htmlspecialchars()

이 장을 학습하면 다음을 할 수 있습니다:

✅ HTML form 태그로 사용자 입력 폼을 만들 수 있습니다.
✅ GET과 POST의 차이를 이해하고 올바르게 사용할 수 있습니다.
✅ PHP에서 $_POST, $_GET으로 폼 데이터를 받을 수 있습니다.
✅ 사용자 입력을 검증할 수 있습니다.
✅ 데이터를 안전하게 처리하여 출력할 수 있습니다.

---

## 1️⃣ Form Basics Review (폼 기초 복습)

### 1-1 What is an HTML Form?

An HTML form is the way users input data and send it to the server. Forms are essential for collecting user information such as login credentials, registration data, search queries, and more.

HTML 폼은 사용자가 데이터를 입력하고 서버로 전송하는 방법입니다. 폼은 로그인 정보, 가입 데이터, 검색 쿼리 등 사용자 정보를 수집하는 데 필수적입니다.

```html
<!-- Basic form structure (기본 폼 구조) -->
<form method="POST" action="process.php">
    <!-- Input fields (입력 필드들) -->
    <input type="text" name="username">
  
    <!-- Submit button (전송 버튼) -->
    <button type="submit">Submit</button>
</form>

<!-- Important attributes: (중요 속성:)
     method: data transmission method - GET or POST (데이터 전송 방식 - GET 또는 POST)
     action: PHP file to process the data (데이터를 처리할 PHP 파일)
     name: name used to receive data in PHP (PHP에서 데이터를 받을 때 사용하는 이름)
-->
```

### 1-2 Major Input Elements (주요 입력 요소들)

```html
<!-- Text input (텍스트 입력) -->
<input type="text" name="username" placeholder="Username">
<br>
<!-- Password input - hidden on screen (비밀번호 입력 - 화면에 안 보임) -->
<input type="password" name="password" placeholder="Password">
<br>
<!-- Email input (이메일 입력) -->
<input type="email" name="email" placeholder="Email">
<br>
<!-- Number input (숫자 입력) -->
<input type="number" name="age" placeholder="Age" min="0" max="150">
<br>
<!-- Multi-line text (여러 줄 텍스트) -->
<textarea name="comment" rows="4" cols="50"></textarea>
<br>
<!-- Dropdown selection (드롭다운 선택) -->
<select name="subject">
    <option value="math">Mathematics</option>
    <option value="english">English</option>
    <option value="science">Science</option>
</select>
<br>
<!-- Checkbox - multiple selection possible (체크박스 - 여러 개 선택 가능) -->
<input type="checkbox" name="hobby" value="reading"> Reading
<input type="checkbox" name="hobby" value="gaming"> Gaming
<br>
<!-- Radio button - only one selection (라디오 버튼 - 하나만 선택) -->
<input type="radio" name="gender" value="male"> Male
<input type="radio" name="gender" value="female"> Female
<br>
```

## 2️⃣ GET vs POST

### 2-1 GET Method

**Characteristics of GET requests:**

- Data appears in the URL (데이터가 URL에 표시됨)
- Purpose: data retrieval such as search and filtering (용도: 데이터 조회 - 검색, 필터링)
- Security: low, never send passwords (보안: 낮음 - 암호는 절대 전송 금지)

```html
<form method="GET" action="search.php">
    <input type="text" name="keyword">
    <button type="submit">Search</button>
</form>
```

**Receiving in PHP:**

```php
<?php
// URL: search.php?keyword=Python
// Receive the search keyword (검색어 받기)
$keyword = $_GET['keyword'];
echo "Search term: " . $keyword;
?>
```

### 2-2 POST Method

**Characteristics of POST requests:**

- Data does not appear in the URL (데이터가 URL에 표시 안 됨)
- Purpose: data transmission such as registration, login, and modification (용도: 데이터 전송 - 가입, 로그인, 수정)
- Security: high (recommended) (보안: 높음 - 권장)

```html
<form method="POST" action="login.php">
    <input type="text" name="username" placeholder="Username">
    <input type="password" name="password" placeholder="Password">
    <button type="submit">Login</button>
</form>
```

**Receiving in PHP:**

```php
<?php
// Receive form data (폼 데이터 받기)
$username = $_POST['username'];
$password = $_POST['password'];

// Validate and process (검증 및 처리)
if ($username && $password) {
    echo "Login attempt: " . $username;
}
?>
```

### 2-3 GET vs POST Comparison

| Item                   | GET            | POST         |
| ---------------------- | -------------- | ------------ |
| **Data Display** | Visible in URL | Hidden       |
| **Security**     | Low            | High         |
| **Purpose**      | Query          | Transmission |
| **Data Size**    | Limited        | Large        |
| **Caching**      | Cached         | Not cached   |

---

## 3️⃣ PHP Form Processing (PHP 폼 처리)

### 3-1 Receiving Data with $_POST ($_POST로 데이터 수신)

```php
<?php

// Step 1: Check if form was submitted (단계 1: 폼이 제출되었는지 확인)
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
  
    // Step 2: Receive form data (단계 2: 폼 데이터 수신)
    $name = $_POST['name'];
    $email = $_POST['email'];
    $age = $_POST['age'];
  
    // Step 3: Process data (단계 3: 데이터 처리)
    echo "Name: " . $name . "<br>";
    echo "Email: " . $email . "<br>";
    echo "Age: " . $age . "<br>";
}

?>
```

### 3-2 isset() and empty() Functions

```php
<?php

// isset(): Check if variable exists and is not NULL
// (변수가 존재하고 NULL이 아닌가?)
if (isset($_POST['username'])) {
    echo "Username has been entered";
}

// empty(): Check if variable is empty (0, "", false, etc.)
// (변수가 비어있는가? - 0, "", false 등)
if (empty($_POST['password'])) {
    echo "Please enter password";
}

// Common pattern for safe form processing
// (안전한 폼 처리 패턴)
if ($_SERVER['REQUEST_METHOD'] === 'POST' && 
    !empty($_POST['username']) && 
    !empty($_POST['password'])) {
  
    $username = $_POST['username'];
    $password = $_POST['password'];
    echo "Processing login...";
}

?>
```

### 3-3 Receiving Data with $_GET ($_GET으로 데이터 수신)

```php
<?php

// Using GET for search functionality
// (검색 기능에 GET 사용)
if (isset($_GET['search'])) {
    $search_keyword = $_GET['search'];
    echo "Search term: " . $search_keyword;
}

// Using GET for filtering
// (필터링에 GET 사용)
if (isset($_GET['category'])) {
    $category = $_GET['category'];
    // Retrieve products in that category
    // (해당 카테고리의 상품 조회)
}

?>
```

### 3-4 Receiving Diverse Form Data in PHP (PHP에서 다양한 폼 데이터 받기)

Let's learn how to receive and process various input elements in PHP.

PHP에서 다양한 입력 요소를 받아서 처리하는 방법을 배워봅시다.

**File: `form_input.html`**

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Form Input Elements</title>
    <style>
        body { font-family: Arial, sans-serif; max-width: 600px; margin: 30px auto; padding: 20px; }
        label { display: block; margin-top: 10px; font-weight: bold; }
        textarea, select { width: 100%; padding: 8px; margin: 5px 0; }
        button { padding: 10px 20px; background: navy; color: white; border: none; cursor: pointer; }
    </style>
</head>
<body>

<h2>📝 User Information Input</h2>

<form method="POST" action="process_input.php">
    <!-- Text input (텍스트 입력) -->
    <label>Username:</label>
    <input type="text" name="username" placeholder="Username" required>
  
    <!-- Password input (비밀번호 입력) -->
    <label>Password:</label>
    <input type="password" name="password" placeholder="Password" required>
  
    <!-- Email input (이메일 입력) -->
    <label>Email:</label>
    <input type="email" name="email" placeholder="Email" required>
  
    <!-- Number input (숫자 입력) -->
    <label>Age:</label>
    <input type="number" name="age" placeholder="Age" min="0" max="150">
  
    <!-- Multi-line text (여러 줄 텍스트) -->
    <label>Bio:</label>
    <textarea name="comment" rows="4" placeholder="Enter your bio"></textarea>
  
    <!-- Dropdown selection (드롭다운 선택) -->
    <label>Major:</label>
    <select name="subject">
        <option value="">Select</option>
        <option value="math">Mathematics</option>
        <option value="english">English</option>
        <option value="science">Science</option>
        <option value="history">History</option>
    </select>
  
    <!-- Checkbox - multiple selection possible (체크박스 - 여러 개 선택 가능) -->
    <label>Hobbies (select all that apply):</label>
    <input type="checkbox" name="hobby[]" value="reading"> Reading
    <input type="checkbox" name="hobby[]" value="gaming"> Gaming
    <input type="checkbox" name="hobby[]" value="sports"> Sports
    <input type="checkbox" name="hobby[]" value="music"> Music
  
    <!-- Radio button - only one selection (라디오 버튼 - 하나만 선택) -->
    <label>Gender:</label>
    <input type="radio" name="gender" value="male"> Male
    <input type="radio" name="gender" value="female"> Female
  
    <button type="submit">Submit</button>
</form>

</body>
</html>
```

**File: `process_input.php`**

```php
<?php

// ============================================
// Check if form was submitted
// (폼 제출 확인)
// ============================================

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
  
    // ============================================
    // 1️⃣ Receive text input (textbox, password)
    // (텍스트 입력 받기)
    // ============================================
  
    // Use htmlspecialchars for safe output
    // (안전한 출력을 위해 htmlspecialchars 사용)
    $username = htmlspecialchars($_POST['username'] ?? '');
    $password = htmlspecialchars($_POST['password'] ?? '');
    echo "Username: " . $username . "<br>";
    echo "Password: (entered)<br>";

    // Null coalescing operator ??
    // If $_POST['username'] exists and is not null, use that value
    // Otherwise use empty string
    // (($_POST['username'] ?? '' - $_POST['username'] 값이 존재하고 null이 아니면 그 값을 사용하고, 그렇지 않으면 빈 문자열('')을 대입)
    // This is similar to using isset() but more concise
    // (이는 isset() 함수를 사용하는 것과 유사하지만 더 간결함)
  
    // ============================================
    // 2️⃣ Receive email input
    // (이메일 입력 받기)
    // ============================================
  
    $email = htmlspecialchars($_POST['email'] ?? '');
    echo "Email: " . $email . "<br>";
  
    // ============================================
    // 3️⃣ Receive number input
    // (숫자 입력 받기)
    // ============================================
  
    $age = isset($_POST['age']) ? (int)$_POST['age'] : 0;
    echo "Age: " . $age . "<br>";
  
    // ============================================
    // 4️⃣ Receive multi-line text (textarea)
    // (여러 줄 텍스트 받기)
    // ============================================

    // nl2br converts line breaks to <br> tags
    // (줄바꿈을 <br> 태그로 변환)
    $comment = nl2br(htmlspecialchars($_POST['comment'] ?? ''));
    echo "Bio:<br> " . $comment . "<br>";
  
    // ============================================
    // 5️⃣ Receive dropdown selected value (select)
    // (드롭다운 선택값 받기)
    // ============================================
  
    $subject = htmlspecialchars($_POST['subject'] ?? '');
    echo "Major: " . $subject . "<br>";
  
    // ============================================
    // 6️⃣ Receive checkbox values (multiple possible)
    // (체크박스 값 받기 - 여러 개 선택 가능)
    // ============================================
  
    // Checkboxes are received as arrays when multiple values are selected
    // (여러 개 선택되면 배열로 받음)
    $hobbies = isset($_POST['hobby']) ? $_POST['hobby'] : array();
  
    // Safely process each array element
    // (배열 각 요소를 안전하게 처리)
    $hobbies = array_map('htmlspecialchars', $hobbies);
  
    echo "Hobbies: " . implode(", ", $hobbies) . "<br>";
  
    // Check checkbox values
    // (체크박스 값 확인 예시)
    if (in_array('reading', $_POST['hobby'] ?? array())) {
        echo "→ You like reading<br>";
    }
  
    // ============================================
    // 7️⃣ Receive radio button values (only one)
    // (라디오 버튼 값 받기 - 하나만)
    // ============================================
  
    $gender = htmlspecialchars($_POST['gender'] ?? '');
    echo "Gender: " . ($gender === 'male' ? 'Male' : 'Female') . "<br>";
}

?>
```

**Key Points:**

중요 포인트:

- **Textbox, password**: Receive directly with `$_POST['name']`
  (텍스트박스, 비밀번호: `$_POST['name']`으로 직접 받음)
- **Email, number**: Receive the same way, use `(int)` for type conversion if needed
  (이메일, 숫자: 동일하게 받지만, 필요시 `(int)` 형변환)
- **textarea**: Receive multi-line text the same way
  (textarea: 여러 줄 텍스트를 동일하게 받음)
- **select**: Receive the value attribute of the selected option
  (select: 선택된 option의 value를 받음)
- **checkbox**: When multiple are selected, received as an array (must check array!)
  (checkbox: 여러 개가 선택되면 배열로 받음 - 반드시 배열 확인!)
- **radio**: Only one is selected, so received as a string
  (radio: 하나만 선택되므로 문자열로 받음)
- **Safety**: Apply `htmlspecialchars()` to all data
  (안전성: 모든 데이터에 `htmlspecialchars()` 적용)

---

## 4️⃣ Basic Data Validation (기본 데이터 검증)

### 4-1 Check for Required Fields (필드 필수 확인)

```php
<?php

$errors = array();

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
  
    // Check if name is required
    // (이름 필수 확인)
    if (empty($_POST['name'])) {
        $errors[] = "Name is required";
    }
  
    // Check if email is required
    // (이메일 필수 확인)
    if (empty($_POST['email'])) {
        $errors[] = "Email is required";
    }
  
    // Check if no errors
    // (오류가 없으면 처리)
    if (count($errors) === 0) {
        echo "Form is valid, processing...";
    } else {
        // Display all errors
        // (모든 오류 표시)
        foreach ($errors as $error) {
            echo "❌ " . $error . "<br>";
        }
    }
}

?>
```

### 4-2 Validate Email Format (이메일 형식 검증)

```php
<?php

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
  
    $email = $_POST['email'] ?? '';
  
    // Check email format using filter_var
    // (filter_var를 사용한 이메일 형식 확인)
    if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
        echo "❌ Invalid email format";
    } else {
        echo "✅ Valid email";
    }
}

?>
```

### 4-3 Validate Length of String (문자열 길이 검증)

```php
<?php

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
  
    $password = $_POST['password'] ?? '';
  
    // Check minimum password length
    // (최소 비밀번호 길이 확인)
    if (strlen($password) < 6) {
        echo "❌ Password must be at least 6 characters";
    } else if (strlen($password) > 20) {
        echo "❌ Password must not exceed 20 characters";
    } else {
        echo "✅ Password length is valid";
    }
}

?>
```

### 4-4 Validate Number Range (숫자 범위 검증)

```php
<?php

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
  
    $age = $_POST['age'] ?? '';
  
    // Check if input is numeric and within range
    // (숫자인지 확인하고 범위 확인)
    if (!is_numeric($age)) {
        echo "❌ Age must be a number";
    } else if ($age < 0 || $age > 150) {
        echo "❌ Age must be between 0 and 150";
    } else {
        echo "✅ Valid age";
    }
}

?>
```

### 4-5 Complete Validation Example (완전한 검증 예제)

```php
<?php

$errors = array();
$name = '';
$email = '';
$age = '';

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
  
    // Validation - Name
    // (검증 - 이름)
    $name = $_POST['name'] ?? '';
    if (empty($name)) {
        $errors[] = "Name is required";
    } else if (strlen($name) < 2) {
        $errors[] = "Name must be at least 2 characters";
    }
  
    // Validation - Email
    // (검증 - 이메일)
    $email = $_POST['email'] ?? '';
    if (empty($email)) {
        $errors[] = "Email is required";
    } else if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
        $errors[] = "Invalid email format";
    }
  
    // Validation - Age
    // (검증 - 나이)
    $age = $_POST['age'] ?? '';
    if (!empty($age)) {
        if (!is_numeric($age) || $age < 0 || $age > 150) {
            $errors[] = "Age must be between 0 and 150";
        }
    }
  
    // Process if validation passes
    // (검증 통과 시 처리)
    if (count($errors) === 0) {
        echo "✅ Form validation successful!";
    } else {
        echo "❌ Please correct the following errors:<br>";
        foreach ($errors as $error) {
            echo "- " . $error . "<br>";
        }
    }
}

?>
```

---

## 5️⃣ Client-Side Validation with JavaScript (JavaScript를 사용한 클라이언트 검증)

### 5-1 JavaScript Validation Functions (JavaScript 검증 함수)

```html
<!DOCTYPE html>
<html>
<head>
    <title>Client-Side Validation</title>
    <style>
        body { font-family: Arial, sans-serif; max-width: 600px; margin: 30px auto; padding: 20px; }
        .form-group { margin: 15px 0; }
        label { display: block; font-weight: bold; margin-bottom: 5px; }
        input, textarea { width: 100%; padding: 8px; border: 1px solid #ddd; }
        button { padding: 10px 20px; background: navy; color: white; border: none; cursor: pointer; }
        .error { color: red; font-size: 12px; margin-top: 3px; display: none; }
        .success { color: green; padding: 10px; margin: 10px 0; display: none; }
    </style>
</head>
<body>

<h2>User Registration Form</h2>

<form id="registerForm">
    <div class="form-group">
        <label for="username">Username:</label>
        <input type="text" id="username" name="username">
        <div class="error" id="usernameError"></div>
    </div>
  
    <div class="form-group">
        <label for="email">Email:</label>
        <input type="email" id="email" name="email">
        <div class="error" id="emailError"></div>
    </div>
  
    <div class="form-group">
        <label for="password">Password:</label>
        <input type="password" id="password" name="password">
        <div class="error" id="passwordError"></div>
    </div>
  
    <button type="submit">Register</button>
</form>

<div class="success" id="successMessage">✅ Registration successful!</div>

<script>
// Validate username
// (사용자명 검증)
function validateUsername() {
    const username = document.getElementById('username').value.trim();
    if (!username) {
        displayError('usernameError', 'Username is required');
        return false;
    } else if (username.length < 3) {
        displayError('usernameError', 'Username must be at least 3 characters');
        return false;
    } else if (!/^[a-zA-Z0-9_]+$/.test(username)) {
        displayError('usernameError', 'Username can only contain letters, numbers, and underscores');
        return false;
    }
    displayError('usernameError', '');
    return true;
}

// Validate email
// (이메일 검증)
function validateEmail() {
    const email = document.getElementById('email').value.trim();
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!email) {
        displayError('emailError', 'Email is required');
        return false;
    } else if (!emailRegex.test(email)) {
        displayError('emailError', 'Invalid email format');
        return false;
    }
    displayError('emailError', '');
    return true;
}

// Validate password
// (비밀번호 검증)
function validatePassword() {
    const password = document.getElementById('password').value;
    if (!password) {
        displayError('passwordError', 'Password is required');
        return false;
    } else if (password.length < 6) {
        displayError('passwordError', 'Password must be at least 6 characters');
        return false;
    }
    displayError('passwordError', '');
    return true;
}

// Validate entire form
// (전체 폼 검증)
function validateForm() {
    return validateUsername() && validateEmail() && validatePassword();
}

// Display error message
// (오류 메시지 표시 함수)
function displayError(elementId, message) {
    const errorElement = document.getElementById(elementId);
    if (message) {
        errorElement.textContent = message;
        errorElement.style.display = 'block';
    } else {
        errorElement.textContent = '';
        errorElement.style.display = 'none';
    }
}

// Form submit event
// (폼 제출 이벤트)
document.getElementById('registerForm').addEventListener('submit', function(e) {
    e.preventDefault();
  
    if (validateForm()) {
        document.getElementById('successMessage').style.display = 'block';
        console.log('Form validation complete! Ready to send to server');
  
        setTimeout(() => {
            document.getElementById('registerForm').reset();
            document.getElementById('successMessage').style.display = 'none';
        }, 2000);
    }
});
</script>
</body>
</html>
```

**Advantages of JavaScript Validation:**(JavaScript 검증의 장점:)

✅ **Improved user experience**
(사용자 경험 개선)

- Immediate error feedback (즉각적인 오류 피드백)
- No server round-trip (서버 왕복 없음)

✅ **Faster input**
(더 빠른 입력)

- Prevent errors before form submission (양식 제출 전 오류 방지)
- Reduce unnecessary server requests (불필요한 서버 요청 감소)

⚠️ **Important note:**

JavaScript validation alone is not sufficient
(JavaScript 검증만으로는 부족함)

**Server-side validation in PHP is also required!** (PHP 서버측 검증도 필수!)
JavaScript can be bypassed (JavaScript는 무시될 수 있음)

Always use dual validation (client + server)
(항상 이중 검증 - 클라이언트 + 서버 - 사용)

**Recommended pattern:**(권장 패턴:)

```
Client: JavaScript provides immediate feedback
(클라이언트: JavaScript로 즉각 피드백)
↓
Server: PHP performs final validation (required!)
(서버: PHP로 최종 검증 - 필수!)
```

---

## 6️⃣ Data Security Processing (데이터 안전성 처리)

### 6-1 htmlspecialchars() Function

**Purpose:** Convert user input so it is not interpreted as HTML tags. (사용자 입력을 HTML 태그로 해석되지 않도록 변환합니다.)

**How it works:**
(작동 방식:)

- Converts special characters like <, >, &, etc.
  (특수문자 <, >, &, 등을 변환)
- Prevents XSS (Cross-Site Scripting) attacks
  (XSS 공격 방지)
- Displays harmful code as plain text
  (해로운 코드를 평문으로 표시)

```php
<?php

$user_input = "<script>alert('attack')</script>";

// ❌ Dangerous method
// (위험한 방법)
echo $user_input;  // Script would execute!
                   // (스크립트 실행됨!)

// ✅ Safe method
// (안전한 방법)
echo htmlspecialchars($user_input);
// Result: <script>alert('attack')</script>
// (결과: <script>alert('attack')</script>)

?>
```

### 6-2 Safe Output Pattern (안전한 출력 패턴)

```php
<?php

// Output data from database
// (데이터베이스에서 가져온 데이터 출력)
$name = $student['name'];

// Always use htmlspecialchars()!
// (항상 htmlspecialchars() 사용!)
echo "Name: " . htmlspecialchars($name);

// When displaying value in HTML form
// (HTML 폼에 값 표시할 때)
?>

<input type="text" value="<?php echo htmlspecialchars($name); ?>">

<?php
// When displaying in table
// (테이블에 표시할 때)
echo "<td>" . htmlspecialchars($name) . "</td>";

?>
```

---

## 7️⃣ Practice Example (실습 예제)

### 7-1 Practice Example: Student Search Form (실습 예제: 학생 검색 폼)

**File: `student_search.php`**

```php
<?php

// Database connection parameters
// (데이터베이스 연결 매개변수)
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

$search_keyword = '';
$students = array();
$error = '';

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
  
    // Check if search term is provided
    // (검색어 확인)
    if (empty($_POST['keyword'])) {
        $error = "Please enter a search term";
    } else {
        $search_keyword = htmlspecialchars($_POST['keyword']);
  
        // Safe search using Prepared Statement
        // (준비된 문장을 사용한 안전한 검색)
        $sql = "SELECT * FROM students 
                WHERE name LIKE ? ORDER BY name";
        $stmt = $pdo->prepare($sql);
        $stmt->execute(['%' . $search_keyword . '%']);
        $students = $stmt->fetchAll(PDO::FETCH_ASSOC);
    }
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>Student Search</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 30px auto;
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
  
        .search-box {
            margin: 20px 0;
            padding: 15px;
            background-color: #f9f9f9;
            border-left: 4px solid #2196F3;
        }
  
        input, button {
            padding: 8px;
            border: 1px solid #ddd;
        }
  
        button {
            background-color: navy;
            color: white;
            cursor: pointer;
        }
  
        .error {
            color: red;
            padding: 10px;
            margin: 10px 0;
        }
  
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
  
        th {
            background-color: navy;
            color: white;
            padding: 10px;
        }
  
        td {
            padding: 10px;
            border-bottom: 1px solid #ddd;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>📚 Student Search</h1>
  
    <!-- Search form (검색 폼) -->
    <div class="search-box">
        <form method="POST">
            <label>Search by student name:</label>
            <input type="text" name="keyword" 
                   value="<?php echo htmlspecialchars($search_keyword); ?>"
                   placeholder="Example: John Smith">
            <button type="submit">Search</button>
        </form>
    </div>
  
    <!-- Error message (오류 메시지) -->
    <?php if ($error): ?>
    <div class="error"><?php echo $error; ?></div>
    <?php endif; ?>
  
    <!-- Search results (검색 결과) -->
    <?php if (count($students) > 0): ?>
    <p>Search results: <?php echo count($students); ?> students</p>
    <table>
        <thead>
            <tr>
                <th>Name</th>
                <th>Email</th>
                <th>Age</th>
                <th>Score</th>
            </tr>
        </thead>
        <tbody>
            <?php foreach ($students as $student): ?>
            <tr>
                <td><?php echo htmlspecialchars($student['name']); ?></td>
                <td><?php echo htmlspecialchars($student['email']); ?></td>
                <td><?php echo $student['age']; ?></td>
                <td><?php echo $student['score']; ?></td>
            </tr>
            <?php endforeach; ?>
        </tbody>
    </table>
    <?php elseif ($_SERVER['REQUEST_METHOD'] === 'POST'): ?>
    <p>No search results found.</p>
    <?php endif; ?>
</div>

</body>
</html>
```

---

## 8️⃣ Assignments (과제)

### Assignment 1: GET and POST Method Selection (과제 1: GET과 POST 사용 구분)

Determine whether to use GET or POST for the following scenarios:

다음 상황에서 GET과 POST 중 어떤 것을 사용할지 선택하세요:

1. Product search form (상품 검색 폼)
2. User registration form (회원가입 폼)
3. Filtering products by category (카테고리별 상품 필터링)
4. Password change form (비밀번호 변경 폼)
5. Pagination (moving to page numbers) (페이지네이션 - 페이지 번호 이동)

---

### Assignment 2: Expand the Student Search Form (과제 2: 학생 검색 폼 확장)

Expand the student search form from the practice example with the following features:

실습 예제의 학생 검색 폼을 다음과 같이 확장하세요:

1. **Step 1: Basic search**
   (기본 검색)

   - Search by student name (as learned in class) (수업에서 학습)
2. **Step 2: Add score search**
   (성적 검색 추가)

   - Add an input field for minimum score (성적이상값 입력 필드 추가)
   - Example: "Find students with scores of 80 or higher" (예: "80점 이상인 학생 찾기")
3. **Step 3: Add validation**
   (검증 추가)

   - Check if search term is at least 3 characters (검색어가 3자 이상인지 확인)
   - Check if score is between 0 and 100 (성적이 0~100 사이인지 확인)
4. **Step 4: Display results**
   (결과 표시)

   - Show search conditions on the screen (검색 조건을 화면에 표시)
   - Display the number of search results (검색 결과 개수 표시)

---

## 9️⃣ Important Points (중요 포인트)

### Always remember (항상 기억하기)

✅ **Check form submission**
(폼 제출 확인)

- Verify $_SERVER['REQUEST_METHOD'] === 'POST' (($_SERVER['REQUEST_METHOD'] === 'POST' 확인)
- Prevent unnecessary processing (불필요한 처리 방지)

✅ **Validate all fields**
(모든 필드 검증)

- Use empty() to check required fields (empty()로 필수 필드 확인)
- Use strlen() to check length (strlen()으로 길이 확인)
- Use filter_var() to check format (filter_var()로 형식 확인)

✅ **Safe output**
(안전한 출력)

- Always use htmlspecialchars() when displaying data (데이터 출력 시 항상 htmlspecialchars() 사용)
- Convert all data coming from user input (사용자 입력에서 온 모든 데이터 변환)

---

Thank you for your attention.   
Cho Jeonghyun ([peterchokr@gmail.com](mailto:peterchokr@gmail.com)). Yeungnam University College

"Produced in collaboration with Claude and Gemini."
