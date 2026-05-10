# Example: REST API Governance

This example shows how Constitutional Governance applies to a team that owns a set of REST APIs.

The platform team sets standards for API design, versioning, and error responses. Product teams own their APIs and must comply with the standards. AI agents assisting API developers query the governance server to learn what is required before generating OpenAPI specs, route handlers, or gateway configuration.

---

## The governance repository structure

```
governance-repo/
├── governance.yml
├── constitution.md
├── constitutions/
│   └── api.md
├── adrs/
│   └── api/
│       ├── 001-url-structure.md
│       ├── 002-versioning-strategy.md
│       ├── 003-error-response-format.md
│       └── 004-pagination-contract.md
├── features/
│   └── api/
│       ├── url-structure.feature        @enforced
│       ├── versioning.feature           @enforced
│       ├── error-format.feature         @enforced
│       └── pagination.feature           @wip
└── conventions/
    └── openapi/
        └── error-schema.yml
```

---

## The constitution (`constitutions/api.md`)

```markdown
# REST API Constitution

## Purpose

APIs are the public contracts of this platform. They are consumed by
internal teams, external partners, and AI agents. A contract that changes
without notice breaks consumers. A contract that is inconsistent across
teams creates integration friction.

## Non-negotiable invariants

1. APIs are versioned at the URL level. Version changes are never silent.
   `/v1/` and `/v2/` may coexist; `/v2/` does not replace `/v1/` without
   a deprecation period defined in an ADR.

2. Error responses follow the RFC 9457 Problem Details format. Any client
   that handles errors from one API can handle errors from any API.

3. Resource names are nouns, not verbs. Actions are HTTP methods.
   `/payments` not `/getPayments`, `/payments/{id}/cancel` not `/cancelPayment`.

4. Pagination is cursor-based for unbounded collections. Offset pagination
   is prohibited for collections that may grow without bound.
```

---

## A naming convention ADR (`adrs/api/001-url-structure.md`)

```markdown
# ADR-001: URL Structure Convention

**Status:** Accepted
**Date:** 2024-03-10

## Decision

URL paths follow the pattern:
`/api/v{N}/{resource}/{id?}/{sub-resource?}`

- Version prefix is mandatory
- Resource names are lowercase plural nouns
- IDs are path parameters, not query parameters
- Sub-resources are used for relationships, not actions

## Rationale

Version prefix in the URL ensures that clients pin to a specific contract.
Header-based versioning was considered and rejected — it is invisible in
browser address bars, logs, and gateway routing rules.

Plural nouns follow REST conventions established by Fielding (2000) and
adopted by the majority of public APIs. Singular nouns create inconsistency
at collection endpoints.

## Alternatives rejected

**Header versioning (`Accept: application/vnd.api+json;version=2`):**
Rejected. Invisible to proxies and harder to test manually. Increases
cognitive load for API consumers.

**Action-based URLs (`/payments/process`):**
Rejected. Verbs in URLs signal RPC thinking, not resource thinking.
The action is the HTTP method.

## Consequences

URL structure is validated at CI time via OpenAPI spec linting.
Any route that does not match the pattern blocks merge.
```

---

## A Gherkin check (`features/api/url-structure.feature`)

```gherkin
@api @enforced
Feature: REST API URL structure

  Background:
    Given the API URL structure rules from ADR-001

  @enforced
  Scenario: Valid resource URL is accepted
    Given the API path "/api/v1/payments"
    When I validate the URL structure
    Then it should be valid

  @enforced
  Scenario: Valid resource with ID is accepted
    Given the API path "/api/v1/payments/pay-123"
    When I validate the URL structure
    Then it should be valid

  @enforced
  Scenario: Missing version prefix is rejected
    Given the API path "/payments/pay-123"
    When I validate the URL structure
    Then it should be invalid
    And the reason should mention "version prefix required"

  @enforced
  Scenario: Verb in URL path is rejected
    Given the API path "/api/v1/processPayment"
    When I validate the URL structure
    Then it should be invalid
    And the reason should mention "resource name must be a noun"

  @enforced
  Scenario: Error response follows RFC 9457 format
    Given an error response with fields "type", "title", "status", "detail"
    When I validate the error response format
    Then it should be valid

  @wip
  Scenario: Offset pagination is rejected for unbounded collections
    Given a collection endpoint without a cursor parameter
    And the collection is marked as unbounded
    When I validate the pagination contract
    Then it should be invalid
```

---

## The `governance.yml` for this domain

```yaml
project:
  name: "Payments Platform Governance"
  description: "API governance for the payments platform"

api:
  versioning:
    strategy: url_prefix        # url_prefix | header | query_param
    current_version: 1
    deprecated_versions: []
    deprecation_notice_days: 90

  url:
    resource_name_pattern: "^[a-z][a-z-]+$"   # lowercase-kebab plural nouns
    id_format: "[a-z]{2,6}-[a-z0-9]+"         # type-prefixed IDs

  error_response:
    format: rfc9457
    required_fields: [type, title, status, detail]
    optional_fields: [instance, errors]

  pagination:
    default_strategy: cursor
    prohibited_for_unbounded: offset
    max_page_size: 100
```

---

## What the AI agent experience looks like

An engineer asks their agent to scaffold a new endpoint for cancelling a payment.

Without governance, the agent might generate:

```
POST /api/v1/cancelPayment
```

With governance, the agent first queries:

```
get_constitution("api")
→ "Resource names are nouns, not verbs. Actions are HTTP methods..."

get_adr("001")
→ "Action-based URLs (/payments/process): Rejected."

validate_url_path("/api/v1/cancelPayment")
→ {valid: false, reason: "resource name must be a noun; use /payments/{id}/cancel"}
```

The agent generates:

```
POST /api/v1/payments/{id}/cancel
```

The governance check catches the conceptual error before the code exists — not in a review cycle after the route is implemented, tested, and deployed.

---

## What teams get from this

- New engineers learn conventions by seeing agent output, not by reading documentation that may be stale
- Code review focuses on logic, not style violations
- A new API team can query `list_constitutions()` and `list_adrs()` to discover the full standard before writing a single line of code
- CI blocks any spec that violates the `@enforced` scenarios — there is no path to production for a malformed URL

---

→ [Back to README](../README.md) | [Kafka platform example](kafka-platform.md)
