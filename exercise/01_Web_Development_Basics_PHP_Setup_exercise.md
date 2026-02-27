# 📝 Chapter 1 Practice Questions: Web Development Basics & PHP Environment Setup

---

## 🎯 Multiple Choice (10 Questions)

#### **Question 1: What is the main reason to use method="POST" in an HTML Form tag?**

① Data is displayed in the URL for quick transmission
② Data is included in the request body, providing higher security and no size limit
③ Images or files cannot be transmitted to the server
④ The browser caches the data for faster loading

---

#### **Question 2: Which of the following input types allows selecting only 1 option from multiple items?**

① `<input type="checkbox">`
② `<input type="radio">`
③ `<input type="select">`
④ `<input type="text">`

---

#### **Question 3: Which correctly lists CSS selector specificity from highest to lowest?**

① Type selector > Class selector > ID selector
② ID selector > Class selector > Type selector
③ Class selector > ID selector > Type selector
④ All three selectors have the same priority

---

#### **Question 4: What is the correct way to select an element with ID "myButton" in JavaScript?**

① `document.selectElementById('myButton')`
② `document.getElementById('myButton')`
③ `document.getID('myButton')`
④ `element.getElementById('myButton')`

---

#### **Question 5: What is the correct role of addEventListener?**

① Deletes an HTML element
② Registers an event listener on an element to detect events (click, mouse movement, etc.)
③ Applies CSS styles to an element
④ Sends form data to the server

---

#### **Question 6: What is the difference between echo and print in PHP?**

① echo can pass multiple arguments separated by commas, print can only pass one
② print can pass multiple arguments, echo can only pass one
③ echo is slower and print is faster
④ There is no functional difference, only syntax differs

---

#### **Question 7: Which is the most appropriate description of PHPMyAdmin's role?**

① Edits and executes PHP code
② Manages MySQL databases in a web browser
③ Controls the Apache web server
④ Debugs JavaScript code

---

#### **Question 8: What is the result of executing the following JavaScript code?**

```javascript
const element = document.getElementById('myDiv');
element.textContent = '<strong>bold text</strong>';
```

① The screen displays "**bold text**" (bold formatting applied)
② The screen displays "`<strong>`bold text`</strong>`" as plain text
③ The HTML tag is interpreted and bold text is displayed
④ The element is deleted and nothing is displayed

---

#### **Question 9: What is the final text color in the following HTML/CSS code?**

```html
<style>
    p { color: green; }
    .highlight { color: blue; }
    #special { color: red; }
</style>

<p class="highlight" id="special">Test</p>
```

① green
② blue
③ red
④ All three colors alternate

---

#### **Question 10: Given the following table structure in MySQL, which interpretation is correct?**

```sql
CREATE TABLE students (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    age INT,
    grade DECIMAL(3,2)
);
```

① More than one student can be registered with the same email
② The name column must have a value, and email cannot be duplicated
③ The grade column can only store integer values
④ The id column must be manually entered

---

## 💻 Practical Tasks (5 Questions)

### **Task 1: Create an HTML Form**

**Requirements:**

- Create a sign-up form
- Fields: Full Name (text), Email (email), Password (password), Agree to Terms (checkbox), Submit button
- Apply basic CSS styling (background color, padding, border, etc.)

**Filename**: `signup.html`

```html
<!-- Write your code here -->

```

---

### **Task 2: CSS Styling**

**Requirements:**

- Style the following HTML with CSS
- ID selector `#title` (blue color, center alignment, size 24px)
- Class selector `.highlight` (yellow background, bold text)
- Type selector `p` tag (gray text color)

```html
<h1 id="title">Title</h1>
<p>This is a regular paragraph.</p>
<p class="highlight">This is a highlighted paragraph.</p>
```

**Write CSS here**:

```css
/* Write your CSS here */

```

---

### **Task 3: JavaScript DOM Manipulation**

**Requirements:**

- When the button is clicked, the content of `<div id="message">` changes to "Button has been clicked!"
- When the button is clicked again, restore to the original content
- Change the text color to red when the button is clicked

**HTML:**

```html
<button id="myButton">Click me</button>
<div id="message">Original message</div>
```

**Write JavaScript here:**

```javascript
// Write your code here

```

---

### **Task 4: Basic PHP Program**

**Requirements:**

- Store a name, math score, and English score in variables
- Calculate the average of the two scores
- Use echo to output in the format: "Name: John Smith, Average: 90.5 points"

**Filename**: `student.php`

```php
<?php
// Write your code here

?>
```

---

### **Task 5: Create an HTML Table with PHP**

**Requirements:**

- Store the following information for 3 students in a PHP array: (Name, Math, Science)
  - James Chen: Math 85, Science 90
  - Sarah Johnson: Math 92, Science 88
  - Michael Park: Math 78, Science 95
- Use PHP loops to dynamically generate an HTML table
- Table format: Name | Math | Science | Total

**Filename**: `score_table.php`

```php
<?php
/**
 * score_table.php
 * 
 * Requirements:
 * 1. Store student data in an array
 * 2. Process each student with foreach loop
 * 3. Use echo to create HTML table
 */

// Write your code here

?>
```

---

---

## ✅ Answers and Explanations

### **Multiple Choice Answers**

| Question | Answer                                                    | Explanation                                                                                              |
| -------- | --------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| 1        | **② POST**                                         | POST sends data in the request body, providing higher security and no size limit                         |
| 2        | **② radio**                                        | Radio buttons allow selecting only 1 option from multiple items. Checkboxes allow multiple selections    |
| 3        | **② ID > Class > Type**                            | CSS selector specificity: ID(100) > Class(10) > Type(1)                                                  |
| 4        | **② getElementById()**                             | Use `getElementById()` method to select an element by its ID in JavaScript                             |
| 5        | **② Register event listener**                      | addEventListener registers an event listener on an element to detect specific events                     |
| 6        | **① echo passes multiple, print passes one**       | echo can pass multiple arguments separated by commas, print can only pass one                            |
| 7        | **② MySQL management**                             | PHPMyAdmin is a tool for managing MySQL databases in a web browser                                       |
| 8        | **② Display as text**                              | textContent does not interpret HTML tags and displays them as plain text                                 |
| 9        | **③ red**                                          | The ID selector (#special) has higher specificity than class selector (.highlight) and type selector (p) |
| 10       | **② name is required, email cannot be duplicated** | NOT NULL means required input, UNIQUE means no duplicates                                                |

---

### **Practical Task Solutions**

#### **Task 1: Create an HTML Form - Answer**

```html
<!DOCTYPE html>
<html>
<head>
    <title>Sign Up</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 400px;
            margin: 50px auto;
        }
      
        .form-container {
            background: #f9f9f9;
            padding: 30px;
            border: 1px solid #ddd;
            border-radius: 5px;
        }
      
        label {
            display: block;
            margin-top: 15px;
            font-weight: bold;
            color: #333;
        }
      
        input[type="text"],
        input[type="email"],
        input[type="password"] {
            width: 100%;
            padding: 10px;
            margin-top: 5px;
            border: 1px solid #ccc;
            border-radius: 3px;
            box-sizing: border-box;
        }
      
        input[type="checkbox"] {
            margin-top: 10px;
        }
      
        button {
            width: 100%;
            padding: 12px;
            margin-top: 20px;
            background: navy;
            color: white;
            border: none;
            border-radius: 3px;
            font-size: 16px;
            cursor: pointer;
        }
      
        button:hover {
            background: #00008b;
        }
    </style>
</head>
<body>

<div class="form-container">
    <h2>Sign Up</h2>
  
    <form method="POST" action="process.php">
        <label for="name">Full Name</label>
        <input type="text" id="name" name="name" placeholder="Enter your full name">
      
        <label for="email">Email</label>
        <input type="email" id="email" name="email" placeholder="Enter your email">
      
        <label for="password">Password</label>
        <input type="password" id="password" name="password" placeholder="Enter your password">
      
        <label>
            <input type="checkbox" name="agree" value="yes"> I agree to the terms and conditions
        </label>
      
        <button type="submit">Sign Up</button>
    </form>
</div>

</body>
</html>
```

---

#### **Task 2: CSS Styling - Answer**

```css
#title {
    color: blue;
    text-align: center;
    font-size: 24px;
}

.highlight {
    background-color: yellow;
    font-weight: bold;
}

p {
    color: gray;
}
```

---

#### **Task 3: JavaScript DOM Manipulation - Answer**

```javascript
const button = document.getElementById('myButton');
const message = document.getElementById('message');
const originalText = "Original message";
let isClicked = false;

button.addEventListener('click', function() {
    if (isClicked) {
        // Restore to original state
        message.textContent = originalText;
        message.style.color = 'black';
        isClicked = false;
    } else {
        // Change to new state
        message.textContent = 'Button has been clicked!';
        message.style.color = 'red';
        isClicked = true;
    }
});
```

---

#### **Task 4: Basic PHP Program - Answer**

```php
<?php
// Store student information
$name = "John Smith";
$math = 90;
$english = 91;

// Calculate average
$average = ($math + $english) / 2;

// Output result
echo "Name: " . $name . ", Average: " . $average . " points";
// Or
// printf("Name: %s, Average: %.1f points", $name, $average);
?>
```

**Output:**

```
Name: John Smith, Average: 90.5 points
```

---

#### **Task 5: Create an HTML Table with PHP - Answer**

```php
<?php
/**
 * score_table.php
 * 
 * Store student scores in an array and
 * create an HTML table using a loop
 */

// 1️⃣ Student data array
$students = array(
    array('name' => 'James Chen', 'math' => 85, 'science' => 90),
    array('name' => 'Sarah Johnson', 'math' => 92, 'science' => 88),
    array('name' => 'Michael Park', 'math' => 78, 'science' => 95)
);

?>

<!DOCTYPE html>
<html>
<head>
    <title>Student Scores</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 600px;
            margin: 30px auto;
        }
      
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
      
        thead {
            background-color: navy;
            color: white;
        }
      
        th, td {
            padding: 12px;
            text-align: center;
            border: 1px solid #ddd;
        }
    </style>
</head>
<body>

<h1>Student Score Report</h1>

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Math</th>
            <th>Science</th>
            <th>Total</th>
        </tr>
    </thead>
    <tbody>
        <?php
        // 2️⃣ Process each student with loop
        foreach ($students as $student) {
            $total = $student['math'] + $student['science'];
          
            // 3️⃣ Create table row with HTML
            echo "<tr>";
            echo "<td>" . $student['name'] . "</td>";
            echo "<td>" . $student['math'] . "</td>";
            echo "<td>" . $student['science'] . "</td>";
            echo "<td>" . $total . "</td>";
            echo "</tr>";
        }
        ?>
    </tbody>
</table>

</body>
</html>
```

**Output:**

| Name          | Math | Science | Total |
| ------------- | ---- | ------- | ----- |
| James Chen    | 85   | 90      | 175   |
| Sarah Johnson | 92   | 88      | 180   |
| Michael Park  | 78   | 95      | 173   |

---

## 💡 Problem-Solving Tips

### **Multiple Choice Strategy**

1. **Read the question twice**: First for overall understanding, second for selecting the answer
2. **Review each option one by one**: Confirm "Is this really correct?"
3. **If unsure, use elimination**: First eliminate obviously incorrect options

### **Practical Task Strategy**

1. **Understand requirements clearly**: Re-read requirements and create a checklist
2. **Start with basics**: Complete basic structure before complex features
3. **Test frequently**: Verify functionality at each step
4. **Add comments**: Include comments explaining the purpose and operation of code

---

**Great effort! You can do this! 💪**

---

Thank you for your attention.   
Cho Jeonghyun ([peterchokr@gmail.com](mailto:peterchokr@gmail.com)). Yeungnam University College

"Produced in collaboration with Claude and Gemini."
