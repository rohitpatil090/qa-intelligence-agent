# Skill: Bug Finding Patterns
## Common Defect Patterns by Domain

Use this skill to systematically check for known bug categories relevant to the story's domain.

---

## REST API Patterns

| Pattern | What to Check |
|---------|--------------|
| HTTP Method mismatch | Does the method match semantics? PUT idempotent? DELETE returns 204? |
| Missing auth on endpoint | Is auth required? Is it enforced for ALL methods on the resource? |
| Missing pagination | Does GET /collection return unlimited results? |
| 404 vs 403 confusion | Does a missing resource return 404 even when user is unauthorized? (Information leak) |
| PATCH partial update | Does PATCH correctly only update provided fields and not null out others? |
| Optimistic locking | On concurrent updates, is the last write wins or is there conflict detection? |

## Input Validation Patterns

| Pattern | What to Check |
|---------|--------------|
| String too long | Is there a max length? What happens if exceeded? |
| Negative numbers | Can numeric fields be negative when they shouldn't be? |
| Zero values | Is 0 a valid value? Is it treated differently? |
| Past dates | Can someone set a date in the past when it should be future only? |
| Future dates | Can someone set a date too far in the future? |
| Invalid enum values | What happens with values outside the defined enum? |
| Whitespace injection | "  " — is whitespace-only string treated as non-empty? |
| HTML/Script injection | Is HTML stripped from text inputs? |

## Database Patterns

| Pattern | What to Check |
|---------|--------------|
| Unique constraint | Is there a DB constraint backing application-level uniqueness check? |
| NOT NULL constraint | Is required data enforced at DB level, not just application? |
| Soft delete | Is `deleted_at` IS NULL included in all queries involving soft-deleted entities? |
| N+1 query | Does fetching a list trigger N additional queries? |
| Transaction missing | Do multi-step writes have a transaction? What happens on partial failure? |

## Kafka / Async Patterns

| Pattern | What to Check |
|---------|--------------|
| At-least-once delivery | Is consumer idempotent? What happens if message consumed twice? |
| Message ordering | Does logic depend on message order? Is ordering guaranteed? |
| Poison message | What happens with a malformed or unexpected message? Is there a DLQ? |
| Offset commit timing | Is offset committed before or after processing? (affects re-delivery behavior) |
| Schema evolution | What happens if producer sends a new field that consumer doesn't know about? |

## Financial / E-Commerce Patterns

| Pattern | What to Check |
|---------|--------------|
| Decimal precision | Are monetary values stored as DECIMAL(10,2) not FLOAT? |
| Rounding | Is rounding consistent? Do rounding errors accumulate? |
| Currency | Is currency validated? Can USD price be compared with EUR? |
| Negative totals | Can discounts make total negative? Is that handled? |
| Tax calculation | Is tax calculated on pre-discount or post-discount price? |

## Authentication / Authorization Patterns

| Pattern | What to Check |
|---------|--------------|
| IDOR | Is resource ID guessable? Is ownership validated? |
| JWT expiry | Is token expiry checked? What happens with expired token? |
| Role check bypass | Can a role check be bypassed by adding a query parameter? |
| Session fixation | After login, is a new session ID issued? |

## State Machine Patterns

| Pattern | What to Check |
|---------|--------------|
| Invalid transitions | Can entity jump from CREATED directly to COMPLETED skipping intermediate states? |
| Missing terminal state check | Can a CANCELLED entity be modified? |
| Concurrent state change | If two threads try to transition the same entity simultaneously, what happens? |

## Integration / Microservice Patterns

| Pattern | What to Check |
|---------|--------------|
| Timeout not configured | Is there a timeout on HTTP calls to other services? Default is often infinite. |
| Retry storm | On failure, do retries cause a thundering herd? Is there exponential backoff? |
| Circuit breaker | After N failures, does the system stop calling the failing service? |
| Stale data | If a downstream service is cached, how stale can the cache be? |
