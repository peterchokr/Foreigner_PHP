# 📝 Chapter 7 Practice Questions: File Upload & Error Handling

---

## 🎯 Multiple Choice (10 Questions)

#### **Question 1: What is essential for file upload in HTML form?**

① `method="GET"`
② `method="POST"` only
③ `enctype="multipart/form-data"` only
④ Both `method="POST"` and `enctype="multipart/form-data"`

---

#### **Question 2: To get the filename from $_FILES array?**

① `$_FILES['upload_file']['name']`
② `$_FILES['upload_file']['type']`
③ `$_FILES['upload_file']['tmp_name']`
④ `$_FILES['upload_file']['error']`

---

#### **Question 3: What is the $_FILES error value when no file is selected?**

① 0
② 1
③ 4
④ 7

---

#### **Question 4: What should be filled in ? in the following code?**

```php
$filename = $_FILES['upload_file']['name'];
$ext = strtolower(pathinfo($filename, ?));
```

① PATHINFO_DIRNAME
② PATHINFO_BASENAME
③ PATHINFO_EXTENSION
④ PATHINFO_FILENAME

---

#### **Question 5: What is 5MB in bytes?**

① 5000000
② 5 * 1024 * 1024
③ 5 * 1000 * 1000
④ 5242880 (results of ② and ④ are the same)

---

#### **Question 6: What is the whitelist approach in file extension validation?**

① List all dangerous extensions and exclude them
② List only allowed extensions and check them
③ Allow all extensions
④ Validate only by filename length

---

#### **Question 7: What is the result of the following code?**

```php
$ext = 'jpg';
$allowed = ['jpg', 'png', 'gif'];
echo in_array($ext, $allowed);
```

① true (output as 1)
② false (empty string)
③ 0
④ Error occurs

---

#### **Question 8: What is the role of move_uploaded_file()?**

① Checks file extension
② Moves temporary file to actual path
③ Encrypts file
④ Compresses file

---

#### **Question 9: What is the role of try-catch block?**

① Upload file
② Handle errors and prevent program interruption
③ Encrypt file
④ Validate form data

---

#### **Question 10: What does throw new Exception("message") do?**

① Terminates the program
② Generates error and moves to catch block
③ Prints message immediately
④ Deletes file

---

## 💻 Practical Tasks (5 Questions)

### **Task 1: File Upload HTML Form**

**Requirements:**

- Create HTML form (method="POST", enctype="multipart/form-data")
- File selection input field (name="upload_file")
- Upload button
- File selection required (required)

**Filename**: `file_upload_form.php`

```php
<?php
// Write your code here

?>
```

---

### **Task 2: File Validation (Size and Extension)**

**Requirements:**

- Receive file via POST
- Check if file is selected (error === 4)
- Validate file size (5MB or less)
- Validate file extension (jpg, png, gif only)
- Use whitelist (in_array)
- Use pathinfo(PATHINFO_EXTENSION)
- Use strtolower() for case uniformity
- Display validation result message

**Filename**: `file_validation.php`

```php
<?php
// Write your code here

?>
```

---

### **Task 3: File Name Sanitization and Saving**

**Requirements:**

- Extract file extension
- Generate new filename (using time() + rand())
- Set save path ('uploads/' directory)
- Move temporary file with move_uploaded_file()
- Display success/failure message
- Include HTML form

**Filename**: `file_save.php`

```php
<?php
// Write your code here

?>
```

---

### **Task 4: Error Handling (try-catch)**

**Requirements:**

- Wrap in try-catch block
- File not selected: throw new Exception()
- File error: throw new Exception()
- File size exceeded: throw new Exception()
- Extension not allowed: throw new Exception()
- File move failed: throw new Exception()
- Display error message in catch block
- Use $e->getMessage()

**Filename**: `file_upload_with_error.php`

```php
<?php
// Write your code here

?>
```

---

### **Task 5: File List Display**

**Requirements:**

- Check uploads/ directory (is_dir)
- Get file list with scandir()
- Remove '.' and '..' with array_diff()
- Extract extension with pathinfo()
- Create image file identification function
- Display images with `<img>` tag
- Display non-images with filename only
- Safe handling with htmlspecialchars()

**Filename**: `file_list.php`

```php
<?php
// Write your code here

?>
```

---

---

## ✅ Answers and Explanations

### **Multiple Choice Answers**

| Question | Answer                                                            | Explanation                                                 |
| -------- | ----------------------------------------------------------------- | ----------------------------------------------------------- |
| 1        | **④ Both method="POST" and enctype="multipart/form-data"** | Both attributes are essential for file upload               |
| 2        | **① `$_FILES['upload_file']['name']`**                   | Original filename is stored in ['name'] key                 |
| 3        | **③ 4**                                                    | When no file is selected, error is 4                        |
| 4        | **③ PATHINFO_EXTENSION**                                   | Use PATHINFO_EXTENSION to extract file extension            |
| 5        | **② or ④ (both 5242880)**                                 | 5MB = 5 * 1024 * 1024 = 5242880 bytes                       |
| 6        | **② List only allowed extensions and check them**          | Whitelist approach explicitly lists only allowed items      |
| 7        | **① true (output as 1)**                                   | in_array('jpg', ['jpg', 'png', 'gif']) returns true         |
| 8        | **② Moves temporary file to actual path**                  | move_uploaded_file() moves temporary file to save path      |
| 9        | **② Handle errors and prevent program interruption**       | try-catch allows program to continue even when error occurs |
| 10       | **② Generates error and moves to catch block**             | throw new Exception() generates an exception                |

---

### **Practical Task Solutions**

#### **Task 1: File Upload HTML Form - Answer**

```php
<?php
/**
 * file_upload_form.php - File upload form
 */

?>

<!DOCTYPE html>
<html>
<head>
    <title>File Upload</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 50px; }
        .container { max-width: 400px; padding: 20px; border: 1px solid #ddd; }
        input[type="file"] { padding: 8px; margin: 10px 0; width: 100%; }
        button { background-color: navy; color: white; padding: 10px; width: 100%; cursor: pointer; }
    </style>
</head>
<body>

<div class="container">
    <h1>📁 File Upload</h1>
  
    <!-- Required attributes: method="POST", enctype="multipart/form-data" -->
    <form method="POST" enctype="multipart/form-data">
        <label>Select file:</label>
        <!-- Required attributes: type="file", required -->
        <input type="file" name="upload_file" required>
      
        <button type="submit">Upload</button>
    </form>
</div>

</body>
</html>
```

**Verification:**
✓ Uses method="POST"
✓ Includes enctype="multipart/form-data"
✓ Contains `<input type="file" name="upload_file">`
✓ Includes required attribute
✓ Contains upload button

---

#### **Task 2: File Validation (Size and Extension) - Answer**

```php
<?php
/**
 * file_validation.php - File size and extension validation
 */

$validation_message = '';

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
  
    // Step 1: Is file selected?
    if ($_FILES['upload_file']['error'] === 4) {
        $validation_message = "❌ Please select a file";
    } else {
      
        // Step 2: Validate file size (5MB or less)
        $max_size = 5 * 1024 * 1024;  // Convert 5MB to bytes
        $file_size = $_FILES['upload_file']['size'];
      
        if ($file_size > $max_size) {
            $validation_message = "❌ File size exceeds 5MB";
        } else {
          
            // Step 3: Validate file extension (whitelist)
            $filename = $_FILES['upload_file']['name'];
            // Extract extension with pathinfo(), unify case with strtolower()
            $ext = strtolower(pathinfo($filename, PATHINFO_EXTENSION));
          
            // Allowed extensions list
            $allowed = ['jpg', 'jpeg', 'png', 'gif'];
          
            // Check with in_array()
            if (!in_array($ext, $allowed)) {
                $validation_message = "❌ Only jpg, png, gif files can be uploaded";
            } else {
                $validation_message = "✅ File validation complete";
            }
        }
    }
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>File Validation</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 50px; }
        .container { max-width: 400px; padding: 20px; border: 1px solid #ddd; }
        input[type="file"] { padding: 8px; margin: 10px 0; width: 100%; }
        button { background-color: navy; color: white; padding: 10px; width: 100%; cursor: pointer; }
        .message { padding: 10px; margin: 10px 0; }
        .success { color: green; }
        .error { color: red; }
    </style>
</head>
<body>

<div class="container">
    <h1>Validation</h1>
  
    <form method="POST" enctype="multipart/form-data">
        <input type="file" name="upload_file" required>
        <button type="submit">Validate</button>
    </form>
  
    <?php if ($validation_message): ?>
        <div class="message <?php echo strpos($validation_message, '✅') ? 'success' : 'error'; ?>">
            <?php echo $validation_message; ?>
        </div>
    <?php endif; ?>
</div>

</body>
</html>
```

**Verification:**
✓ Checks error === 4 for no file selection
✓ Validates file size (5MB)
✓ Extracts extension with pathinfo(PATHINFO_EXTENSION)
✓ Unifies case with strtolower()
✓ Whitelist ['jpg', 'jpeg', 'png', 'gif']
✓ Checks with in_array()
✓ Displays validation result message

---

#### **Task 3: File Name Sanitization and Saving - Answer**

```php
<?php
/**
 * file_save.php - File name sanitization and saving
 */

$save_message = '';
$saved_filename = '';

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
  
    // Step 1: Extract file extension
    $filename = $_FILES['upload_file']['name'];
    $ext = strtolower(pathinfo($filename, PATHINFO_EXTENSION));
  
    // Step 2: Generate new filename (using time() + rand())
    // time(): current time (Unix timestamp)
    // rand(1000, 9999): random number between 1000-9999
    // Result: 1673456789_5432.jpg
    $new_filename = time() . '_' . rand(1000, 9999) . '.' . $ext;
  
    // Step 3: Set save path
    $upload_dir = 'uploads/';
    $destination = $upload_dir . $new_filename;
  
    // Step 4: Move temporary file with move_uploaded_file()
    $tmp_file = $_FILES['upload_file']['tmp_name'];
  
    if (move_uploaded_file($tmp_file, $destination)) {
        $save_message = "✅ File '{$new_filename}' has been saved";
        $saved_filename = $new_filename;
    } else {
        $save_message = "❌ File save failed";
    }
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>File Save</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 50px; }
        .container { max-width: 400px; padding: 20px; border: 1px solid #ddd; }
        input[type="file"] { padding: 8px; margin: 10px 0; width: 100%; }
        button { background-color: navy; color: white; padding: 10px; width: 100%; cursor: pointer; }
        .message { padding: 10px; margin: 10px 0; }
        .success { color: green; }
        .error { color: red; }
    </style>
</head>
<body>

<div class="container">
    <h1>File Save</h1>
  
    <form method="POST" enctype="multipart/form-data">
        <label>Select file:</label>
        <input type="file" name="upload_file" required>
        <button type="submit">Save</button>
    </form>
  
    <?php if ($save_message): ?>
        <div class="message <?php echo strpos($save_message, '✅') ? 'success' : 'error'; ?>">
            <?php echo $save_message; ?>
        </div>
    <?php endif; ?>
  
    <?php if ($saved_filename): ?>
        <p>Saved file: <strong><?php echo htmlspecialchars($saved_filename); ?></strong></p>
    <?php endif; ?>
</div>

</body>
</html>
```

**Verification:**
✓ Extracts extension with pathinfo(PATHINFO_EXTENSION)
✓ Unifies case with strtolower()
✓ Generates new filename with time() + rand(1000, 9999)
✓ Uses 'uploads/' save directory
✓ Moves temporary file with move_uploaded_file()
✓ Displays success/failure message
✓ Safe handling with htmlspecialchars()

---

#### **Task 4: Error Handling (try-catch) - Answer**

```php
<?php
/**
 * file_upload_with_error.php - Error handling with try-catch
 */

$upload_message = '';

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
  
    try {
        // Step 1: Is file selected?
        if ($_FILES['upload_file']['error'] === 4) {
            throw new Exception("Please select a file");
        }
      
        // Step 2: Check upload error
        if ($_FILES['upload_file']['error'] !== 0) {
            throw new Exception("Error occurred during file upload");
        }
      
        // Step 3: Validate file size
        $max_size = 5 * 1024 * 1024;
        if ($_FILES['upload_file']['size'] > $max_size) {
            throw new Exception("File size exceeds 5MB");
        }
      
        // Step 4: Validate file extension
        $filename = $_FILES['upload_file']['name'];
        $ext = strtolower(pathinfo($filename, PATHINFO_EXTENSION));
        $allowed = ['jpg', 'jpeg', 'png', 'gif'];
      
        if (!in_array($ext, $allowed)) {
            throw new Exception("Only image files can be uploaded");
        }
      
        // Step 5: Move file
        $new_filename = time() . '_' . rand(1000, 9999) . '.' . $ext;
        $destination = 'uploads/' . $new_filename;
      
        if (!move_uploaded_file($_FILES['upload_file']['tmp_name'], $destination)) {
            throw new Exception("File save failed");
        }
      
        // Success
        $upload_message = "✅ File '{$new_filename}' has been uploaded";
      
    } catch (Exception $e) {
        // Error handling: return message with $e->getMessage()
        $upload_message = "❌ " . $e->getMessage();
    }
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>File Upload (Error Handling)</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 50px; }
        .container { max-width: 400px; padding: 20px; border: 1px solid #ddd; }
        input[type="file"] { padding: 8px; margin: 10px 0; width: 100%; }
        button { background-color: navy; color: white; padding: 10px; width: 100%; cursor: pointer; }
        .message { padding: 10px; margin: 10px 0; }
        .success { color: green; background-color: #f0f0f0; }
        .error { color: red; background-color: #ffe6e6; }
    </style>
</head>
<body>

<div class="container">
    <h1>File Upload (Error Handling)</h1>
  
    <form method="POST" enctype="multipart/form-data">
        <label>Select file:</label>
        <input type="file" name="upload_file" required>
        <button type="submit">Upload</button>
    </form>
  
    <?php if ($upload_message): ?>
        <div class="message <?php echo strpos($upload_message, '✅') ? 'success' : 'error'; ?>">
            <?php echo $upload_message; ?>
        </div>
    <?php endif; ?>
</div>

</body>
</html>
```

**Verification:**
✓ try-catch block structure
✓ Generates error with throw new Exception("message")
✓ Checks error === 4 for no file selection
✓ Checks error !== 0 for upload error
✓ Validates file size
✓ Validates extension
✓ Handles move_uploaded_file() failure
✓ Uses $e->getMessage() in catch block

---

#### **Task 5: File List Display - Answer**

```php
<?php
/**
 * file_list.php - Display file list in uploads/ directory
 */

// Image extension list
$image_extensions = ['jpg', 'jpeg', 'png', 'gif'];

// Get file list from uploads directory
$upload_dir = 'uploads/';
$files = [];

// is_dir(): Check if directory exists
if (is_dir($upload_dir)) {
    // scandir(): Return all files and folders in directory as array
    // array_diff(): Remove array2 elements from array1
    // ['.', '..'] = current and parent directory to remove
    $files = array_diff(scandir($upload_dir), ['.', '..']);
}

/**
 * Function to check if file is image
 * @param string $filename Filename
 * @param array $allowed_ext Allowed extensions
 * @return bool True if image, false otherwise
 */
function isImage($filename, $allowed_ext) {
    // Extract extension with pathinfo()
    $ext = strtolower(pathinfo($filename, PATHINFO_EXTENSION));
  
    // Check with in_array()
    return in_array($ext, $allowed_ext);
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>File List</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 50px; }
        .container { max-width: 800px; }
        h1 { color: navy; }
        .file-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap: 20px; }
        .file-item { border: 1px solid #ddd; padding: 15px; text-align: center; }
        .file-item img { max-width: 100%; max-height: 150px; margin-bottom: 10px; }
        .file-name { font-size: 12px; color: #666; word-break: break-all; }
    </style>
</head>
<body>

<div class="container">
    <h1>📁 Uploaded File List</h1>
  
    <div class="file-grid">
        <?php foreach ($files as $file): ?>
            <div class="file-item">
                <!-- Image detection: use isImage() function -->
                <?php if (isImage($file, $image_extensions)): ?>
                    <!-- Image file: display with <img> tag -->
                    <img src="<?php echo htmlspecialchars($upload_dir . $file); ?>" 
                         alt="<?php echo htmlspecialchars($file); ?>">
                <?php else: ?>
                    <!-- Non-image file: display file icon -->
                    <div style="font-size: 48px;">📄</div>
                <?php endif; ?>
              
                <!-- Display filename (safe handling with htmlspecialchars) -->
                <div class="file-name"><?php echo htmlspecialchars($file); ?></div>
            </div>
        <?php endforeach; ?>
    </div>
  
    <!-- Display message if no files -->
    <?php if (empty($files)): ?>
        <p>No uploaded files.</p>
    <?php endif; ?>
</div>

</body>
</html>
```

**Verification:**
✓ Checks directory existence with is_dir()
✓ Gets file list with scandir()
✓ Removes '.' and '..' with array_diff()
✓ Extracts extension with pathinfo(PATHINFO_EXTENSION)
✓ Unifies case with strtolower()
✓ Creates isImage() function
✓ Detects images with in_array()
✓ Displays images with `<img>` tag
✓ Displays non-images with filename only
✓ Safe handling with htmlspecialchars()

---

## 💡 Problem-Solving Tips

### **Multiple Choice Strategy**

1. **File Upload Essential**: method="POST" + enctype="multipart/form-data"
2. **$_FILES Structure**: ['name', 'type', 'size', 'tmp_name', 'error']
3. **No File Selected**: error === 4
4. **Extract Extension**: pathinfo(PATHINFO_EXTENSION) + strtolower()
5. **Size Calculation**: 5MB = 5 * 1024 * 1024
6. **Whitelist**: Explicitly list only allowed items
7. **Array Check**: in_array($ext, $allowed)
8. **Move File**: move_uploaded_file($tmp, $dest)
9. **Error Handling**: Wrap with try-catch
10. **Raise Exception**: throw new Exception()

### **Practical Task Strategy**

1. **Form Creation**: method="POST" + enctype="multipart/form-data" + type="file"
2. **Validation**: Check error → Check size → Check extension (order matters)
3. **Filename**: time() + rand() + extension (prevent duplicates)
4. **Save**: move_uploaded_file($tmp, $dest)
5. **Error Handling**: try-catch + throw new Exception
6. **File List**: is_dir() → scandir() → array_diff() → pathinfo()
7. **Output**: Safe handling with htmlspecialchars()

---

**Great effort! Keep it up! 💪**

---

Thank you for your attention.   
Cho Jeonghyun ([peterchokr@gmail.com](mailto:peterchokr@gmail.com)). Yeungnam University College

"Produced in collaboration with Claude and Gemini."
