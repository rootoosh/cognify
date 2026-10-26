## Purpose

Define generic test-data entry and safe CSV import without inventing an exam.

## ADDED Requirements

### Requirement: Versioned generic test data
The system SHALL accept a versioned `TestDataEnvelope` with an opaque schema
identifier and bounded validated values; it SHALL NOT prescribe questionnaire
items, scoring, or diagnostic interpretation in this phase.

#### Scenario: Unsupported version
- **WHEN** a client submits an unsupported envelope version
- **THEN** the system rejects it with a stable validation error and stores no data

#### Scenario: Oversized or invalid data
- **WHEN** the envelope exceeds configured limits or violates its declared schema
- **THEN** the system rejects it before persistence or job enqueue

### Requirement: Manual data entry
The system SHALL let an authenticated educator create and edit generic test data
only for a student they own.

#### Scenario: Data entered for owned student
- **WHEN** valid test data is submitted for an owned student
- **THEN** the system stores it with its schema identifier and version

### Requirement: CSV import
The system SHALL validate CSV structure, size, encoding, columns, and every row;
it SHALL return actionable row-level errors and SHALL not partially commit a
failed transactional import.

#### Scenario: Invalid CSV row
- **WHEN** an import contains an invalid row
- **THEN** the response identifies the row and validation issue and the import
  follows the documented all-or-nothing transaction policy

#### Scenario: Cross-owner row reference
- **WHEN** an imported row references a student not owned by the uploader
- **THEN** the system rejects that row without exposing another user's data