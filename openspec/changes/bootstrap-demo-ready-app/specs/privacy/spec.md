## Purpose

Protect potentially minor student data through minimization, anonymization,
retention, and clear launch constraints.

## ADDED Requirements

### Requirement: Minimize and pseudonymize
The system SHALL minimize stored student data and SHALL pseudonymize direct
identifiers before every AnalysisAgent invocation.

#### Scenario: Direct identifier present in storage
- **WHEN** an analysis request is prepared
- **THEN** the port payload contains a pseudonymous reference and no direct
  student identifier

### Requirement: Retention and deletion
The system SHALL document retention periods and SHALL provide a deletion path
consistent with approved retention, billing, and audit obligations.

#### Scenario: Retention period expires
- **WHEN** a record reaches its approved retention limit
- **THEN** it is deleted or irreversibly anonymized according to the policy

### Requirement: Privacy policy and disclosure
Before collecting student data, the project SHALL document and present a privacy
policy/notice that identifies data use, retention periods, deletion/export,
consent assumptions, and any external processing. Production launch SHALL be
blocked until jurisdiction-specific review approves the policy.

#### Scenario: No external analysis configured
- **WHEN** the demo placeholder is used
- **THEN** the UI and privacy notice do not claim that an external AI model
  analyzed the data

### Requirement: Jurisdictional launch review
Production launch SHALL be blocked until applicable jurisdiction, consent,
minors, educational-record, and potentially regulated health-data requirements
are reviewed and documented.