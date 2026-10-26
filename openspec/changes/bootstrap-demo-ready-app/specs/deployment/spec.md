## Purpose

Define reproducible packaging and a provider-neutral path from local demo to
production deployment.

## ADDED Requirements

### Requirement: Reproducible container packaging
The project SHALL provide a Dockerfile and `.dockerignore` and SHALL document
how to build and run the same application image reproducibly.

#### Scenario: Build application image
- **WHEN** CI builds the image from the repository and locked dependencies
- **THEN** the build succeeds without embedding secrets or relying on local
  developer-only files

### Requirement: Separate web and worker processes
The image SHALL support explicit web and worker startup commands so both
processes can run separately while sharing the same application release.

#### Scenario: Start processes independently
- **WHEN** an operator starts web and worker commands against the same configured
  environment and database
- **THEN** each process starts its role without requiring another runtime adapter

### Requirement: Environment and secrets separation
The system SHALL load secrets, database connection details, allowed hosts, and
environment-specific settings externally. Production SHALL use `DEBUG=False`,
secure cookies, HTTPS-related settings, CSRF protection, secure headers, and
fail-closed validation of required configuration.

#### Scenario: Missing production secret
- **WHEN** production starts without a required secret or valid database setting
- **THEN** the application refuses to start and does not fall back to insecure defaults

### Requirement: SQLite and PostgreSQL
The system SHALL support SQLite for local/demo use and PostgreSQL for production
through configuration and migrations, without changing business logic.

#### Scenario: Apply migrations
- **WHEN** documented migrations are applied to SQLite or PostgreSQL
- **THEN** both databases reach the supported schema version

### Requirement: Persistent production data
Production SHALL NOT rely on container-local filesystem persistence for database
or media data. Static assets SHALL be collected and served using a documented
production approach.

#### Scenario: Container replacement
- **WHEN** a production container is replaced
- **THEN** application records and approved persistent assets remain available

### Requirement: Health and structured logs
The system SHALL expose non-sensitive liveness/readiness checks and emit
structured logs with correlation identifiers while excluding secrets and
student payloads.

#### Scenario: Health check
- **WHEN** an operator probes application health
- **THEN** the response indicates service readiness without exposing protected data

### Requirement: CI deployability gates
GitHub Actions SHALL run tests, linting, type checking, import boundary checks,
security scanning, dependency auditing, production configuration checks,
migration checks, and Docker image build before a release is considered deployable.

#### Scenario: Quality gate fails
- **WHEN** any required gate fails
- **THEN** the build is not marked deployable

### Requirement: Explicit production deployment
The project SHALL document provider-neutral prerequisites and deployment order,
including configuration, database provisioning, migrations, static assets,
health checks, backup/restore, upgrades, and rollback. CI MUST NOT deploy to
production without explicit configuration and human approval.