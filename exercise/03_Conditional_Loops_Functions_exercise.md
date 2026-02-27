# 📝 Chapter 3 Practice Questions: Conditional Statements, Loops & Functions

---

## 🎯 Multiple Choice (10 Questions)

#### **Question 1: What is the output of the following code?**

```php
$score = 85;

if ($score >= 90) {
    echo "Grade A";
} elseif ($score >= 80) {
    echo "Grade B";
} else {
    echo "Grade C";
}
```

① Grade A
② Grade B
③ Grade C
④ Error occurs

---

#### **Question 2: What is the role of break in a switch statement?**

① Terminates the entire program
② Prevents moving to the next case
③ Terminates a loop
④ Resets the value of a variable

---

#### **Question 3: What is the result of the following code?**

```php
for ($i = 1; $i <= 3; $i++) {
    echo $i . " ";
}
```

① 1 2 3
② 0 1 2
③ 1 2 3 4
④ 3 2 1

---

#### **Question 4: Which syntax is used to execute a while loop at least once?**

① while ($condition)
② do { } while ($condition)
③ for ($i = 0; $i < 10; $i++)
④ foreach ($array as $item)

---

#### **Question 5: To get both the key and value from an array in a foreach loop, which syntax is correct?**

① `foreach ($arr as $item)`
② `foreach ($arr as $key => $value)`
③ `foreach ($arr) { $key, $value }`
④ `foreach ($arr as [$key, $value])`

---

#### **Question 6: What is the return value of the following function?**

```php
function add($a, $b = 5) {
    return $a + $b;
}

echo add(10);
```

① 5
② 10
③ 15
④ Error occurs

---

#### **Question 7: What is the difference between break and continue in a loop?**

① break stops the loop, continue skips the current iteration
② continue stops the loop, break skips the current iteration
③ Both have the same role
④ break exits outside the function

---

#### **Question 8: What is the result of the following code?**

```php
function factorial($n) {
    if ($n <= 1) {
        return 1;
    }
    return $n * factorial($n - 1);
}

echo factorial(4);
```

① 4
② 10
③ 24
④ Infinite loop

---

#### **Question 9: To use a global variable inside a function in PHP, which is correct?**

① Pass it as a function parameter
② Use the global keyword
③ Use the $GLOBALS array
④ All of ①, ②, and ③ are possible

---

#### **Question 10: What is the output of the following code?**

```php
for ($i = 1; $i <= 5; $i++) {
    if ($i == 3) {
        continue;
    }
    echo $i . " ";
}
```

① 1 2 3 4 5
② 1 2 4 5
③ 1 2
④ 3 4 5

---

## 💻 Practical Tasks (5 Questions)

### **Task 1: Grade Determination Program**

**Requirements:**

- Receive a score and determine the grade (if/elseif/else)
- 90 or higher: A, 80 or higher: B, 70 or higher: C, 60 or higher: D, Below 60: F
- Output in the format: "Score: 85 points, Grade: B"

**Filename**: `grade.php`

```php
<?php
// Write your code here

?>
```

---

### **Task 2: Multiplication Table Output (Specific Table)**

**Requirements:**

- Print the 5 times table using a for loop
- Format: "5 × 1 = 5", "5 × 2 = 10", ... , "5 × 9 = 45"
- Repeat at least 9 times

**Filename**: `multiplication.php`

```php
<?php
// Write your code here

?>
```

---

### **Task 3: Array Sum and Average**

**Requirements:**

- Store 5 student scores in an array: [80, 85, 90, 75, 88]
- Iterate through the array with foreach and calculate the sum
- Calculate the average (sum / count)
- Output in the format: "Sum: 418 points, Average: 83.6 points"

**Filename**: `score_average.php`

```php
<?php
// Write your code here

?>
```

---

### **Task 4: Number Determination Function**

**Requirements:**

- Write an isEven($num) function (return true if even, false if odd)
- Determine even numbers from 1 to 10 using the function
- Output in the format: "2 is even", "4 is even"

**Filename**: `even_odd.php`

```php
<?php
// Write your code here

?>
```

---

### **Task 5: Email Validation Function and User Processing**

**Requirements:**

- Write an is_valid_email($email) function (validate by checking for @ symbol)
- Store information for 3 users in an array
- Iterate with foreach and validate using the function
- Output in the format: "John Smith(john@example.com) - Valid", "Sarah Johnson(invalid-email) - Invalid"

**Filename**: `email_validation.php`

```php
<?php
// Write your code here

?>
```

---

---

## ✅ Answers and Explanations

### **Multiple Choice Answers**

| Question | Answer                                                                                                              | Explanation                                                                                                           |
| -------- | ------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| 1        | **② Grade B**                                                                                                | The elseif statement checks conditions in order. 85 is not >= 90, but it is >= 80, so Grade B is returned             |
| 2        | **② Prevents moving to the next case**                                                                       | Without break, a fall-through occurs and the code of the next case is also executed                                   |
| 3        | **① 1 2 3**                                                                                                  | The for loop starts at$i=1 and increments by 1 until $i<=3, so 1, 2, 3 are output                                   |
| 4        | **② do { } while ($condition)**                                                                              | The do-while statement executes code first, then checks the condition, so it executes at least once                   |
| 5        | **② `foreach ($arr as $key => $value)`** | To get both key and value, use the `as $key => $value` format |                                                                                                                       |
| 6        | **③ 15**                                                                                                     | When add(10) is called,$a=10 and $b has the default value 5. Therefore, 10+5=15 is returned                         |
| 7        | **① break stops the loop, continue skips the current iteration**                                             | break terminates the loop completely, while continue only skips the current iteration and continues with the next one |
| 8        | **③ 24**                                                                                                     | factorial(4) = 4 × factorial(3) = 4 × 3 × 2 × 1 = 24                                                              |
| 9        | **④ All of ①, ②, and ③ are possible**                                                                     | All three methods of using global variables are possible in PHP                                                       |
| 10       | **② 1 2 4 5**                                                                                                | continue skips the current iteration, so when $i=3, echo is not executed                                              |

---

### **Practical Task Solutions**

#### **Task 1: Grade Determination Program - Answer**

```php
<?php
$score = 85;

if ($score >= 90) {
    $grade = "A";
} elseif ($score >= 80) {
    $grade = "B";
} elseif ($score >= 70) {
    $grade = "C";
} elseif ($score >= 60) {
    $grade = "D";
} else {
    $grade = "F";
}

echo "Score: " . $score . " points, Grade: " . $grade;
?>
```

**Output:**

```
Score: 85 points, Grade: B
```

---

#### **Task 2: Multiplication Table Output (Specific Table) - Answer**

```php
<?php
$table = 5;

echo "<h3>" . $table . " Times Table</h3>";

for ($i = 1; $i <= 9; $i++) {
    $result = $table * $i;
    echo $table . " × " . $i . " = " . $result . "<br>";
}
?>
```

**Output:**

```
5 Times Table
5 × 1 = 5
5 × 2 = 10
5 × 3 = 15
5 × 4 = 20
5 × 5 = 25
5 × 6 = 30
5 × 7 = 35
5 × 8 = 40
5 × 9 = 45
```

---

#### **Task 3: Array Sum and Average - Answer**

```php
<?php
$scores = array(80, 85, 90, 75, 88);

// Calculate sum
$total = 0;
foreach ($scores as $score) {
    $total += $score;
}

// Calculate average
$average = $total / count($scores);

echo "Sum: " . $total . " points, Average: " . $average . " points";

// Or using built-in function
// $total = array_sum($scores);
// $average = $total / count($scores);
?>
```

**Output:**

```
Sum: 418 points, Average: 83.6 points
```

---

#### **Task 4: Number Determination Function - Answer**

```php
<?php
/**
 * isEven function - Determine if a number is even
 */
function isEven($num) {
    return $num % 2 == 0;
}

// Determine even numbers from 1 to 10
for ($i = 1; $i <= 10; $i++) {
    if (isEven($i)) {
        echo $i . " is even<br>";
    }
}
?>
```

**Output:**

```
2 is even
4 is even
6 is even
8 is even
10 is even
```

---

#### **Task 5: Email Validation Function and User Processing - Answer**

```php
<?php
/**
 * is_valid_email function - Validate email format
 */
function is_valid_email($email) {
    return strpos($email, '@') !== false;
}

// User information array
$users = array(
    array('name' => 'John Smith', 'email' => 'john@example.com'),
    array('name' => 'Sarah Johnson', 'email' => 'invalid-email'),
    array('name' => 'Michael Park', 'email' => 'michael@domain.com')
);

// Validate and output user information
echo "<h3>【 Email Validation 】</h3>";
foreach ($users as $user) {
    $name = $user['name'];
    $email = $user['email'];
  
    if (is_valid_email($email)) {
        echo $name . "(" . $email . ") - Valid<br>";
    } else {
        echo $name . "(" . $email . ") - Invalid<br>";
    }
}
?>
```

**Output:**

```
【 Email Validation 】
John Smith(john@example.com) - Valid
Sarah Johnson(invalid-email) - Invalid
Michael Park(michael@domain.com) - Valid
```

---

## 💡 Problem-Solving Tips

### **Multiple Choice Strategy**

1. **Conditional Statement Problems**: Check conditions in order and find the first true condition
2. **Loop Problems**: Accurately identify the initial value, condition, and increment
3. **Function Problems**: Check parameters, return values, and scope
4. **break/continue**: break terminates the loop, continue skips only the current iteration

### **Practical Task Strategy**

1. **Conditional Statements**: Use if/elseif/else to handle various cases
2. **Loops**: for is suitable for fixed iterations, foreach for array traversal
3. **Functions**: Separate reusable logic into functions
4. **Array Sum**: Iterate with foreach and accumulate using += operator
5. **Function Return Values**: Use the return value to proceed with further operations

---

**Great effort! Keep it up! 💪**

---

Thank you for your attention.   
Cho Jeonghyun ([peterchokr@gmail.com](mailto:peterchokr@gmail.com)). Yeungnam University College

"Produced in collaboration with Claude and Gemini."
