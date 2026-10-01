# Test Plan — Student Management API

## Objective

Verify that the Student Management API handles authentication, CRUD operations for students, courses, and enrollments, and properly rejects invalid input. The tests run against a Postman Mock Server, so they validate request/response contracts rather than actual backend logic.

## Scope

### In Scope

- Authentication endpoint (login with valid/invalid credentials)
- Student endpoints: create, read, update, partial update, delete
- Course endpoints: create, read, update, delete
- Enrollment endpoints: create, read, delete
- Positive testing (valid inputs, expected responses)
- Negative testing (invalid input, missing fields, bad IDs)
- Boundary testing (age limits, string lengths, ID edge cases)
- Response validation (status codes, JSON structure, field types)
- Mock API response contract validation

### Out of Scope

- Performance / load testing
- Security testing beyond basic auth checks
- UI testing
- Database-level validation
- Integration with a real backend
- Cross-browser or mobile testing

## Test Environment

| Component         | Details                           |
| ----------------- | --------------------------------- |
| API               | Postman Mock Server               |
| Tool              | Postman (latest desktop version)  |
| CLI               | Postman CLI                       |
| CI                | GitHub Actions                    |
| Test Data         | Synthetic / hardcoded in examples |

## Test Data

All test data is synthetic. No real student records, emails, or personal information is used.

Sample data includes:
- Students: Maria Santos, James Chen, Aisha Patel (all fictional)
- Courses: CS101, MATH201, BIO110
- Emails: `maria.santos@example.com`, `james.chen@example.com`

Boundary values:
- Age: 16 (minimum), 65 (maximum), 15 (below min), 66 (above max)
- IDs: 0, -1, 999 (nonexistent)
- Strings: empty, single character, 300+ characters

## Test Types

| Type                | What it covers                                              |
| ------------------- | ----------------------------------------------------------- |
| Functional          | Do endpoints return correct data for valid requests?        |
| Positive            | Happy-path scenarios with valid input                       |
| Negative            | Invalid input, missing fields, bad IDs, unauthorized access |
| Boundary            | Edge values for age, string length, IDs                     |
| Validation          | Response structure, field types, required fields            |
| Authentication      | Login flow, token handling, unauthorized access              |
| Regression (Smoke)  | Quick pass through critical endpoints                       |

## Entry Criteria

- Postman collection is importable without errors
- Mock server is set up and returning expected examples
- Environment variables are configured with the mock server URL

## Exit Criteria

- All positive tests pass against the mock server
- All negative tests return expected error responses
- No test script has syntax errors
- Test results can be captured and reviewed

## Assumptions

- The mock server is the only system under test — it returns pre-defined responses
- Mock responses do not change between runs (deterministic)
- Business rules (like age range 16-65) are assumed for demonstration purposes and documented in test cases
- The mock server does not validate request bodies — it matches examples by URL, method, and optionally by `x-mock-response-name` header
- Tests validate response contracts, not actual server-side processing

## Risks and Limitations

| Risk / Limitation                                          | Impact                                              |
| ---------------------------------------------------------- | --------------------------------------------------- |
| Mock server returns static responses                       | Tests cannot catch real backend bugs                |
| No actual database behind the API                          | CRUD operations don't persist data between requests |
| Mock matching may not differentiate all negative scenarios  | Some negative tests may need `x-mock-response-name` |
| Postman CLI requires API key for cloud-synced collections  | CI setup requires secrets configuration             |
| Test data is hardcoded                                     | Not data-driven yet                                 |

## Test Execution

1. Import the collection into Postman
2. Import the environment and configure `baseUrl` with the mock server URL
3. Run the "01 - Authentication" folder first to set `authToken`
4. Run remaining folders or run the entire collection
5. For regression, run the "07 - Regression Smoke Suite" folder
6. Review the Postman test results panel for pass/fail
7. For CLI execution, use the Postman CLI with the collection and environment IDs
