# Skill: GitHub / GitLab PR Analysis

## GitHub Authentication
```
Authorization: Bearer {githubToken}
Accept: application/vnd.github+json
X-GitHub-Api-Version: 2022-11-28
```

## Key Endpoints

### Get PR Files (Diffs)
```
GET /repos/{owner}/{repo}/pulls/{pullNumber}/files
```
Returns: `filename`, `status` (added/modified/removed), `patch` (unified diff)

### Get PR Commits
```
GET /repos/{owner}/{repo}/pulls/{pullNumber}/commits
```

### Get File Content at PR Branch
```
GET /repos/{owner}/{repo}/contents/{path}?ref={branchName}
```
Response: base64 encoded content — decode before parsing.

### Compare Branch to Base
```
GET /repos/{owner}/{repo}/compare/{base}...{head}
```

## Parsing Unified Diffs

A patch looks like:
```
@@ -23,7 +23,12 @@
 // existing context line
-    // removed line
+    // added line
+    // another added line
 // more context
```

Focus on `+` lines (additions) for what the developer implemented.
Focus on `-` lines (removals) for what was changed/removed.

## GitLab Alternative
Replace `github.com/repos` with `gitlab.com/api/v4/projects/{id}/merge_requests/{mr_iid}/diffs`

## Code Pattern Recognition

When analyzing Java/Kotlin:
- `@RestController` + `@RequestMapping` → API endpoint
- `@KafkaListener` → Kafka consumer
- `@Transactional` → DB transaction boundary
- `@Valid` / `@Validated` → input validation active
- `try { } catch (Exception e) { log.error() }` → check if exception is re-thrown

When analyzing Python:
- `@app.route` / `@router.post` → FastAPI/Flask endpoint
- `@consumer.subscribe` → Kafka consumer
- `try: except Exception:` → check for silent swallowing

When analyzing Node.js:
- `router.post('/path', ...)` → Express endpoint
- `consumer.run({ eachMessage: ... })` → Kafka consumer

## Finding Missing Validations

Look for fields in request DTOs that have:
- No `@NotNull` / `@NotBlank` / `@Size` / `@Min` / `@Max` annotations
- No manual validation code (if statements checking the field)
- These fields → potential for null pointer exceptions or invalid data

## Test File Detection
Test files are typically in:
- `src/test/java/**/*Test.java` (Java)
- `tests/**/*_test.py` (Python)
- `**/*.spec.ts` or `**/*.test.js` (Node)

Analyze test coverage gaps by comparing tested methods against all service methods.
