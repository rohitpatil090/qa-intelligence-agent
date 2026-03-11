# Skill: HTML Report Builder

## Design System

All reports share these CSS variables and base styles.

### Color Palette
```css
:root {
  --bg-primary: #0f1117;
  --bg-secondary: #1a1d2e;
  --bg-card: #1e2130;
  --bg-card-hover: #252840;
  --border: #2a2d3e;
  --accent-blue: #4f8ef7;
  --accent-purple: #7c5cbf;
  --text-primary: #e2e8f0;
  --text-secondary: #94a3b8;
  --text-muted: #64748b;
  --critical: #ef4444;
  --high: #f97316;
  --medium: #eab308;
  --low: #22c55e;
  --success: #10b981;
  --warning: #f59e0b;
  --info: #3b82f6;
}
```

### Severity Badges
```html
<span class="badge badge-critical">Critical</span>
<span class="badge badge-high">High</span>
<span class="badge badge-medium">Medium</span>
<span class="badge badge-low">Low</span>
```
```css
.badge { padding: 2px 10px; border-radius: 12px; font-size: 11px; font-weight: 600; }
.badge-critical { background: rgba(239,68,68,0.15); color: #ef4444; border: 1px solid rgba(239,68,68,0.3); }
.badge-high     { background: rgba(249,115,22,0.15); color: #f97316; border: 1px solid rgba(249,115,22,0.3); }
.badge-medium   { background: rgba(234,179,8,0.15); color: #eab308; border: 1px solid rgba(234,179,8,0.3); }
.badge-low      { background: rgba(34,197,94,0.15); color: #22c55e; border: 1px solid rgba(34,197,94,0.3); }
```

### Status Icons
- ✅ Fully Covered / Pass
- ⚠️ Partial / Warning  
- ❌ Not Covered / Fail
- 🐛 Bug Found
- 🔐 Security
- ⚡ Performance
- 🤖 Automation Candidate

### Report Header Template
```html
<header class="report-header">
  <div class="report-meta">
    <span class="story-id">{{storyId}}</span>
    <span class="report-type">{{reportName}}</span>
  </div>
  <div class="story-title">{{storyTitle}}</div>
  <div class="report-footer-meta">
    Generated: {{timestamp}} | QA Intelligence Agent v2.0
  </div>
  <nav class="report-nav">
    <a href="R1-code-requirements-mapping.html">R1: Code Map</a>
    <a href="R2-requirements-test-mapping.html">R2: Req Trace</a>
    <a href="R3-implementation-bug-findings.html" class="nav-critical">R3: Bug Report ⭐</a>
    <a href="R4-test-coverage-matrix.html">R4: Test Coverage</a>
    <a href="R5-automation-blueprint.html">R5: Automation</a>
  </nav>
</header>
```

### Collapsible Section
```html
<details class="section">
  <summary class="section-title">Section Title</summary>
  <div class="section-content">
    <!-- content -->
  </div>
</details>
```

### Data Table Template
```html
<table class="data-table">
  <thead>
    <tr>
      <th>Column 1</th>
      <th>Column 2</th>
    </tr>
  </thead>
  <tbody>
    <tr class="row-critical"> <!-- or row-high, row-medium, row-low -->
      <td>...</td>
      <td>...</td>
    </tr>
  </tbody>
</table>
```

### Bug Card Template
```html
<div class="bug-card severity-critical">
  <div class="bug-card-header">
    <span class="bug-id">BUG-001</span>
    <span class="badge badge-critical">Critical</span>
    <span class="bug-confidence">Confidence: High</span>
  </div>
  <div class="bug-title">Bug title here</div>
  <div class="bug-meta">
    <span>Requirement: AC-3</span>
    <span>Code: OrderService.java:L67</span>
  </div>
  <details>
    <summary>Reproduction Steps</summary>
    <ol class="steps-list">
      <li>Step 1</li>
      <li>Step 2</li>
    </ol>
    <div class="expected-actual">
      <div class="expected">✅ Expected: ...</div>
      <div class="actual">❌ Actual: ...</div>
    </div>
  </details>
  <div class="bug-impact">💥 Impact: ...</div>
</div>
```

## Required Base CSS (include in every report)
```css
* { box-sizing: border-box; margin: 0; padding: 0; }
body { font-family: 'JetBrains Mono', 'Fira Code', monospace; background: var(--bg-primary); color: var(--text-primary); line-height: 1.6; }
.container { max-width: 1400px; margin: 0 auto; padding: 24px; }
.data-table { width: 100%; border-collapse: collapse; }
.data-table th { background: var(--bg-secondary); padding: 10px 14px; text-align: left; font-size: 12px; text-transform: uppercase; letter-spacing: 0.05em; color: var(--text-secondary); border-bottom: 1px solid var(--border); }
.data-table td { padding: 10px 14px; border-bottom: 1px solid var(--border); font-size: 13px; }
.data-table tr:hover td { background: var(--bg-card-hover); }
summary { cursor: pointer; user-select: none; }
details[open] summary { margin-bottom: 12px; }
@media print { .report-nav { display: none; } body { background: white; color: black; } }
```

## Font Import
```html
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@300;400;500;600;700&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
```
