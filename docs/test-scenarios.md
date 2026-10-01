# Test Scenarios — Student Management API

This document groups test scenarios by feature area. Each scenario describes a general situation to test. The specific test cases (with inputs, expected results, etc.) are in [test-cases.md](test-cases.md).

---

## 1. Authentication

| Scenario ID | Scenario                                      |
| ----------- | --------------------------------------------- |
| SCN-AUTH-01 | User logs in with valid credentials            |
| SCN-AUTH-02 | User logs in with wrong password               |
| SCN-AUTH-03 | User submits login without username             |
| SCN-AUTH-04 | User submits login without password             |
| SCN-AUTH-05 | User submits login with empty request body      |

**What I'm checking:** The login endpoint returns a token for valid credentials and meaningful error messages for invalid ones. The token gets saved as an environment variable for later requests.

---

## 2. Student Management

| Scenario ID | Scenario                                      |
| ----------- | --------------------------------------------- |
| SCN-STU-01  | Retrieve all students                          |
| SCN-STU-02  | Retrieve a specific student by ID              |
| SCN-STU-03  | Create a new student with valid data           |
| SCN-STU-04  | Update all fields of an existing student       |
| SCN-STU-05  | Partially update a student (PATCH)             |
| SCN-STU-06  | Delete an existing student                     |

**What I'm checking:** Standard CRUD operations work as expected. Responses include all required fields with correct data types. IDs are consistent.

---

## 3. Course Management

| Scenario ID | Scenario                                      |
| ----------- | --------------------------------------------- |
| SCN-CRS-01  | Retrieve all courses                           |
| SCN-CRS-02  | Retrieve a specific course by ID               |
| SCN-CRS-03  | Create a new course with valid data            |
| SCN-CRS-04  | Update an existing course                      |
| SCN-CRS-05  | Delete a course                                |

**What I'm checking:** Course endpoints return proper structure (id, courseCode, courseName, credits). Create returns 201 and the created resource.

---

## 4. Enrollment Management

| Scenario ID | Scenario                                      |
| ----------- | --------------------------------------------- |
| SCN-ENR-01  | Retrieve all enrollments                       |
| SCN-ENR-02  | Create a new enrollment                        |
| SCN-ENR-03  | Retrieve a specific enrollment by ID           |
| SCN-ENR-04  | Delete an enrollment                           |

**What I'm checking:** Enrollment responses link a student to a course with a date and status. Deleting an enrollment returns a confirmation.

---

## 5. Negative Testing

| Scenario ID | Scenario                                      |
| ----------- | --------------------------------------------- |
| SCN-NEG-01  | Request a student that doesn't exist           |
| SCN-NEG-02  | Create a student without a required field      |
| SCN-NEG-03  | Create a student with an invalid email format  |
| SCN-NEG-04  | Create a student with an out-of-range age      |
| SCN-NEG-05  | Create a student with a duplicate email        |
| SCN-NEG-06  | Request a course that doesn't exist            |
| SCN-NEG-07  | Create a course without a name                 |
| SCN-NEG-08  | Create a duplicate enrollment                  |
| SCN-NEG-09  | Create enrollment with invalid student ID      |
| SCN-NEG-10  | Create enrollment with invalid course ID       |

**What I'm checking:** The API rejects bad input with appropriate HTTP status codes (400, 404, 409) and returns error messages that make sense.

---

## 6. Validation Testing (Boundaries)

| Scenario ID | Scenario                                      |
| ----------- | --------------------------------------------- |
| SCN-VAL-01  | Student age at minimum boundary (16)           |
| SCN-VAL-02  | Student age at maximum boundary (65)           |
| SCN-VAL-03  | Student age below minimum (15)                 |
| SCN-VAL-04  | Student age above maximum (66)                 |
| SCN-VAL-05  | Student name with very long value              |
| SCN-VAL-06  | Request student with ID = 0                    |
| SCN-VAL-07  | Request student with negative ID               |

**What I'm checking:** The API handles edge values correctly — accepting values at the boundary and rejecting values outside it. The assumed business rule is age 16-65; this is documented in the test cases.

---

## 7. Regression Testing (Smoke)

| Scenario ID | Scenario                                      |
| ----------- | --------------------------------------------- |
| SCN-REG-01  | Login → Get students → Get courses → Create enrollment |
| SCN-REG-02  | Verify critical endpoints still return expected status codes |

**What I'm checking:** A quick pass through the most important endpoints to catch obvious breakages. This is the suite I'd run after any change to make sure nothing major broke.

---

## Notes

- These scenarios are designed for a mock API. The mock server returns pre-defined responses, so these tests validate the response contract rather than real processing logic.
- Boundary values (age 16-65) are assumptions I made for this project. A real project would get these from actual requirements.
- I organized negative tests in their own section rather than mixing them into each feature area because it made the collection easier to navigate during reviews.
