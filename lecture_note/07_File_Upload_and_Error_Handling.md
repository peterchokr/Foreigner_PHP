# Chapter 7. File Upload and Error Handling

---

## 📚 Learning Objectives

After completing this chapter, you will be able to:

✅ Create HTML forms for file upload
✅ Understand the structure of the $_FILES array
✅ Validate file extension and size
✅ Save uploaded files to the server
✅ Handle errors that occur and display appropriate messages

이 장을 학습하면 다음을 할 수 있습니다:

✅ 파일 업로드 HTML 폼을 작성할 수 있습니다.
✅ $_FILES 배열의 구조를 이해하고 사용할 수 있습니다.
✅ 파일의 확장자와 크기를 검증할 수 있습니다.
✅ 업로드된 파일을 서버에 저장할 수 있습니다.
✅ 발생하는 에러를 처리하고 적절한 메시지를 표시할 수 있습니다.

---

## 1️⃣ File Upload Basics 

### 1-1 What is File Upload? 

File upload is a function that allows users to send files from their computer to the server.

파일 업로드는 사용자가 자신의 컴퓨터에 있는 파일을 서버로 전송하는 기능입니다.

**File upload process:**(파일 업로드 과정:)

```
1. User selects file from HTML form
   (사용자가 HTML 폼에서 파일 선택)

2. Form is submitted and file is sent to server
   (폼을 제출하면 파일이 서버로 전송)

3. PHP server receives file information in $_FILES array
   (서버의 PHP가 $_FILES 배열에서 파일 정보 수신)

4. File is validated (extension, size, malware check)
   (파일을 검증 - 확장자, 크기, 악성코드 등)

5. After validation, file is saved to server
   (검증 완료하면 서버에 저장)

6. File path is recorded in database (optional)
   (데이터베이스에 파일 경로 기록 - 옵션)

Characteristics: (특징:)

- Various file types can be uploaded: images, documents, videos, etc.
  (이미지, 문서, 영상 등 다양한 파일 업로드 가능)

- Security is very important (prevent malicious files)
  (보안이 매우 중요함 - 악성 파일 방지)

- File size limit is necessary (manage server storage)
  (파일 크기 제한 필요 - 서버 용량 관리)
```

### 1-2 File Upload HTML Form (파일 업로드 HTML 폼)

**Creating a file upload form:**(파일 업로드 폼 작성:)

Three things are required to upload files in HTML form:

HTML 폼에서 파일을 업로드하려면 다음 3가지가 필수입니다:

```html
<!-- 1. enctype="multipart/form-data" is required! (필수!) -->
<!-- 2. method="POST" is required! (필수!) -->
<!-- 3. <input type="file"> is required! (필수!) -->

<form method="POST" enctype="multipart/form-data">
    <input type="file" name="upload_file" required>
    <button type="submit">Upload</button>
</form>
```

**Meaning of each attribute:**(각 항목의 의미:)

```
enctype="multipart/form-data"

- "multipart" = ability to transmit multiple types of data
  ("multipart" = 여러 종류의 데이터 전송 가능)

- This attribute is required when sending files
  (파일을 전송할 때는 반드시 이 속성 필요)

- Without it, files are not transmitted properly
  (없으면 파일이 제대로 전송되지 않음)

method="POST"

- GET appends data to URL (files not possible)
  (GET은 URL에 데이터를 붙여서 전송 - 파일 불가)

- POST includes data in request body (files possible)
  (POST는 요청 본문에 데이터를 포함해서 전송 - 파일 가능)

- POST must be used for file upload
  (파일 업로드 시 반드시 POST 사용)

<input type="file">

- Setting type="file" displays file selection dialog
  (type="file"로 설정해야 파일 선택 대화상자 표시)

- name attribute distinguishes files ($_FILES['upload_file'])
  (name 속성으로 파일을 구분)
```

### 1-3 Structure of $_FILES Array ($_FILES 배열의 구조)

**File information is stored in the $_FILES array:**

(파일 정보는 $_FILES 배열에 저장됩니다:)

```
When user uploads file with name="upload_file",
(사용자가 name="upload_file"로 파일을 업로드하면)

PHP automatically creates $_FILES['upload_file'] array
(PHP가 자동으로 $_FILES['upload_file'] 배열을 생성합니다)

Structure: (구조:)

$_FILES['upload_file']['name']     // Original filename (원본 파일명 - user_image.jpg)
$_FILES['upload_file']['type']     // MIME type (MIME 타입 - image/jpeg)
$_FILES['upload_file']['size']     // File size in bytes (파일 크기 - 바이트 단위)
$_FILES['upload_file']['tmp_name'] // Temporary path (임시 저장 경로 - /tmp/php123...)
$_FILES['upload_file']['error']    // Error code (에러 코드 - 0 = success/성공)

Example: (예시:)

Array
(
    [upload_file] => Array
    (
        [name]     => 'photo.jpg'
        [type]     => 'image/jpeg'
        [size]     => 1024000
        [tmp_name] => '/tmp/php123abc'
        [error]    => 0
    )
)
```

---

## 2️⃣ File Validation (파일 검증)

### 2-1 Why is File Validation Necessary? (왜 파일 검증이 필요한가?)

**Risks of file upload:**(파일 업로드의 위험성:)

```
❌ Dangerous situations: (위험한 상황들:)

- Uploading malicious PHP file can hack the server
  (악성 PHP 파일을 업로드하면 서버를 해킹할 수 있음)

- Very large file upload exceeds server storage
  (용량이 너무 큰 파일을 업로드하면 서버 용량 초과)

- Uploading different file format than expected
  (mp4를 jpg라고 위장해서 업로드)
  (예상과 다른 파일 형식 업로드)

- Special characters in filename cause problems
  (파일명에 특수문자가 있으면 저장 시 문제 발생)

✅ Solution: (해결 방법:)

- File extension validation (use whitelist)
  (파일 확장자 검증 - 화이트리스트 사용)

- File size validation (파일 크기 검증)

- MIME type validation (MIME 타입 검증)

- Filename sanitization (파일명 정제)
```

### 2-2 File Extension Validation (파일 확장자 검증)

**Checking file extension:**

```php
<?php

// Get uploaded filename
// (업로드된 파일명 가져오기)
$filename = $_FILES['upload_file']['name'];

// Extract file extension
// (파일 확장자 추출)

// pathinfo($path, PATHINFO_EXTENSION)
// = Extract only extension part from file path
// (= 파일 경로에서 확장자 부분만 추출)
// Example: "photo.jpg" → "jpg"
// (예: "photo.jpg" → "jpg")

// strtolower() = Convert characters to lowercase
// (= 문자를 소문자로 변환)
// Example: "JPG" → "jpg" (unify case)
// (예: "JPG" → "jpg" - 대소문자 통일)
$ext = strtolower(pathinfo($filename, PATHINFO_EXTENSION));

// Allowed extension list (whitelist)
// (허용하는 확장자 목록 - 화이트리스트)
$allowed = ['jpg', 'jpeg', 'png', 'gif'];

// Validate extension
// (확장자 검증)

// in_array($needle, $haystack)
// = Does $needle exist in $haystack array?
// (= $needle이 $haystack 배열에 존재하는가?)
// in_array('jpg', ['jpg', 'png']) → true
// in_array('exe', ['jpg', 'png']) → false

// !in_array = return true if does not exist
// (!in_array = 존재하지 않으면 true)
if (!in_array($ext, $allowed)) {
    echo "❌ Only jpg, png, gif files can be uploaded";
    exit;
}

echo "✅ File extension is correct";

?>
```

### 2-3 File Size Validation (파일 크기 검증)

**Checking file size:**

```php
<?php

// Get file size (in bytes)
// (파일 크기 가져오기 - 바이트 단위)
$file_size = $_FILES['upload_file']['size'];

// Set maximum size (5MB = 5 * 1024 * 1024 bytes)
// (최대 크기 설정 - 5MB = 5 * 1024 * 1024 바이트)
$max_size = 5 * 1024 * 1024;

// Validate size
// (크기 검증)
if ($file_size > $max_size) {
    // Display file size in MB
    // (파일 크기를 MB 단위로 표시)
    // (1024 * 1024) = convert 1MB to bytes
    // ((1024 * 1024) = 1MB를 바이트로 환산)
    $max_mb = $max_size / (1024 * 1024);
    echo "❌ File size must be {$max_mb}MB or less";
    exit;
}

// Display file size in MB
// (파일 크기를 MB 단위로 표시)

// round($number, decimal places) = round to specified decimals
// (round($number, 소수점자릿수) = 지정된 자릿수로 반올림)
// round(1024.56789, 2) → 1024.57
// (round(1024.56789, 2) → 1024.57)
$file_size_mb = round($file_size / (1024 * 1024), 2);
echo "✅ File size: {$file_size_mb}MB (allowed)";

?>
```

### 2-4 MIME Type Validation (MIME 타입 검증)

**Checking file's actual format:**

```php
<?php

// Get MIME type
// (MIME 타입 가져오기)

// Browser automatically sets this when user uploads
// (사용자가 업로드할 때 브라우저가 자동으로 설정)
$mime_type = $_FILES['upload_file']['type'];

// Allowed MIME types (more secure)
// (허용하는 MIME 타입 - 더 안전함)
$allowed_mime = [
    'image/jpeg',
    'image/png',
    'image/gif'
];

// Validate MIME type
// (MIME 타입 검증)
if (!in_array($mime_type, $allowed_mime)) {
    echo "❌ Only image files can be uploaded";
    echo "Received type: {$mime_type}";
    exit;
}

echo "✅ MIME type is correct";

?>
```

### 2-5 Upload Error Code Check (업로드 에러 코드 확인)

**Checking errors that occurred during file upload:**

```php
<?php

// $_FILES['upload_file']['error'] = error code
// ($_FILES['upload_file']['error'] = 에러 코드)
// 0 = success (성공), other = error occurred (그 외 = 에러 발생)

$error_code = $_FILES['upload_file']['error'];

// Error messages for each code
// (에러 코드별 메시지)
$error_messages = [
    0 => 'File upload successful',
    1 => 'File size too large (php.ini limit)',
    2 => 'File size exceeds HTML form limit',
    3 => 'File uploaded partially',
    4 => 'No file selected',
    6 => 'Temporary directory not found',
    7 => 'File cannot be saved to server',
];

// Check error
// (에러 확인)
if ($error_code !== 0) {
    echo "❌ " . $error_messages[$error_code];
    exit;
}

echo "✅ File upload successful";

?>
```

---

## 3️⃣ File Save and Move (파일 저장 및 이동)

### 3-1 move_uploaded_file() Function 

**Saving uploaded file to server:**(업로드된 파일을 서버에 저장하기:)

```php
<?php

// Set temporary path and save path
// (임시 경로와 저장할 경로 설정)
$tmp_path = $_FILES['upload_file']['tmp_name'];  // Temporary path (임시 경로)
$upload_dir = 'uploads/';                         // Directory to save (저장할 디렉토리)
$filename = $_FILES['upload_file']['name'];      // Original filename (원본 파일명)
$destination = $upload_dir . $filename;          // Final path (최종 경로)

// move_uploaded_file() = Move temporary file to actual path
// (move_uploaded_file() = 임시 파일을 실제 경로로 이동)

// First parameter: temporary path
// (첫 번째 인자: 임시 경로)

// Second parameter: final path to save
// (두 번째 인자: 저장할 최종 경로)
if (move_uploaded_file($tmp_path, $destination)) {
    // File move successful
    // (파일 이동 성공)
    echo "✅ File saved to {$destination}";
} else {
    // File move failed
    // (파일 이동 실패)
    echo "❌ File save failed";
}

?>
```

### 3-2 File Name Sanitization and Duplication Prevention (파일명 정제 및 중복 방지)

**Saving with safe filename:**(안전한 파일명으로 저장하기:)

```php
<?php

// Step 1: Separate extension from original filename
// (단계 1: 원본 파일명에서 확장자 분리)
$original_name = $_FILES['upload_file']['name'];

// pathinfo() = Extract information from file path
// (pathinfo() = 파일 경로에서 정보 추출)

// PATHINFO_EXTENSION = Extract only extension
// (PATHINFO_EXTENSION = 확장자만 추출)
// Example: "photo.jpg" → "jpg"
// (예: "photo.jpg" → "jpg")
$ext = strtolower(pathinfo($original_name, PATHINFO_EXTENSION));

// Step 2: Generate new filename (timestamp + random number)
// (단계 2: 새로운 파일명 생성 - 타임스탬프 + 난수)

// Purpose: Prevent duplication + Remove special characters
// (목적: 중복 방지 + 특수문자 제거)

// time() = Return current time as Unix timestamp
// (time() = 현재 시간을 유닉스 타임스탐프로 반환)
// Example: 1673456789 (seconds since January 1, 1970)
// (예: 1673456789 - 1970년 1월 1일부터의 초 단위)

// rand(1000, 9999) = Generate random number between 1000-9999
// (rand(1000, 9999) = 1000~9999 사이의 난수 생성)
// Example: 5432
// (예: 5432)

// Result: "1673456789_5432.jpg"
// (결과: "1673456789_5432.jpg")
$new_filename = time() . '_' . rand(1000, 9999) . '.' . $ext;

// Step 3: Set save path
// (단계 3: 저장 경로 설정)
$upload_dir = 'uploads/';
$destination = $upload_dir . $new_filename;

// Step 4: Move file
// (단계 4: 파일 이동)

// move_uploaded_file($from, $to)
// = Move file from temporary path to actual path
// (= 임시 경로의 파일을 실제 경로로 이동)

// Return value: true if successful, false if failed
// (반환값: 성공하면 true, 실패하면 false)
if (move_uploaded_file($_FILES['upload_file']['tmp_name'], $destination)) {
    echo "✅ File saved: {$new_filename}";
} else {
    echo "❌ File save failed";
}

?>
```

---

## 4️⃣ Error Handling 

### 4-1 What is try-catch Block? 

**Not stopping program even if error occurs:**(에러가 발생해도 프로그램을 중단시키지 않기:)

```
try-catch syntax:

try {
    // Write code that may cause errors here
    // (이 영역에서 에러 발생 가능한 코드 작성)
    // Examples: file access, database query, file upload
    // (예: 파일 접근, 데이터베이스 조회, 파일 업로드)
} catch (Exception $e) {
    // This block executes if error occurs
    // (에러가 발생하면 이 블록 실행)
    // $e = error information object
    // ($e = 에러 정보 객체)
    // Display error message, save log, etc.
    // (에러 메시지 표시, 로그 저장 등)
}

Characteristics: (특징:)

- Error occurs during code execution in try block
  (try 블록의 코드 실행 중 에러 발생)

- Move to catch block to handle error
  (catch 블록으로 이동하여 에러 처리)

- Program continues to run (not stopped)
  (프로그램은 계속 실행됨 - 중단 안 함)
```

### 4-2 File Upload Error Handling (파일 업로드 에러 처리)

**Entire file upload process (with error handling):**

(파일 업로드 전체 과정 - 에러 처리 포함:)

```php
<?php

$upload_message = '';

// Has file been uploaded?
// (파일이 업로드되었는가?)
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
  
    try {
        // Step 1: Check if file was selected
        // (단계 1: 파일이 선택되었는지 확인)
        if ($_FILES['upload_file']['error'] === 4) {
            // Error code 4 = No file selected
            // (에러 코드 4 = 파일이 선택되지 않음)
            throw new Exception("Please select a file");
        }
  
        // Step 2: Check upload error
        // (단계 2: 업로드 에러 확인)
        if ($_FILES['upload_file']['error'] !== 0) {
            // Error occurred during upload
            // (업로드 중 에러 발생)
            throw new Exception("File upload error occurred");
        }
  
        // Step 3: Check file size
        // (단계 3: 파일 크기 확인)
        $max_size = 5 * 1024 * 1024;  // 5MB
        if ($_FILES['upload_file']['size'] > $max_size) {
            throw new Exception("File size exceeds 5MB");
        }
  
        // Step 4: Check file extension
        // (단계 4: 파일 확장자 확인)
        $ext = strtolower(pathinfo($_FILES['upload_file']['name'], PATHINFO_EXTENSION));
        $allowed = ['jpg', 'jpeg', 'png', 'gif'];
        if (!in_array($ext, $allowed)) {
            throw new Exception("Only image files can be uploaded");
        }
  
        // Step 5: Move file
        // (단계 5: 파일 이동)
        $new_filename = time() . '_' . rand(1000, 9999) . '.' . $ext;
        $destination = 'uploads/' . $new_filename;
  
        // throw new Exception()
        // = Generate error and move to catch block
        // (= 에러를 발생시키고 catch 블록으로 이동)
        if (!move_uploaded_file($_FILES['upload_file']['tmp_name'], $destination)) {
            throw new Exception("File save failed");
        }
  
        // Success message
        // (성공 메시지)
        $upload_message = "✅ File '{$new_filename}' uploaded successfully";
  
    } catch (Exception $e) {
        // Handle error
        // (에러 처리)
        // $e->getMessage() = Get error message stored in Exception object
        // ($e->getMessage() = Exception 객체에 저장된 메시지 반환)
        $upload_message = "❌ " . $e->getMessage();
    }
}

?>
```

---

## 5️⃣ Practice Example (실습 예제)

### 5-1 Complete File Upload System (완전한 파일 업로드 시스템)

**File 1: upload.php (Upload Form)**

```php
<?php

$upload_message = '';
$uploaded_file = '';

// Check if form was submitted
// (폼이 제출되었는지 확인)
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
  
    try {
        // Step 1: Check file selection
        // (단계 1: 파일 선택 확인)
        if ($_FILES['upload_file']['error'] === 4) {
            throw new Exception("Please select a file");
        }
  
        // Step 2: Check upload error
        // (단계 2: 업로드 에러 확인)
        if ($_FILES['upload_file']['error'] !== 0) {
            throw new Exception("File upload error occurred");
        }
  
        // Step 3: Check file size (5MB limit)
        // (단계 3: 파일 크기 확인 - 5MB 제한)
        $max_size = 5 * 1024 * 1024;
        if ($_FILES['upload_file']['size'] > $max_size) {
            throw new Exception("File size exceeds 5MB limit");
        }
  
        // Step 4: Check file extension
        // (단계 4: 파일 확장자 확인)
        $ext = strtolower(pathinfo($_FILES['upload_file']['name'], PATHINFO_EXTENSION));
        $allowed = ['jpg', 'jpeg', 'png', 'gif'];
        if (!in_array($ext, $allowed)) {
            throw new Exception("Only image files (jpg, png, gif) can be uploaded");
        }
  
        // Step 5: Create uploads directory if not exists
        // (단계 5: uploads 디렉토리가 없으면 생성)
        if (!is_dir('uploads/')) {
            // is_dir($path) = Check if directory exists
            // (is_dir($path) = 디렉토리가 존재하는가?)
            // mkdir($path) = Create directory
            // (mkdir($path) = 디렉토리 생성)
            mkdir('uploads/');
        }
  
        // Step 6: Generate safe filename and move file
        // (단계 6: 안전한 파일명 생성 및 파일 이동)
        $new_filename = time() . '_' . rand(1000, 9999) . '.' . $ext;
        $destination = 'uploads/' . $new_filename;
  
        if (!move_uploaded_file($_FILES['upload_file']['tmp_name'], $destination)) {
            throw new Exception("File save failed");
        }
  
        // Success message
        // (성공 메시지)
        $upload_message = "✅ File '{$new_filename}' uploaded successfully";
        $uploaded_file = $new_filename;
  
    } catch (Exception $e) {
        // Error handling
        // (에러 처리)
        $upload_message = "❌ " . $e->getMessage();
    }
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>File Upload</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 500px;
            margin: 50px auto;
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
  
        .form-group {
            margin: 20px 0;
        }
  
        label {
            display: block;
            margin-bottom: 10px;
            font-weight: bold;
        }
  
        input[type="file"] {
            padding: 10px;
            border: 1px solid #ddd;
            width: 100%;
            box-sizing: border-box;
        }
  
        button {
            width: 100%;
            padding: 10px;
            background-color: navy;
            color: white;
            border: none;
            cursor: pointer;
            font-size: 16px;
            margin-top: 10px;
        }
  
        .message {
            margin-top: 20px;
            padding: 15px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
  
        .success {
            background-color: #d4edda;
            color: #155724;
            border-color: #c3e6cb;
        }
  
        .error {
            background-color: #f8d7da;
            color: #721c24;
            border-color: #f5c6cb;
        }
  
        .preview {
            margin-top: 20px;
            text-align: center;
        }
  
        .preview img {
            max-width: 300px;
            border: 1px solid #ddd;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>📸 Image Upload</h1>
  
    <form method="POST" enctype="multipart/form-data">
        <div class="form-group">
            <label>Select file:</label>
            <input type="file" name="upload_file" accept="image/*" required>
        </div>
  
        <button type="submit">Upload</button>
    </form>
  
    <?php if ($upload_message): ?>
        <div class="message <?php echo strpos($upload_message, '✅') ? 'success' : 'error'; ?>">
            <?php echo $upload_message; ?>
        </div>
    <?php endif; ?>
  
    <?php if ($uploaded_file): ?>
        <div class="preview">
            <h3>Uploaded image:</h3>
            <img src="uploads/<?php echo htmlspecialchars($uploaded_file); ?>" alt="Uploaded image">
        </div>
    <?php endif; ?>
</div>

</body>
</html>
```

**File 2: list_uploads.php (Upload List)**

```php
<?php

// Image file extensions list
// (이미지 파일 확장자 목록)
$image_extensions = ['jpg', 'jpeg', 'png', 'gif'];

// Get all files from uploads directory
// (uploads 디렉토리의 모든 파일 가져오기)
$upload_dir = 'uploads/';
$files = [];

if (is_dir($upload_dir)) {
    // scandir($path) = Return all files and folders in directory as array
    // (scandir($path) = 디렉토리의 모든 파일과 폴더를 배열로 반환)
    // Example: ['.', '..', 'file1.jpg', 'file2.jpg', ...]
    // (예: ['.', '..', 'file1.jpg', 'file2.jpg', ...])
  
    // '.' = current directory, '..' = parent directory
    // ('.' = 현재 디렉토리, '..' = 상위 디렉토리)
  
    // array_diff($array1, $array2) = Remove $array2 from $array1
    // (array_diff($array1, $array2) = $array1에서 $array2 제거)
    // Example: array_diff(['.', '..', 'photo.jpg'], ['.', '..'])
    //          Result: ['photo.jpg']
    // (예: array_diff(['.', '..', 'photo.jpg'], ['.', '..'])
    //     결과: ['photo.jpg'])
    $files = array_diff(scandir($upload_dir), ['.', '..']);
}

// Function to check if file is image
// (파일이 이미지인지 확인하는 함수)
function isImage($filename, $allowed_ext) {
    // pathinfo() = Extract information from file path
    // (pathinfo() = 파일 경로에서 정보 추출)
  
    // PATHINFO_EXTENSION = Extract only extension
    // (PATHINFO_EXTENSION = 확장자만 추출)
  
    // strtolower() = Convert to lowercase
    // (strtolower() = 소문자로 변환)
    $ext = strtolower(pathinfo($filename, PATHINFO_EXTENSION));
  
    // in_array($needle, $haystack)
    // = Does $needle exist in $haystack array?
    // (= $needle이 $haystack 배열에 존재하는가?)
    // in_array('jpg', ['jpg', 'png', 'gif']) → true
    // in_array('exe', ['jpg', 'png', 'gif']) → false
    return in_array($ext, $allowed_ext);
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>Upload List</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 50px auto;
            padding: 20px;
        }
  
        h1 {
            color: navy;
        }
  
        .file-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }
  
        .file-item {
            border: 1px solid #ddd;
            padding: 15px;
            text-align: center;
            background: #f9f9f9;
        }
  
        .file-item img {
            max-width: 100%;
            max-height: 150px;
            margin-bottom: 10px;
        }
  
        .file-name {
            font-size: 12px;
            color: #666;
            word-break: break-all;
        }
  
        .file-icon {
            font-size: 48px;
            margin: 20px 0;
        }
    </style>
</head>
<body>

<h1>📁 Uploaded Files List</h1>

<div class="file-grid">
    <?php foreach ($files as $file): ?>
        <div class="file-item">
            <?php if (isImage($file, $image_extensions)): ?>
                <!-- Image file (이미지 파일) -->
                <img src="<?php echo htmlspecialchars($upload_dir . $file); ?>" alt="<?php echo htmlspecialchars($file); ?>">
            <?php else: ?>
                <!-- Non-image file: display icon (비이미지 파일 - 아이콘 표시) -->
                <div class="file-icon">📄</div>
            <?php endif; ?>
            <div class="file-name"><?php echo htmlspecialchars($file); ?></div>
        </div>
    <?php endforeach; ?>
</div>

<?php if (empty($files)): ?>
    <p>No uploaded files.</p>
<?php endif; ?>

<p><a href="upload.php">← Back to upload</a></p>

</body>
</html>
```

---

---

## 6️⃣ Useful Functions (유용한 함수들)

| Function                 | Description                                                       |
| ------------------------ | ----------------------------------------------------------------- |
| `pathinfo()`           | Extract information from file path (파일 경로에서 정보 추출)      |
| `move_uploaded_file()` | Move temporary file to actual path (임시 파일을 실제 경로로 이동) |
| `is_dir()`             | Check if directory exists (디렉토리 존재 여부 확인)               |
| `mkdir()`              | Create directory (디렉토리 생성)                                  |
| `scandir()`            | Get list of files in directory (디렉토리의 파일 목록 가져오기)    |
| `unlink()`             | Delete file (파일 삭제)                                           |
| `getimagesize()`       | Get image information (이미지 정보 가져오기)                      |

---

## 7️⃣ Assignments (과제)

### Assignment 1: Implement File Upload System (과제 1: 파일 업로드 시스템 구현)

Create a file upload system with the following features:

다음 기능을 포함한 파일 업로드 시스템을 만드세요:

1. **Step 1: Basic Upload**
   (기본 업로드)

   - Select file from HTML form (HTML 폼에서 파일 선택)
   - Validate in upload.php (upload.php에서 파일 검증)
   - Save to uploads/ directory (uploads/ 디렉토리에 저장)
2. **Step 2: Add Validation**
   (검증 추가)

   - File size: 10MB or less (파일 크기: 10MB 이하)
   - File extension: jpg, png, gif, pdf (파일 확장자: jpg, png, gif, pdf)
   - Prevent duplication: Generate new filename (중복 방지: 새 파일명 생성)
3. **Step 3: Add Feedback**
   (피드백 추가)

   - Success: "File uploaded successfully" (성공: "파일이 업로드되었습니다")
   - Failure: Specific error messages (실패: 구체적인 에러 메시지)

---

### Assignment 2: Upload List Page (과제 2: 업로드 목록 페이지)

Create a page that lists uploaded files:

업로드된 파일을 나열하는 페이지를 만드세요:

1. Display file list from uploads/ directory
   (uploads/ 디렉토리의 파일 목록 표시)
2. Show thumbnails for images (이미지는 썸네일 표시)
3. Show filename for PDF (PDF는 파일명 표시)
4. Add delete button (optional) (삭제 버튼 추가 - 선택사항)

---

Thank you for your attention.
Cho Jeonghyun ([peterchokr@gmail.com](mailto:peterchokr@gmail.com)). Yeungnam University College

"Produced in collaboration with Claude and Gemini."
