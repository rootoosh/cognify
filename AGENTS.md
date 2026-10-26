# AI Contributor Instructions

## Mandatory Workflow

- Read `openspec/constitution.md`, `openspec/config.yaml`, and the active change
  under `openspec/changes/` before proposing or editing behavior.
- Every behavior or architecture change starts with an OpenSpec proposal/spec
  delta and tasks. Do not implement until the human approves those artifacts.
- Implement one ordered task at a time. Run the narrowest relevant test or
  quality gate immediately after each edit; do not archive a change before all
  tasks and acceptance criteria pass.
- The bootstrap change is currently a Phase A proposal. Phase A contains only
  specifications and documentation. Stop before application code until the
  human explicitly approves Phase B.
- Do not deploy to a cloud environment or configure automatic production deploys
  without explicit human approval.

## Architecture

- Keep dependencies inward: domain -> application/use cases -> ports -> adapters.
  Django views and HTMX templates are outer delivery code and contain no business
  rules. Use import-linter to enforce boundaries.
- Keep Django, Stripe, cloud SDKs, and concrete job runners out of domain and
  application code. Use the documented ports for PaymentProvider,
  AnalysisAgent, JobRunner, StudentRepository, ReportRepository, Anonymizer,
  and UIComposition.
- Plan one runtime adapter per port. The only permitted placeholder is the one
  deterministic AnalysisAgent implementation. Test doubles are test-only.
- Version data contracts crossing ports. Validate input and ownership at every
  boundary; make retries and external events idempotent.

## Product and Privacy Constraints

- Do not invent exam questions, questionnaire content, scoring, diagnostic
  mapping, or real report-generation logic. The demo agent must be deterministic
  and visibly non-diagnostic.
- Treat student information as sensitive and assume students may be minors.
  Minimize stored data and pseudonymize direct identifiers before any analysis
  port invocation. Do not claim legal compliance without jurisdictional review.
- Enforce user ownership for students, test data, jobs, reports, and billing.
  Preserve CSRF, secure headers/cookies, webhook signature verification, and
  secret isolation.
- The UI is Hebrew-first and RTL. Preserve accessible empty/loading/error states.

## Cloud and Operations

- Keep business logic cloud-neutral. SQLite is for local/demo; PostgreSQL is the
  production database target through Django migrations.
- Use Docker with separate web and worker processes. Never rely on container
  local storage for production persistence.
- Keep secrets external and production settings fail-closed. No production
  deployment is implied by a successful Docker build or CI run.
- Keep JobRunner behind its port. Do not add Redis, Celery, Kubernetes, or a
  second runner without an approved requirement and spec delta.
- Follow the deployment, migration, backup/restore, logging, health, upgrade,
  and rollback requirements in the active OpenSpec change.

## Editing and Verification

- Preserve existing user changes; inspect Git status before editing and never
  revert unrelated work.
- Prefer small changes and existing project conventions. Do not add unrelated
  refactors or dependencies.
- Run focused tests first, then required CI gates for the approved task. Report
  exactly what passed, failed, or could not be run; never imply unrun checks
  passed.
- Keep README and agent instructions consistent with the actual implementation
  state. Do not document planned features as if they already work.