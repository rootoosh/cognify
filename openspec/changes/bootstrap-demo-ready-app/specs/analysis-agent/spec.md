## Purpose

Define a replaceable, versioned AnalysisAgent port and its only initial,
deterministic placeholder adapter.

## ADDED Requirements

### Requirement: Versioned analysis contract
The AnalysisAgent port SHALL accept a versioned validated generic data envelope,
a pseudonymous subject reference, and an idempotency key; it SHALL return a
versioned typed result or stable typed error.

#### Scenario: Unsupported contract version
- **WHEN** a caller submits an unsupported AnalysisAgent contract version
- **THEN** the adapter rejects it explicitly and does not return a report

### Requirement: Deterministic non-diagnostic placeholder
The initial implementation SHALL be a single deterministic placeholder. It
MUST be visibly labeled demo-only and non-diagnostic, and MUST NOT invent
learning difficulties, questionnaire content, or real report logic.

#### Scenario: Same request retried
- **WHEN** an identical valid request with the same idempotency key is processed
  more than once
- **THEN** it returns the same placeholder result without creating duplicate
  report effects

### Requirement: No direct identifiers to analysis
The application SHALL pseudonymize direct student identifiers before invoking
the AnalysisAgent port and SHALL send only data required by the versioned contract.

#### Scenario: Agent invocation
- **GIVEN** stored student details and valid test data
- **WHEN** the application invokes AnalysisAgent
- **THEN** the request contains no direct student identifiers