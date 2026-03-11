# Setup & Configuration Guide

## Prerequisites

- **Node.js** 18+ 
- **Claude Code** CLI installed: `npm install -g @anthropic-ai/claude-code`
- Access to Jira Cloud (with API token)
- Access to Confluence Cloud
- Access to GitHub or GitLab (with token)
- Zephyr Scale license on your Jira project

---

## Installation

```bash
# 1. Clone the repo
git clone https://github.com/YOUR_ORG/qa-intelligence-agent.git
cd qa-intelligence-agent

# 2. Copy config template
cp config/connections.example.json config/connections.json

# 3. Edit with your credentials
nano config/connections.json  # or use your editor

# 4. Install any Node helpers (optional)
npm install
```

---

## Getting API Tokens

### Jira API Token
1. Go to https://id.atlassian.com/manage-profile/security/api-tokens
2. Click "Create API token"
3. Copy the token into `connections.json` → `jira.apiToken`

### Zephyr Scale Token
1. In Jira, go to Apps → Zephyr Scale
2. Go to Settings → API Access Tokens
3. Create a new token
4. Copy into `connections.json` → `zephyr.apiToken`

### GitHub Token
1. GitHub → Settings → Developer Settings → Personal Access Tokens
2. Create token with `repo` scope
3. Copy into `connections.json` → `github.token`

---

## Running the Agent

### Basic Run
```bash
cd qa-intelligence-agent
claude "Analyze user story PROJ-1234 and generate full QA reports"
```

### With PR Reference
```bash
claude "Analyze user story PROJ-1234 with PR #89"
```

### With Branch Reference  
```bash
claude "Analyze PROJ-1234, branch feature/payment-gateway-v2"
```

### Partial Run (Skip Phases)
```bash
claude "Only run bug finding for PROJ-1234, skip automation analysis"
```

---

## Output Location

All reports are saved to:
```
reports/output/{STORY_ID}/
├── phase1-story-analysis.json
├── phase2-code-analysis.json
├── phase3-requirements-mapping.json
├── phase4-zephyr-scan.json
├── phase5-scenarios.json
├── phase6-bug-findings.json
├── phase7-test-cases.json
├── phase8-automation-blueprint.json
├── R1-code-requirements-mapping.html
├── R2-requirements-test-mapping.html
├── R3-implementation-bug-findings.html  ← Open this first
├── R4-test-coverage-matrix.html
└── R5-automation-blueprint.html
```

Open `R3-implementation-bug-findings.html` in your browser first — it contains the most critical information.

---

## Troubleshooting

### "401 Unauthorized" from Jira
- Check `email` and `apiToken` in connections.json
- Ensure the API token is for the correct Atlassian account
- Verify the account has access to the project

### "Zephyr tests not found"
- Verify `zephyr.projectId` matches the Jira project key
- Ensure Zephyr Scale is installed on your Jira instance (not Zephyr for Jira)

### "PR files not found"
- For GitHub, ensure `owner` and `repo` are correct
- Ensure the GitHub token has `repo` read scope
- The PR must be on the repo specified in config
