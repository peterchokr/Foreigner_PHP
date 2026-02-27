# Chapter 1. Web Development Basics and PHP Setup

---

## 📚 Learning Objectives

After completing this chapter, you will be able to:

✅ Review and understand the core concepts of HTML/CSS/JavaScript
✅ Review the basic syntax of MySQL
✅ Set up and configure a PHP development environment
✅ Create and run your first PHP file and verify the results
✅ Use MySQL Workbench to connect to a database

이 장을 학습하면 다음을 할 수 있습니다:

✅ HTML/CSS/JavaScript의 핵심 개념을 다시 정리할 수 있습니다.
✅ MySQL의 기본 문법을 복습할 수 있습니다.
✅ PHP 개발 환경을 구축하고 설정할 수 있습니다.
✅ 첫 번째 PHP 파일을 실행하고 결과를 확인할 수 있습니다.
✅ MySQL Workbench를 사용하여 데이터베이스에 접속할 수 있습니다.

---

## 1️⃣ HTML/CSS/JavaScript Fundamentals Review (HTML/CSS/JavaScript 핵심 복습)

### 1-1 Essential HTML Tags (주요 HTML 태그)

This section reviews the fundamental HTML tags that form the foundation of web development.

이 섹션에서는 웹 개발의 기초가 되는 HTML 태그들을 복습합니다.

#### **Form Tag (사용자 입력)**

Form tags are used to collect user input and send it to a server for processing. The `method` attribute specifies how data is transmitted (GET or POST), and the `action` attribute defines which server file will process the data.

Form 태그는 사용자 입력을 수집하여 서버로 전송하는 데 사용됩니다. `method` 속성은 데이터 전송 방식을 지정하고(GET 또는 POST), `action` 속성은 데이터를 처리할 서버 파일을 정의합니다.

```php
<!-- Basic form structure (기본 form 구조) -->
<form method="POST" action="process.php">
    <input type="text" name="username" placeholder="Username">
    <input type="password" name="password" placeholder="Password">
    <input type="submit" value="Submit">
</form>
```

**Key Attributes (주요 속성):**

- `method`: Data transmission method (GET or POST) / 데이터 전송 방식 (GET 또는 POST)
- `action`: Server file path to process the data / 데이터를 받을 서버 파일 경로

#### **Input Tag (다양한 입력 필드)**

The input tag creates various types of form fields for different data types. Each type has specific validation and presentation features appropriate for the data being collected.

Input 태그는 다양한 데이터 유형에 대한 여러 형태의 입력 필드를 생성합니다. 각 유형은 수집하는 데이터에 적합한 검증 및 표시 기능을 가지고 있습니다.

```html
<input type="text" name="name" placeholder="Full Name">           <!-- Text input (텍스트) -->
<input type="number" name="age" min="0" max="120">              <!-- Number input (숫자) -->
<input type="email" name="email">                               <!-- Email input (이메일) -->
<input type="password" name="password">                         <!-- Password input (비밀번호) -->
<input type="checkbox" name="hobby" value="coding">             <!-- Checkbox (체크박스) -->
<input type="radio" name="gender" value="male">                 <!-- Radio button (라디오 버튼) -->
<input type="submit" value="Submit">                            <!-- Submit button (제출 버튼) -->
```

#### **Table Tag (표 작성)**

Tables are used to organize and display data in rows and columns. The `<thead>` section contains header information, while `<tbody>` contains the actual data rows.

표는 행과 열로 데이터를 조직화하고 표시하는 데 사용됩니다. `<thead>` 섹션은 헤더 정보를 포함하고, `<tbody>`는 실제 데이터 행을 포함합니다.

```html
<table border="1">
    <thead>
        <tr><th>Name</th><th>Age</th><th>Profession</th></tr>
    </thead>
    <tbody>
        <tr><td>John Smith</td><td>25</td><td>Developer</td></tr>
        <tr><td>Mary Johnson</td><td>23</td><td>Designer</td></tr>
    </tbody>
</table>
```

---

### 1-2 CSS Selectors and Styling Basics (CSS 선택자와 스타일링 기초)

CSS selectors allow you to target specific HTML elements and apply styles. Understanding selector specificity is crucial for effective CSS management. Selector priority determines which style is applied when multiple rules target the same element.

CSS 선택자를 사용하면 특정 HTML 요소를 대상으로 스타일을 적용할 수 있습니다. 선택자 우선순위를 이해하는 것은 효과적인 CSS 관리에 중요합니다. 같은 요소를 대상으로 하는 여러 규칙이 있을 때 어떤 스타일이 적용되는지 결정합니다.

```css
/* Type selector (타입 선택자) */
p { color: blue; }

/* Class selector (클래스 선택자) */
.highlight { background-color: yellow; font-weight: bold; }

/* ID selector (ID 선택자) */
#header { background-color: navy; color: white; }

/* Attribute selector (속성 선택자) */
input[type="text"] { border: 1px solid gray; padding: 5px; }

/* Pseudo-class (의사 클래스) */
a:hover { color: red; text-decoration: underline; }
```

**Selector Priority (선택자 우선순위):** ID selector > Class selector > Type selector

#### **Basic Styling Properties (기본 스타일링 속성)**

Common CSS properties control text appearance, spacing, and layout. Learning these fundamental properties provides the foundation for more advanced styling techniques.

일반적인 CSS 속성은 텍스트 모양, 간격 및 레이아웃을 제어합니다. 이 기본 속성들을 배우면 더 고급 스타일링 기법의 기초가 됩니다.

```css
/* Text styling (텍스트 스타일) */
.text { color: #333; font-size: 16px; font-weight: bold; text-align: center; }

/* Box model (박스 모델) */
.box { width: 200px; padding: 15px; margin: 10px; border: 2px solid gray; border-radius: 5px; }

/* Layout (레이아웃) */
.flex { display: flex; justify-content: center; align-items: center; }
```

---

### 1-3 JavaScript Practice Examples (JavaScript 실습 예제)

#### **Example 1: Simple Form Submission (예제 1: 간단한 폼 제출)**

This example demonstrates how to handle form submission and display user input without refreshing the page. By using JavaScript's `preventDefault()` method, the form data is processed without a page reload.

이 예제는 페이지 새로고침 없이 폼 제출을 처리하고 사용자 입력을 표시하는 방법을 보여줍니다. JavaScript의 `preventDefault()` 메서드를 사용하면 페이지 새로고침 없이 폼 데이터가 처리됩니다.

**File name: form_practice.html**

Save the code below as `form_practice.html` and open it in your browser:

아래 코드를 `form_practice.html`로 저장하고 브라우저에서 열면 됩니다:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Form Practice</title>
    <style>
        body { font-family: Arial, sans-serif; max-width: 500px; margin: 50px auto; padding: 20px; }
        input { width: 100%; padding: 8px; margin: 8px 0; border: 1px solid #ddd; box-sizing: border-box; }
        button { width: 100%; padding: 10px; background: navy; color: white; border: none; cursor: pointer; }
        #result { margin-top: 20px; padding: 10px; background: #e3f2fd; display: none; }
    </style>
</head>
<body>

<h2>📝 User Information</h2>
<form id="myForm">
    <input type="text" id="name" placeholder="Full Name" required>
    <input type="email" id="email" placeholder="Email" required>
    <button type="submit">Submit</button>
</form>

<div id="result"></div>

<script>
// Handle form submission 
document.getElementById('myForm').addEventListener('submit', function(e) {
    e.preventDefault();  // Prevent default form behavior 
    const name = document.getElementById('name').value;
    const email = document.getElementById('email').value;
    document.getElementById('result').innerHTML = `✅ Name: ${name}, Email: ${email}`;
    document.getElementById('result').style.display = 'block';
    this.reset();  // Clear form 
});
</script>

</body>
</html>
```

---

#### **Example 2: Button Click Event (예제 2: 버튼 클릭 이벤트)**

This example shows how to handle multiple button click events and dynamically modify element properties. Each button demonstrates different manipulation techniques: changing styles, updating dimensions, and modifying content.

이 예제는 여러 버튼 클릭 이벤트를 처리하고 요소 속성을 동적으로 수정하는 방법을 보여줍니다. 각 버튼은 다양한 조작 기법을 보여줍니다: 스타일 변경, 치수 업데이트, 콘텐츠 수정.

**File name: button_practice.html**

Save the code below as `button_practice.html`:

아래 코드를 `button_practice.html`로 저장하세요:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Button Click Practice</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 50px; }
        button { padding: 10px 20px; margin: 10px; background: navy; color: white; border: none; cursor: pointer; }
        .box { width: 100px; height: 100px; background: red; margin-top: 20px; }
    </style>
</head>
<body>

<h2>🎨 Click the buttons</h2>
<button id="btn1">Change Color</button>
<button id="btn2">Change Size</button>
<button id="btn3">Change Text</button>

<div class="box" id="box"></div>

<script>
// Change background color 
document.getElementById('btn1').addEventListener('click', () => {
    document.getElementById('box').style.backgroundColor = 'blue';
});

// Change dimensions
document.getElementById('btn2').addEventListener('click', () => {
    document.getElementById('box').style.width = '200px';
    document.getElementById('box').style.height = '200px';
});

// Change text content 
document.getElementById('btn3').addEventListener('click', () => {
    document.getElementById('box').textContent = 'Changed!';
});
</script>

</body>
</html>
```

---

#### **Example 3: Real-time Input Verification (예제 3: 입력값 실시간 확인)**

This example demonstrates real-time input validation and character counting. As the user types, JavaScript immediately processes the input and displays the results, providing instant feedback.

이 예제는 실시간 입력 검증 및 문자 수 세기를 보여줍니다. 사용자가 입력하는 동시에 JavaScript가 입력을 처리하고 결과를 표시하여 즉각적인 피드백을 제공합니다.

**File name: input_practice.html**

Save the code below as `input_practice.html`:

아래 코드를 `input_practice.html`로 저장하세요:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Input Practice</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 50px; }
        input { padding: 8px; width: 300px; font-size: 14px; }
        #output { margin-top: 20px; padding: 10px; background: #e3f2fd; min-height: 30px; }
    </style>
</head>
<body>

<h2>📝 Input Verification</h2>
<input type="text" id="input1" placeholder="Enter text... )">

<div id="output">Waiting for input...</div>

<script>
// Monitor input and display character count 
document.getElementById('input1').addEventListener('input', function() {
    const value = this.value;
    const length = value.length;
    document.getElementById('output').innerHTML = `Input: ${value}<br>Character count: ${length}`;
});
</script>

</body>
</html>
```

**Create all three files and open them in your browser!**

**3개의 파일을 각각 저장해서 브라우저에서 열어보세요!**

---

## 2️⃣ MySQL Basic Syntax Review (MySQL 기본 문법 복습)

### 2-1 Basic SQL Commands (기본 SQL 문법)

SQL provides essential commands for database operations. This section covers the fundamental operations: creating databases and tables, inserting data, querying information, and modifying or deleting records. Understanding these commands is essential for database management.

SQL은 데이터베이스 작업을 위한 필수 명령어들을 제공합니다. 이 섹션에서는 기본 작업들을 다룹니다: 데이터베이스와 테이블 생성, 데이터 삽입, 정보 조회, 레코드 수정 또는 삭제. 이 명령어들을 이해하는 것은 데이터베이스 관리에 필수적입니다.

```sql
-- Create a database (데이터베이스 생성)
CREATE DATABASE my_database;

-- Create a table (테이블 생성)
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50),
    age INT,
    email VARCHAR(100)
);

-- Insert data (데이터 삽입)
INSERT INTO users (name, age, email) VALUES ('John Smith', 25, 'john@example.com');

-- Query data (데이터 조회)
SELECT * FROM users;
SELECT name, email FROM users WHERE age > 20;

-- Update data (데이터 수정)
UPDATE users SET age = 26 WHERE name = 'John Smith';

-- Delete data (데이터 삭제)
DELETE FROM users WHERE id = 1;
```

---

## 3️⃣ PHP Development Environment Setup (PHP 개발 환경 설정)

### 3-1 XAMPP Installation (XAMPP 설치)

XAMPP is a free and open-source cross-platform web server solution package that includes Apache, MySQL, and PHP. It simplifies the setup process and allows you to develop and test web applications locally on your computer.

XAMPP는 Apache, MySQL, PHP를 포함하는 무료 오픈 소스 크로스 플랫폼 웹 서버 솔루션 패키지입니다. 설정 과정을 단순화하고 컴퓨터에서 로컬로 웹 애플리케이션을 개발하고 테스트할 수 있습니다.

#### **Installation Steps for Windows (Windows 설치 절차)**

1. **Download**: Visit `https://www.apachefriends.org/download.html`
2. **Install**: Recommended installation path is `C:\xampp`
3. **Run**: Start Apache and MySQL from the XAMPP Control Panel

다운로드: `https://www.apachefriends.org/download.html`을 방문하세요.
설치: 기본 경로 `C:\xampp` 권장
실행: XAMPP Control Panel에서 Apache와 MySQL 시작

**Verification Method (확인 방법):**

- `http://localhost` → "It works!" message displays ✅
- File location: `C:\xampp\htdocs`

### 3-2 php.ini Configuration (php.ini 설정)

The php.ini file contains important configuration settings that control PHP behavior. Key settings include character encoding, error display, file upload limits, and database extensions.

php.ini 파일은 PHP 동작을 제어하는 중요한 구성 설정을 포함합니다. 주요 설정은 문자 인코딩, 에러 표시, 파일 업로드 제한 및 데이터베이스 확장입니다.

```ini
; Character encoding (문자 인코딩)
default_charset = utf-8

; Error display (에러 표시)
display_errors = On
error_reporting = E_ALL

; File upload limits (파일 업로드 제한)
upload_max_filesize = 20M
post_max_size = 20M

; MySQL extensions (MySQL 확장)
extension=mysqli
extension=pdo_mysql
```

---

## 4️⃣ Running Your First PHP File (첫 번째 PHP 파일 실행)

### 4-1 "Hello, PHP World!" (기본 출력)

Your first PHP program will be a simple script that displays a greeting message. This demonstrates that your PHP installation is working correctly and the web server can execute PHP code.

첫 번째 PHP 프로그램은 인사말 메시지를 표시하는 간단한 스크립트입니다. 이것은 PHP 설치가 올바르게 작동하고 웹 서버가 PHP 코드를 실행할 수 있음을 보여줍니다.

**File name: hello.php**

```php
<?php
// Display greeting message
echo "Hello, PHP World!";
?>
```

**Execution Method (실행 방법):**

1. Save the file as `C:\xampp\htdocs\hello.php`
2. Open your browser and navigate to `http://localhost/hello.php`
3. You should see the message "Hello, PHP World!" ✅

파일을 `C:\xampp\htdocs\hello.php`에 저장합니다.
브라우저에서 `http://localhost/hello.php` 접속합니다.
"Hello, PHP World!" 메시지가 표시되어야 합니다. ✅

### 4-2 Basic PHP Output (기본 PHP 출력)

This section demonstrates fundamental PHP output operations. You will learn how to declare and display variables, work with arrays, and inspect variable information using debugging functions.

이 섹션은 기본적인 PHP 출력 작업을 보여줍니다. 변수를 선언하고 표시하는 방법, 배열로 작업하는 방법, 디버깅 함수를 사용하여 변수 정보를 검사하는 방법을 배울 것입니다.

```php
<?php
// Declare and display variables (변수 선언 및 출력)
$name = "John Smith";
$age = 25;

echo "Name: " . $name . "<br>";
echo "Age: " . $age . "<br>";

// Work with arrays (배열 사용)
$fruits = ["Apple", "Banana", "Orange"];
echo $fruits[0];  // Apple

// Use var_dump() to inspect variables (var_dump()로 변수 정보 확인)
var_dump($name);  // string(10) "John Smith"
?>
```

### 4-3 PHP and HTML Integration (PHP와 HTML 혼합)

Combining PHP with HTML allows you to create dynamic web pages. PHP generates the HTML content dynamically, making it possible to display different information based on data stored in your application. This example demonstrates how to loop through a data array and generate table rows dynamically.

PHP를 HTML과 결합하면 동적 웹 페이지를 만들 수 있습니다. PHP는 HTML 콘텐츠를 동적으로 생성하여 애플리케이션에 저장된 데이터를 기반으로 다양한 정보를 표시할 수 있습니다. 이 예제는 데이터 배열을 반복하고 테이블 행을 동적으로 생성하는 방법을 보여줍니다.

**File name: student_list.php**

```php
<?php
// Student data array 
$students = [
    ['name' => 'John Smith', 'score' => 85],
    ['name' => 'Mary Johnson', 'score' => 92],
    ['name' => 'Michael Brown', 'score' => 88]
];
?>

<!DOCTYPE html>
<html>
<head>
    <title>Student Grades</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        table { border-collapse: collapse; width: 100%; }
        th, td { border: 1px solid #ddd; padding: 8px; text-align: center; }
        th { background: navy; color: white; }
    </style>
</head>
<body>

<h2>📊 Student Grade Management </h2>

<table>
    <thead>
        <tr><th>Name</th><th>Score</th></tr>
    </thead>
    <tbody>
        <!-- Loop through student array and display each row -->
        <?php foreach ($students as $student): ?>
            <tr>
                <td><?php echo $student['name']; ?></td>
                <td><?php echo $student['score']; ?></td>
            </tr>
        <?php endforeach; ?>
    </tbody>
</table>

</body>
</html>
```

---

## 5️⃣ Database Management with MySQL Workbench (MySQL Workbench를 통한 데이터베이스 관리)

### 5-1 MySQL Workbench Installation and Connection (설치 및 연결)

MySQL Workbench is a visual database design and management tool. It provides a graphical interface for creating and managing databases, tables, and data. Unlike command-line interfaces, Workbench allows you to interact with databases using a visual approach.

MySQL Workbench는 시각적 데이터베이스 디자인 및 관리 도구입니다. 데이터베이스, 테이블 및 데이터를 생성하고 관리하기 위한 그래픽 인터페이스를 제공합니다. 명령줄 인터페이스와 달리 Workbench는 시각적 방식을 사용하여 데이터베이스와 상호 작용할 수 있습니다.

#### **Installation and Connection Setup (설치 및 연결 설정)**

**Installation:**

1. Download MySQL Workbench from the official MySQL website
2. Install the application
3. Launch MySQL Workbench

다운로드: MySQL 공식 웹사이트에서 MySQL Workbench를 다운로드합니다.
설치: 애플리케이션을 설치합니다.
실행: MySQL Workbench를 실행합니다.

**Connection Configuration (연결 설정):**

1. Click "MySQL Connections"
2. Click the "+" button to add a new connection
3. Enter connection settings:
   - Connection Name: `XAMPP Local`
   - Hostname: `localhost`
   - Port: `3306`
   - Username: `root`
   - Password: (empty by default, or your custom password)
4. Click "Test Connection" to verify

"MySQL Connections" 클릭
"+" 버튼으로 새 연결 추가
설정값 입력:

- Connection Name: `XAMPP Local`
- Hostname: `localhost`
- Port: `3306`
- Username: `root`
- Password: (기본값: 공란)
  "Test Connection" 클릭하여 확인

### 5-2 Creating Databases and Working with Tables (데이터베이스 생성 및 테이블 작업)

#### **Creating a Database (데이터베이스 생성)**

Database creation is the first step in setting up your data structure. Specify character encoding to ensure proper handling of international characters and special symbols.

데이터베이스 생성은 데이터 구조를 설정하는 첫 번째 단계입니다. 국제 문자 및 특수 기호를 올바르게 처리하도록 문자 인코딩을 지정합니다.

Enter the following code in the SQL editor and execute it:

SQL 창에서 다음 코드를 입력하고 실행합니다:

```sql
-- Create database with UTF-8 encoding (UTF-8 인코딩으로 데이터베이스 생성)
CREATE DATABASE my_database CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
USE my_database;
```

#### **Creating a Table (테이블 생성)**

Tables organize data into rows and columns. Each column has a specific data type and constraints. The PRIMARY KEY ensures each row is uniquely identifiable, and AUTO_INCREMENT automatically generates unique ID values.

테이블은 데이터를 행과 열로 조직화합니다. 각 열은 특정 데이터 유형과 제약 조건을 가집니다. PRIMARY KEY는 각 행이 고유하게 식별되도록 보장하고, AUTO_INCREMENT는 고유 ID 값을 자동으로 생성합니다.

```sql
-- Create students table 
CREATE TABLE students (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL,
    age INT,
    email VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### **Inserting Data (데이터 삽입)**

Data insertion adds records to your table. You can insert one or multiple rows at a time using the INSERT statement.

데이터 삽입은 테이블에 레코드를 추가합니다. INSERT 문을 사용하여 한 번에 하나 또는 여러 행을 삽입할 수 있습니다.

```sql
-- Insert multiple student records
INSERT INTO students (name, age, email) VALUES
('John Smith', 25, 'john@example.com'),
('Mary Johnson', 23, 'mary@example.com'),
('Michael Brown', 24, 'michael@example.com');
```

#### **Querying Data (데이터 조회)**

SELECT statements retrieve data from your database. You can select all columns using *, or specific columns by name. The WHERE clause filters results based on conditions.

SELECT 문은 데이터베이스에서 데이터를 검색합니다. *를 사용하여 모든 열을 선택하거나 이름으로 특정 열을 선택할 수 있습니다. WHERE 절은 조건을 기반으로 결과를 필터링합니다.

```sql
-- Select all data
SELECT * FROM students;

-- Select specific columns with condition
SELECT name, email FROM students WHERE age >= 23;
```

#### **Updating Data (데이터 수정)**

UPDATE statements modify existing records. The WHERE clause specifies which records to update, preventing accidental changes to unintended data.

UPDATE 문은 기존 레코드를 수정합니다. WHERE 절은 업데이트할 레코드를 지정하여 의도하지 않은 데이터의 실수로인 변경을 방지합니다.

```sql
-- Update a student's age 
UPDATE students SET age = 26 WHERE name = 'John Smith';
```

#### **Deleting Data (데이터 삭제)**

DELETE statements remove records from your table. Always use a WHERE clause to specify which records to delete to avoid accidentally deleting all data.

DELETE 문은 테이블에서 레코드를 제거합니다. 실수로 모든 데이터를 삭제하지 않도록 항상 WHERE 절을 사용하여 삭제할 레코드를 지정하세요.

```sql
-- Delete a student record 
DELETE FROM students WHERE id = 1;
```

## ✅ Practice Assignments (연습과제)

### Assignment 1: Setting Up Your Local Environment (과제 1: 로컬 환경 구축)

Complete the following tasks to verify that your development environment is properly configured:

다음 작업을 완료하여 개발 환경이 올바르게 구성되었음을 확인하세요:

1. Verify Apache is running by visiting `http://localhost` - you should see the XAMPP home page
2. Create and run your first PHP file (`hello.php`)
3. Verify MySQL Workbench connection to your local MySQL server
4. Create a database named `test_db` using MySQL Workbench

Apache 실행 확인 (`http://localhost`)
`hello.php` 생성 및 실행
MySQL Workbench 연결 확인
MySQL Workbench에서 `test_db` 데이터베이스 생성

### Assignment 2: Completing Practical Examples (과제 2: 실습 예제 완성)

Complete the following practical exercises:

다음 실습 연습을 완료하세요:

1. Create all three JavaScript example files (`form_practice.html`, `button_practice.html`, `input_practice.html`) and test them in your browser
2. Create and run the `student_list.php` file
3. Use MySQL Workbench to:
   - Create a `students` table with the specified structure
   - Insert at least 5 student records
   - Execute various SELECT queries to retrieve and filter data

JavaScript 3개 예제 파일 생성 및 실행
`student_list.php` 생성 및 실행
MySQL Workbench에서:

- `students` 테이블 생성
- 5명 이상의 학생 정보 삽입
- 다양한 SELECT 쿼리 실행

---

Thank you for your attention.   
Cho Jeonghyun ([peterchokr@gmail.com](mailto:peterchokr@gmail.com)). Yeungnam University College

"Produced in collaboration with Claude and Gemini."
