# 🧠 QA Intelligence Agent — v2.0

> An AI-powered QA automation framework built for Claude Code that transforms Jira user stories into comprehensive test coverage, bug-finding intelligence, and automation blueprints.

---

## 🏗️ Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                    QA INTELLIGENCE ORCHESTRATOR                  │
│                        (CLAUDE.md / Main Agent)                  │
└─────────────────────────┬───────────────────────────────────────┘
                           │ coordinates
     ┌─────────────────────┼──────────────────────┐
     ▼                     ▼                       ▼
┌──────────┐        ┌──────────────┐       ┌─────────────────┐
│  INTAKE  │        │   ANALYSIS   │       │    REPORTING    │
│  LAYER   │        │    LAYER     │       │     LAYER       │
└──────────┘        └──────────────┘       └─────────────────┘
     │                     │                       │
     ▼                     ▼                       ▼
• story-analyzer     • requirements-mapper   • report-generator
• code-analyzer      • test-scenario-gen     (5 HTML reports)
• zephyr-scanner     • bug-finder
• confluence-reader  • test-case-designer
                     • automation-analyzer
```

---

## 🤖 Agents

| Agent | Role |
|-------|------|
| `orchestrator` | Master coordinator — runs the full pipeline |
| `story-analyzer` | Reads Jira story, comments, linked Confluence pages |
| `code-analyzer` | Parses PR diffs, commits, implementation flow |
| `requirements-mapper` | Maps ACs to code line-by-line |
| `test-scenario-generator` | Generates all positive/negative/edge scenarios |
| `bug-finder` | Identifies what the developer missed |
| `test-case-designer` | Writes detailed, best-practice test cases |
| `zephyr-scanner` | Scans existing Zephyr test cases for reference |
| `automation-analyzer` | Identifies automation candidates + approach |
| `report-generator` | Produces all 5 HTML reports |

---

## 📁 File Structure

```
qa-intelligence-agent/
├── CLAUDE.md                        ← Entry point for Claude Code
├── README.md
├── config/
│   ├── connections.example.json     ← Jira/Confluence/GitHub config template
│   └── agent-config.json            ← Agent behavior settings
├── agents/
│   ├── orchestrator.md              ← Main orchestrator prompt
│   ├── story-analyzer.md
│   ├── code-analyzer.md
│   ├── requirements-mapper.md
│   ├── test-scenario-generator.md
│   ├── bug-finder.md
│   ├── test-case-designer.md
│   ├── zephyr-scanner.md
│   ├── automation-analyzer.md
│   └── report-generator.md
├── skills/
│   ├── jira-integration.md          ← Skill: Jira API patterns
│   ├── zephyr-integration.md        ← Skill: Zephyr Scale API patterns
│   ├── confluence-reader.md         ← Skill: Confluence API patterns
│   ├── github-analyzer.md           ← Skill: GitHub/GitLab PR analysis
│   ├── test-case-writing.md         ← Skill: Best practices test case writing
│   ├── bug-finding-patterns.md      ← Skill: Common bug patterns by domain
│   └── html-report-builder.md       ← Skill: Report generation patterns
├── reports/
│   └── templates/
│       ├── R1-code-requirements-mapping.html
│       ├── R2-requirements-test-mapping.html
│       ├── R3-implementation-bug-findings.html
│       ├── R4-test-coverage-matrix.html
│       └── R5-automation-blueprint.html
├── tools/
│   ├── fetch-jira-story.js          ← Helper: fetch story + comments + links
│   ├── fetch-pr-diff.js             ← Helper: fetch and parse PR diff
│   ├── fetch-zephyr-tests.js        ← Helper: fetch existing test cases
│   └── generate-report.js           ← Helper: render HTML reports
├── examples/
│   └── sample-run-output/           ← Example outputs for reference
└── docs/
    ├── setup.md                     ← Installation & configuration
    ├── how-it-works.md              ← Deep dive on agent pipeline
    └── customization.md             ← How to extend agents/skills
```

---

## 🚀 Quick Start

### 1. Prerequisites
```bash
# Install Claude Code
npm install -g @anthropic-ai/claude-code

# Clone this repo
git clone https://github.com/YOUR_ORG/qa-intelligence-agent.git
cd qa-intelligence-agent

# Install dependencies
npm install
```

### 2. Configure Connections
```bash
cp config/connections.example.json config/connections.json
# Edit connections.json with your Jira, Confluence, GitHub tokens
```

### 3. Run the Agent
```bash
# Full analysis on a user story
claude "Analyze user story PROJECT-1234 and generate full QA report"

# Or with explicit config
claude "Run QA Intelligence on STORY-456, PR #89, project KEY"
```

---

## 📊 Reports Generated

| Report | Description |
|--------|-------------|
| **R1** — Code × Requirements | Maps developer code to each acceptance criterion |
| **R2** — Requirements × Tests | Maps requirements to test scenarios & test cases |
| **R3** — Implementation + Bug Findings | Critical report: what dev missed, breaking scenarios |
| **R4** — Test Coverage Matrix | Categorized test cases: Critical/High/Medium, +ve/-ve/Bug |
| **R5** — Automation Blueprint | Full automation plan with APIs, Kafka, validators |

---

## ⚙️ Configuration

```json
// config/connections.json
{
  "jira": {
    "baseUrl": "https://yourorg.atlassian.net",
    "email": "your@email.com",
    "apiToken": "YOUR_JIRA_TOKEN",
    "projectKey": "YOUR_PROJECT"
  },
  "confluence": {
    "baseUrl": "https://yourorg.atlassian.net/wiki",
    "apiToken": "YOUR_CONFLUENCE_TOKEN"
  },
  "github": {
    "baseUrl": "https://api.github.com",
    "token": "YOUR_GITHUB_TOKEN",
    "repo": "org/repo"
  },
  "zephyr": {
    "apiToken": "YOUR_ZEPHYR_TOKEN",
    "projectId": "YOUR_PROJECT_ID"
  }
}
```

---

## 🧩 Extending

- Add new **bug patterns** → edit `skills/bug-finding-patterns.md`
- Add new **report section** → edit `agents/report-generator.md` + add template
- Add new **integration** → create new skill in `skills/` + reference in orchestrator

---

## 📄 License

MIT — use freely, contribute back.
