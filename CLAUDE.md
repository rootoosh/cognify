# CLAUDE.md – Project Working Guidelines

> This file is read at the start of every session. It defines **how** we work on this project.
> The binding principles (**what** is non-negotiable) live in `specs/000-constitution.md`.
> In case of conflict, the constitution wins.

---

## 1. About the project

A web system that generates a **strengths and insights report for a student** using AI, based on exam data entered by the user.

- **Users:** teachers, counselors, parents (later: institutions).
- **Business model:** credits. A subscription grants a monthly report quota; pay-per-use buys individual credits.
- **Language and direction:** Hebrew-only UI, RTL.
- **Current phase goal:** a working demo in the cloud, with readable, documented code.

---

## 2. Stack

| Area | Choice |
|---|---|
| Language | Python 3.12+ |
| Framework | Django 5.2 LTS (5.1+ is required for `LoginRequiredMiddleware`) |
| Interactivity | HTMX (no SPA, no React) |
| Styling | Tailwind CSS (standalone CLI, no Node – see ADR-001 in `docs/ADR.md`) |
| Database | PostgreSQL – locally in Docker, in the cloud on Neon (see ADR-004) |
| Package management | uv |
| Lint and format | ruff |
| Testing | pytest + pytest-django |
| Authentication | django-allauth |
| Configuration | django-environ (environment variables only) |
| Static files | WhiteNoise |
| Hosting | Render (Web Service, see ADR-003) |
| Payments | Behind a `PaymentProvider` interface; Fake provider until feature 007 (see ADR-002) |
| CI | GitHub Actions |
| Error monitoring | Sentry (free tier) |

All ADRs are in `docs/ADR.md`. Do not add a new dependency without asking and justifying it (see section 5).

---

## 3. Commands

> To be finalized during task 001. Until then, these are the planned commands.

```bash
# Run the local environment (app + Postgres)
docker compose up

# Install dependencies
uv sync

# Build CSS (the only entry point for asset building – see ADR-001)
scripts/build_css

# Migrations
uv run python manage.py migrate

# Tests
uv run pytest

# Lint and format
uv run ruff check .
uv run ruff format .

# Production readiness check
uv run python manage.py check --deploy

# Demo data
uv run python manage.py seed_demo
```

Before every commit request, run `ruff check`, `ruff format --check` and `pytest`, and show the results.

**Windows:** `scripts/build_css` is a shell script and does not run in PowerShell. Run it through Docker (`docker compose run --rm web scripts/build_css`) or Git Bash.

---

## 4. Repository structure

```
CLAUDE.md
README.md
docs/
  ROADMAP.md     ← ordered list of features and their status
  ADR.md         ← all architecture decision records, one short entry each
specs/
  000-constitution.md
  NNN-feature-name/
    spec.md      ← what and why (no code)
    plan.md      ← how (technical decisions)
    tasks.md     ← list of small tasks with status
scripts/         ← build and utility scripts (e.g. build_css)
config/          ← settings, urls, wsgi
apps/
  core/          ← base template, home page, health check
  accounts/
  exams/
  reports/
  billing/
templates/
static/
tests/           ← or tests/ inside each app
```

### Layers inside each app

```
apps/<name>/
  models.py      ← data only, no business logic
  services.py    ← all business logic; pure functions where possible
  views.py       ← receive request → call service → render template; no logic
  forms.py
  providers/     ← interfaces to external services + implementations (including Fake)
  templates/<name>/
  tests/
```

**Golden rule:** a view never accesses an external provider directly and contains no calculations. Only services do.

---

## 5. Workflow (SDD)

0. **The next feature comes from `docs/ROADMAP.md`** – the first one not marked `Done`. Update its status when starting and when merged. Do not change the order or scope without the user's decision.
1. **No code without a spec.** Every feature starts with `specs/NNN-name/spec.md`.
2. **spec → plan → tasks.** Each stage is approved by the user before moving to the next.
3. **One task at a time** from `tasks.md`. Do not start a new task before the previous one is marked done.
4. **Test before code** where possible: write a failing test, then implement.
5. **Mark a task done** (`- [x]`) only after tests pass and the user has approved.
6. **Requirements change mid-task:** stop, update `spec.md`/`plan.md` first, then continue.
7. **At the end of each feature:** update `README.md` if anything changed that affects installation, running or usage.

### Proposing a new dependency
Before adding a package, state: the package name, why it is needed, the alternative without it, and its maintenance status. Wait for approval.

### Architecture decisions
Reversible decisions that affect more than one feature (hosting, database provider, build tooling, payment provider) are recorded in `docs/ADR.md`. Read it before changing anything in these areas, and follow the instructions at the top of that file when adding or reversing a decision.

---

## 6. Approval rules – mandatory

- **Before creating, editing or deleting any file** – show what will change and wait for approval.
- **Before any state-changing command** (install, migration, deletion, docker) – explain in one sentence what it does and wait for approval.
- **Before every commit** – show: the list of files, test and lint results, and the proposed commit message. Wait for approval.
- **Never** run `push`, `push --force`, `reset --hard`, delete a branch, or perform a cloud action (Render/Neon) without an explicit request.
- **Never** write secrets (API keys, passwords, connection strings) to a file tracked by Git. Only to `.env` (which is in `.gitignore`) and to `.env.example` with dummy values.

---

## 7. Code conventions

- Code, variable names, code comments and project documentation – **English**. Text shown to the end user – **Hebrew**.
- Type hints on every public function.
- A short docstring on every public service: what it does, what it returns, which exceptions it raises.
- A function does one thing. Above ~40 lines – consider splitting.
- No `print` – use `logging` only.
- No magic values – constants at the top of the module or in settings.
- All user-facing text goes through templates, never built in Python.
- Efficient by default, without premature optimization: avoid N+1 queries (`select_related` / `prefetch_related`), paginate every list, index fields used in filters, and never call an external provider inside a loop when one batched call will do.

### HTMX
- A response to an HTMX request returns a **partial** (`templates/<app>/partials/...`), not a full page.
- Detect HTMX requests via the `HX-Request` header.
- Long-running operations (report generation) are not done inside the request. Create a job and show its status with polling.

### Credits and payments
- The credit ledger is append-only: entries are never edited or deleted. A correction is a new entry.
- A credit is **reserved** when a report job starts and **committed** when it succeeds. On failure the reservation is released, so a report never charges twice and a failure never loses a credit.
- Ledger changes happen inside a database transaction.
- Payment provider events (webhooks) are idempotent: the same event received twice has no extra effect (store the provider's event ID and ignore repeats).

### RTL and Tailwind
- `<html lang="he" dir="rtl">` in the base template.
- Logical classes only: `ms-`/`me-`/`ps-`/`pe-`/`start-`/`end-`, `text-start`/`text-end`. **Never** `ml-`/`mr-`/`left-`/`right-`.
- Numbers, dates and code inside Hebrew text – wrap in `<bdi>` or `dir="ltr"` when needed.
- The built CSS file is generated, never committed to Git.

---

## 8. Security

Standard secure access for a web application holding sensitive data. No new dependencies are needed for these rules; anything that would require one is deferred to feature 008 and proposed separately.

### Authentication (django-allauth)
- Email verification is mandatory in production.
- Django's password validators are enabled (minimum length, common passwords, similarity, numeric-only).
- allauth's built-in rate limits are enabled for login, sign-up and password reset.
- Login and password-reset responses never reveal whether an email is registered.

### Sessions and cookies
- `SESSION_COOKIE_SECURE`, `CSRF_COOKIE_SECURE`, `HttpOnly`, `SameSite=Lax`.
- Session lifetime is configured from settings, not left open-ended.
- Changing a password invalidates the user's other sessions.

### Transport
- In production: `SECURE_SSL_REDIRECT`, `SECURE_PROXY_SSL_HEADER` for the hosting proxy, and HSTS (start with a short `max-age`, increase after it is verified).

### Authorization
- Every view requires login by default (`LoginRequiredMiddleware`). Public views are marked explicitly.
- Ownership is checked in the service layer for every object a user reads or changes. Another user's object returns **404**, not 403, so its existence is not revealed.
- The Django admin is served at a non-default path taken from an environment variable, and is available to staff only.

### Requests and input
- CSRF protection on every state-changing request, including HTMX (the token is sent in a header from the base template).
- All input goes through Django forms or explicit validation. No raw SQL built with string formatting.
- File uploads (CSV): enforced size limit, type check, parsed in memory, never executed or served back as-is.
- Template auto-escaping is never disabled for user-supplied data (no `|safe` on user input).

### Student data and the AI provider
- Data sent to the AI is built by one service function from an explicit allow-list of fields. Nothing else is sent.
- Send derived values, not raw details: age instead of birth date, a grade level instead of a school name.
- Free-text fields (e.g. teacher notes) may contain names. They are cleaned before sending: the student's known name and other known identifiers are replaced with placeholders. Tests cover this.
- The mapping between a student and their pseudonym stays in our database only.
- Exam and CSV content is placed in the prompt as data only, clearly delimited, and never treated as instructions (prompt injection). The output schema validation (constitution, principle 7) is the second line of defense.
- The AI provider must be configured so it does not retain data or train on it. Verify this when choosing the provider in feature 005.
- Prefer an EU region for the database and hosting; record the final choice in `docs/ADR.md`.
- Israeli privacy law (including Amendment 13) and the rules on transferring data abroad apply. The pseudonymization design is reviewed against them in feature 003; the full review is in feature 008.

### Response headers
- `X_FRAME_OPTIONS = "DENY"`, `SECURE_CONTENT_TYPE_NOSNIFF`, a strict `Referrer-Policy`.
- A Content Security Policy is added in feature 008.

### Logging and monitoring
- Never log passwords, tokens, session IDs or identifying student data.
- Sentry runs with `send_default_pii=False`, plus a `before_send` hook that removes request bodies, form data and cookies from events. `send_default_pii=False` alone does not cover these.

### Dependencies
- Dependency security alerts are enabled on the repository; security updates are applied promptly and in their own commit.

---

## 9. Testing

- Every service – unit tests.
- Every view – at least one test asserting status 200 and the correct template, plus a permission test where relevant.
- Every view or service that touches user data – a test that an anonymous user is redirected to login and that another user's object returns 404.
- External providers (AI, payments) – **always** through a Fake implementation in tests. No network calls in tests.
- Tests run against Postgres (as in production), not SQLite. In CI, Postgres runs as a GitHub Actions service container.

---

## 10. Git

- Branch flow: `feature/NNN-short-name` → `develop` → `main`.
  - Each feature branch starts from `develop` and is merged back into `develop` when CI passes and the feature is approved.
  - `main` is production: every commit to `main` deploys automatically (constitution, principle 1). Merging `develop` into `main` is a release and happens only on the project owner's explicit request.
  - Never commit directly to `main`.
- One branch per feature.
- Commit messages in Conventional Commits format, in English:
  `feat(reports): add report job status polling`
  `fix(exams): validate score range`
  `docs: update README setup steps`
  `test(billing): cover credit deduction`
- Small commits – one commit per task in `tasks.md`.

---

## 11. Don'ts

- Do not build a feature that is not in an approved spec.
- Do not "improve" code unrelated to the current task – propose it separately.
- Do not use SQLite.
- Do not send a student's name, ID number or any identifying detail to an AI provider.
- Do not disable a test to make CI pass.
- Do not weaken a security setting (section 8) to make something work – stop and ask.
- Do not invent command output – run the command and show the real result.
