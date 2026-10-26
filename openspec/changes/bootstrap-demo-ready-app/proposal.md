## Why

Cognify needs an agreed product and engineering contract before implementation.
The app handles sensitive student data and paid analysis requests, so its
authorization, privacy, idempotency, and deployment boundaries must be explicit
before code exists. The current repository is only a README and license.

## What Changes

- Define the end-to-end educator workflow: accounts, students, generic test-data
  entry/import, analysis request, report history, and credit entitlements.
- Define ports and versioned contracts for replaceable infrastructure, including
  one deterministic, non-diagnostic AnalysisAgent placeholder.
- Specify a layered Django/HTMX architecture, Hebrew RTL experience, privacy,
  security, testing, packaging, and a provider-neutral production path.
- Provide an ordered, gated Phase B implementation plan with deployment-readiness
  acceptance criteria.

## Out of Scope

- Real exam questionnaire, scoring, diagnostic mapping, or report-generation
  logic; real external AI calls; a second implementation of any port.
- Production cloud deployment, Kubernetes, a second payment provider, or a
  second background runner.
- Per-student subscriptions in the first demo. Keep their entitlement extension
  documented; the initial billing path is credits.

## Capabilities

- `auth`: registration, login, account management, authentication and ownership.
- `students`: per-user student record lifecycle.
- `data-entry`: generic manual test-data input and validated CSV import.
- `analysis-agent`: versioned port contract and deterministic placeholder.
- `reports`: asynchronous requests, report storage and per-user history.
- `billing`: Stripe credits, quotas, checkout, webhook entitlements and idempotency.
- `privacy`: minimization, anonymization, retention and deletion constraints.
- `ui-rtl`: Hebrew RTL accessible HTMX interface and UIComposition boundary.
- `deployment`: Docker, environment separation, database and CI readiness.
- `operations`: runbooks for startup, deployment, data, health, logs and recovery.

## Impact

- Adds normative OpenSpec deltas and project-level architecture/security policy.
- No runtime code or dependencies are introduced by this Phase A change.
- Phase B will add Django application code, database migrations, Docker and CI
  files, documented operational procedures, and a locally runnable demo.
- PostgreSQL and cloud deployment are readiness targets; no cloud provider is
  selected and no deployment occurs as part of this proposal.

## Approval Gate

Phase A is complete when the constitution, all capability deltas, architecture,
ordered Phase B tasks, and open decisions are reviewed. Keep this change active
and unarchived. Do not implement application code until the human approves the
Phase A artifacts.