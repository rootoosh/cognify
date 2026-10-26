## Purpose

Define educator-owned student record lifecycle without collecting unnecessary
personal information.

## ADDED Requirements

### Requirement: Student lifecycle
The system SHALL let an authenticated educator create, view, edit, and delete
only their own student records.

#### Scenario: Create student
- **WHEN** an educator submits valid, bounded student fields
- **THEN** the student is stored under that educator's ownership

#### Scenario: Another educator's student
- **WHEN** an educator requests a student owned by another account
- **THEN** the system denies access without disclosing student data

### Requirement: Data minimization
The system SHALL collect only student attributes required by the approved
workflow and SHALL support deletion according to the documented retention policy.

#### Scenario: Delete student
- **WHEN** an authorized owner deletes a student
- **THEN** associated data follows the approved retention/deletion rules and is no
  longer available through normal application views