## Purpose

Define secure educator accounts and ownership boundaries.

## ADDED Requirements

### Requirement: Account access
The system SHALL allow an educator to register, log in, log out, and manage
account details using authenticated, CSRF-protected web flows.

#### Scenario: Successful login
- **WHEN** a registered educator submits valid credentials
- **THEN** the system authenticates that educator and establishes a secure session

#### Scenario: Invalid credentials
- **WHEN** an unauthenticated visitor submits invalid credentials
- **THEN** the system denies access without revealing whether the account exists

### Requirement: User-owned resources
The system SHALL authorize access to students, test data, jobs, reports, and
entitlements by authenticated owner, not by an identifier supplied by the client.

#### Scenario: Cross-user access denied
- **GIVEN** a resource belongs to another educator
- **WHEN** the current educator requests or mutates that resource
- **THEN** the system denies the request without exposing its contents

### Requirement: Session and request security
Production SHALL use secure cookies, CSRF protection, secure headers, configured
HTTPS behavior, and rate limits for sensitive authentication actions.

#### Scenario: Production configuration invalid
- **WHEN** a required security setting is missing or invalid in production
- **THEN** startup or deployment checks fail closed