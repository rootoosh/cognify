## Purpose

Define a Hebrew-first, accessible, replaceable web presentation layer.

## ADDED Requirements

### Requirement: Hebrew RTL interface
The user interface SHALL be Hebrew-first and use RTL document direction with
logical layout properties so content remains correct across responsive sizes.

#### Scenario: Render primary workflow
- **WHEN** an authenticated educator opens the application
- **THEN** navigation, forms, errors, and report status render in Hebrew and RTL

### Requirement: Accessible workflow states
The interface SHALL target WCAG 2.2 AA and provide clear empty, loading,
validation-error, service-error, and success states with keyboard operation and
programmatic labels.

#### Scenario: Analysis is pending
- **WHEN** an analysis job is queued or running
- **THEN** the status is announced accessibly and the page does not imply a
  completed diagnosis

### Requirement: UI composition port
The system SHALL isolate UI composition behind UIComposition so replacing HTMX
presentation does not require changing application use cases.

#### Scenario: Alternate presentation adapter
- **WHEN** a future approved UI adapter invokes the same use cases
- **THEN** domain rules and application behavior remain unchanged

### Requirement: Self-hosted Hebrew font
The production UI SHALL use a self-hosted Hebrew-capable font with compatible
licensing and SHALL not depend on a third-party font CDN.