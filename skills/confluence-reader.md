# Skill: Confluence Reader

## Authentication
Same credentials as Jira (Basic Auth with email:apiToken).
Base URL: `{confluenceBaseUrl}/rest/api/content`

## Fetch Page by ID
```
GET {baseUrl}/rest/api/content/{pageId}?expand=body.storage,body.view,version,ancestors
```

## Fetch Page by Title
```
GET {baseUrl}/rest/api/content?title={encoded_title}&spaceKey={space}&expand=body.storage
```

## Fetch Page Children
```
GET {baseUrl}/rest/api/content/{pageId}/child/page
```

## Parse Confluence Storage Format

Confluence pages are stored in XHTML-like "storage format". Parse:
- `<h1>`, `<h2>`, `<h3>` — section headers
- `<p>` — paragraph text
- `<ul>`, `<ol>`, `<li>` — lists
- `<ac:structured-macro ac:name="code">` — code blocks
- `<table>` — data tables (extract headers + rows)
- `<ac:link>` — internal links

Strip all HTML tags to get plain text for analysis.

## Key Content to Extract

When reading a design spec / requirements page:
1. **Data models** — field names, types, constraints
2. **API contracts** — endpoint definitions, schemas
3. **Business rules** — numbered or bulleted rules
4. **State machines** — diagrams or tables showing valid transitions
5. **Error codes** — defined error responses

## Error Handling
- 404: Page not found or no access
- Some Confluence pages may require specific space permissions
- If page unavailable, log the page URL and title and proceed without it
