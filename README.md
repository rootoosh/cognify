# Cognify

A web system that generates a **strengths and insights report for a student** using AI, based on exam data entered by the user.

- **Users:** teachers, counselors and parents (later: institutions).
- **Business model:** credits. A subscription grants a monthly report quota; pay-per-use buys individual credits.
- **Language:** Hebrew-only UI, right-to-left (RTL).

**Status:** early development. The current goal is a working demo in the cloud. See [docs/ROADMAP.md](docs/ROADMAP.md) for progress.

## Documentation

| File | What it contains |
|---|---|
| [specs/000-constitution.md](specs/000-constitution.md) | Binding project principles. Everything else must comply with them. |
| [CLAUDE.md](CLAUDE.md) | How we work: stack, commands, structure, conventions, security and testing rules. |
| [docs/ROADMAP.md](docs/ROADMAP.md) | Ordered list of features and their status. |
| [docs/ADR.md](docs/ADR.md) | Architecture decision records. |
| `specs/NNN-feature-name/` | Spec, plan and tasks for each feature. |

## How we work

The project follows spec-driven development:

1. The next feature is the first one in the roadmap not marked `Done`.
2. Each feature gets a `spec.md` (what and why), then a `plan.md` (how), then a `tasks.md` (small tasks). Each stage is approved before the next.
3. Tasks are done one at a time, each with passing tests and its own commit.
4. Branch flow: `feature/NNN-short-name` → `develop` → `main`. Every commit to `main` deploys automatically.

## Stack

Python 3.12+, Django 5.2 LTS, HTMX, Tailwind CSS (standalone CLI), PostgreSQL, uv, ruff, pytest. Hosted on Render with the database on Neon, CI on GitHub Actions, error monitoring with Sentry.

## Getting started

Setup and run instructions will be added in feature 001. The planned commands are listed in [CLAUDE.md](CLAUDE.md#3-commands).

## External services

Each service, its environment variables and its free-tier limits will be documented here as it is added (constitution, principle 10).

## License

See [LICENSE](LICENSE).
