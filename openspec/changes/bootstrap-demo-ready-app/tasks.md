## Phase B Implementation Tasks

Phase A only defines this sequence. Do not start these tasks until the human
approves this proposal and its spec deltas. Each numbered step is a separate
OpenSpec change with its own approved delta, focused tests, gates, and archive.
Finish and verify a step before beginning the next.

- [ ] 1. Project foundation: initialize Django/Python package with uv; add
  dev/prod settings, environment validation, Dockerfile and `.dockerignore`,
  GitHub Actions CI, import-linter contracts, SQLite/PostgreSQL configuration,
  migration baseline, root README, `AGENTS.md`, Copilot instructions, and
  engineering/layer READMEs. Test reproducible install, Django checks, import
  rules, migration check, and Docker build.
- [ ] 2. Identity and account security: registration, login, logout, account
  management, CSRF/security settings, rate limits, and user-owned access. Test
  authentication flows and cross-user authorization.
- [ ] 3. Students and data entry: student lifecycle, generic versioned test-data
  envelope, manual entry, and validated transactional CSV import with row-level
  errors and limits. Test validation, atomicity, ownership, and malformed/large
  input behavior.
- [ ] 4. Privacy and AnalysisAgent: versioned port schemas, single deterministic
  non-diagnostic placeholder, anonymizer, minimization and retention behavior.
  Contract-test the adapter; prove identifiers are removed before agent calls
  and identical requests produce stable placeholder results.
- [ ] 5. Background processing and reports: choose and integrate one maintained
  DB-backed runner behind JobRunner; implement durable states, bounded retries,
  restart safety, report history and idempotency. Test duplicate delivery,
  worker restart, terminal failure, and no duplicate reports/quota effects.
- [ ] 6. Billing and quotas: Stripe checkout, signature-verified/idempotent
  webhooks, credit entitlements and application-layer quota enforcement. Test
  replayed events, failed/cancelled analysis compensation, and no duplicate
  charge or credit consumption. Keep per-student subscriptions deferred.
- [ ] 7. Hebrew RTL UI: accessible Django/HTMX templates, Tailwind logical
  properties, licensed self-hosted Hebrew font, keyboard/screen-reader behavior,
  and clear empty/loading/error states through UIComposition. Verify WCAG 2.2 AA
  target against automated and manual checks.
- [ ] 8. Deployability and operations: complete Docker/local instructions,
  PostgreSQL migration and production-check evidence, static serving, structured
  logs, health endpoints, backup/restore and upgrade/rollback runbooks, and
  privacy policy/retention documentation, and integrated demo verification.
  Verify web and worker separately, required settings fail closed, secrets are
  absent from Git, and CI gates pass. Do not deploy to production without
  explicit configuration and human approval.

## Completion Gate

- [ ] Every Phase B change has approved specs, focused unit/integration/port
  contract tests, passing required quality gates, and a completed verification
  record before archive.
- [ ] Documented Docker build/start works; web and worker run as separate
  processes; migrations apply on SQLite and PostgreSQL; production settings,
  health checks, logging, and secret externalization are verified.
- [ ] CI passes tests, Ruff, mypy, import-linter, Bandit, pip-audit, Django
  production checks, migration checks, and image build.
- [ ] Background processing is safe across retries/restarts and duplicate work
  cannot create duplicate reports, charges, or quota consumption.
- [ ] Deployment procedure names required services, configuration, migration
  order, static assets, backups/restores, health checks, rollout, rollback, and
  operator actions. Cloud deployment itself is not required for readiness.
- [ ] Privacy policy, retention/deletion rules, and applicable jurisdictional
  review are complete before collecting real student data in production.