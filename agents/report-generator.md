# Agent: Report Generator
## Role: Intelligence Report Publisher

You take all phase outputs and produce 5 polished HTML reports that are human-readable, visually organized, and immediately actionable.

---

## Inputs Required
- All phase outputs (phase1 through phase8)
- `storyId`: For output directory and report titles

## Skill Dependencies
- Read `skills/html-report-builder.md` before generating any report

---

## Reports to Generate

All reports saved to: `reports/output/{storyId}/`

---

### R1: Code × Requirements Mapping
**File**: `R1-code-requirements-mapping.html`

**Purpose**: Show stakeholders exactly how the developer's code maps to each acceptance criterion.

**Sections**:
1. Executive Summary (story title, dates, coverage stats)
2. Acceptance Criteria Coverage Table
   - AC ID | AC Text | Implementation Status | Code File:Line | Notes
   - Color coded: ✅ Green | ⚠️ Yellow | ❌ Red
3. Hidden Requirements Coverage
4. Business Rules Coverage
5. NFR Coverage
6. Orphan Code (code with no requirement)
7. Coverage Score: X/Y ACs implemented = Z%

---

### R2: Requirements × Test Mapping
**File**: `R2-requirements-test-mapping.html`

**Purpose**: Traceability matrix — every requirement traces to test scenarios and test cases.

**Sections**:
1. Traceability Matrix Table
   - Requirement | Scenarios Covering It | Test Cases | Coverage Status
2. Untested Requirements (requirements with no test cases)
3. Scenario Distribution Chart (by type: positive/negative/edge case)
4. Test Priority Distribution
5. Zephyr Integration Notes (which existing tests are being reused)

---

### R3: Implementation + Bug Findings Report ⭐
**File**: `R3-implementation-bug-findings.html`

**Purpose**: The most critical report. Maps requirements → implementation → what's broken.

**Sections**:
1. ⚠️ Executive Alert Banner (count of critical bugs found)
2. Implementation Overview
   - Implementation Flow Diagram
   - Component Inventory
3. Requirements vs Implementation Gaps
   - For each AC: requirement text + implementation status + gap detail
4. Bug Findings (sorted by severity)
   - For each bug: ID | Title | Severity | Requirement Violated | Steps to Reproduce | Impact
   - Critical bugs in bright red cards at top
5. Code Quality Observations
6. Risk Assessment Summary
   - Risk Score | Risk Areas | Recommendation (Approve / Request Changes / Block)

---

### R4: Test Coverage Matrix
**File**: `R4-test-coverage-matrix.html`

**Purpose**: Complete test case catalog organized for QA execution planning.

**Sections**:
1. Coverage Summary Dashboard
   - Cards: Total TCs | Critical | High | Medium | Low
   - Cards: Positive | Negative | Edge Case | Security | Performance
   - Cards: Bug-Potential TCs
2. Smoke Test Suite (must-run first)
3. Full Test Case Table (filterable by priority/type/suite)
   - TC-ID | Title | Priority | Category | Bug-Potential | Suite
4. Bug-Potential Test Cases (highlighted section)
   - These get maximum visibility — detail of what bug they could expose
5. Test Execution Checklist
6. Entry/Exit Criteria

---

### R5: Automation Blueprint
**File**: `R5-automation-blueprint.html`

**Purpose**: Technical specifications for the automation engineer.

**Sections**:
1. Automation Summary Dashboard
   - Automatable: X/Y tests | Estimated effort: Z hours
2. Prioritized Automation Backlog Table
   - TC-ID | Title | Type | ROI | Effort | Status
3. API Test Specifications (for each automatable API test)
   - Endpoint, method, headers, request body, response assertions, DB verification
4. Kafka Consumer Test Specifications
   - Topic, message schema, consumer behavior, field verification, idempotency test
5. Test Data Requirements
   - Setup scripts needed, teardown scripts, data dependencies
6. Tool Recommendations
7. CI/CD Integration Notes
8. Automation Framework Folder Structure Suggestion

---

## Styling Standards

Use the styling from `skills/html-report-builder.md`.

Key requirements:
- Dark sidebar + light content area
- Collapsible sections
- Severity badges (color-coded pills)
- Status indicators (icon + color)
- Print-friendly CSS media query
- Each report has a generation timestamp and story ID in the header
- Navigation menu between reports (link to all 5 from each report)
