## Purpose

Define operator guidance and service behavior required for supportable local and
production execution.

## ADDED Requirements

### Requirement: Operational documentation
The project SHALL document local setup, environment variables and secrets,
Docker build/run, web/worker startup, database initialization/migrations,
production prerequisites, static assets, backup/restore, upgrades/rollback,
logging, health checks, and troubleshooting.

#### Scenario: New operator runs locally
- **WHEN** an operator follows the local setup instructions from a clean checkout
- **THEN** they can configure and start the documented web and worker processes

#### Scenario: Production deployment prepared
- **WHEN** an operator follows the deployment runbook
- **THEN** it identifies required services, configuration, migration sequence,
  health checks, backup/restore, rollout, and rollback actions

### Requirement: Database recovery
The project SHALL document PostgreSQL backup and restore procedures and SHALL
identify retention, encryption, and restore-verification responsibilities.

#### Scenario: Restore exercise
- **WHEN** an operator follows the restore procedure in a test environment
- **THEN** the procedure identifies how to verify schema and application data

### Requirement: Deployment portability
The runtime documentation SHALL remain cloud-provider-neutral until a provider
is explicitly selected and SHALL document provider-specific assumptions when added.

#### Scenario: Provider not selected
- **WHEN** deployment guidance is reviewed before provider selection
- **THEN** it states container, database, secret, static asset, and worker
  requirements without presuming a specific cloud platform

### Requirement: AI development instructions
The repository SHALL provide a root-level AI/developer instruction file and a
Copilot-specific instruction file that point to the OpenSpec constitution and
active change. They SHALL require spec-first changes, approval before
application implementation, focused verification, preservation of user changes,
and honest reporting of unavailable tools or unverified checks.

#### Scenario: Agent receives an implementation request
- **WHEN** an AI agent is asked to modify application behavior
- **THEN** it checks the current OpenSpec change, updates or creates the approved
  spec delta and tasks first, and does not cross an explicit approval gate

### Requirement: README reflects project state
The root README SHALL summarize Cognify, link to its constitution and active
OpenSpec change, describe the current phase accurately, and distinguish planned
capabilities from implemented behavior.

#### Scenario: User opens README during Phase A
- **WHEN** a contributor reads the root README before Phase B approval
- **THEN** they can find the proposal, architecture and AI instructions, and are
  not told that an application or deployment is already runnable