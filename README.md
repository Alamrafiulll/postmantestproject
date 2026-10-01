# Student Management API Testing with Postman

I built this project to practice and demonstrate API testing with Postman. The idea is straightforward: I created a fictional Student Management API, mocked it with Postman's Mock Server, and wrote automated tests to validate the endpoints.

The API doesn't have a real backend — everything runs through Postman's mock responses. This means you can clone the project, import the collection, and run the tests without setting up any servers or databases. I focused on testing the request/response contracts: correct status codes, proper JSON structure, required fields, error handling, and edge cases.

I tried to cover both happy-path scenarios and the kind of negative tests that matter in real work — invalid inputs, missing fields, boundary values, and duplicate detection.

---

## Project Overview

This is an open-source API testing portfolio project for a fictional Student Management System. It demonstrates REST API testing through Postman, covering authentication, student CRUD operations, course management, and enrollment handling.

The project is about **quality assurance and API testing**, not software development. There's no backend code — the entire API is simulated using Postman Mock Server with saved response examples.

## What I Tested

- **Authentication** — login with valid/invalid credentials, missing fields, empty body
- **Students** — full CRUD (create, read, update, partial update, delete)
- **Courses** — CRUD operations and validation
- **Enrollments** — create, read, delete, duplicate detection
- **Negative testing** — invalid inputs, nonexistent resources, duplicate entries, unauthorized access
- **Boundary testing** — age limits, string length limits, invalid IDs (zero, negative)
- **Response validation** — status codes, JSON structure, field types, error messages

## Tools

| Tool                | Used For                                    |
| ------------------- | ------------------------------------------- |
| Postman             | API testing, collection organization        |
| Postman Mock Server | Simulating API responses                    |
| JavaScript          | Test assertions inside Postman              |
| Postman CLI         | Running collection from the command line    |
| Git                 | Version control                             |
| GitHub Actions      | CI workflow for automated test execution    |

## Test Coverage

| Area               | Requests | Notes                                     |
| ------------------ | -------- | ----------------------------------------- |
| Authentication     | 5        | Valid login + 4 negative scenarios         |
| Students           | 6        | Full CRUD including PATCH                  |
| Courses            | 5        | CRUD operations                            |
| Enrollments        | 4        | Create, read, delete                       |
| Negative Tests     | 10       | Missing fields, invalid data, duplicates   |
| Validation Tests   | 7        | Boundary values for age, name length, IDs  |
| Regression Smoke   | 5        | Quick pass through critical endpoints      |
| **Total**          | **42**   | Includes collection-level assertions       |

> Each request also runs 2 collection-level tests (Content-Type header check, response time check), so the actual assertion count per run is higher than the request count.

## Repository Structure

```
├── .github/
│   └── workflows/
│       └── postman-tests.yml        # GitHub Actions CI workflow
├── docs/
│   ├── test-plan.md                 # Test plan
│   ├── test-cases.md                # 37 documented test cases
│   ├── test-scenarios.md            # Test scenarios by feature area
│   ├── bug-report-template.md       # Bug report template with example
│   └── evidence.md                  # How to capture test evidence
├── postman/
│   ├── Student-Management-API-QA.postman_collection.json   # Main collection
│   └── environments/
│       └── qa.environment.example.json                     # Environment template
├── .gitignore
├── LICENSE
└── README.md
```

## How to Run

### Prerequisites

- [Postman](https://www.postman.com/downloads/) (desktop app)
- A free Postman account (for Mock Server)

### Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/Alamrafiulll/student-management-api-testing.git
   ```

2. **Import the collection**
   - Open Postman
   - Click "Import" (top left)
   - Select `postman/Student-Management-API-QA.postman_collection.json`

3. **Import the environment**
   - Click "Import" again
   - Select `postman/environments/qa.environment.example.json`

4. **Create a Mock Server**
   - In Postman, right-click the imported collection
   - Select "Mock collection"
   - Give it a name (e.g., "Student Management Mock")
   - Copy the mock server URL (looks like `https://xxxxxxxx-xxxx.mock.pstmn.io`)

5. **Configure the environment**
   - Select the "Student Management API - QA" environment from the dropdown (top right)
   - Set `baseUrl` to your mock server URL
   - Save

6. **Run the collection**
   - Click on the collection name
   - Click "Run collection"
   - Make sure the environment is selected
   - Click "Run Student Management API - QA Automation"

### Running the Regression Suite Only

To run just the smoke tests:
1. Expand the collection → "07 - Regression Smoke Suite"
2. Right-click the folder → "Run folder"

## Postman Mock Server

The collection includes **saved response examples** for every request. These examples are what the Mock Server uses to return responses. When you create a Mock Server from this collection, Postman uses the saved examples to simulate API behavior.

How it works:
- Each request has one or more saved responses (examples)
- The mock server matches incoming requests to examples by URL, method, and headers
- For requests that share the same URL (like multiple `POST /students` negative tests), the `x-mock-response-name` header tells the mock server which example to return
- Responses cover status codes: 200, 201, 400, 401, 404, 409

**Important:** The mock server returns static responses. It doesn't actually process request bodies or maintain state between requests. The tests validate the response contract, not real business logic.

## Automated Assertions

Every meaningful request in the collection includes JavaScript test assertions. Here's what I'm validating:

- **Status codes** — each request checks for the expected HTTP status
- **Response format** — responses are validated as JSON
- **Required fields** — student, course, and enrollment responses are checked for all expected fields
- **Data types** — field types are verified (number, string, array)
- **Value matching** — specific values are compared against expectations
- **Error messages** — negative tests verify that error responses contain relevant information
- **Collection-level** — every response is checked for Content-Type header and response time under 5 seconds

Example (from "Get All Students"):
```javascript
pm.test('Each student has required fields', function () {
    var jsonData = pm.response.json();
    jsonData.forEach(function (student) {
        pm.expect(student).to.have.property('id');
        pm.expect(student).to.have.property('name');
        pm.expect(student).to.have.property('email');
        pm.expect(student).to.have.property('age');
        pm.expect(student).to.have.property('major');
        pm.expect(student).to.have.property('status');
    });
});
```

## CLI Execution

You can run the collection from the command line using the Postman CLI:

```bash
# Install Postman CLI (if not already installed)
# See: https://learning.postman.com/docs/postman-cli/postman-cli-installation/

# Login
postman login --with-api-key YOUR_POSTMAN_API_KEY

# Run the collection
postman collection run YOUR_COLLECTION_ID --environment YOUR_ENVIRONMENT_ID
```

**To find your IDs:**
- Collection ID: Right-click the collection in Postman → Share → Via API → copy the UID
- Environment ID: Same process for the environment

> Replace `YOUR_POSTMAN_API_KEY`, `YOUR_COLLECTION_ID`, and `YOUR_ENVIRONMENT_ID` with your actual values. Don't commit your API key.

## GitHub Actions

The repository includes a CI workflow at `.github/workflows/postman-tests.yml`. It runs the Postman collection automatically on pushes to `main` and on pull requests.

**To set it up:**
1. Add your Postman API key as a GitHub secret named `POSTMAN_API_KEY`
2. Update the workflow file to replace `<YOUR_COLLECTION_ID>` and `<YOUR_ENVIRONMENT_ID>` with your actual IDs
3. Push to `main` to trigger the workflow

The workflow will fail until you configure the secrets and IDs — this is expected.

## Test Evidence

This repository includes:
- ✅ Exported Postman collection (importable)
- ✅ Environment template (safe, no secrets)
- ✅ 37 documented test cases
- ✅ Test plan
- ✅ Test scenarios
- ✅ Bug report template
- ✅ Mock API examples (embedded in collection)
- ✅ CI workflow
- ✅ Evidence capture guide

For instructions on how to capture actual test execution evidence (screenshots, CLI output, CI results), see [docs/evidence.md](docs/evidence.md).

## Limitations

- This is a **portfolio project** using a Postman Mock Server, not a real backend
- The mock server returns static responses — it doesn't actually validate business rules or persist data
- Negative tests rely on `x-mock-response-name` headers to get the right mock response, which is a workaround for mock server limitations
- Test data is hardcoded in the saved examples, not generated dynamically
- The mock server won't catch real bugs because there's no real code behind it

I built this to demonstrate testing methodology, not to test a production system.

## Future Improvements

- Add data-driven testing with CSV/JSON data files
- Add JSON schema validation for more requests
- Integrate with a real staging API if one becomes available
- Add contract testing (e.g., with Pact)
- Connect to a defect tracking tool
- Add more boundary tests for string fields and special characters

## License

This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.
