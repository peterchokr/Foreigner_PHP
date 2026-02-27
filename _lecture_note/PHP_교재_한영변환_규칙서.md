# 📋 PHP 교재 영한 병행 변환 규칙서 (Style Guide)

**작성일**: 2026-02-15  
**버전**: 1.0  
**적용 대상**: PHP 교재 (Chapter 1-4 이상)  
**상태**: 완성 및 검증됨

---

## 📌 개요

이 규칙서는 PHP 웹 개발 교재를 영한 병행(Bilingual Korean-English) 형식으로 변환할 때 적용해야 하는 모든 규칙을 정의합니다. Chapter 1-4에서 검증되었으며, 이후 모든 Chapter에 동일하게 적용됩니다.

---

## 1️⃣ 제목 구조 및 형식

### 1-1 전체 장 제목 (Main Title)

**형식:**
```
# Chapter [번호]. [영문 제목만]
```

**규칙:**
- 영문만 사용 (한글 절대 금지)
- 첫 글자는 대문자
- 부제 또는 한글은 포함하지 않음

**예시:**
```
# Chapter 1. Web Development Basics and PHP Setup
# Chapter 2. PHP Fundamentals, Variables, and Data Types
# Chapter 3. Control Structures, Loops, and Functions
# Chapter 4. MySQL Review and PHP-MySQL Integration
```

**❌ 잘못된 예시:**
```
# 1장: PHP 웹 개발 기초 (한글 포함)
# Chapter 1: Web Development Basics & PHP Setup (부제 포함)
```

---

### 1-2 섹션 제목 (Section Headers)

**형식:**
```
## [번호]️⃣ [영문 제목] ([한글 제목])
```

**규칙:**
- 번호는 1️⃣-9️⃣ 이모지 사용
- 영문 제목과 한글 제목을 괄호로 구분
- 각 Chapter마다 4-5개의 섹션 포함

**예시:**
```
## 1️⃣ Conditional Statements (조건문)
## 2️⃣ Loops (반복문)
## 3️⃣ Functions (함수)
## 4️⃣ Built-in Functions (내장 함수)
## 5️⃣ Practice Example (실습 예제)
```

---

### 1-3 소제목 (Subsection Headers) - 중요!

**형식:**
```
### [섹션번호]-[소제목번호] [영문 제목] ([한글 제목])
```

**규칙:**
- **섹션번호**: 상위 섹션의 번호 (1, 2, 3, 4, 5)
- **소제목번호**: 1부터 시작 (순차적으로 증가)
- 영문과 한글을 괄호로 구분

**예시:**
```
### 1-1 If / Else / Elseif Statements (if / else / elseif 문)
### 1-2 Switch Statement (switch 문)
### 1-3 Ternary Operator (? :) (삼항 연산자)
### 1-4 Practice Example: User Validation (실습 예제: 회원 유효성 검사)
### 2-1 For Loop (for 루프)
### 3-1 Function Basics (함수의 기본)
```

**중요한 규칙:**
- 모든 소제목에 번호를 붙여야 함 (원본 구조 유지)
- 번호는 원본 문서의 구조와 동일하게
- 한 섹션 내 소제목은 연속 번호

---

### 1-4 소제목 아래 추가 제목 (Sub-subsection)

**형식:**
```
#### **[영문 제목] ([한글 제목])**
```

**규칙:**
- 4개의 # 사용 (#### 형식)
- 굵은 글씨 (**) 사용
- 영문과 한글을 괄호로 구분

**예시:**
```
#### **Basic Structure (기본 구조)**
#### **When to Use (언제 사용?)**
#### **Nested Conditional Statements (중첩된 조건문)**
```

---

## 2️⃣ 본문 작성 규칙

### 2-1 영한 완전 분리 (Complete Separation)

**원칙:**
- 영문 문단과 한글 문단을 **절대** 같은 문장에 섞지 않음
- 한 문단은 영문 또는 한글 중 하나만 사용

**구조:**
```
[영문 문단 1-3개 문장]

[빈 줄]

[한글 문단 1-3개 문장]
```

**✅ 올바른 예시:**
```
Conditional statements allow your program to make decisions based on whether 
conditions are true or false. By checking conditions, you can execute different 
code paths depending on the situation. This is fundamental to creating responsive 
and dynamic programs.

조건문은 프로그램이 조건의 참/거짓 여부에 따라 결정을 내릴 수 있도록 합니다. 
조건을 확인함으로써 상황에 따라 다른 코드 경로를 실행할 수 있습니다. 
이것은 반응형이고 동적인 프로그램을 만드는 데 기본입니다.
```

**❌ 잘못된 예시:**
```
Conditional statements (조건문)를 사용하면 프로그램이 decisions를 내릴 수 있습니다.
(같은 문장에 영한 혼용)
```

---

### 2-2 문단 길이

**규칙:**
- 각 문단은 최소 2-3개 문장 이상
- 너무 짧은 문단은 이웃 문단과 병합
- 너무 긴 문단은 2-3개로 분할

**팁:**
- 개념 설명은 영문 먼저 (2-3문장)
- 그 다음 한글로 동일 내용 (2-3문장)
- 각각을 공백 줄로 구분

---

### 2-3 용어 번역 일관성

**이미 정한 용어:**
```
Function = 함수
Parameter = 매개변수
Return value = 반환값
Scope = 스코프
Variable = 변수
Constant = 상수
Array = 배열
Loop = 루프
Conditional = 조건문
Database = 데이터베이스
Table = 테이블
Query = 쿼리
Prepared Statement = 준비된 문장
SQL Injection = SQL Injection (영문 유지)
```

**규칙:**
- 첫 사용 시 영문 (한글) 형식
- 이후로는 주로 한글만 사용
- 기술 용어는 영문과 한글 병행 가능

---

## 3️⃣ 코드 블록 규칙

### 3-1 코드 블록 형식

**언어별 구분:**
```
```php
// PHP 코드
?>
```

```sql
-- SQL 코드
```

```html
<!-- HTML 코드 -->
```
```

**규칙:**
- 언어 명시 필수 (```php, ```sql, ```html)
- 백틱 3개로 감싸기
- 종료도 백틱 3개 사용

---

### 3-2 코드 주석 (중요!)

**형식:**
```
// English Comment (한글 주석)
```

**규칙:**
- 모든 코드 주석을 이중언어로 작성
- 영문 먼저, 한글은 괄호 안에
- 주석과 코드는 분리

**✅ 올바른 예시:**
```php
<?php

// Database connection parameters (데이터베이스 연결 매개변수)
$host = 'localhost';
$dbname = 'test_db';

// Execute secure query with prepared statement (준비된 문장으로 안전한 쿼리 실행)
$stmt = $pdo->prepare($sql);
$stmt->execute([$min_score]);

// Retrieve all results (모든 결과 검색)
$results = $stmt->fetchAll(PDO::FETCH_ASSOC);

?>
```

**❌ 잘못된 예시:**
```php
// 데이터베이스 연결 (한글만)
$host = 'localhost';

// Connect database (영문만)
$host = 'localhost';
```

---

### 3-3 코드 블록 개수 (짝수 규칙)

**규칙:**
- 각 Chapter의 **총 코드 블록 개수는 반드시 짝수**
- PHP, SQL, HTML 포함하여 계산

**검증 방법:**
```bash
# 코드 블록 개수 세기
grep -c '```' filename.md

# 결과가 짝수여야 함 (시작과 끝 합쳐서)
# 예: 36 (짝수 ✓), 37 (홀수 ✗)
```

**조정 방법:**
- 예제 추가 (새로운 코드 블록 2개)
- 설명 추가 (기존 코드에 주석 추가)

---

## 4️⃣ 문화 현지화 규칙

### 4-1 이름 변환

**한국식 이름 제거:**
```
❌ 홍길동, 김영희, 이순신, 강감찬, 세종대왕
✅ John Smith, Mary Johnson, Michael Brown, Sarah Davis, Robert Wilson
```

**규칙:**
- 한국식 이름은 모두 영문으로 변환
- 이미 정한 이름들로 통일 (일관성)
- 새로운 예제에서도 동일한 이름 사용

**정한 이름 목록 (재사용):**
- John Smith (주 인물)
- Mary Johnson (여자 인물 1)
- Michael Brown (남자 인물 2)
- Sarah Davis (여자 인물 2)
- Robert Wilson (남자 인물 3)

---

### 4-2 지역 참조 변환

**예시:**
```
❌ 영남이공대학교
✅ Yeungnam University College

❌ 한국
✅ Korea (필요시만)
```

---

### 4-3 이메일 및 연락처

**형식:**
```
peterchokr@gmail.com
(항상 영문 이메일 사용)
```

---

## 5️⃣ 학습목표 및 과제 규칙

### 5-1 학습목표 (Learning Objectives)

**위치:**
```
## 📚 Learning Objectives
(이 섹션은 학습목표용 예약됨)
```

**형식:**
```
After completing this chapter, you will be able to:

✅ Understand and use... (1번 목표)
✅ Connect to... (2번 목표)
[etc...]

이 장을 학습하면 다음을 할 수 있습니다:

✅ [한글 1번 목표]
✅ [한글 2번 목표]
[etc...]
```

**규칙:**
- 5개 이상의 학습목표
- 체크마크(✅) 사용
- 영문과 한글 분리

---

### 5-2 퀴즈 및 과제 처리

**규칙:**
- **퀴즈는 제거** (실시하지 않음)
- **과제만 포함** (Assignment 1, Assignment 2)
- 최소 2개 이상의 과제

**과제 형식:**
```
### Assignment 1: [제목] (과제 1: [한글 제목])

Implement the following: (다음을 구현하세요:)

1. [첫 번째 요구사항]
   - [상세사항]
   
2. [두 번째 요구사항]
   - [상세사항]
```

---

## 6️⃣ 서명 (Signature)

**형식:**
```
---

Thank you for your attention.

Professor Cho Jeonghyun (peterchokr@gmail.com)
Yeungnam University College
```

**규칙:**
- 항상 이 형식으로 일관성 있게
- 한글 작사 불가 (영문만)
- 이메일 포함

---

## 7️⃣ 파일명 규칙

**형식:**
```
Chapter_[번호]_[영문_제목]_EN.md
```

**예시:**
```
Chapter_01_Web_Development_Basics_and_PHP_Setup_EN.md
Chapter_02_PHP_Fundamentals_Variables_and_Data_Types_EN.md
Chapter_03_Control_Structures_Loops_and_Functions_EN.md
Chapter_04_MySQL_Review_and_PHP-MySQL_Integration_EN.md
```

---

## 8️⃣ 보고서 파일명

**형식:**
```
Chapter_[번호]_최종_보고서.md
```

**예시:**
```
Chapter_01_최종_보고서.md (또는 변환_최종_보고서.md)
Chapter_02_최종_보고서.md
Chapter_03_최종_보고서.md
Chapter_04_최종_보고서.md
```

---

## 9️⃣ 검증 체크리스트

모든 Chapter 완성 후 이 체크리스트로 검증:

```
✅ 1. 제목 형식 검증
   - [ ] 전체 제목: # Chapter [N]. [영문만]
   - [ ] 섹션: [N]️⃣ [영문] ([한글])
   - [ ] 소제목: [N]-[N] [영문] ([한글])

✅ 2. 본문 검증
   - [ ] 영한 완전 분리 (같은 문단 혼용 금지)
   - [ ] 각 문단 2-3개 문장 이상
   - [ ] 용어 번역 일관성

✅ 3. 코드 검증
   - [ ] 모든 주석 이중언어 처리
   - [ ] 코드 블록 개수 짝수
   - [ ] 언어 명시 (php, sql, html)

✅ 4. 문화 검증
   - [ ] 한국식 이름 0개
   - [ ] 영문 이름 사용
   - [ ] 이메일/연락처 일관성

✅ 5. 구조 검증
   - [ ] 학습목표 5개 이상
   - [ ] 과제 2개 이상
   - [ ] 서명 올바른 형식

✅ 6. 최종 검증
   - [ ] 모든 파일명 규칙 준수
   - [ ] 보고서 파일 생성
   - [ ] 통계 정보 정확성
```

---

## 🔟 자주하는 실수와 해결법

### 실수 1: 같은 문장에 영한 혼용

**❌ 잘못된 예:**
```
Conditional statements (조건문)는 프로그램이 decisions를 내릴 수 있도록 합니다.
```

**✅ 올바른 예:**
```
Conditional statements allow your program to make decisions. This is fundamental 
to creating responsive and dynamic programs.

조건문은 프로그램이 결정을 내릴 수 있도록 합니다. 이것은 반응형이고 동적인 
프로그램을 만드는 데 기본입니다.
```

---

### 실수 2: 코드 주석 한글만 또는 영문만

**❌ 잘못된 예:**
```php
// 데이터베이스 연결
$pdo = new PDO(...);

// Database query execution
$stmt = $pdo->query($sql);
```

**✅ 올바른 예:**
```php
// Database connection (데이터베이스 연결)
$pdo = new PDO(...);

// Execute database query (데이터베이스 쿼리 실행)
$stmt = $pdo->query($sql);
```

---

### 실수 3: 소제목 번호 빠뜨림

**❌ 잘못된 예:**
```
### If / Else / Elseif Statements
### Switch Statement
### Ternary Operator
```

**✅ 올바른 예:**
```
### 1-1 If / Else / Elseif Statements
### 1-2 Switch Statement
### 1-3 Ternary Operator
```

---

### 실수 4: 코드 블록 홀수 개

**문제:** 총 코드 블록이 홀수면 규칙 위반

**해결:**
1. 새로운 코드 블록 추가 (2개)
2. 또는 기존 예제에 코드 추가

---

### 실수 5: 한국식 이름 남겨두기

**❌:**
```
$student = array("name" => "홍길동", "score" => 85);
```

**✅:**
```
$student = array("name" => "John Smith", "score" => 85);
```

---

## 1️⃣1️⃣ 예제 구조

### 예제 파일 형식

**파일 이름:**
```
function_name.php
form_processing.php
database_query.php
```

**파일 내 구조:**
```php
<?php
/**
 * filename.php - Brief description
 * 
 * Purpose: (목적:)
 * 1. First purpose (첫 번째 목적)
 * 2. Second purpose (두 번째 목적)
 */

// ============================================
// Key 1: Initialization (핵심 1: 초기화)
// ============================================

// Code here

// ============================================
// Key 2: Processing (핵심 2: 처리)
// ============================================

// Code here

?>

<!DOCTYPE html>
<html>
<head>
    <!-- HTML -->
</head>
<body>
    <!-- HTML and PHP mixed -->
</body>
</html>
```

---

## 1️⃣2️⃣ 번역 표준 용어집

이 용어들은 **반드시** 일관되게 사용:

| 영문 | 한글 | 사용 예 |
|------|------|--------|
| Function | 함수 | `function greet()` |
| Parameter | 매개변수 | `function add($a, $b)` |
| Return value | 반환값 | `return $result;` |
| Variable | 변수 | `$variable = 10;` |
| Constant | 상수 | `define('CONSTANT')` |
| Array | 배열 | `$array = [1, 2, 3];` |
| Loop | 루프 | `for`, `foreach`, `while` |
| Conditional | 조건문 | `if`, `else`, `switch` |
| Scope | 스코프 | `local scope`, `global scope` |
| Method | 메서드 | `$object->method()` |
| Property | 속성 | `$object->property` |
| Class | 클래스 | `class MyClass {}` |
| Object | 객체 | `$object = new Class()` |
| Database | 데이터베이스 | `MySQL`, `PDO` |
| Query | 쿼리 | `SELECT`, `INSERT` |
| Table | 테이블 | `students` table |
| Record | 레코드 | `1 record from table` |
| Field | 필드 | `name field`, `score field` |
| Primary Key | 기본 키 | `PRIMARY KEY` |
| Foreign Key | 외래 키 | `FOREIGN KEY` |
| Index | 인덱스 | `CREATE INDEX` |

---

## 1️⃣3️⃣ 통계 수집 기준

각 Chapter 완성 후 수집할 통계:

```
| 항목 | 수치 |
|------|------|
| 전체 라인 | [라인 수] |
| 섹션 수 | [개수] |
| 소제목 수 | [개수] |
| 코드 블록 수 | [짝수 확인] |
| PHP 블록 | [개수] |
| SQL 블록 | [개수] |
| HTML 블록 | [개수] |
| 학습목표 | [개수] |
| 과제 | [개수] |
| 영문 이름 사용 | [개수] |
| 이중언어 주석 | [개수] |
| 파일 크기 | [바이트] |
```

---

## 최종 요약

### 5단계 검증 프로세스

1. **형식 검증**: 제목, 섹션, 소제목 구조 확인
2. **본문 검증**: 영한 분리, 용어 일관성 확인
3. **코드 검증**: 주석 이중언어, 블록 짝수 확인
4. **문화 검증**: 이름, 용어 현지화 확인
5. **최종 검증**: 서명, 과제, 통계 확인

---

## 📞 추가 팁

- **Python 검증 스크립트**: 자동으로 코드 블록 개수 확인
- **정규식**: 소제목 번호 패턴 검증
- **비교**: 이전 Chapter와 구조 비교
- **반복 검토**: 최소 2회 검증

---

**이 규칙서는 지속적으로 업데이트되며, 모든 변경사항은 버전 명시와 함께 기록됩니다.**

**Last Updated**: 2026-02-15  
**Version**: 1.0 (Stable)
