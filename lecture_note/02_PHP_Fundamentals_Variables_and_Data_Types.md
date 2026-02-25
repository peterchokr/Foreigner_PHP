# Chapter 2. PHP Fundamentals, Variables, and Data Types

---

## 📚 Learning Objectives

After completing this chapter, you will be able to:

✅ Understand the basic syntax and rules of PHP
✅ Declare and use variables
✅ Identify and use PHP's major data types
✅ Define and utilize constants
✅ Use arithmetic, comparison, and logical operators
✅ Receive and process user input
✅ Create a simple calculator program

이 장을 학습하면 다음을 할 수 있습니다:

✅ PHP의 기본 문법과 규칙을 이해할 수 있습니다.
✅ 변수를 선언하고 사용할 수 있습니다.
✅ PHP의 주요 데이터 타입을 구분하고 사용할 수 있습니다.
✅ 상수를 정의하고 활용할 수 있습니다.
✅ 산술, 비교, 논리 연산자를 사용할 수 있습니다.
✅ 사용자 입력을 받아서 처리할 수 있습니다.
✅ 간단한 계산기 프로그램을 만들 수 있습니다.

---

## 1️⃣ PHP Basic Syntax

### 1-1 PHP Tags and Output

This section covers the most fundamental method of writing PHP code. Understanding how to properly structure PHP code and display output is essential for every PHP developer.

이 섹션에서는 PHP 코드를 작성하는 가장 기본적인 방법을 다룹니다. 올바른 PHP 코드 구조와 출력 방법을 이해하는 것은 모든 PHP 개발자에게 필수적입니다.

#### **PHP Tags**

All PHP code must be enclosed within PHP tags. Code between these tags is executed on the server, and only the output is sent to the browser. This is what makes PHP a server-side language.

모든 PHP 코드는 PHP 태그 안에 포함되어야 합니다. 이 태그 사이의 코드는 서버에서 실행되며, 결과만 브라우저로 전송됩니다. 이것이 PHP를 서버 측 언어로 만드는 특징입니다.

```php
<?php
// PHP code execution area (PHP 코드 작성 영역)
// All code here is executed on the server (이 안의 모든 코드는 서버에서 실행됩니다)

?>
```

**Important Points :**

- Start with `<?php` and end with `?>` (`<?php`로 시작, `?>`로 끝남)
- Can include PHP code within HTML files (HTML 파일 내에서 PHP 코드 포함 가능)
- Executed on the server and only results are sent to the browser (서버에서 실행되고 결과만 브라우저로 전송)

#### **echo and print Differences (echo와 print의 차이)**

PHP provides two constructs for output: echo and print. While they appear similar, they have important differences in performance and functionality. Echo is generally preferred in production code because it is faster and more flexible.

PHP는 출력을 위한 두 가지 구조를 제공합니다: echo와 print입니다. 외모는 비슷하지만 성능과 기능에 중요한 차이가 있습니다. Echo는 더 빠르고 유연하기 때문에 일반적으로 프로덕션 코드에서 선호됩니다.

```php
<?php

// ============================================
// 1️⃣ echo - faster and accepts multiple arguments (더 빠르고 여러 인자 가능)
// ============================================

echo "Hello World<br>";
echo "Hello", " ", "World";  // Multiple arguments possible (여러 인자 가능)

// echo has no return value (void) (echo는 반환값이 없음 (void))
// echo semicolon (;) can be omitted but not recommended (세미콜론 생략 가능하지만 권장하지 않음)

// ============================================
// 2️⃣ print - slower but has return value (더 느리지만 반환값 있음)
// ============================================

print "Hello World";  // Only one argument at a time (한 번에 하나의 인자만)

// print has return value of 1 (print는 반환값이 1)
$result = print "test";  // $result stores 1 ($result에 1이 저장됨)

// print always requires semicolon (;) (항상 세미콜론(;) 필요)
// print "Hello", "World";  // ❌ Error! (오류!)

// ============================================
// 📝 Conclusion: use echo in practical development (결론: 실무에서는 echo를 주로 사용)
// ============================================

?>
```

**Practical Recommendation (실무 권장):**

- Use `echo`: faster and more flexible (더 빠르고 유연함)
- Use `print`: only in special cases (특별한 경우만)

#### **Comments**

Comments are used to explain code and make it more readable. Well-written comments help other developers understand your code and make future maintenance easier. PHP supports several types of comments for different situations.

주석은 코드를 설명하고 읽기 쉽게 만들기 위해 사용됩니다. 잘 작성된 주석은 다른 개발자가 코드를 이해하도록 도와주고 향후 유지보수를 더 쉽게 만듭니다. PHP는 다양한 상황을 위한 여러 유형의 주석을 지원합니다.

```php
<?php

// ============================================
// Single-line comments (한 줄 주석)
// ============================================

// This is a single-line comment (이것은 한 줄 주석입니다)
# Python-style comments are also possible (파이썬 스타일 주석도 가능합니다)

// ============================================
// Multi-line comments (여러 줄 주석)
// ============================================

/*
This is a
multi-line
comment
*/

/* Single-line version also works (한 줄 버전도 가능합니다) */

// ============================================
// Comment usage tips (주석 활용 팁)
// ============================================

// ✅ Good example: explain code intent (좋은 예: 코드의 의도를 설명)
$user_age = 25;  // User's age (사용자의 나이)

// ❌ Bad example: explain obvious code (나쁜 예: 명백한 코드를 설명)
$x = 25;  // Set x to 25 (x를 25로 설정)

// ✅ Good example: explain complex logic (좋은 예: 복잡한 로직을 설명)
// Filter only numbers greater than 0 and less than 100 (0보다 크고 100보다 작은 숫자만 필터링)
$numbers = [-5, 10, 50, 100, 150];
$valid_numbers = array_filter($numbers, fn($n) => $n > 0 && $n < 100);
print_r($valid_numbers);

?>
```

#### **var_dump() - Variable Detailed Information (변수 상세 정보)**

The var_dump() function is a powerful debugging tool that displays the type and value of variables. This is especially useful during development when you need to understand what data your code is working with.

var_dump() 함수는 변수의 타입과 값을 표시하는 강력한 디버깅 도구입니다. 이것은 특히 개발 중에 코드가 어떤 데이터로 작동하는지 이해해야 할 때 유용합니다.

```php
<?php

// ============================================
// var_dump() - Display variable type and value to screen (변수의 타입과 값을 화면에 출력)
// ============================================

$number = 42;
var_dump($number);
// Result: int(42) 

$text = "Hello";
var_dump($text);
// Result: string(5) "Hello" 

$is_active = true;
var_dump($is_active);
// Result: bool(true) 

// ============================================
// Display multiple variables at once (여러 변수 한 번에 출력)
// ============================================

$name = "John Smith";
$age = 25;
$email = "john@example.com";

var_dump($name, $age, $email);

// Result: 
// string(10) "John Smith"
// int(25)
// string(18) "john@example.com"

?>
```

### 1-2 Practice Example: Understanding Basic Syntax (기본 문법 이해하기)

Now let's use the basic syntax we've learned in a practical example.

이제 배운 기본 문법을 실제 예제에서 사용해봅시다.

**File name: basic_syntax.php**

```php
<?php
/**
 * basic_syntax.php - PHP Basic Syntax Practice
 * 
 * Purpose: (목적:)
 * 1. Verify PHP tag usage (PHP 태그 사용 확인)
 * 2. Compare echo and print (echo, print 비교)
 * 3. Learn comment writing (주석 작성 방법)
 * 4. Use var_dump() (var_dump() 활용)
 */

?>

<!DOCTYPE html>
<html>
<head>
    <title>PHP Basic Syntax</title>
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
  
        .section {
            margin: 20px 0;
            padding: 15px;
            background-color: #f9f9f9;
            border-left: 4px solid #2196F3;
        }
  
        .section h2 {
            margin-top: 0;
            color: #2196F3;
        }
  
        .code-output {
            background-color: #f0f0f0;
            padding: 10px;
            font-family: monospace;
            font-size: 14px;
            margin: 10px 0;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>📝 PHP Basic Syntax</h1>
  
    <!-- Using echo -->
    <div class="section">
        <h2>1️⃣ Using echo</h2>
        <?php
        // Display text with echo (echo로 텍스트 출력)
        echo "Text output with echo<br>";
        echo "This is ", "output with ", "multiple arguments<br>";
        ?>
    </div>
  
    <!-- Using print -->
    <div class="section">
        <h2>2️⃣ Using print</h2>
        <?php
        // Display text with print (print로 텍스트 출력)
        print "Text output with print<br>";
        ?>
    </div>
  
    <!-- Variable output -->
    <div class="section">
        <h2>3️⃣ Variable Output</h2>
        <?php
        // Declare and display variables (변수 선언 및 출력)
        $title = "PHP Learning";
        $version = 8.0;
        $is_fun = true;
  
        echo "Title: " . $title . "<br>";
        echo "Version: " . $version . "<br>";
        echo "Is it fun? " . ($is_fun ? "Yes!" : "No") . "<br>";
        ?>
    </div>
  
    <!-- var_dump usage -->
    <div class="section">
        <h2>4️⃣ var_dump() - Variable Detailed Information</h2>
        <p>You can check the type and value of variables:</p>
        <div class="code-output">
            <?php
            // Display variable type and value (변수의 타입과 값 표시)
            $age = 25;
            $name = "John Smith";
            $height = 180.5;
            $is_student = true;
  
            var_dump($age);
            var_dump($name);
            var_dump($height);
            var_dump($is_student);
            ?>
        </div>
    </div>
</div>

</body>
</html>
```

**When you run this file at `http://localhost/basic_syntax.php`: **

- Understand how to use echo and print (echo와 print의 사용 방법 확인)
- Learn how to display variables (변수 출력 방식 이해)
- Check variable types with var_dump() (var_dump()로 변수의 타입 확인) ✅

---

## 2️⃣ Variables and Constants (변수와 상수)

### 2-1 Variable Declaration Rules (변수 선언 규칙)

Variables are containers for storing data values. Understanding PHP's variable naming rules and conventions ensures your code is readable and consistent with PHP best practices. Proper variable naming makes code more maintainable.

변수는 데이터 값을 저장하기 위한 컨테이너입니다. PHP의 변수 명명 규칙과 관례를 이해하면 코드가 읽기 쉽고 PHP 모범 사례와 일치합니다. 적절한 변수 명명은 코드를 더욱 유지보수하기 쉽게 만듭니다.


```php
<?php

// ============================================
// Variable declaration rules (변수 선언 규칙)
// ============================================

// ✅ Correct examples (올바른 예)
$name = "John Smith";           // String 
$age = 25;                      // Integer
$height = 180.5;                // Float
$is_student = true;             // Boolean

$_name = "Mary Johnson";         // Can start with underscore (언더스코어 시작 가능)
$name2 = "Michael Brown";        // Can include numbers (단, 첫 글자는 불가) (숫자 포함 가능)

// ❌ Incorrect examples (틀린 예)
// $2name = "David Lee";          // ❌ Cannot start with number (숫자로 시작 불가)
// $my-name = "Sarah Kim";        // ❌ Cannot use hyphen (-) (하이픈(-) 사용 불가)
// $my name = "James Park";       // ❌ Cannot use whitespace (공백 사용 불가)

// ============================================
// Variable naming conventions (recommended) (변수 명명 규칙 (권장))
// ============================================

// camelCase: typically used in class methods (주로 클래스 메서드)
$userName = "John Smith";
$userAge = 25;

// snake_case: PHP recommended style (PHP 권장)
$user_name = "John Smith";
$user_age = 25;

// CONSTANT_STYLE: for constants (상수용)
define('MAX_AGE', 100);

// ============================================
// Use meaningful variable names (의미 있는 변수명 사용)
// ============================================

// ✅ Good examples
$user_email = "john@example.com";
$total_price = 50000;

// ❌ Bad examples
$x = "john@example.com";
$a = 50000;

?>
```

#### **Dynamic Type System (동적 타입 시스템)**

PHP is a dynamically typed language, meaning variable types are determined at runtime. A variable that holds an integer can later be reassigned to hold a string. This flexibility is powerful but requires careful attention to data types.

PHP는 동적으로 타입이 지정되는 언어입니다. 즉, 변수의 타입이 런타임에 결정됩니다. 정수를 보유하는 변수는 나중에 문자열을 보유하도록 다시 할당될 수 있습니다. 이러한 유연성은 강력하지만 데이터 타입에 주의해야 합니다.

```php
<?php

// ============================================
// PHP is a dynamically typed language (PHP는 동적 타입 언어)
// Variable type is determined at runtime (변수의 타입이 런타임에 결정됨)
// ============================================

$value = 10;           // Integer
var_dump($value);      // int(10)

$value = "Hello";      // Changed to string
var_dump($value);      // string(5) "Hello"

$value = 3.14;         // Changed to float
var_dump($value);      // float(3.14)

$value = true;         // Changed to boolean
var_dump($value);      // bool(true)

// ============================================
// Check if variable exists (변수 존재 확인)
// ============================================

if (isset($user_name)) {
    echo "Variable is defined<br>";  
} else {
    echo "Variable is not defined<br>";  
}

// ============================================
// Initialize and reset variables (변수 초기화 및 값 리셋)
// ============================================

$temp = "Temporary data";
var_dump($temp);  // string(16) "Temporary data"

unset($temp);     // Delete variable 
var_dump($temp);  // ❌ Notice: Undefined variable

?>
```

#### **Defining Constants (상수 정의)**

Constants are identifiers for values that cannot be changed during script execution. Unlike variables, constants don't have the dollar sign prefix and are typically written in uppercase. Constants are useful for storing values that should not be modified.

상수는 스크립트 실행 중에 변경할 수 없는 값의 식별자입니다. 변수와 달리 상수에는 달러 기호 접두사가 없으며 일반적으로 대문자로 작성됩니다. 상수는 수정하면 안 되는 값을 저장하는 데 유용합니다.

```php
<?php

// ============================================
// Define constants: define() function (상수 정의: define() 함수)
// ============================================

define('SITE_NAME', 'My PHP Website');
define('MAX_USERS', 100);
define('PI', 3.14159);

echo SITE_NAME . "<br>";  // My PHP Website
echo MAX_USERS . "<br>";  // 100
echo PI . "<br>";         // 3.14159

// ============================================
// Constants cannot be changed (상수는 변경할 수 없음)
// ============================================

// SITE_NAME = 'New Name';  // ❌ Error!

// ============================================
// Check if constant is defined (상수가 정의되었는지 확인)
// ============================================

if (defined('SITE_NAME')) {
    echo "Constant is defined<br>";  // 상수가 정의되었습니다
}

// ============================================
// Magic constants (매직 상수)
// ============================================

echo __FILE__ . "<br>";        // Current file path (현재 파일 경로)
echo __LINE__ . "<br>";        // Current line number (현재 라인 번호)
echo __DIR__ . "<br>";         // Current directory (현재 디렉토리)
echo __FUNCTION__ . "<br>";    // Current function name (현재 함수명)
echo __CLASS__ . "<br>";       // Current class name (현재 클래스명)

?>
```

---

## 3️⃣ Data Types (데이터 타입)

### 3-1 String Data Type (문자열 데이터 타입)

Strings are sequences of characters used to represent text. PHP provides flexible string handling with multiple ways to define and manipulate strings, including variable interpolation and string concatenation.

문자열은 텍스트를 나타내는 데 사용되는 문자 시퀀스입니다. PHP는 변수 보간 및 문자열 연결을 포함하여 문자열을 정의하고 조작하는 여러 방법을 제공하는 유연한 문자열 처리를 제공합니다.

```php
<?php

// ============================================
// Single quotes (작은따옴표)
// ============================================

$name = 'John Smith';
echo $name . "<br>";           // John Smith
echo 'Hello $name' . "<br>";   // Hello $name (variable not interpolated)

// ============================================
// Double quotes (큰따옴표)
// ============================================

$name = "John Smith";
echo $name . "<br>";           // John Smith
echo "Hello $name" . "<br>";   // Hello John Smith (변수가 보간됨)
echo "Hello {$name}" . "<br>"; // Hello John Smith (더 명시적)

// ============================================
// String concatenation (문자열 연결)
// ============================================

$first = "John";
$last = "Smith";
$full_name = $first . " " . $last;
echo $full_name . "<br>";      // John Smith

// ============================================
// String functions (문자열 함수)
// ============================================

echo strlen($name) . "<br>";        // 10 (length of string)
echo strtoupper($name) . "<br>";    // JOHN SMITH (convert to uppercase)
echo strtolower($name) . "<br>";    // john smith (convert to lowercase)
echo str_replace("Smith", "Doe", $name) . "<br>";  // John Doe

?>
```

### 3-2 Numeric Data Types (숫자형 데이터 타입)

PHP supports two numeric data types: integers (whole numbers) and floats (decimal numbers). Understanding the differences between them is important for performing correct calculations and avoiding unexpected type conversions.

PHP는 두 가지 숫자 데이터 타입을 지원합니다: 정수(정수)와 실수(소수점 수). 그 차이를 이해하는 것은 올바른 계산을 수행하고 예기치 않은 타입 변환을 피하는 데 중요합니다.

```php
<?php

// ============================================
// Integer (정수)
// ============================================

$age = 25;
$count = -100;
$result = 0;

// Check if integer (정수 여부 확인)
if (is_int($age)) {
    echo "It's an integer<br>";  // 정수입니다
}

// Integer range (정수 범위)
$max_int = PHP_INT_MAX;     // Maximum integer value
$min_int = PHP_INT_MIN;     // Minimum integer value

// ============================================
// Float (실수)
// ============================================

$price = 19.99;
$ratio = 0.5;
$pi = 3.14159;

// Check if float (실수 여부 확인)
if (is_float($price)) {
    echo "It's a float<br>";     // 실수입니다
}

// ============================================
// Type conversion (타입 변환)
// ============================================

$int_value = 42;
$float_value = (float)$int_value;  // 42.0

$float_value = 3.14;
$int_value = (int)$float_value;    // 3

// ============================================
// Mathematical functions (수학 함수)
// ============================================

echo abs(-10) . "<br>";        // 10 (absolute value)
echo round(3.7) . "<br>";      // 4 (round to nearest integer)
echo ceil(3.2) . "<br>";       // 4 (round up)
echo floor(3.9) . "<br>";      // 3 (round down)
echo pow(2, 3) . "<br>";       // 8 (2^3)
echo sqrt(16) . "<br>";        // 4 (square root)

?>
```

### 3-3 Boolean and NULL Data Types (불린과 NULL 데이터 타입)

Boolean values represent true or false, used in conditional statements and logical operations. NULL represents the absence of a value and is distinct from zero or empty strings.

불린 값은 참 또는 거짓을 나타내며 조건문과 논리 연산에 사용됩니다. NULL은 값의 부재를 나타내며 0이나 빈 문자열과는 다릅니다.

```php
<?php

// ============================================
// Boolean (불린)
// ============================================

$is_logged_in = true;
$is_admin = false;

if ($is_logged_in) {
    echo "User is logged in<br>";  // 사용자가 로그인했습니다
}

// Check if boolean (불린 여부 확인)
if (is_bool($is_logged_in)) {
    echo "It's a boolean<br>";     // 불린입니다
}

// Values that evaluate to false (거짓으로 평가되는 값)
$false_values = [
    false,           // Boolean false
    0,               // Integer zero
    0.0,             // Float zero
    "",              // Empty string
    "0",             // String zero
    null,            // NULL value
    [],              // Empty array
];

// ============================================
// NULL (NULL)
// ============================================

$value = null;

// Check if NULL (NULL 여부 확인)
if (is_null($value)) {
    echo "Variable is null<br>";   // 변수가 null입니다
}

// Check if variable is set (변수가 설정되었는지 확인)
if (isset($value)) {
    echo "Variable is set<br>";    // 변수가 설정되었습니다
} else {
    echo "Variable is not set<br>";  // 변수가 설정되지 않았습니다
}

?>
```

### 3-4 Array Data Type (배열 데이터 타입)

Arrays are collections of data stored in a single variable. PHP supports both indexed arrays (accessed by position) and associative arrays (accessed by keys). Arrays are fundamental to PHP programming.

배열은 단일 변수에 저장된 데이터 모음입니다. PHP는 인덱스 배열(위치별로 액세스)과 연관 배열(키로 액세스)을 모두 지원합니다. 배열은 PHP 프로그래밍의 기본입니다.

```php
<?php

// ============================================
// Indexed arrays (인덱스 배열)
// ============================================

$fruits = ["Apple", "Banana", "Orange"];
echo $fruits[0] . "<br>";      // Apple
echo $fruits[1] . "<br>";      // Banana

// Array functions (배열 함수)
echo count($fruits) . "<br>";  // 3 (number of elements)

// Add element to array (배열에 요소 추가)
$fruits[] = "Grape";
array_push($fruits, "Mango");

// Loop through array (배열 반복)
foreach ($fruits as $fruit) {
    echo $fruit . "<br>";
}

// ============================================
// Associative arrays (연관 배열)
// ============================================

$person = [
    "name" => "John Smith",
    "age" => 25,
    "email" => "john@example.com"
];

echo $person["name"] . "<br>";   // John Smith
echo $person["age"] . "<br>";    // 25

// Loop through associative array (연관 배열 반복)
foreach ($person as $key => $value) {
    echo "$key: $value<br>";
}

// ============================================
// Multidimensional arrays (다차원 배열)
// ============================================

$users = [
    ["name" => "John Smith", "age" => 25],
    ["name" => "Mary Johnson", "age" => 23],
    ["name" => "Michael Brown", "age" => 24]
];

echo $users[0]["name"] . "<br>";  // John Smith
echo $users[1]["age"] . "<br>";   // 23

?>
```

---

## 4️⃣ Operators (연산자)

### 4-1 Arithmetic and Assignment Operators (산술 및 할당 연산자)

Arithmetic operators perform mathematical calculations, while assignment operators combine operations with value assignment. Understanding these operators is essential for manipulating numbers in PHP.

산술 연산자는 수학적 계산을 수행하고 할당 연산자는 작업을 값 할당과 결합합니다. 이 연산자들을 이해하는 것은 PHP에서 숫자를 조작하는 데 필수적입니다.

```php
<?php

// ============================================
// Arithmetic operators (산술 연산자)
// ============================================

$a = 10;
$b = 3;

echo $a + $b . "<br>";         // 13 (addition)
echo $a - $b . "<br>";         // 7 (subtraction)
echo $a * $b . "<br>";         // 30 (multiplication)
echo $a / $b . "<br>";         // 3.33... (division)
echo $a % $b . "<br>";         // 1 (modulo - remainder)
echo $a ** $b . "<br>";        // 1000 (exponentiation)

// ============================================
// Assignment operators (할당 연산자)
// ============================================

$count = 10;
$count += 5;            // $count = $count + 5; (15)
$count -= 3;            // $count = $count - 3; (12)
$count *= 2;            // $count = $count * 2; (24)
$count /= 4;            // $count = $count / 4; (6)

// ============================================
// Increment and decrement operators (증감 연산자)
// ============================================

$x = 5;
$x++;                   // 6 (post-increment)
++$x;                   // 7 (pre-increment)
$x--;                   // 6 (post-decrement)
--$x;                   // 5 (pre-decrement)

// ============================================
// Difference between pre and post (전위와 후위의 차이)
// ============================================

$value = 10;
$result1 = $value++;    // $result1 = 10, $value = 11
$result2 = ++$value;    // $result2 = 12, $value = 12

?>
```

### 4-2 Comparison and Logical Operators (비교 및 논리 연산자)

Comparison operators are used in conditional statements to compare values, while logical operators combine multiple conditions. The distinction between `==` and `===` is particularly important in PHP.

비교 연산자는 조건문에서 값을 비교하는 데 사용되고 논리 연산자는 여러 조건을 결합합니다. PHP에서 `==`와 `===`의 구분은 특히 중요합니다.

```php
<?php

// ============================================
// Comparison operators (비교 연산자)
// ============================================

$a = 10;
$b = 20;

$a == $b;       // false (value comparison, type ignored) (값 비교, 타입 무시)
$a === $b;      // false (value and type comparison) (값과 타입 비교)
$a != $b;       // true (values are different) (값이 다름)
$a !== $b;      // true (value and type are different) (값과 타입이 다름)
$a < $b;        // true (10 < 20)
$a > $b;        // false (10 > 20)
$a <= $b;       // true (10 <= 20)
$a >= $b;       // false (10 >= 20)

// ============================================
// == vs === (Important!)
// ============================================

"10" == 10;     // true (value comparison only) (값만 비교)
"10" === 10;    // false (type also compared) (타입도 비교)

0 == false;     // true (value comparison only)
0 === false;    // false (type also compared)

null == "";     // true
null === "";    // false

// ✅ Recommendation: use === (권장: === 사용)
if ($age === 20) {
    echo "Exactly 20 years old<br>";  // 정확히 20세입니다
}

// ============================================
// Logical operators (논리 연산자)
// ============================================

$age = 25;
$has_license = true;

// AND operator (모두 참이어야 참)
if ($age >= 18 && $has_license) {
    echo "Can drive<br>";          // 운전 가능
}

// OR operator (하나라도 참이면 참)
$is_weekend = false;
$is_holiday = true;

if ($is_weekend || $is_holiday) {
    echo "Day off<br>";            // 휴무일
}

// NOT operator (참을 거짓으로, 거짓을 참으로)
$is_logged_in = false;

if (!$is_logged_in) {
    echo "Please log in<br>";      // 로그인하세요
}

?>
```

### 4-3 String Concatenation Operator (문자열 연결 연산자)

The dot (.) operator is used to concatenate strings in PHP. This fundamental operation is used constantly when building output strings and working with text data.

점(.) 연산자는 PHP에서 문자열을 연결하는 데 사용됩니다. 이 기본 작업은 출력 문자열을 구축하고 텍스트 데이터로 작업할 때 지속적으로 사용됩니다.

```php
<?php

// ============================================
// String concatenation (.) (문자열 연결 (.))
// ============================================

$first_name = "John";
$last_name = "Smith";
$full_name = $first_name . " " . $last_name;  // John Smith

// Mix variables and strings (변수와 문자열 섞기)
$age = 25;
$message = "Age: " . $age . " years old";  // Age: 25 years old

// Concatenation assignment (문자열 연결 단축)
$text = "Hello";
$text .= " World";  // $text = $text . " World";
echo $text;         // Hello World

?>
```

### 4-4 Example: Simple Calculator (예제: 간단한 계산기)

**File name: calculator.php**

```php
<?php
/**
 * calculator.php - Simple Calculator
 * 
 * Purpose: (목적:)
 * 1. Receive two numbers and an operator from user (사용자로부터 두 수와 연산자 입력받기)
 * 2. Perform calculation (연산 수행)
 * 3. Display result (결과 출력)
 */

// ============================================
// Key: Initialize input data (핵심: 입력 데이터 초기화)
// ============================================

$num1 = isset($_POST['num1']) ? floatval($_POST['num1']) : 0;
$num2 = isset($_POST['num2']) ? floatval($_POST['num2']) : 0;
$operator = isset($_POST['operator']) ? $_POST['operator'] : '+';
$result = null;
$error = '';

// ============================================
// Key: Perform calculation (핵심: 연산 수행)
// ============================================

if ($_SERVER['REQUEST_METHOD'] === 'POST' && $num1 !== 0 && $num2 !== 0) {
    switch ($operator) {
        case '+':
            $result = $num1 + $num2;
            break;
        case '-':
            $result = $num1 - $num2;
            break;
        case '*':
            $result = $num1 * $num2;
            break;
        case '/':
            if ($num2 == 0) {
                $error = "❌ Error: Cannot divide by zero!";  // 오류: 0으로 나눌 수 없습니다!
            } else {
                $result = $num1 / $num2;
            }
            break;
        default:
            $error = "❌ Error: Choose correct operator!";  // 오류: 올바른 연산자를 선택하세요!
    }
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>Calculator</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 500px;
            margin: 50px auto;
            padding: 20px;
        }
  
        .calculator {
            background: white;
            padding: 30px;
            border: 1px solid #ddd;
        }
  
        h1 {
            text-align: center;
            color: navy;
        }
  
        .form-group {
            margin: 15px 0;
        }
  
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
  
        input[type="number"],
        select {
            width: 100%;
            padding: 8px;
            border: 1px solid #ddd;
            box-sizing: border-box;
        }
  
        .button-group {
            display: flex;
            gap: 10px;
            margin-top: 20px;
        }
  
        button {
            flex: 1;
            padding: 10px;
            background-color: navy;
            color: white;
            border: none;
            cursor: pointer;
        }
  
        .result-box {
            margin-top: 20px;
            padding: 15px;
            background-color: #e3f2fd;
            border-left: 4px solid navy;
            text-align: center;
        }
  
        .result-label {
            color: #666;
            font-size: 14px;
        }
  
        .result-value {
            color: navy;
            font-size: 32px;
            font-weight: bold;
            margin-top: 10px;
        }
  
        .error {
            background-color: #ffebee;
            color: #c62828;
            padding: 12px;
            border-left: 4px solid #c62828;
            margin-top: 10px;
        }
    </style>
</head>
<body>

<div class="calculator">
    <h1>🧮 Calculator</h1>
  
    <form method="POST">
        <!-- First number -->
        <div class="form-group">
            <label for="num1">First Number:</label>
            <input type="number" id="num1" name="num1" step="0.01" 
                   value="<?php echo $num1 !== 0 ? $num1 : ''; ?>" required>
        </div>
  
        <!-- Operator selection -->
        <div class="form-group">
            <label for="operator">Operator:</label>
            <select id="operator" name="operator">
                <option value="+" <?php echo $operator === '+' ? 'selected' : ''; ?>>Addition (+)</option>
                <option value="-" <?php echo $operator === '-' ? 'selected' : ''; ?>>Subtraction (-)</option>
                <option value="*" <?php echo $operator === '*' ? 'selected' : ''; ?>>Multiplication (*)</option>
                <option value="/" <?php echo $operator === '/' ? 'selected' : ''; ?>>Division (/)</option>
            </select>
        </div>
  
        <!-- Second number -->
        <div class="form-group">
            <label for="num2">Second Number:</label>
            <input type="number" id="num2" name="num2" step="0.01" 
                   value="<?php echo $num2 !== 0 ? $num2 : ''; ?>" required>
        </div>
  
        <!-- Buttons -->
        <div class="button-group">
            <button type="submit">Calculate)</button>
            <button type="reset">Clear</button>
        </div>
    </form>
  
    <!-- Display result -->
    <?php if ($result !== null && !$error): ?>
    <div class="result-box">
        <div class="result-label">
            <?php echo $num1 . " " . $operator . " " . $num2; ?>
        </div>
        <div class="result-value">
            <?php 
            // Format result for nice display (결과를 보기 좋게 포맷)
            if (is_float($result)) {
                echo round($result, 2);  // Display up to 2 decimal places (소수점 2자리)
            } else {
                echo $result;
            }
            ?>
        </div>
    </div>
    <?php endif; ?>
  
    <!-- Error message -->
    <?php if ($error): ?>
    <div class="error">
        <?php echo $error; ?>
    </div>
    <?php endif; ?>
</div>

</body>
</html>
```

**When you run this file at `http://localhost/calculator.php`: (이 파일을 `http://localhost/calculator.php`로 실행하면:)**

- Display form to input two numbers and operator (두 수와 연산자를 입력받는 폼 표시)
- Perform the selected calculation (선택한 연산 수행)
- Calculate and display result (결과 계산 및 표시) ✅
- Handle division by zero error ()0으로 나누기 오류 처리) ✅

---

## ✅ Practice Assignments (연습과제)

### Assignment 1: Data Type Identification Program (과제 1: 데이터 타입 판별 프로그램)

Implement the following:

다음을 구현하세요:

1. **Declare variables of various data types** (다양한 데이터 타입 변수 선언)

   - String, Integer, Float, Boolean, Array, NULL
   - Declare at least 2 of each type (각각 최소 2개 이상)
2. **Check type with gettype() function** (gettype() 함수로 타입 확인)

   - Print type of all variables (모든 변수의 타입 출력)
   - Organize in HTML table (HTML 테이블로 정리)
3. **Practice type conversion** (타입 변환 실습)

   - Convert string to integer (문자열을 정수로 변환)
   - Convert integer to string (정수를 문자열로 변환)
   - Convert float to integer (실수를 정수로 변환)

### Assignment 2: Extend Calculator Program (과제 2: 계산기 프로그램 확장)

Extend the calculator.php above with the following features:

위의 calculator.php를 다음과 같이 확장하세요:

1. **Implement additional operators** (추가 연산자 구현)

   - Exponentiation (**)
   - Modulo (%)
2. **Input validation** (입력 유효성 검사)

   - Verify input is numeric (입력값이 숫자인지 확인)
   - Check for empty values (빈 값 검사)
3. **Result formatting** (결과 포맷팅)

   - Display only up to 2 decimal places (소수점 2자리까지만 표시)
   - Display negative numbers in red (음수는 빨간색으로 표시)
4. **Display calculation history** (계산 이력 표시)

   - Display previous calculation results in table  (이전 계산 결과를 테이블로 표시)

---

Thank you for your attention.
Cho Jeonghyun ([peterchokr@gmail.com](mailto:peterchokr@gmail.com)). Yeungnam University College

"Produced in collaboration with Claude and Gemini."
