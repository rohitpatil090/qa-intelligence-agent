# How It Works — Agent Pipeline Deep Dive

## Overview

The QA Intelligence Agent is a 9-phase pipeline orchestrated by Claude Code. Each phase builds on the previous, creating an increasingly rich picture of the feature, its implementation, and its test coverage.

```
Phase 1: STORY INTAKE        → What needs to be built?
Phase 2: CODE ANALYSIS       → What was actually built?
Phase 3: REQUIREMENTS MAP    → Does code match requirements?
Phase 4: HISTORICAL SCAN     → What do we know from the past?
Phase 5: SCENARIO GENERATION → What could go wrong?
Phase 6: BUG FINDING         → What DID go wrong?
Phase 7: TEST CASE DESIGN    → How do we test it?
Phase 8: AUTOMATION ANALYSIS → What can we automate?
Phase 9: REPORT GENERATION   → Human-readable output
```

---

## Phase 1: Story Intake

The agent reads the Jira story, not just the main description, but ALL associated context:

**What gets fetched**:
- Main story description + acceptance criteria
- ALL comments (often contain hidden requirements from POs, architects, other engineers)
- Linked Confluence pages (design specs, API contracts, data models)
- Related stories (epic siblings, dependencies, sub-tasks)

**Why comments matter**: POs often clarify edge cases in comments that don't make it into the formal AC. The agent specifically looks for language patterns like "but also", "note that", "important:", "edge case:", "don't forget" — these are almost always hidden requirements.

---

## Phase 2: Code Analysis

The agent reads the PR diff and builds an implementation map:

**What gets analyzed**:
- Every modified file in the PR
- API endpoints (method, path, request/response schemas, auth)
- Service layer logic (business rules, external calls)
- Database operations (queries, transactions, constraints)
- Kafka producers (topic, message schema, trigger conditions)
- Kafka consumers (topic, handler logic, error handling)
- Existing tests (what's covered, what isn't)

**The key output** is an implementation flow diagram that shows how data flows from the API request all the way through to the database and any downstream events.

---

## Phase 6: Bug Finding (Most Important)

This is where the agent earns its keep. It doesn't just list test scenarios — it actively looks for bugs by:

1. **Cross-referencing requirements and code**: For each AC, does the code FULLY satisfy it? Not just partially?

2. **Applying 15 bug pattern categories**: Each pattern (boundary violations, null handling, race conditions, etc.) is checked against the actual code.

3. **Tracing unhappy paths**: For every external call (DB, API, Kafka), what happens when it fails?

4. **Checking business rule enforcement**: Are business rules enforced at every entry point? Can they be bypassed?

The output is a list of concrete scenarios that will expose bugs, with exact steps to reproduce, expected vs actual behavior, and severity.

---

## Report Hierarchy

| Report | Audience | Purpose |
|--------|----------|---------|
| R3 Bug Findings | Dev + QA + PM | Most critical — what's broken |
| R4 Test Matrix | QA | Full test execution plan |
| R1 Code Map | Tech Lead + QA | Code review support |
| R2 Req Trace | BA + QA | Traceability & coverage |
| R5 Automation | Automation Engineer | Technical specs |

**Always open R3 first.**
