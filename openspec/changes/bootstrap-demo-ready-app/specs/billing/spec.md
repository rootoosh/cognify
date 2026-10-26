## Purpose

Define initial Stripe credit billing and application-layer quota enforcement.

## ADDED Requirements

### Requirement: Credit entitlement
The system SHALL expose a credits-based pay-per-use entitlement through the
PaymentProvider port; the initial runtime adapter SHALL use Stripe.

#### Scenario: Insufficient credits
- **WHEN** an educator requests analysis without sufficient entitlement
- **THEN** the application denies enqueue and does not consume a credit

#### Scenario: Credit available
- **WHEN** an educator requests one eligible analysis with sufficient credit
- **THEN** the application reserves or consumes entitlement exactly once

### Requirement: Checkout and webhook safety
The system SHALL verify Stripe webhook signatures before processing events and
SHALL process repeated event identifiers idempotently.

#### Scenario: Replayed webhook
- **WHEN** the same valid Stripe event is delivered more than once
- **THEN** the entitlement is applied once and the event is recorded idempotently

### Requirement: Quotas belong to application layer
The application layer SHALL enforce quotas independently of Django views, and
payment operations SHALL be idempotent across retries and job failures.

#### Scenario: Failed analysis
- **WHEN** an accepted analysis reaches terminal failure
- **THEN** the documented compensation policy restores or preserves credit exactly once

### Requirement: Deferred subscription option
The entitlement model SHOULD permit a future per-student subscription path, but
the initial demo MUST NOT implement a second payment provider or subscription flow.