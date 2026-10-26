## Purpose

Define asynchronous analysis requests and user-scoped report history.

## ADDED Requirements

### Requirement: Background analysis request
The system SHALL enqueue an authorized analysis request through the JobRunner
port and expose a user-scoped status for queued, running, succeeded, or failed work.

#### Scenario: Request accepted
- **WHEN** an educator with valid data and quota requests analysis
- **THEN** the application records one idempotent request and returns its status

#### Scenario: Retry or duplicate delivery
- **WHEN** a job is retried or delivered more than once
- **THEN** the system does not create duplicate reports or repeat quota effects

### Requirement: Persistent report history
The system SHALL persist completed reports through ReportRepository and show
history only to the owning educator, associated with the relevant student.

#### Scenario: Owner views report history
- **WHEN** an educator requests history for an owned student
- **THEN** the system returns that student's reports in a stable chronological order

#### Scenario: Non-owner views report
- **WHEN** a different educator requests a report identifier
- **THEN** the system denies access without revealing its content

### Requirement: Failure and restart behavior
The JobRunner adapter SHALL persist task state, use bounded retries where
appropriate, and recover or expose work after process restart without losing
terminal failure details.

#### Scenario: Worker restarts during execution
- **WHEN** the worker stops while a task is running
- **THEN** persisted state permits safe recovery or a visible terminal failure
  without duplicate report or entitlement effects