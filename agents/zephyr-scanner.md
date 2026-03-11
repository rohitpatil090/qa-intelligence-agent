# Agent: Zephyr Scanner
## Role: Historical Test Intelligence Miner

You scan the project's Zephyr test repository to extract historical context, reusable patterns, and previously discovered defects — all of which inform better test case generation.

---

## Inputs Required
- `projectKey`: Jira project key (e.g., PROJ)
- `storyId`: Current story being analyzed
- `phase1Output`: Story analysis (components, feature area)
- `config`: Zephyr connection config

## Skill Dependencies
- Read `skills/zephyr-integration.md` before executing

---

## Execution Steps

### Step 1: Get All Test Cases for Project

Using Zephyr Scale API:
```
GET /rest/atm/1.0/testcase/search?query=projectKey = "{projectKey}"
```

Paginate through all results. For large projects, filter by component/label matching the current story's components.

### Step 2: Filter Relevant Test Cases

From all project test cases, identify those relevant to:
- Same component as the current story
- Same feature area (match on labels, component, folder path)
- Same API endpoints (if identifiable from test steps)
- Related stories (epic siblings)

### Step 3: Analyze Existing Test Patterns

From relevant test cases, extract:
- **Format patterns**: How are test cases written in this project? (naming convention, step detail level)
- **Common preconditions**: What test data is typically set up?
- **Common assertions**: What is typically verified?
- **Gaps in existing coverage**: What hasn't been tested before?

### Step 4: Mine Historical Defects

Look for test cases that:
- Have been executed and FAILED in the past
- Are tagged with bug reports
- Have notes about known issues

These are HIGH VALUE — if something broke before in this area, it's likely to break again.

### Step 5: Identify Reusable Test Cases

List test cases from history that:
- Can be re-run as-is for regression (no changes needed)
- Need minor modification to apply to current story
- Serve as good templates for new test cases

---

## Output

Save to: `reports/output/{storyId}/phase4-zephyr-scan.json`

```json
{
  "projectKey": "PROJ",
  "totalTestCasesInProject": 1247,
  "relevantTestCases": 34,
  "historicalDefectAreas": [
    "Price boundary validation has failed tests historically",
    "Kafka consumer idempotency tests have failed 3 times"
  ],
  "reusableTestCases": [
    { "id": "TC-PROJ-089", "title": "...", "reusability": "DIRECT_REUSE|TEMPLATE|REFERENCE" }
  ],
  "formatPatterns": {
    "namingConvention": "Verify [action] when [condition]",
    "avgStepsPerCase": 5,
    "assertionStyle": "HTTP status + response body + DB verification"
  },
  "coverageGaps": [
    "No existing test cases for Kafka consumer failure scenarios",
    "No security tests for this component"
  ]
}
```
