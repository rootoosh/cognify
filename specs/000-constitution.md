# 000 – Project Constitution

> This document defines the binding principles of the project.
> Every spec, plan and line of code must comply with them.
> The constitution changes only by an explicit decision of the project owner, recorded in the "Change history" section at the end.

**Version:** 1.1
**Date:** 2026-10-26

---

## Principle 1 – Production from day one

- The system **must** run in the cloud from the very first task, and remain deployable at any moment.
- The development environment **must** be as close as possible to production: the same Dockerfile, the same database (Postgres), the same way of loading configuration.
- Every commit to `main` is deployed automatically.

**Why:** so we never discover at the end of the project that it only works inside the IDE.

---

## Principle 2 – Layer separation

- **Views** – input and output only.
- **Services** – all business logic.
- **Models** – data only.
- **Providers** – every external dependency (AI, payments, email, storage) is accessed only through an interface, and every interface has a `Fake` implementation.

**Why:** replacing a provider, changing the UI or adding a future API will not require rewriting the business logic.

---

## Principle 3 – Spec-driven development (SDD)

- No code without an approved `spec.md`.
- Fixed order: spec → plan → tasks → implementation.
- The documents are the source of truth. When code and document disagree, one of them is corrected explicitly; no gap is left behind.
- Every task is small enough to be tested and approved on its own.

---

## Principle 4 – Tests are part of the task

- A task is not "done" without passing tests.
- CI blocks merging when tests, lint or `check --deploy` fail.
- No real network calls in tests.

---

## Principle 5 – Human in the loop

- Every file change, state-changing command and commit happens only after the project owner's approval.
- Cloud actions, `push` and irreversible operations happen only on explicit request.

---

## Principle 6 – Privacy and security

- Student data is sensitive information, and sometimes belongs to minors.
- **Only** pseudonymous data is sent to an AI provider: no name, ID number, address or any other identifying detail.
- Every user sees only their own data (a permission check in every service that accesses student data).
- Secrets live only in environment variables. No secret ever enters Git.
- HTTPS only in production; `DEBUG=False`; secure cookies.
- Compliance with Israeli privacy law must be reviewed before opening the system to real users.

---

## Principle 7 – Reliability of AI output

- AI output is received in a defined structure (JSON) and passes schema validation before it is displayed.
- Every report stores: the prompt version, the input used (in its pseudonymous form), and the output.
- Prompts are stored in versioned files, not as strings scattered across the code.
- An AI provider failure does not bring the system down: the report is marked "failed" and no credit is deducted.

---

## Principle 8 – Hebrew user experience

- The entire UI is in Hebrew, RTL, with basic accessibility (contrast, field labels, keyboard navigation).
- Mobile-friendly.
- Error messages are written for the user, not in technical terms.

---

## Principle 9 – Simplicity and readability

- Prefer the simple solution that works. Additional complexity only when the need is proven.
- A new dependency only with approval and justification.
- Code is written so a new developer can understand it from the README and the specs alone.

---

## Principle 10 – Reliable services, free tiers preferred

- Prefer managed, well-known services with a free tier.
- Every external service is documented in the README: what it does, which environment variables it requires, and the limits of its free tier.

---

## Change history

| Version | Date | Change |
|---|---|---|
| 1.0 | 2026-10-26 | First version (Hebrew) |
| 1.1 | 2026-10-26 | Translated to English; no change in content |
