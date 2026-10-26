# Cognify

Cognify is planned as a paid, Hebrew-first application for educators to manage
student test data and request assessment reports. The real questionnaire,
analysis agent, and report-generation logic have not been supplied and are not
being invented here.

## Project Status

This repository is in the Phase A specification and review stage. The active
OpenSpec change defines the product contract and an ordered Phase B plan. There
is no runnable web application, Docker image, worker, or cloud deployment yet.
Do not treat planned capabilities as implemented, and do not begin application
implementation until the Phase A artifacts are explicitly approved.

## Product and Architecture

The planned stack is Python, Django, HTMX, and Tailwind CSS. The architecture
keeps dependencies inward: domain -> application/use cases -> ports -> adapters,
with Django views and HTMX templates at the web boundary. Replaceable services
are defined by ports, including payments, analysis, background jobs, student and
report persistence, anonymization, and UI composition.

The demo analysis adapter will be deterministic and visibly non-diagnostic.
Initial billing is planned around Stripe credits. SQLite is the local/demo
database; PostgreSQL is the production target. Docker will support separate web
and worker processes without coupling business logic to a cloud provider.

## OpenSpec Workflow

Read [`AGENTS.md`](AGENTS.md) and the [engineering constitution](openspec/constitution.md)
before contributing. The active proposal is
[`bootstrap-demo-ready-app`](openspec/changes/bootstrap-demo-ready-app/proposal.md),
with its [design](openspec/changes/bootstrap-demo-ready-app/design.md),
[implementation tasks](openspec/changes/bootstrap-demo-ready-app/tasks.md), and
capability specifications under its `specs/` directory.

Every change follows proposal -> spec delta -> tasks -> human approval ->
implementation -> verification -> archive. The bootstrap proposal remains
unarchived until approved implementation and verification are complete.

When the OpenSpec CLI is available, validate the proposal before approval with:

```powershell
openspec validate bootstrap-demo-ready-app --strict
```

## Cloud Readiness

Cloud deployment is not required for the demo. The specifications require a
reproducible Docker build, externalized configuration and secrets, PostgreSQL
migrations, persistent production data, health checks, structured logs, CI
quality gates, and documented deployment, backup/restore, upgrade, and rollback
procedures. No production deployment should occur without explicit approval.

See the [deployment specification](openspec/changes/bootstrap-demo-ready-app/specs/deployment/spec.md)
and [operations specification](openspec/changes/bootstrap-demo-ready-app/specs/operations/spec.md).
