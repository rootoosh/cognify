# Roadmap

The ordered list of features. Each feature becomes a folder `specs/NNN-name/` with its own spec, plan and tasks.

**How to use this file**
- The next feature to work on is the first one whose status is not `Done`.
- Update the status when a feature's spec is started (`In progress`) and when it is merged (`Done`).
- The order or scope changes only by the project owner's decision. Record the change in the history table.
- Each line here is a one-sentence goal, not a spec. Details belong in the feature's `spec.md`.

## Features

| ID | Feature | Goal | Depends on | Status |
|---|---|---|---|---|
| 001 | Walking skeleton | A Hebrew RTL home page live in the cloud, with CI, auto-deploy and monitoring. | – | In progress |
| 002 | Accounts and base layout | Sign-up, login, logout and a shared RTL layout for logged-in pages. | 001 | Not started |
| 003 | Exam data input | Create students (pseudonymous in storage for AI) and enter exam results via a form and CSV upload, with validation; define who may create a student and the consent required, and review the pseudonymization design against Israeli privacy law. | 002 | Not started |
| 004 | Report generation (Fake AI) | Create a report job, show its status with HTMX polling, and display a structured report produced by the Fake AI provider. | 003 | Not started |
| 005 | Real AI provider | Connect a real AI provider behind the existing interface, with versioned prompts and schema validation. | 004 | Not started |
| 006 | Credits and billing (Fake payments) | Append-only credit ledger, subscription quota and pay-per-use purchase, using the Fake payment provider; a credit is reserved when a report starts and committed only on success. | 004 | Not started |
| 007 | Real payment provider | Connect the payment provider chosen in ADR-002 behind the existing interface, with idempotent webhook handling. | 006 | Not started |
| 009 | Data retention and deletion | Let users delete a student and their account with all related data, and enforce a defined retention period for reports and AI inputs. | 003 | Not started |
| 008 | Hardening | Rate limiting, database backups, full privacy review against Israeli law, and accessibility pass. | 007, 009 | Not started |

## Change history

| Date | Change |
|---|---|
| 2026-10-26 | First version |
| 2026-10-26 | Added 009 (data retention and deletion), placed before 008. 003 now includes consent rules and an early privacy review of pseudonymization. 006/007 goals specify the ledger and webhook rules. |
