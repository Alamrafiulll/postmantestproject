# Test Evidence — How to Capture It

This document explains how to create genuine test evidence after you've set up and run the project. None of the evidence below has been pre-generated — you need to run the project yourself and capture the results.

---

## What to Capture

### 1. Postman Collection Structure

Take a screenshot of the collection sidebar in Postman showing:
- The folder structure (Authentication, Students, Courses, etc.)
- The request names inside each folder

**How:** Open the collection in Postman → expand all folders → take a screenshot.

### 2. Successful Collection Run

Run the full collection and capture the results.

**How:**
1. Select the collection in Postman
2. Click "Run collection"
3. Make sure the correct environment is selected
4. Click "Run Student Management API - QA Automation"
5. Screenshot the results summary showing passed/failed counts

### 3. Failed Test Example

Intentionally break a test to show you understand what failure looks like.

**How:**
1. Pick any request (e.g., "Get All Students")
2. Temporarily change an assertion (e.g., change expected status from 200 to 500)
3. Run just that request
4. Screenshot the test results showing the failure
5. Revert your change after capturing

### 4. Test Assertions Detail

Show the assertions running for a specific request.

**How:**
1. Open any request (e.g., "Create Student")
2. Go to the "Scripts > Post-response" tab
3. Screenshot the JavaScript test code
4. Run the request and screenshot the "Test Results" panel at the bottom

### 5. Mock Server Configuration

Show how the mock server is set up.

**How:**
1. Open your Postman Mock Server settings
2. Screenshot the mock server URL and configuration
3. Optionally screenshot a saved example response

### 6. CLI Execution

Run the collection from the command line.

**How:**
```bash
# Login to Postman CLI
postman login --with-api-key YOUR_API_KEY

# Run the collection
postman collection run YOUR_COLLECTION_ID --environment YOUR_ENVIRONMENT_ID
```

Screenshot the terminal output showing the test results.

### 7. GitHub Actions Execution

After pushing the workflow and configuring secrets, show the CI run.

**How:**
1. Go to your GitHub repository → Actions tab
2. Trigger the workflow (push to main or manual trigger)
3. Screenshot the workflow run showing the job results
4. If it fails due to missing secrets, that's expected — screenshot it and note why

### 8. Final Test Report

Create a summary of your actual test results.

**How:** After running the full collection, document:
- Total tests run
- Tests passed
- Tests failed
- Any issues found
- Date and time of execution

---

## Where to Save Evidence

Save your evidence in a `docs/screenshots/` folder or wherever makes sense for your repository. If you include screenshots in the README or documentation, make sure they reflect actual execution — not fabricated results.

## Important

- Only include evidence from runs you've actually performed
- Don't edit screenshots to change pass/fail counts
- If some tests fail, document why (it might be a mock configuration issue, which is fine to explain)
- Date your evidence so reviewers know when it was captured
