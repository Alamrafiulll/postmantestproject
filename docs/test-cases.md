# Test Cases — Student Management API

Each test case maps to a request in the Postman collection. "Actual Result" is left as "To be captured after execution" unless noted otherwise, because I don't want to include results I haven't verified.

**Assumed business rules** (for boundary testing):
- Student age must be between 16 and 65
- Student name and email are required
- Email must be a valid format
- Course name is required
- A student cannot be enrolled in the same course twice

---

## Authentication

| Field | TC-AUTH-001 |
|---|---|
| **Test Name** | Login with valid credentials |
| **Category** | Authentication - Positive |
| **Endpoint** | POST /auth/login |
| **Method** | POST |
| **Purpose** | Verify that valid credentials return a token |
| **Preconditions** | Mock server is running |
| **Request Data** | `{"username": "admin", "password": "password123"}` |
| **Expected Result** | Status 200. Response contains `token` (string) and `expiresIn` (number). Token is saved to environment. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-AUTH-002 |
|---|---|
| **Test Name** | Login with invalid credentials |
| **Category** | Authentication - Negative |
| **Endpoint** | POST /auth/login |
| **Method** | POST |
| **Purpose** | Verify that wrong credentials are rejected |
| **Preconditions** | Mock server is running |
| **Request Data** | `{"username": "admin", "password": "wrongpassword"}` |
| **Expected Result** | Status 401. Response contains an `error` field with a message. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-AUTH-003 |
|---|---|
| **Test Name** | Login with missing username |
| **Category** | Authentication - Negative |
| **Endpoint** | POST /auth/login |
| **Method** | POST |
| **Purpose** | Verify that missing username returns 400 |
| **Preconditions** | Mock server is running |
| **Request Data** | `{"password": "password123"}` |
| **Expected Result** | Status 400. Response contains error about missing username. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-AUTH-004 |
|---|---|
| **Test Name** | Login with missing password |
| **Category** | Authentication - Negative |
| **Endpoint** | POST /auth/login |
| **Method** | POST |
| **Purpose** | Verify that missing password returns 400 |
| **Preconditions** | Mock server is running |
| **Request Data** | `{"username": "admin"}` |
| **Expected Result** | Status 400. Response contains error about missing password. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-AUTH-005 |
|---|---|
| **Test Name** | Login with empty request body |
| **Category** | Authentication - Negative |
| **Endpoint** | POST /auth/login |
| **Method** | POST |
| **Purpose** | Verify that empty body returns 400 |
| **Preconditions** | Mock server is running |
| **Request Data** | `{}` |
| **Expected Result** | Status 400. Response contains error message. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

## Students

| Field | TC-STU-001 |
|---|---|
| **Test Name** | Get all students |
| **Category** | Students - Positive |
| **Endpoint** | GET /students |
| **Method** | GET |
| **Purpose** | Verify that the endpoint returns a list of students |
| **Preconditions** | Authenticated (authToken set) |
| **Request Data** | None |
| **Expected Result** | Status 200. Response is a JSON array. Each student has id, name, email, age, major, status. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-STU-002 |
|---|---|
| **Test Name** | Get student by valid ID |
| **Category** | Students - Positive |
| **Endpoint** | GET /students/{{studentId}} |
| **Method** | GET |
| **Purpose** | Verify that a specific student can be retrieved by ID |
| **Preconditions** | Authenticated. studentId = 1 |
| **Request Data** | None |
| **Expected Result** | Status 200. Response has id = 1, name, email, age, major, status. All types correct. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-STU-003 |
|---|---|
| **Test Name** | Create student with valid data |
| **Category** | Students - Positive |
| **Endpoint** | POST /students |
| **Method** | POST |
| **Purpose** | Verify that a student can be created with valid data |
| **Preconditions** | Authenticated |
| **Request Data** | `{"name": "Test Student", "email": "test.student@example.com", "age": 20, "major": "Physics"}` |
| **Expected Result** | Status 201. Response contains the student with an assigned id, status = "active". |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-STU-004 |
|---|---|
| **Test Name** | Update student with valid data |
| **Category** | Students - Positive |
| **Endpoint** | PUT /students/{{studentId}} |
| **Method** | PUT |
| **Purpose** | Verify that a student's data can be fully updated |
| **Preconditions** | Authenticated. studentId = 1 |
| **Request Data** | `{"name": "Maria Santos Updated", "email": "maria.updated@example.com", "age": 22, "major": "Data Science", "status": "active"}` |
| **Expected Result** | Status 200. Response reflects updated values. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-STU-005 |
|---|---|
| **Test Name** | Partial update student (PATCH) |
| **Category** | Students - Positive |
| **Endpoint** | PATCH /students/{{studentId}} |
| **Method** | PATCH |
| **Purpose** | Verify that individual fields can be updated |
| **Preconditions** | Authenticated. studentId = 1 |
| **Request Data** | `{"major": "Software Engineering"}` |
| **Expected Result** | Status 200. Response shows updated major, other fields unchanged. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-STU-006 |
|---|---|
| **Test Name** | Delete student |
| **Category** | Students - Positive |
| **Endpoint** | DELETE /students/{{studentId}} |
| **Method** | DELETE |
| **Purpose** | Verify that a student can be deleted |
| **Preconditions** | Authenticated. studentId = 1 |
| **Request Data** | None |
| **Expected Result** | Status 200. Response contains a confirmation message. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-STU-007 |
|---|---|
| **Test Name** | Get nonexistent student |
| **Category** | Students - Negative |
| **Endpoint** | GET /students/999 |
| **Method** | GET |
| **Purpose** | Verify that requesting a missing student returns 404 |
| **Preconditions** | Authenticated |
| **Request Data** | None |
| **Expected Result** | Status 404. Response contains error message about student not found. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-STU-008 |
|---|---|
| **Test Name** | Create student - missing required field |
| **Category** | Students - Negative |
| **Endpoint** | POST /students |
| **Method** | POST |
| **Purpose** | Verify that creating a student without a name is rejected |
| **Preconditions** | Authenticated |
| **Request Data** | `{"email": "noname@example.com", "age": 20, "major": "Physics"}` |
| **Expected Result** | Status 400. Response contains error about missing name field. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-STU-009 |
|---|---|
| **Test Name** | Create student - invalid email |
| **Category** | Students - Negative |
| **Endpoint** | POST /students |
| **Method** | POST |
| **Purpose** | Verify that an invalid email format is rejected |
| **Preconditions** | Authenticated |
| **Request Data** | `{"name": "Bad Email", "email": "not-an-email", "age": 20, "major": "Physics"}` |
| **Expected Result** | Status 400. Response contains error about invalid email. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-STU-010 |
|---|---|
| **Test Name** | Create student - invalid age (below minimum) |
| **Category** | Students - Negative / Boundary |
| **Endpoint** | POST /students |
| **Method** | POST |
| **Purpose** | Verify that age below 16 is rejected |
| **Preconditions** | Authenticated |
| **Request Data** | `{"name": "Young Student", "email": "young@example.com", "age": 10, "major": "Physics"}` |
| **Expected Result** | Status 400. Response contains error about invalid age. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-STU-011 |
|---|---|
| **Test Name** | Create student - duplicate email |
| **Category** | Students - Negative |
| **Endpoint** | POST /students |
| **Method** | POST |
| **Purpose** | Verify that duplicate email addresses are rejected |
| **Preconditions** | Authenticated. A student with this email already exists. |
| **Request Data** | `{"name": "Duplicate", "email": "maria.santos@example.com", "age": 20, "major": "Physics"}` |
| **Expected Result** | Status 409. Response contains error about duplicate email. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

## Courses

| Field | TC-CRS-001 |
|---|---|
| **Test Name** | Get all courses |
| **Category** | Courses - Positive |
| **Endpoint** | GET /courses |
| **Method** | GET |
| **Purpose** | Verify that all courses are returned |
| **Preconditions** | Authenticated |
| **Request Data** | None |
| **Expected Result** | Status 200. Response is a JSON array. Each course has id, courseCode, courseName, credits. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-CRS-002 |
|---|---|
| **Test Name** | Get course by valid ID |
| **Category** | Courses - Positive |
| **Endpoint** | GET /courses/{{courseId}} |
| **Method** | GET |
| **Purpose** | Verify that a specific course can be retrieved |
| **Preconditions** | Authenticated. courseId = 1 |
| **Request Data** | None |
| **Expected Result** | Status 200. Response has id = 1, courseCode, courseName, credits. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-CRS-003 |
|---|---|
| **Test Name** | Create course with valid data |
| **Category** | Courses - Positive |
| **Endpoint** | POST /courses |
| **Method** | POST |
| **Purpose** | Verify that a course can be created |
| **Preconditions** | Authenticated |
| **Request Data** | `{"courseCode": "PHYS101", "courseName": "Introductory Physics", "credits": 4}` |
| **Expected Result** | Status 201. Response contains the course with an assigned id. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-CRS-004 |
|---|---|
| **Test Name** | Update course with valid data |
| **Category** | Courses - Positive |
| **Endpoint** | PUT /courses/{{courseId}} |
| **Method** | PUT |
| **Purpose** | Verify that a course can be fully updated |
| **Preconditions** | Authenticated. courseId = 1 |
| **Request Data** | `{"courseCode": "CS101", "courseName": "Intro to CS - Updated", "credits": 4}` |
| **Expected Result** | Status 200. Response reflects updated values. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-CRS-005 |
|---|---|
| **Test Name** | Delete course |
| **Category** | Courses - Positive |
| **Endpoint** | DELETE /courses/{{courseId}} |
| **Method** | DELETE |
| **Purpose** | Verify that a course can be deleted |
| **Preconditions** | Authenticated. courseId = 1 |
| **Request Data** | None |
| **Expected Result** | Status 200. Response contains a confirmation message. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-CRS-006 |
|---|---|
| **Test Name** | Get nonexistent course |
| **Category** | Courses - Negative |
| **Endpoint** | GET /courses/999 |
| **Method** | GET |
| **Purpose** | Verify that requesting a missing course returns 404 |
| **Preconditions** | Authenticated |
| **Request Data** | None |
| **Expected Result** | Status 404. Response contains error message. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-CRS-007 |
|---|---|
| **Test Name** | Create course - missing name |
| **Category** | Courses - Negative |
| **Endpoint** | POST /courses |
| **Method** | POST |
| **Purpose** | Verify that creating a course without a name is rejected |
| **Preconditions** | Authenticated |
| **Request Data** | `{"courseCode": "NONE01", "credits": 3}` |
| **Expected Result** | Status 400. Response contains error about missing courseName. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

## Enrollments

| Field | TC-ENR-001 |
|---|---|
| **Test Name** | Get all enrollments |
| **Category** | Enrollments - Positive |
| **Endpoint** | GET /enrollments |
| **Method** | GET |
| **Purpose** | Verify that all enrollments are returned |
| **Preconditions** | Authenticated |
| **Request Data** | None |
| **Expected Result** | Status 200. Response is a JSON array. Each enrollment has id, studentId, courseId, enrollmentDate, status. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-ENR-002 |
|---|---|
| **Test Name** | Create enrollment with valid data |
| **Category** | Enrollments - Positive |
| **Endpoint** | POST /enrollments |
| **Method** | POST |
| **Purpose** | Verify that a student can be enrolled in a course |
| **Preconditions** | Authenticated |
| **Request Data** | `{"studentId": 1, "courseId": 2}` |
| **Expected Result** | Status 201. Response contains the enrollment with id, enrollmentDate, status = "enrolled". |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-ENR-003 |
|---|---|
| **Test Name** | Get enrollment by valid ID |
| **Category** | Enrollments - Positive |
| **Endpoint** | GET /enrollments/{{enrollmentId}} |
| **Method** | GET |
| **Purpose** | Verify that a specific enrollment can be retrieved |
| **Preconditions** | Authenticated. enrollmentId = 1 |
| **Request Data** | None |
| **Expected Result** | Status 200. Response has id, studentId, courseId, enrollmentDate, status. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-ENR-004 |
|---|---|
| **Test Name** | Delete enrollment |
| **Category** | Enrollments - Positive |
| **Endpoint** | DELETE /enrollments/{{enrollmentId}} |
| **Method** | DELETE |
| **Purpose** | Verify that an enrollment can be deleted |
| **Preconditions** | Authenticated. enrollmentId = 1 |
| **Request Data** | None |
| **Expected Result** | Status 200. Response contains a confirmation message. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-ENR-005 |
|---|---|
| **Test Name** | Duplicate enrollment |
| **Category** | Enrollments - Negative |
| **Endpoint** | POST /enrollments |
| **Method** | POST |
| **Purpose** | Verify that enrolling the same student in the same course twice is rejected |
| **Preconditions** | Authenticated. Student 1 is already enrolled in course 1. |
| **Request Data** | `{"studentId": 1, "courseId": 1}` |
| **Expected Result** | Status 409. Response contains error about duplicate enrollment. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-ENR-006 |
|---|---|
| **Test Name** | Create enrollment - invalid student ID |
| **Category** | Enrollments - Negative |
| **Endpoint** | POST /enrollments |
| **Method** | POST |
| **Purpose** | Verify that enrolling a nonexistent student is rejected |
| **Preconditions** | Authenticated |
| **Request Data** | `{"studentId": 9999, "courseId": 1}` |
| **Expected Result** | Status 400. Response contains error about invalid student ID. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-ENR-007 |
|---|---|
| **Test Name** | Create enrollment - invalid course ID |
| **Category** | Enrollments - Negative |
| **Endpoint** | POST /enrollments |
| **Method** | POST |
| **Purpose** | Verify that enrolling in a nonexistent course is rejected |
| **Preconditions** | Authenticated |
| **Request Data** | `{"studentId": 1, "courseId": 9999}` |
| **Expected Result** | Status 400. Response contains error about invalid course ID. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

## Validation / Boundary Tests

| Field | TC-VAL-001 |
|---|---|
| **Test Name** | Student age at minimum boundary (16) |
| **Category** | Validation - Boundary |
| **Endpoint** | POST /students |
| **Method** | POST |
| **Purpose** | Verify that age 16 is accepted (minimum valid age) |
| **Preconditions** | Authenticated |
| **Request Data** | `{"name": "Min Age Student", "email": "minage@example.com", "age": 16, "major": "Art"}` |
| **Expected Result** | Status 201. Student is created successfully. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-VAL-002 |
|---|---|
| **Test Name** | Student age at maximum boundary (65) |
| **Category** | Validation - Boundary |
| **Endpoint** | POST /students |
| **Method** | POST |
| **Purpose** | Verify that age 65 is accepted (maximum valid age) |
| **Preconditions** | Authenticated |
| **Request Data** | `{"name": "Max Age Student", "email": "maxage@example.com", "age": 65, "major": "History"}` |
| **Expected Result** | Status 201. Student is created successfully. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-VAL-003 |
|---|---|
| **Test Name** | Student age below minimum (15) |
| **Category** | Validation - Boundary |
| **Endpoint** | POST /students |
| **Method** | POST |
| **Purpose** | Verify that age 15 is rejected (below minimum) |
| **Preconditions** | Authenticated |
| **Request Data** | `{"name": "Too Young", "email": "tooyoung@example.com", "age": 15, "major": "Art"}` |
| **Expected Result** | Status 400. Response contains error about age out of range. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-VAL-004 |
|---|---|
| **Test Name** | Student age above maximum (66) |
| **Category** | Validation - Boundary |
| **Endpoint** | POST /students |
| **Method** | POST |
| **Purpose** | Verify that age 66 is rejected (above maximum) |
| **Preconditions** | Authenticated |
| **Request Data** | `{"name": "Too Old", "email": "tooold@example.com", "age": 66, "major": "History"}` |
| **Expected Result** | Status 400. Response contains error about age out of range. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-VAL-005 |
|---|---|
| **Test Name** | Student name with very long value |
| **Category** | Validation - Boundary |
| **Endpoint** | POST /students |
| **Method** | POST |
| **Purpose** | Verify behavior when name exceeds reasonable length (300+ chars) |
| **Preconditions** | Authenticated |
| **Request Data** | Name field set to a 300-character string |
| **Expected Result** | Status 400. Response contains error about name too long. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-VAL-006 |
|---|---|
| **Test Name** | Get student with ID = 0 |
| **Category** | Validation - Boundary |
| **Endpoint** | GET /students/0 |
| **Method** | GET |
| **Purpose** | Verify that ID 0 is handled (edge case) |
| **Preconditions** | Authenticated |
| **Request Data** | None |
| **Expected Result** | Status 400 or 404. Response contains error. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

| Field | TC-VAL-007 |
|---|---|
| **Test Name** | Get student with negative ID |
| **Category** | Validation - Boundary |
| **Endpoint** | GET /students/-1 |
| **Method** | GET |
| **Purpose** | Verify that negative IDs are handled |
| **Preconditions** | Authenticated |
| **Request Data** | None |
| **Expected Result** | Status 400. Response contains error about invalid ID. |
| **Actual Result** | To be captured after execution |
| **Status** | — |

---

## Summary

| Category       | Test Case Count |
| -------------- | --------------- |
| Authentication | 5               |
| Students       | 11              |
| Courses        | 7               |
| Enrollments    | 7               |
| Validation     | 7               |
| **Total**      | **37**          |
