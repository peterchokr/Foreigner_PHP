# Chapter 3. Control Structures, Loops, and Functions

---

## 📚 Learning Objectives

After completing this chapter, you will be able to:

✅ Control program flow using conditional statements
✅ Perform repetitive tasks using loops
✅ Define and call functions to reuse code
✅ Create functions with parameters and return values
✅ Use PHP built-in functions
✅ Implement real-world logic like user registration validation

이 장을 학습하면 다음을 할 수 있습니다:

✅ 조건문으로 프로그램의 흐름을 제어할 수 있습니다.
✅ 반복문으로 같은 작업을 여러 번 수행할 수 있습니다.
✅ 함수를 정의하고 호출하여 코드를 재사용할 수 있습니다.
✅ 매개변수와 반환값을 이용한 함수를 만들 수 있습니다.
✅ PHP의 내장 함수들을 활용할 수 있습니다.
✅ 회원가입 유효성 검사 같은 실무 로직을 구현할 수 있습니다.

---

## 1️⃣ Conditional Statements (조건문)

### 1-1 If / Else / Elseif Statements (if / else / elseif 문)

Conditional statements allow your program to make decisions based on whether conditions are true or false. By checking conditions, you can execute different code paths depending on the situation. This is fundamental to creating responsive and dynamic programs.

조건문은 프로그램이 조건의 참/거짓 여부에 따라 결정을 내릴 수 있도록 합니다. 조건을 확인함으로써 상황에 따라 다른 코드 경로를 실행할 수 있습니다. 이것은 반응형이고 동적인 프로그램을 만드는 데 기본입니다.

#### **Basic Structure (기본 구조)**

```php
<?php

// ============================================
// 1️⃣ if statement - Execute if condition is true (조건이 참이면 실행)
// ============================================

$age = 20;

if ($age >= 18) {
    echo "You are an adult<br>";  // 이 코드가 실행됨
}

// ============================================
// 2️⃣ else statement - Execute if condition is false (조건이 거짓이면 실행)
// ============================================

$age = 15;

if ($age >= 18) {
    echo "You are an adult";
} else {
    echo "You are a minor<br>";  // 이 코드가 실행됨
}

// ============================================
// 3️⃣ elseif statement - Check multiple conditions (여러 조건 확인)
// ============================================

$score = 85;

if ($score >= 90) {
    echo "Grade A";
} elseif ($score >= 80) {
    echo "Grade B<br>";  // 이 코드가 실행됨
} elseif ($score >= 70) {
    echo "Grade C";
} else {
    echo "Grade F";
}

// ============================================
// 📝 Important: Order of elseif (elseif의 순서)
// ============================================

/*
Check conditions in order from first.
If condition is true, execute that code and 
ignore the rest.

In the above example, score = 85:
- 85 >= 90? NO (check)
- 85 >= 80? YES (execute) → print "Grade B", done!
- Rest are not executed
*/

?>
```

#### **Nested Conditional Statements (중첩된 조건문)**

Nesting conditional statements allows you to check multiple conditions in sequence. However, you can often simplify nested conditions using logical operators like && (AND), making code more readable.

조건문을 중첩하면 여러 조건을 차례로 확인할 수 있습니다. 그러나 && (AND)와 같은 논리 연산자를 사용하여 중첩된 조건을 단순화하면 코드가 더 읽기 쉬워집니다.

```php
<?php

// ============================================
// You can nest multiple conditional statements (조건문을 여러 개 중첩할 수 있습니다)
// ============================================

$age = 20;
$has_license = true;

if ($age >= 18) {
    if ($has_license) {
        echo "You can drive<br>";  // 이것이 실행됨
    } else {
        echo "You need a license";
    }
} else {
    echo "You are not an adult";
}

// ============================================
// Using logical operators - cleaner (논리 연산자 사용 (더 깔끔함))
// ============================================

$age = 20;
$has_license = true;

if ($age >= 18 && $has_license) {
    echo "You can drive";
}

?>
```

### 1-2 Switch Statement (switch 문)

The switch statement is used when checking if a single variable matches one of several values. It's often more readable than multiple if/elseif statements, especially when comparing a variable against many specific values.

switch 문은 하나의 변수가 여러 값 중 하나와 일치하는지 확인할 때 사용됩니다. 특히 변수를 많은 특정 값과 비교할 때 여러 if/elseif 문보다 읽기 쉬운 경우가 많습니다.

#### **When to Use Switch (언제 사용?)**

Use switch when you need to check if one variable equals one of several specific values. Compare multiple if/elseif statements with a single switch statement to see which is more readable in your situation.

하나의 변수가 여러 특정 값 중 하나와 같은지 확인해야 할 때 switch를 사용합니다. 여러 if/elseif 문과 단일 switch 문을 비교하여 상황에서 더 읽기 쉬운 것이 무엇인지 확인합니다.

```php
<?php

// ============================================
// if/elseif approach (hard to read) (if/elseif 사용 (읽기 어려움))
// ============================================

$day = 3;

if ($day == 1) {
    echo "Monday";
} elseif ($day == 2) {
    echo "Tuesday";
} elseif ($day == 3) {
    echo "Wednesday";
} elseif ($day == 4) {
    echo "Thursday";
} elseif ($day == 5) {
    echo "Friday";
}

// ============================================
// switch approach (easier to read) (switch 사용 (더 읽기 쉬움))
// ============================================

$day = 3;

switch ($day) {
    case 1:
        echo "Monday";
        break;  // Important: prevent fall-through to next case! (중요: 다음 case로 넘어가지 않도록!)
    case 2:
        echo "Tuesday";
        break;
    case 3:
        echo "Wednesday<br>";  // 이것이 실행됨
        break;
    case 4:
        echo "Thursday";
        break;
    case 5:
        echo "Friday";
        break;
    default:  // When no case matches (모든 case에 맞지 않을 때)
        echo "Weekend";
}

// ============================================
// 📝 Importance of break (break의 중요성)
// ============================================

/*
What happens without break?

case 3:
    echo "Wednesday";
    // No break!
case 4:
    echo "Thursday";
    
Result: "WednesdayThursday" is printed!
This is called "fall-through".

Therefore, you must always add break at the end of each case.
(You can intentionally use fall-through if needed)
*/

?>
```

#### **Practice Example: Grade Determination (실습 예제: 학점 판정)**

```php
<?php

$score = 85;

// Determine grade using switch statement (switch 문으로 학점 판정)
switch (true) {
    case $score >= 90:
        $grade = "A";
        $comment = "Excellent";
        break;
    case $score >= 80:
        $grade = "B";
        $comment = "Good";
        break;
    case $score >= 70:
        $grade = "C";
        $comment = "Average";
        break;
    default:
        $grade = "F";
        $comment = "Needs improvement";
}

echo "Grade: " . $grade . "<br>";
echo "Evaluation: " . $comment . "<br>";

?>
```

### 1-3 Ternary Operator (? :) (삼항 연산자)

The ternary operator is a shorthand for if/else statements when the condition is simple. It allows you to write conditional logic in a single line: `condition ? value_if_true : value_if_false`

삼항 연산자는 조건이 간단할 때 if/else 문의 축약형입니다. `condition ? value_if_true : value_if_false` 형식으로 조건문을 한 줄에 쓸 수 있습니다.

#### **When to Use (언제 사용?)**

Use the ternary operator for simple conditions. For complex conditions or multiple statements, use if/else instead for better readability.

간단한 조건에는 삼항 연산자를 사용합니다. 복잡한 조건이나 여러 문장의 경우 가독성을 위해 if/else를 사용합니다.

```php
<?php

// ============================================
// Format: (condition) ? (true value) : (false value) (형식: (조건) ? (참일 때) : (거짓일 때))
// ============================================

$age = 20;

// if/else approach (if/else 방식)
if ($age >= 18) {
    $status = "Adult";
} else {
    $status = "Minor";
}

// Ternary operator approach (더 간결함) (삼항 연산자 방식)
$status = ($age >= 18) ? "Adult" : "Minor";

echo $status . "<br>";  // Adult

// ============================================
// Practical examples (실제 사용 예)
// ============================================

$score = 75;
echo "Result: " . ($score >= 60 ? "Pass" : "Fail");

$is_logged_in = false;
echo $is_logged_in ? "Welcome" : "Please log in";

?>
```

### 1-4 Practice Example: User Validation (실습 예제: 회원 유효성 검사)

**File name: validation.php**

This example demonstrates how to validate user input from a registration form. It checks username length, password strength, and email format using built-in PHP functions.

이 예제는 가입 폼에서 사용자 입력을 검증하는 방법을 보여줍니다. 내장 PHP 함수를 사용하여 사용자명 길이, 비밀번호 강도, 이메일 형식을 확인합니다.

```php
<?php
/**
 * validation.php - User Registration Validation
 * 
 * Purpose: (목적:)
 * 1. Receive user input (사용자 입력 받기)
 * 2. Validate username, password, email (아이디, 비밀번호, 이메일 검사)
 * 3. Display validation results (검사 결과 표시)
 */

// ============================================
// Initialize input data (입력 데이터 초기화)
// ============================================

$username = isset($_POST['username']) ? trim($_POST['username']) : '';
$password = isset($_POST['password']) ? $_POST['password'] : '';
$password_confirm = isset($_POST['password_confirm']) ? $_POST['password_confirm'] : '';
$email = isset($_POST['email']) ? trim($_POST['email']) : '';

$errors = array();
$is_valid = true;

// ============================================
// Validation logic (유효성 검사 로직)
// ============================================

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    
    // 1️⃣ Check username (아이디 검사)
    if (empty($username)) {
        $errors[] = "Enter username";
        $is_valid = false;
    } elseif (strlen($username) < 5) {
        $errors[] = "Username must be 5 or more characters";
        $is_valid = false;
    } elseif (strlen($username) > 20) {
        $errors[] = "Username must be 20 characters or less";
        $is_valid = false;
    }
    
    // 2️⃣ Check password (비밀번호 검사)
    if (empty($password)) {
        $errors[] = "Enter password";
        $is_valid = false;
    } elseif (strlen($password) < 6) {
        $errors[] = "Password must be 6 or more characters";
        $is_valid = false;
    }
    
    // 3️⃣ Confirm password (비밀번호 확인)
    if ($password !== $password_confirm) {
        $errors[] = "Passwords do not match";
        $is_valid = false;
    }
    
    // 4️⃣ Check email (이메일 검사)
    if (empty($email)) {
        $errors[] = "Enter email";
        $is_valid = false;
    } elseif (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
        // Use PHP built-in function to validate email format (PHP 내장 함수로 이메일 형식 검사)
        $errors[] = "Invalid email format";
        $is_valid = false;
    }
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>User Registration (회원가입)</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 500px;
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
        
        .form-group {
            margin: 15px 0;
        }
        
        label {
            display: block;
            font-weight: bold;
            margin-bottom: 5px;
        }
        
        input {
            width: 100%;
            padding: 8px;
            border: 1px solid #ddd;
            box-sizing: border-box;
        }
        
        button {
            width: 100%;
            padding: 10px;
            background-color: navy;
            color: white;
            border: none;
            margin-top: 15px;
            cursor: pointer;
        }
        
        .error-list {
            background-color: #ffebee;
            color: #c62828;
            padding: 15px;
            border-left: 4px solid #c62828;
            margin: 15px 0;
        }
        
        .error-list li {
            margin: 5px 0;
        }
        
        .success-message {
            background-color: #e8f5e9;
            color: #2e7d32;
            padding: 15px;
            border-left: 4px solid #2e7d32;
            margin: 15px 0;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>📝 User Registration</h1>
    
    <!-- Display error messages (오류 메시지 표시) -->
    <?php if (!empty($errors)): ?>
        <div class="error-list">
            <strong>❌ Validation Error:</strong>
            <ul>
                <?php foreach ($errors as $error): ?>
                    <li><?php echo htmlspecialchars($error); ?></li>
                <?php endforeach; ?>
            </ul>
        </div>
    <?php endif; ?>
    
    <!-- Display success message (성공 메시지 표시) -->
    <?php if ($is_valid && $_SERVER['REQUEST_METHOD'] === 'POST'): ?>
        <div class="success-message">
            <strong>✅ Registration successful!</strong><br>
            Welcome, <?php echo htmlspecialchars($username); ?>!
        </div>
    <?php endif; ?>
    
    <!-- Registration form (가입 폼) -->
    <form method="POST">
        <div class="form-group">
            <label for="username">Username: (아이디:)</label>
            <input type="text" id="username" name="username" 
                   value="<?php echo htmlspecialchars($username); ?>" required>
        </div>
        
        <div class="form-group">
            <label for="password">Password: (비밀번호:)</label>
            <input type="password" id="password" name="password" required>
        </div>
        
        <div class="form-group">
            <label for="password_confirm">Confirm Password: (비밀번호 확인:)</label>
            <input type="password" id="password_confirm" name="password_confirm" required>
        </div>
        
        <div class="form-group">
            <label for="email">Email: (이메일:)</label>
            <input type="email" id="email" name="email" 
                   value="<?php echo htmlspecialchars($email); ?>" required>
        </div>
        
        <button type="submit">Register (가입하기)</button>
    </form>
</div>

</body>
</html>
```

---

## 2️⃣ Loops (반복문)

Loops allow you to execute the same code multiple times. PHP provides several types of loops for different situations. Choosing the right loop for your situation makes code more readable and efficient.

반복문을 사용하면 같은 코드를 여러 번 실행할 수 있습니다. PHP는 다양한 상황에 대해 여러 유형의 루프를 제공합니다. 상황에 맞는 올바른 루프를 선택하면 코드가 더 읽기 쉽고 효율적입니다.

### 2-1 For Loop (for 루프)

The for loop is ideal when you know exactly how many times you want to repeat code. It consists of three parts: initialization, condition, and increment. This makes it perfect for iterating through arrays using numeric indices.

for 루프는 코드를 정확히 몇 번 반복해야 하는지 알고 있을 때 이상적입니다. 초기화, 조건, 증가의 세 부분으로 구성됩니다. 이것은 숫자 인덱스를 사용하여 배열을 반복하기에 완벽합니다.

```php
<?php

// ============================================
// Basic for loop (기본 for 루프)
// ============================================

for ($i = 0; $i < 5; $i++) {
    echo "Number: " . $i . "<br>";  // Prints 0 to 4
}

// ============================================
// Loop through array with numeric index (숫자 인덱스로 배열 반복)
// ============================================

$fruits = array("Apple", "Banana", "Orange");

for ($i = 0; $i < count($fruits); $i++) {
    echo $fruits[$i] . "<br>";
}

?>
```

### 2-2 Foreach Loop (foreach 루프)

The foreach loop is the most convenient way to loop through arrays, especially when you need the values. It automatically gets each element without needing to manage array indices. For associative arrays, you can also get both keys and values.

foreach 루프는 배열을 반복하는 가장 편리한 방법이며, 특히 값이 필요할 때 그렇습니다. 배열 인덱스를 관리할 필요 없이 각 요소를 자동으로 가져옵니다. 연관 배열의 경우 키와 값을 모두 얻을 수 있습니다.

```php
<?php

// ============================================
// Loop through simple array (단순 배열 반복)
// ============================================

$fruits = array("Apple", "Banana", "Orange");

foreach ($fruits as $fruit) {
    echo $fruit . "<br>";
}

// ============================================
// Loop through associative array (연관 배열 반복)
// ============================================

$person = array(
    "name" => "John Smith",
    "age" => 25,
    "email" => "john@example.com"
);

foreach ($person as $key => $value) {
    echo $key . ": " . $value . "<br>";
}

?>
```

### 2-3 While and Do-While Loops (while / do-while 루프)

While loops continue executing as long as a condition is true. The do-while loop is a variation that always executes at least once before checking the condition. Use these loops when you don't know exactly how many iterations you need.

while 루프는 조건이 참인 동안 계속 실행됩니다. do-while 루프는 조건을 확인하기 전에 항상 최소 한 번 실행하는 변형입니다. 정확히 몇 번의 반복이 필요한지 모를 때 이 루프들을 사용합니다.

```php
<?php

// ============================================
// while loop (while 루프)
// ============================================

$count = 0;

while ($count < 5) {
    echo "Count: " . $count . "<br>";
    $count++;
}

// ============================================
// do-while loop (do-while 루프)
// Execute at least once, then check condition (최소 한 번은 실행, 그 다음 조건 확인)
// ============================================

$count = 0;

do {
    echo "Count: " . $count . "<br>";
    $count++;
} while ($count < 5);

?>
```

### 2-4 Break and Continue (break와 continue)

The break statement exits a loop immediately, while continue skips the rest of the current iteration and goes to the next one. These statements give you fine control over loop execution.

break 문은 루프를 즉시 종료하고, continue는 현재 반복의 나머지를 건너뛰고 다음 반복으로 갑니다. 이 문장들은 루프 실행에 대한 세밀한 제어를 제공합니다.

```php
<?php

// ============================================
// break - exit loop (루프 종료)
// ============================================

for ($i = 0; $i < 10; $i++) {
    if ($i === 5) {
        break;  // Exit loop when i equals 5 (i가 5일 때 루프 종료)
    }
    echo $i . " ";
}
// Result: 0 1 2 3 4

// ============================================
// continue - skip to next iteration (다음 반복으로 스킵)
// ============================================

for ($i = 0; $i < 5; $i++) {
    if ($i === 2) {
        continue;  // Skip when i equals 2 (i가 2일 때 건너뜀)
    }
    echo $i . " ";
}
// Result: 0 1 3 4

?>
```

### 2-5 Practice Example: Multiplication Table (실습 예제: 구구단 출력 프로그램)

**File name: multiplication_table.php**

```php
<?php

// ============================================
// Display multiplication table (구구단 출력)
// ============================================

$table = isset($_GET['table']) ? intval($_GET['table']) : 2;

// Validate input (입력값 검증)
if ($table < 2 || $table > 9) {
    $table = 2;
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>Multiplication Table (구구단)</title>
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
            text-align: center;
        }
        
        .selector {
            text-align: center;
            margin: 20px 0;
        }
        
        select, button {
            padding: 8px 15px;
            font-size: 16px;
            margin: 0 5px;
        }
        
        .table-container {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 20px;
            margin-top: 30px;
        }
        
        .table-box {
            border: 1px solid #2196F3;
            padding: 15px;
            border-radius: 5px;
        }
        
        .table-box h2 {
            color: #2196F3;
            text-align: center;
            margin-top: 0;
        }
        
        .table-box p {
            margin: 8px 0;
            font-size: 16px;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>📊 Multiplication Table (구구단)</h1>
    
    <div class="selector">
        <form method="GET">
            <label for="table">Select table: (구구단 선택:)</label>
            <select id="table" name="table">
                <?php
                // Generate options for tables 2-9 (2단부터 9단까지 옵션 생성)
                for ($i = 2; $i <= 9; $i++) {
                    $selected = ($i === $table) ? 'selected' : '';
                    echo "<option value='$i' $selected>" . $i . " × (곱하기)</option>";
                }
                ?>
            </select>
            <button type="submit">Display (표시)</button>
        </form>
    </div>
    
    <!-- Display single table (단일 구구단 표시) -->
    <div class="table-box" style="grid-column: 1 / -1;">
        <h2><?php echo $table; ?> × Table (곱셈표)</h2>
        <?php
        // Display multiplication table (구구단 출력)
        for ($i = 1; $i <= 9; $i++) {
            $result = $table * $i;
            echo "<p>" . $table . " × " . $i . " = " . $result . "</p>";
        }
        ?>
    </div>
    
    <!-- Display all tables (모든 구구단 표시) -->
    <h2 style="grid-column: 1 / -1; margin-top: 30px;">All Multiplication Tables (전체 구구단)</h2>
    
    <div class="table-container">
        <?php
        // Nested loop to display all multiplication tables (중첩 루프로 모든 구구단 표시)
        for ($table_num = 2; $table_num <= 9; $table_num++) {
            echo "<div class='table-box'>";
            echo "<h2>" . $table_num . " × Table</h2>";
            
            for ($i = 1; $i <= 9; $i++) {
                $result = $table_num * $i;
                echo "<p>" . $table_num . " × " . $i . " = " . $result . "</p>";
            }
            
            echo "</div>";
        }
        ?>
    </div>
</div>

</body>
</html>
```

---

## 3️⃣ Functions (함수)

Functions are reusable blocks of code that perform specific tasks. They reduce code duplication and make programs more organized and maintainable. A well-designed function should have a single, clear purpose.

함수는 특정 작업을 수행하는 재사용 가능한 코드 블록입니다. 코드 중복을 줄이고 프로그램을 더 체계적이고 유지보수하기 쉽게 만듭니다. 잘 설계된 함수는 단일하고 명확한 목적을 가져야 합니다.

### 3-1 Function Basics (함수의 기본)

Functions consist of declaration (defining) and calling (using). When you define a function, you specify what it does. When you call it, the function executes. Parameters allow functions to work with different data.

함수는 선언(정의)과 호출(사용)로 구성됩니다. 함수를 정의할 때 그것이 무엇을 하는지 지정합니다. 호출할 때 함수가 실행됩니다. 매개변수를 사용하면 함수가 다양한 데이터로 작동할 수 있습니다.

```php
<?php

// ============================================
// Function definition (함수 정의)
// ============================================

function greet($name) {
    echo "Hello, " . $name . "!<br>";
}

// Function call (함수 호출)
greet("John Smith");      // Hello, John Smith!
greet("Mary Johnson");     // Hello, Mary Johnson!

// ============================================
// Function with return value (반환값이 있는 함수)
// ============================================

function add($a, $b) {
    return $a + $b;
}

$result = add(10, 20);
echo "Result: " . $result . "<br>";  // Result: 30

// ============================================
// Function with multiple parameters (여러 매개변수를 가진 함수)
// ============================================

function calculate_average($num1, $num2, $num3) {
    $sum = $num1 + $num2 + $num3;
    $average = $sum / 3;
    return $average;
}

$avg = calculate_average(80, 90, 85);
echo "Average: " . $avg . "<br>";  // Average: 85

// ============================================
// Default parameter values (기본 매개변수 값)
// ============================================

function greet_with_greeting($name, $greeting = "Hello") {
    echo $greeting . ", " . $name . "!<br>";
}

greet_with_greeting("John Smith");               // Hello, John Smith!
greet_with_greeting("Mary Johnson", "Hi");       // Hi, Mary Johnson!

?>
```

### 3-2 Function Scope (함수 스코프)

Scope determines where variables can be accessed. Variables defined inside a function are local to that function. Variables defined outside functions are global. Understanding scope prevents unexpected behavior in your code.

스코프는 변수에 어디서 액세스할 수 있는지 결정합니다. 함수 내부에 정의된 변수는 그 함수의 로컬 변수입니다. 함수 외부에 정의된 변수는 전역 변수입니다. 스코프를 이해하면 코드의 예상치 못한 동작을 방지합니다.

```php
<?php

// ============================================
// Local scope (로컬 스코프)
// ============================================

function test_local() {
    $local_var = "I'm local";
    echo $local_var . "<br>";  // Works (작동함)
}

test_local();
// echo $local_var;  // Error! Variable not defined (오류! 변수가 정의되지 않음)

// ============================================
// Global scope (전역 스코프)
// ============================================

$global_var = "I'm global";

function test_global() {
    global $global_var;  // Access global variable (전역 변수 액세스)
    echo $global_var . "<br>";  // Works (작동함)
}

test_global();

// ============================================
// Static variables (정적 변수)
// Retain value between function calls (함수 호출 간 값 유지)
// ============================================

function counter() {
    static $count = 0;
    $count++;
    echo "Count: " . $count . "<br>";
}

counter();  // Count: 1
counter();  // Count: 2
counter();  // Count: 3

?>
```

### 3-3 Recursive Functions (재귀 함수)

A recursive function calls itself to solve a problem by breaking it down into smaller subproblems. Recursion is useful for tasks like traversing tree structures or calculating factorials. Always ensure your recursive function has a base case to prevent infinite recursion.

재귀 함수는 문제를 더 작은 부분 문제로 분해하여 자신을 호출합니다. 재귀는 트리 구조를 순회하거나 팩토리얼을 계산하는 작업에 유용합니다. 항상 무한 재귀를 방지하기 위해 재귀 함수에 기본 사례가 있는지 확인합니다.

```php
<?php

// ============================================
// Recursive function - calculate factorial (재귀 함수 - 팩토리얼 계산)
// ============================================

function factorial($n) {
    // Base case (기본 사례)
    if ($n === 1) {
        return 1;
    }
    
    // Recursive case (재귀 사례)
    return $n * factorial($n - 1);
}

echo factorial(5) . "<br>";  // 120 (5 × 4 × 3 × 2 × 1)

// ============================================
// Recursive function - sum of array elements (배열 요소의 합)
// ============================================

function sum_array($arr, $index = 0) {
    // Base case (기본 사례)
    if ($index === count($arr)) {
        return 0;
    }
    
    // Recursive case (재귀 사례)
    return $arr[$index] + sum_array($arr, $index + 1);
}

$numbers = array(1, 2, 3, 4, 5);
echo sum_array($numbers) . "<br>";  // 15

?>
```

#### **More Recursive Examples (추가 재귀 예제)**

```php
<?php

// ============================================
// Recursive function - search directory (재귀적 디렉토리 검색)
// ============================================

function count_files($directory) {
    // Base case (기본 사례)
    if (!is_dir($directory)) {
        return 0;
    }
    
    $count = 0;
    $files = scandir($directory);
    
    // Recursive case (재귀 사례)
    foreach ($files as $file) {
        if ($file !== '.' && $file !== '..') {
            $path = $directory . '/' . $file;
            if (is_file($path)) {
                $count++;
            } elseif (is_dir($path)) {
                $count += count_files($path);  // Recursive call (재귀 호출)
            }
        }
    }
    
    return $count;
}

// Usage (사용법)
// $total_files = count_files('./uploads');
// echo "Total files: " . $total_files;

?>
```

### 3-4 Practice Example: Practical Functions (실습 예제: 실무 함수들)

```php
<?php

// ============================================
// Function: Validate email (이메일 검증)
// ============================================

function is_valid_email($email) {
    return filter_var($email, FILTER_VALIDATE_EMAIL) !== false;
}

// Usage (사용법)
if (is_valid_email("john@example.com")) {
    echo "Valid email<br>";
}

// ============================================
// Function: Generate random password (무작위 비밀번호 생성)
// ============================================

function generate_password($length = 8) {
    $characters = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
    $password = "";
    
    for ($i = 0; $i < $length; $i++) {
        $password .= $characters[rand(0, strlen($characters) - 1)];
    }
    
    return $password;
}

// Usage (사용법)
echo "Generated password: " . generate_password(12) . "<br>";

// ============================================
// Function: Calculate percentage (퍼센티지 계산)
// ============================================

function calculate_percentage($value, $total) {
    if ($total === 0) {
        return 0;
    }
    return round(($value / $total) * 100, 2);
}

// Usage (사용법)
$percentage = calculate_percentage(25, 100);
echo "Percentage: " . $percentage . "%<br>";

// ============================================
// Function: Format currency (통화 형식)
// ============================================

function format_currency($amount) {
    return "$" . number_format($amount, 2);
}

// Usage (사용법)
echo "Price: " . format_currency(1234.5) . "<br>";

?>
```

---

## 4️⃣ Built-in Functions (내장 함수)

PHP provides numerous built-in functions for common tasks like manipulating strings, working with arrays, and performing mathematical operations. Learning to use these functions efficiently makes your code cleaner and more powerful.

PHP는 문자열 조작, 배열 작업, 수학 연산 등 일반적인 작업을 위한 수많은 내장 함수를 제공합니다. 이러한 함수를 효율적으로 사용하면 코드가 더 깔끔하고 강력해집니다.

### 4-1 String Functions (문자열 함수)

```php
<?php

// ============================================
// strlen() - String length (문자열 길이)
// ============================================

echo strlen("Hello") . "<br>";  // 5

// ============================================
// strtoupper() / strtolower() - Change case (대소문자 변경)
// ============================================

echo strtoupper("hello") . "<br>";  // HELLO
echo strtolower("HELLO") . "<br>";  // hello

// ============================================
// trim() - Remove whitespace (공백 제거)
// ============================================

$text = "  hello world  ";
echo trim($text) . "<br>";  // hello world

// ============================================
// str_replace() - Replace string (문자열 바꾸기)
// ============================================

$text = "I like apple";
echo str_replace("apple", "orange", $text) . "<br>";  // I like orange

?>
```

### 4-2 Array Functions (배열 함수)

```php
<?php

// ============================================
// count() - Array size (배열 크기)
// ============================================

$fruits = array("Apple", "Banana", "Orange");
echo count($fruits) . "<br>";  // 3

// ============================================
// array_push() - Add element to array (배열에 요소 추가)
// ============================================

$fruits = array("Apple", "Banana");
array_push($fruits, "Orange");
// Result: array("Apple", "Banana", "Orange")

// ============================================
// array_pop() - Remove last element (마지막 요소 제거)
// ============================================

$fruits = array("Apple", "Banana", "Orange");
$last = array_pop($fruits);
// $last = "Orange"
// $fruits = array("Apple", "Banana")

// ============================================
// in_array() - Check if value exists (값이 존재하는지 확인)
// ============================================

$fruits = array("Apple", "Banana", "Orange");
if (in_array("Banana", $fruits)) {
    echo "Banana exists<br>";
}

// ============================================
// array_merge() - Merge arrays (배열 병합)
// ============================================

$arr1 = array("Apple", "Banana");
$arr2 = array("Orange", "Grape");
$merged = array_merge($arr1, $arr2);
// Result: array("Apple", "Banana", "Orange", "Grape")

// ============================================
// array_keys() - Get all keys (모든 키 반환)
// ============================================

$student = array("name" => "John Smith", "age" => 20, "grade" => "A");
$keys = array_keys($student);
// Result: array("name", "age", "grade")

?>
```

### 4-3 Math Functions (수학 함수)

```php
<?php

// ============================================
// abs() - Absolute value (절댓값)
// ============================================

echo abs(-10) . "<br>";  // 10
echo abs(10) . "<br>";   // 10

// ============================================
// round() - Round number (반올림)
// ============================================

echo round(3.7) . "<br>";         // 4
echo round(3.14159, 2) . "<br>";  // 3.14

// ============================================
// max() / min() - Maximum/Minimum value (최댓값/최솟값)
// ============================================

echo max(10, 20, 30) . "<br>";  // 30
echo min(10, 20, 30) . "<br>";  // 10

// ============================================
// pow() - Exponentiation (거듭제곱)
// ============================================

echo pow(2, 3) . "<br>";  // 8 (2의 3제곱)

// ============================================
// sqrt() - Square root (제곱근)
// ============================================

echo sqrt(16) . "<br>";  // 4

?>
```

### 4-4 Practice Example: Student Grade Management (실습 예제: 학생 성적 처리)

**File name: student_management.php**

```php
<?php
/**
 * student_management.php - Student Grade Management
 * 
 * Purpose: (목적:)
 * 1. Store student data (학생 데이터 저장)
 * 2. Filter students by criteria (조건에 맞는 학생 필터링)
 * 3. Calculate statistics (통계 정보 계산)
 */

// Student data (학생 데이터)
$students = array(
    array("name" => "John Smith", "score" => 85),
    array("name" => "Mary Johnson", "score" => 92),
    array("name" => "Michael Brown", "score" => 78),
    array("name" => "Sarah Davis", "score" => 88),
    array("name" => "Robert Wilson", "score" => 95)
);

// ============================================
// Function: Calculate average score (평균 계산)
// ============================================

function get_average_score($students) {
    $total = 0;
    foreach ($students as $student) {
        $total += $student["score"];
    }
    return round($total / count($students), 2);
}

// ============================================
// Function: Find top student (최고 점수 학생 찾기)
// ============================================

function find_top_student($students) {
    $top = $students[0];
    foreach ($students as $student) {
        if ($student["score"] > $top["score"]) {
            $top = $student;
        }
    }
    return $top;
}

// ============================================
// Function: Filter by score (성적으로 필터링)
// ============================================

function filter_by_score($students, $min_score) {
    $filtered = array();
    foreach ($students as $student) {
        if ($student["score"] >= $min_score) {
            $filtered[] = $student;
        }
    }
    return $filtered;
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>Student Grade Management (학생 성적 관리)</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 900px;
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
        
        .section {
            margin: 20px 0;
            padding: 15px;
            background-color: #f9f9f9;
            border-left: 4px solid #2196F3;
        }
        
        .section h2 {
            color: #2196F3;
            margin-top: 0;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
            margin: 10px 0;
        }
        
        th {
            background-color: #2196F3;
            color: white;
            padding: 10px;
            text-align: left;
        }
        
        td {
            padding: 10px;
            border-bottom: 1px solid #ddd;
        }
        
        .stat {
            padding: 10px;
            background-color: #fff;
            border: 1px solid #ddd;
            margin: 5px 0;
        }
        
        .stat-value {
            color: #2196F3;
            font-weight: bold;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>📊 Student Grade Management (학생 성적 관리)</h1>
    
    <!-- All student grades -->
    <div class="section">
        <h2>1️⃣ All Student Grades (전체 학생 성적)</h2>
        <table>
            <thead>
                <tr>
                    <th>Name (이름)</th>
                    <th>Score (점수)</th>
                    <th>Grade (등급)</th>
                </tr>
            </thead>
            <tbody>
                <?php
                // Display each student (각 학생 표시)
                foreach ($students as $student) {
                    $grade = ($student["score"] >= 90) ? "A" : 
                             (($student["score"] >= 80) ? "B" : "C");
                    echo "<tr>";
                    echo "<td>" . $student["name"] . "</td>";
                    echo "<td>" . $student["score"] . "</td>";
                    echo "<td>" . $grade . "</td>";
                    echo "</tr>";
                }
                ?>
            </tbody>
        </table>
    </div>
    
    <!-- Statistics -->
    <div class="section">
        <h2>2️⃣ Grade Statistics (성적 통계)</h2>
        <?php
        $average = get_average_score($students);
        $top_student = find_top_student($students);
        
        echo "<div class='stat'>";
        echo "<span>Average Score: </span>";
        echo "<span class='stat-value'>" . $average . " points</span>";
        echo "</div>";
        
        echo "<div class='stat'>";
        echo "<span>Highest Score: </span>";
        echo "<span class='stat-value'>" . $top_student["name"] . " (" . $top_student["score"] . " points)</span>";
        echo "</div>";
        ?>
    </div>
    
    <!-- Top students (80 points or above) -->
    <div class="section">
        <h2>3️⃣ Top Students - 80+ Points (우등생)</h2>
        <?php
        $top_students = filter_by_score($students, 80);
        ?>
        <table>
            <thead>
                <tr>
                    <th>Name (이름)</th>
                    <th>Score (점수)</th>
                </tr>
            </thead>
            <tbody>
                <?php
                if (count($top_students) > 0) {
                    foreach ($top_students as $student) {
                        echo "<tr>";
                        echo "<td>" . $student["name"] . "</td>";
                        echo "<td>" . $student["score"] . "</td>";
                        echo "</tr>";
                    }
                } else {
                    echo "<tr><td colspan='2'>No top students</td></tr>";
                }
                ?>
            </tbody>
        </table>
    </div>
</div>

</body>
</html>
```

---

## ✅ Practice Assignments (연습과제)

### Assignment 1: Filter Students by Criteria (과제 1: 조건을 만족하는 학생 필터링)

Implement the following:

다음을 구현하세요:

1. **Create student array** (학생 배열 생성)
   - Include name, age, and grade / 이름, 나이, 성적 포함
   - Minimum 5 students / 최소 5명 이상

2. **Write filtering functions** (필터링 함수 작성)
   - Extract students with score 80 or above / 성적 80점 이상인 학생만 추출
   - Extract students age 20 or above / 나이 20세 이상인 학생만 추출

3. **Write statistics functions** (통계 함수 작성)
   - Find student with highest grade / 가장 높은 성적 학생 찾기
   - Calculate average grade / 평균 성적 계산
   - Calculate students per grade level / 각 학년별 인원 수 계산

4. **Display in HTML table** (HTML 테이블로 표시)
   - All student list / 전체 학생 목록
   - Filtered student list / 필터링된 학생 목록
   - Statistics information / 통계 정보

### Assignment 2: Practical Program (과제 2: 실무 프로그램)

Choose one and implement:

다음 중 하나를 선택하여 구현하세요:

**Option A: User Registration and Login (선택지 A: 회원가입 및 로그인)**
- Store user information in array / 회원정보를 배열에 저장
- Validation (username, password) / 유효성 검사 (아이디, 비밀번호)
- Login function (verify username and password) / 로그인 기능 (username, password 확인)

**Option B: Shopping Cart Management (선택지 B: 쇼핑 카트 관리)**
- Product info (name, price, quantity) / 상품 정보 (이름, 가격, 수량)
- Add/remove products from cart / 장바구니에 상품 추가/제거
- Calculate total price / 총 가격 계산
- Apply discount function / 할인율 적용 함수

**Option C: Grade Management System (선택지 C: 학점 관리 시스템)**
- Store student info and grades / 학생 정보와 성적 저장
- Calculate GPA (Grade Point Average) / 학점 평균 계산 (GPA)
- Group students by grade / 학점별 학생 그룹핑
- Calculate ranking / 석차 계산

---

Thank you for your attention.

Professor Cho Jeonghyun (peterchokr@gmail.com)
Yeungnam University College
