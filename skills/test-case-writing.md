# Skill: Test Case Writing Best Practices

## Core Principles

1. **One test, one objective** — each test case tests exactly one thing
2. **Self-contained** — a tester can run it without asking questions
3. **Specific expected results** — not "should work" but exact values, statuses, payloads
4. **Reproducible** — same result every time, on any environment
5. **Independent** — doesn't depend on another test case's output

## Naming Convention
Format: `Verify [system/component] [action/behavior] when [condition/input]`

Examples:
- ✅ `Verify order creation returns HTTP 201 when all required fields are provided`
- ✅ `Verify order creation returns HTTP 400 when price exceeds maximum of $10,000`
- ✅ `Verify order.created Kafka event contains correct customerId after order creation`
- ❌ `Test order API`
- ❌ `Order creation negative`

## Priority Assignment

| Condition | Priority |
|-----------|----------|
| Core business function + Critical AC | Critical |
| Secondary function + High AC | High |
| Supporting features | Medium |
| Nice-to-have, cosmetic | Low |

## Precondition Writing Rules
- Be specific: "Customer with ID 'CUST-001' must exist in the DB" not "Customer must exist"
- Include auth state: "User must be logged in as ROLE_ADMIN"
- Include data state: "Order PROJ-ORDER-001 must be in status PENDING"

## Step Writing Rules
- Each step = one atomic action
- Include exact data values in steps
- Don't combine multiple actions in one step
- Distinguish between UI actions and API calls clearly

## Expected Results Writing Rules
- For APIs: HTTP status code + response body fields + DB state + any downstream effects
- For Kafka: topic name + field values in the message payload
- For UI: what the user sees + any data changes

## Anti-patterns to Avoid
- Vague steps: "Navigate to the order page" → Instead: "Navigate to https://app.example.com/orders"
- Missing data: "Create a customer" → Instead: "Create a customer with name='Test User', email='test@example.com'"
- Vague expected: "Error message appears" → Instead: "HTTP 400 with body `{ 'error': 'PRICE_EXCEEDS_MAXIMUM', 'message': 'Price cannot exceed $10,000' }`"
- Chained tests: "Use the order from the previous test case" → Use setup steps to create fresh data
