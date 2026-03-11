# Skill: Zephyr Scale Integration

## Authentication
Zephyr Scale uses its own API token (separate from Jira).
```
Authorization: Bearer {zephyrApiToken}
Content-Type: application/json
```
Base URL for Zephyr Scale Cloud: `https://api.zephyrscale.smartbear.com/v2`

## Key Endpoints

### Get All Test Cases for Project
```
GET /testcases?projectKey={projectKey}&maxResults=100&startAt=0
```
Paginate: increment `startAt` by `maxResults` until `isLast: true`.

### Get Test Case Details
```
GET /testcases/{testCaseKey}
```

### Get Test Case Steps
```
GET /testcases/{testCaseKey}/teststeps
```

### Search Test Cases
```
GET /testcases?projectKey={projectKey}&query={keyword}&maxResults=50
```

### Get Test Executions (to find failures)
```
GET /testexecutions?projectKey={projectKey}&testCaseKey={key}
```
Look for executions with `status.name = "Fail"` — these are previously found bugs.

### Get Folders (Test Suites)
```
GET /folders?projectKey={projectKey}&folderType=TEST_CASE
```

## Filtering by Component/Feature

Use JQL-style queries in Zephyr:
```
GET /testcases?projectKey=PROJ&query=folder = "Orders Module" AND priority = "High"
```

Or filter by label:
```
GET /testcases?projectKey=PROJ&query=label = "order-management"
```

## Pagination Pattern
```javascript
let allTests = [];
let startAt = 0;
const maxResults = 100;
let isLast = false;

while (!isLast) {
  const response = await fetch(`/testcases?projectKey=${key}&maxResults=${maxResults}&startAt=${startAt}`);
  const data = await response.json();
  allTests = allTests.concat(data.values);
  isLast = data.isLast;
  startAt += maxResults;
}
```

## Error Handling
- 401: Invalid Zephyr token (note: this is DIFFERENT from Jira token)
- 403: License issue or project access
- Check `x-zephyr-scale-api-deprecated` header for deprecation warnings
