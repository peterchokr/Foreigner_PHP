# 📝 Chapter 2 Practice Questions: PHP Basics & Variables, Data Types

---

## 🎯 Multiple Choice (10 Questions)

#### **Question 1: What is the most significant difference between echo and print in PHP?**

① echo outputs only HTML and print outputs only PHP
② echo can pass multiple arguments separated by commas, print can only pass one
③ echo executes on the server and print executes in the browser
④ print is much faster than echo

---

#### **Question 2: Which of the following is a valid variable name in PHP?**

① `$2user`
② `$user-name`
③ `$user_name`
④ `$user name`

---

#### **Question 3: What is the result of the following code?**

```php
$value = 10;
$value = "Hello";
$value = 3.14;
echo $value;
```

① 10
② "Hello"
③ 3.14
④ Error occurs

---

#### **Question 4: Which is NOT a PHP data type?**

① String
② Integer
③ Double
④ Record

---

#### **Question 5: What is the role of the var_dump() function?**

① Deletes a variable
② Outputs the type and value of a variable to the screen
③ Outputs only the variable name
④ Sends the variable to another file

---

#### **Question 6: Which is a correct characteristic of constants?**

① The value can be changed after declaration
② Must be prefixed with $
③ Can be redefined at any time during program execution
④ Once defined, it cannot be changed

---

#### **Question 7: What is the role of the isset() function?**

① Sets the value of a variable
② Checks if a variable is defined
③ Changes the data type of a variable
④ Outputs the content of a variable

---

#### **Question 8: What is the output of the following code?**

```php
$x = 10;
$y = "10";
echo $x == $y;  // == compares only values
```

① 10
② 10 (string)
③ 1 (true)
④ 0 (false)

---

#### **Question 9: What is the result of the following type casting?**

```php
$number = "123abc";
$int = (int)$number;
echo $int;
```

① 123
② 0
③ "123abc"
④ Error occurs

---

#### **Question 10: Which statement about arrays is correct?**

① PHP arrays can only use numeric indices
② Arrays are divided into associative arrays (key-value) and indexed arrays
③ Arrays can only contain a single data type
④ Echoing an array directly displays all its contents

---

## 💻 Practical Tasks (5 Questions)

### **Task 1: Variable Declaration and Output**

**Requirements:**

- Store name, age, and height in variables
- Check the type of each variable using the gettype() function
- Output in the following format: "John Smith is 25 years old and 180.5cm tall"

**Filename**: `variables.php`

```php
<?php
// Write your code here

?>
```

---

### **Task 2: Data Type Verification (var_dump)**

**Requirements:**

- Create 5 variables of different data types (String, Integer, Float, Boolean, Array)
- Output each variable using var_dump()
- Clearly identify the type of each variable

**Filename**: `datatypes.php`

```php
<?php
// Write your code here

?>
```

---

### **Task 3: Constants and Variables Usage**

**Requirements:**

- Define 3 constants (company name, website, maximum file size)
- Create 3 variables (username, login status, signup date)
- Output user information using constants and variables (table format)

**Filename**: `constants_variables.php`

```php
<?php
// Write your code here

?>
```

---

### **Task 4: String Concatenation and Basic Calculation**

**Requirements:**

- Store product name, price, and quantity in variables
- Calculate total amount (price × quantity)
- Output in the following format: "Product: Laptop, Price: 1000000 USD, Quantity: 2, Total: 2000000 USD"

**Filename**: `calculator.php`

```php
<?php
// Write your code here

?>
```

---

### **Task 5: Array Manipulation and Loop**

**Requirements:**

- Store 3 students' names and scores in an array (associative array)
  - John Smith: 85 points
  - Sarah Johnson: 92 points
  - Michael Park: 78 points
- Iterate through array values with foreach and output
- Output in the format: "John Smith's score: 85 points"

**Filename**: `array_loop.php`

```php
<?php
// Write your code here

?>
```

---

---

## ✅ Answers and Explanations

### **Multiple Choice Answers**

| Question | Answer                                                                                                                                            | Explanation                                                                                                                         |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| 1        | **② echo passes multiple, print passes one**                                                                                               | echo can pass multiple arguments separated by commas with `echo "a", "b", "c"`, but print only works with format `print "test"` |
| 2        | **③$user_name** | Variable names must start with $ and can only contain letters, numbers, and underscores (first character cannot be a number) |                                                                                                                                     |
| 3        | **③ 3.14**                                                                                                                                 | PHP is a dynamically typed language where variable types can be freely changed. The last assigned value 3.14 is output              |
| 4        | **④ Record**                                                                                                                               | PHP's basic data types: String, Integer, Float(Double), Boolean, Array, Object, NULL (Record does not exist)                        |
| 5        | **② Output the type and value of a variable**                                                                                              | var_dump() displays the data type and value of a variable in detail                                                                 |
| 6        | **④ Once defined, it cannot be changed**                                                                                                   | Constants are defined with define() and cannot be changed. They also cannot be redefined unless the 4th parameter of define is true |
| 7        | **② Check if a variable is defined**                                                                                                       | isset() checks if a variable exists and is not NULL                                                                                 |
| 8        | **③ 1 (true)**                                                                                                                             | == compares only values, so 10 and "10" are considered equal and returns true(1)                                                    |
| 9        | **① 123**                                                                                                                                  | Type casting with (int) converts only the leading numeric portion of a string to an integer ("123abc" → 123)                       |
| 10       | **② Arrays are divided into associative arrays (key-value) and indexed arrays**                                                            | PHP arrays support both indexed arrays and associative arrays                                                                       |

---

### **Practical Task Solutions**

#### **Task 1: Variable Declaration and Output - Answer**

```php
<?php
// Declare variables
$name = "John Smith";
$age = 25;
$height = 180.5;

// Check types
$name_type = gettype($name);        // string
$age_type = gettype($age);          // integer
$height_type = gettype($height);    // double

// Output
echo $name . " is " . $age . " years old and " . $height . "cm tall<br>";
echo "Types: " . $name_type . ", " . $age_type . ", " . $height_type;
?>
```

**Output:**

```
John Smith is 25 years old and 180.5cm tall
Types: string, integer, double
```

---

#### **Task 2: Data Type Verification (var_dump) - Answer**

```php
<?php
// Declare variables of different data types
$name = "John Smith";           // String
$age = 25;                      // Integer
$height = 180.5;                // Float
$is_student = true;             // Boolean
$skills = array("PHP", "MySQL", "JavaScript");  // Array

// Output each variable with var_dump()
echo "1️⃣ String:<br>";
var_dump($name);

echo "<br>2️⃣ Integer:<br>";
var_dump($age);

echo "<br>3️⃣ Float:<br>";
var_dump($height);

echo "<br>4️⃣ Boolean:<br>";
var_dump($is_student);

echo "<br>5️⃣ Array:<br>";
var_dump($skills);
?>
```

**Output:**

```
1️⃣ String:
string(10) "John Smith"

2️⃣ Integer:
int(25)

3️⃣ Float:
float(180.5)

4️⃣ Boolean:
bool(true)

5️⃣ Array:
array(3) { [0]=> string(3) "PHP" [1]=> string(5) "MySQL" [2]=> string(10) "JavaScript" }
```

---

#### **Task 3: Constants and Variables Usage - Answer**

```php
<?php
// Define constants
define('COMPANY_NAME', 'XYZ Tech University');
define('WEBSITE', 'https://www.xyztechuniversity.com');
define('MAX_FILE_SIZE', 10485760);  // 10MB

// Declare variables
$user_name = "John Smith";
$is_logged_in = true;
$signup_date = "2024-01-15";

?>

<!DOCTYPE html>
<html>
<head>
    <title>Company Information and User</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        table { width: 100%; border-collapse: collapse; margin: 20px 0; }
        th, td { padding: 10px; border: 1px solid #ddd; text-align: left; }
        th { background-color: navy; color: white; }
    </style>
</head>
<body>

<h1>📋 Company Information (Constants)</h1>
<table>
    <tr>
        <th>Item</th>
        <th>Value</th>
    </tr>
    <tr>
        <td>Company Name</td>
        <td><?php echo COMPANY_NAME; ?></td>
    </tr>
    <tr>
        <td>Website</td>
        <td><?php echo WEBSITE; ?></td>
    </tr>
    <tr>
        <td>Maximum File Size</td>
        <td><?php echo MAX_FILE_SIZE . " bytes"; ?></td>
    </tr>
</table>

<h1>👤 User Information (Variables)</h1>
<table>
    <tr>
        <th>Item</th>
        <th>Value</th>
    </tr>
    <tr>
        <td>Username</td>
        <td><?php echo $user_name; ?></td>
    </tr>
    <tr>
        <td>Login Status</td>
        <td><?php echo $is_logged_in ? "Logged In" : "Logged Out"; ?></td>
    </tr>
    <tr>
        <td>Signup Date</td>
        <td><?php echo $signup_date; ?></td>
    </tr>
</table>

</body>
</html>
```

---

#### **Task 4: String Concatenation and Basic Calculation - Answer**

```php
<?php
// Product information variables
$product_name = "Laptop";
$price = 1000000;
$quantity = 2;

// Calculate total amount
$total_price = $price * $quantity;

// Output result
echo "Product: " . $product_name . ", ";
echo "Price: " . $price . " USD, ";
echo "Quantity: " . $quantity . ", ";
echo "Total: " . $total_price . " USD";

// Or using printf
echo "<br><br>";
printf("Product: %s, Price: %d USD, Quantity: %d, Total: %d USD", 
       $product_name, $price, $quantity, $total_price);
?>
```

**Output:**

```
Product: Laptop, Price: 1000000 USD, Quantity: 2, Total: 2000000 USD

Product: Laptop, Price: 1000000 USD, Quantity: 2, Total: 2000000 USD
```

---

#### **Task 5: Array Manipulation and Loop - Answer**

```php
<?php
// Student score associative array
$students = array(
    "John Smith" => 85,
    "Sarah Johnson" => 92,
    "Michael Park" => 78
);

// Method 1: Iterate with foreach
echo "【 Student Scores 】<br>";
foreach ($students as $name => $score) {
    echo $name . "'s score: " . $score . " points<br>";
}

echo "<br>【 Total and Average 】<br>";
// Calculate total
$total_score = array_sum($students);
$average_score = $total_score / count($students);

echo "Total: " . $total_score . " points<br>";
printf("Average: %.2f points", $average_score);
?>
```

**Output:**

```
【 Student Scores 】
John Smith's score: 85 points
Sarah Johnson's score: 92 points
Michael Park's score: 78 points

【 Total and Average 】
Total: 255 points
Average: 85.00 points
```

---

## 💡 Problem-Solving Tips

### **Multiple Choice Strategy**

1. **Data Type Problems**: Make sure you know PHP's 8 basic types (String, Integer, Float, Boolean, Array, Object, NULL, Resource)
2. **echo vs print**: The key is whether multiple arguments are possible (echo ○, print ×)
3. **Variable Naming Rules**: Start with $, can use letters/numbers/underscore (first character cannot be a number)
4. **Type Casting**: Convert types with (int), (string), (bool), (array), etc.

### **Practical Task Strategy**

1. **Variables First**: Declare and initialize all necessary variables first
2. **Type Check**: Verify data types with var_dump(), gettype()
3. **String Concatenation**: Use `.` operator to combine strings
4. **Arrays**: Distinguish between indexed arrays `[]` and associative arrays `["key" => "value"]`
5. **Loops**: Use `as $key => $value` when iterating through arrays with foreach

---

Thank you for your attention.   
Cho Jeonghyun ([peterchokr@gmail.com](mailto:peterchokr@gmail.com)). Yeungnam University College

"Produced in collaboration with Claude and Gemini."
