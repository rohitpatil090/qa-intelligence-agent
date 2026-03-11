# Skill: Jira Integration

## Authentication
All requests use Basic Auth with base64(email:apiToken) in Authorization header.
```
Authorization: Basic {base64(email:apiToken)}
Content-Type: application/json
```

## Key Endpoints

### Fetch Story
```
GET {baseUrl}/rest/api/3/issue/{issueId}
GET {baseUrl}/rest/api/3/issue/{issueId}?expand=renderedFields,names,changelog
```

### Fetch Comments
```
GET {baseUrl}/rest/api/3/issue/{issueId}/comment?maxResults=100&orderBy=created
```

### Fetch Remote Links (Confluence)
```
GET {baseUrl}/rest/api/3/issue/{issueId}/remotelink
```

### Fetch Linked Issues
From issue response: `fields.issuelinks[].outwardIssue` and `fields.issuelinks[].inwardIssue`

### Search Issues (for historical)
```
POST {baseUrl}/rest/api/3/search
Body: { "jql": "project = PROJ AND component = 'Orders' ORDER BY created DESC", "maxResults": 50 }
```

## Acceptance Criteria Location

Check these in order:
1. `fields.description` — look for "Acceptance Criteria" heading section
2. `fields.customfield_10016` — common AC custom field
3. `fields.customfield_10014` — alternate AC field
4. Rendered fields: `renderedFields.description`

## Parsing Atlassian Document Format

Story descriptions use ADF (Atlassian Document Format). Parse recursively:
- `type: "paragraph"` → join all `text` content nodes
- `type: "bulletList"` → extract each `listItem` text
- `type: "heading"` → treat as section separator
- `type: "codeBlock"` → preserve as code

## Error Handling
- 401: Invalid credentials
- 403: Permission denied — story not accessible
- 404: Story not found
- 429: Rate limit — wait and retry with exponential backoff
