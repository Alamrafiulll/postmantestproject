# Bug Report Template

Use this template to document defects found during testing. Copy a blank template for each new bug.

---

## Template

| Field            | Value                          |
| ---------------- | ------------------------------ |
| **Bug ID**       | BUG-XXX                        |
| **Title**        |                                |
| **Environment**  |                                |
| **Endpoint**     |                                |
| **Method**       |                                |
| **Severity**     | Critical / High / Medium / Low |
| **Priority**     | P1 / P2 / P3 / P4             |
| **Status**       | Open / In Progress / Closed    |

### Preconditions

(What needs to be true before reproducing this bug)

### Steps to Reproduce

1. 
2. 
3. 

### Expected Result

(What should happen)

### Actual Result

(What actually happens)

### Evidence

(Screenshots, response body, status code, console output — attach or paste here)

### Notes

(Any additional context)

---

## Example Bug Report (Synthetic)

> **Note:** This is a made-up example to show how I'd fill in the template. This bug was not found in a real system. It's based on a scenario where the mock server might be configured to return an unexpected status code.

| Field            | Value                                                        |
| ---------------- | ------------------------------------------------------------ |
| **Bug ID**       | BUG-001                                                      |
| **Title**        | POST /students returns 200 instead of 201 on successful creation |
| **Environment**  | Postman Mock Server                                          |
| **Endpoint**     | POST /students                                               |
| **Method**       | POST                                                         |
| **Severity**     | Low                                                          |
| **Priority**     | P3                                                           |
| **Status**       | Open                                                         |

### Preconditions

- Mock server is running
- User is authenticated (valid `authToken` in environment)

### Steps to Reproduce

1. Send a POST request to `{{baseUrl}}/students` with a valid student JSON body
2. Check the response status code

### Expected Result

- Status code: **201 Created**
- Response body contains the created student with an `id` field

### Actual Result

- Status code: **200 OK** (should be 201 for resource creation)
- Response body is correct otherwise

### Evidence

```
Status: 200 OK

Response body:
{
  "id": 4,
  "name": "New Student",
  "email": "new.student@example.com",
  "age": 20,
  "major": "Physics",
  "status": "active"
}
```

### Notes

This is a mock configuration issue — the saved example response needs its status code updated to 201. In a real API, this would be a minor backend issue but still worth logging because clients checking for 201 specifically would break.
