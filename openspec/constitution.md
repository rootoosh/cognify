# Cognify Engineering Constitution

This constitution governs all application changes. OpenSpec proposals and their
specifications are the source of truth. No application code may be written
before its approved spec delta and tasks exist.

## Architecture and Dependencies

- Keep business rules in a framework-independent domain and application layer.
- Use cases depend on domain rules and documented ports; ports define contracts,
  not infrastructure details.
- Adapters implement ports. Django views and HTMX templates are delivery
  adapters and must not contain business rules.
- Dependencies point inward. Domain imports no application, port implementation,
  Django, Stripe, or cloud code. Application/use cases import no adapters, web,
  Django, or provider SDKs. Enforce package boundaries with import-linter in CI.
- Everything replaceable is behind a port: PaymentProvider, AnalysisAgent,
  JobRunner, StudentRepository, ReportRepository, Anonymizer, and UIComposition.
- Plan one runtime implementation per port. The only permitted placeholder is a
  single deterministic AnalysisAgent implementation. Test doubles belong only
  in tests; Stripe test mode is permitted for provider integration tests.
- UIComposition owns UI composition behind a port so HTMX may later be replaced
  without changing use cases.

## Contracts and Validation

- Version every schema crossing a port. Validate and size-limit data at every
  trust boundary; reject unsupported schema versions explicitly.
- Make port contracts precise about identity, ownership, errors, idempotency,
  retries, and persistence where applicable.
- Keep student/test-data input generic until a separately approved questionnaire
  and agent contract exist. Do not invent real questions, diagnostic rules, or
  report content.

## Security and Privacy

- Require authentication and object-level authorization; users can access only
  their own students, test data, reports, jobs, and entitlements.
- Preserve CSRF protection, secure headers, request validation, rate limits, and
  Stripe webhook signature verification and idempotency.
- Keep secrets out of Git and source; load credentials and deployment settings
  through environment variables or a cloud secret manager. Production settings
  fail closed when required values are missing or invalid.
- Assume students may be minors. Minimize retained data, pseudonymize identifiers
  before any AnalysisAgent call, document retention/deletion, and avoid sending
  direct identifiers to an external model.
- Applicable jurisdiction, consent, educational-record, and health-data duties
  require review before launch. Do not claim legal compliance without it.

## Persistence and Cloud Portability

- Use SQLite for local development/demo and PostgreSQL in production without
  changing domain or application behavior. Schema changes use Django migrations.
- Never rely on container-local files for persistent production data. Persistent
  media requires a separately specified storage port.
- Package reproducibly with Docker. Run web and worker as separate processes
  from the same image, with explicit startup commands and external configuration.
- Keep deployment provider-neutral. Do not add Kubernetes, Redis, Celery, or
  provider-specific runtime coupling without a concrete approved requirement.
- Document migrations, static files, backups/restores, upgrades/rollbacks,
  health checks, logs, and operational limits.

## Background Work and Idempotency

- Keep execution behind JobRunner. Persist task state and define retry,
  terminal-failure, restart, visibility, and duplicate-delivery behavior.
- Assume at-least-once execution. Use durable idempotency keys and database
  constraints/transactions so retries cannot create duplicate reports, charges,
  or quota consumption.
- Select a minimal maintained Django-compatible database-backed runner only
  after verifying reliability requirements. Document demo limitations and the
  exact production replacement boundary; do not ship a second runner now.

## Testing and Quality Gates

- Every implementation step includes unit, integration, and relevant adapter
  contract tests. Test authorization, input validation, failures, privacy,
  retries, idempotency, and migration behavior at the appropriate boundaries.
- Require at least 90% total statement coverage and 95% coverage for domain,
  application, and port-contract code. Do not exclude security-sensitive code
  merely to meet a threshold.
- Before a build is deployable, CI must pass tests, Ruff lint/format checks,
  mypy, import-linter, Bandit, pip-audit, Django production checks, migration
  checks, and Docker image build.
- Never deploy automatically to production. Production deployment requires
  explicit configuration and human approval.

## Readability and Documentation

- Keep modules small, names explicit, and public contracts documented with
  docstrings and versioned schemas.
- Maintain a root README and per-layer guidance, including how to extend ports
  and how to replace a provider without changing use cases.
- Prefer maintained libraries and a minimal runtime dependency set. Record
  technology decisions, assumptions, limitations, and security consequences in
  OpenSpec design artifacts.

## Change Lifecycle

- Every change follows OpenSpec proposal, spec delta, tasks, implementation,
  focused verification, and archive. Do not skip approval or verification.
- Phase A for the initial bootstrap ends with specifications and a Phase B plan;
  it contains no application code. Stop for human approval before Phase B.
- Implement Phase B in ordered, small changes. Each change must pass its stated
  acceptance criteria before the next change starts or the current change is
  archived.