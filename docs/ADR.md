# Architecture Decision Records

Short records of reversible architecture decisions: what was decided, why, and when to revisit.
Binding, non-reversible principles live in `specs/000-constitution.md`, not here.

**How to use this file**
- Add a new entry at the bottom with the next number. Keep each entry short (about 10 lines).
- Never delete an entry. To reverse a decision, set its status to `Superseded by ADR-NNN` and add a new entry.
- Update the index table when adding or changing an entry.

## Index

| ID | Decision | Status | Date |
|---|---|---|---|
| ADR-001 | Tailwind without Node | Accepted | 2026-10-26 |
| ADR-002 | Payments behind an interface; Fake provider until feature 007 | Accepted | 2026-10-26 |
| ADR-003 | Hosting on Render | Accepted | 2026-10-26 |
| ADR-004 | Database on Neon | Accepted | 2026-10-26 |

---

## ADR-001 – Tailwind without Node

**Status:** Accepted · **Date:** 2026-10-26

- **Context:** The UI is HTMX + server-rendered Django templates. No JavaScript app, no bundler needed. Node would be used only to run Tailwind.
- **Decision:** Use the Tailwind standalone CLI. CSS building is isolated behind one command (`scripts/build_css`) that Dockerfile, CI and README call. A separate Dockerfile stage builds assets; only the built CSS reaches the final image. The built CSS is not committed to Git.
- **Pros:** One runtime (Python) in production; faster, simpler builds; fewer dependencies.
- **Cons:** npm-only Tailwind plugins are not directly available (some official plugins are bundled in the CLI – verify per plugin). The CLI binary is OS-specific.
- **Revisit when:** a needed plugin or component library is npm-only; more than two JS libraries must be bundled; a UI part that HTMX cannot handle reasonably.
- **How to switch:** change `scripts/build_css` to run through npm; replace the assets stage in the Dockerfile with a Node image (final image stays Node-free); supersede this entry.

---

## ADR-002 – Payments behind an interface; Fake provider until feature 007

**Status:** Accepted · **Date:** 2026-10-26

- **Context:** Stripe is not available to Israeli merchants (as of October 2026). The business model needs both subscriptions and pay-per-use, and the real provider is not yet chosen.
- **Decision:** All payment logic goes through a `PaymentProvider` interface in `apps/billing/providers/`. A single credit ledger serves both models: a subscription grants a monthly credit quota, a pay-per-use purchase adds credits. Until feature 007, only `FakePaymentProvider` is used, including in the cloud demo.
- **Pros:** Billing can be built and demoed without a merchant account; the provider choice does not block development; switching providers touches one class.
- **Cons:** No real money flow in the demo. Provider-specific features (refunds, invoices, webhooks) are designed generically and may need adjustment.
- **Revisit when:** starting feature 007, or if Stripe launches for Israeli merchants.
- **Options for 007:** an Israeli payment gateway (e.g. Cardcom, Tranzila, PayPlus, Grow) or a merchant-of-record service (e.g. Paddle). Verify support for an Israeli business, subscription support, webhook support and fees before choosing. Record the choice as a new ADR.

---

## ADR-003 – Hosting on Render

**Status:** Accepted · **Date:** 2026-10-26

- **Context:** The demo needs a public URL from task 001, auto-deploy from Git, HTTPS, and ideally no cost.
- **Decision:** A Render Web Service, built from the project's Dockerfile, auto-deployed from `main`. Free tier during the demo phase. Render's own Postgres is not used (the free database expires after 30 days – see ADR-004).
- **Pros:** Git-based deploys with no CLI and no credit card; built-in HTTPS; Docker-based, so the same image runs locally and in the cloud.
- **Cons:** The free web service sleeps after 15 minutes of inactivity and takes about a minute to wake up. Monthly cap of 750 free instance hours. No free background worker.
- **Revisit when:** real users arrive (upgrade to a paid always-on instance, from about $7/month), or report generation needs a separate background worker.
- **How to switch:** the app is fully defined by the Dockerfile and environment variables, so moving to another container host is a platform configuration change with no code change. Supersede this entry.

---

## ADR-004 – Database on Neon

**Status:** Accepted · **Date:** 2026-10-26

- **Context:** The constitution requires Postgres in every environment. The cloud database must be managed, reliable and free for the demo, and must not expire.
- **Decision:** Managed Postgres on Neon in the cloud; Postgres in Docker locally. The app connects only through `DATABASE_URL`.
- **Pros:** A permanent free plan (not a trial). Compute scales to zero after 5 minutes of inactivity and resumes on the next query.
- **Cons:** Free plan limits of 100 compute-hours and 0.5 GB storage per project. Compute is suspended when the monthly limit is reached. A short delay on the first query after idle.
- **Revisit when:** usage approaches the free limits, or real users require stronger backup and retention guarantees.
- **How to switch:** any managed Postgres works by changing `DATABASE_URL` and migrating the data (dump and restore). No code change. Supersede this entry.
