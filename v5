# Codebase Snapshot — Agent Instructions

## Objective
Perform a comprehensive, read-only audit of this codebase and produce one factual snapshot of its current state — detailed enough that another engineer could understand its purpose, architecture, data, configuration, behavior, and health without exploring the code themselves. Depth matters as much as coverage: cite exact field names, variable names, and file paths, not generic descriptions.

## Hard Rules
- **Read-only against the project.** Only run commands that inspect state (`cat`, `grep`, directory listings, `git log`, `git diff --stat`, dependency version checks, etc.). Never edit, delete, install, migrate, or run anything with side effects on the codebase. The one exception is writing the final report.
- **No guessing.** Every non-trivial claim needs a file path, line number, or command output behind it.
- **Missing evidence → `UNKNOWN`.** Do not infer or fill gaps with assumptions. If something is ambiguous or contradictory, say so and mark it `UNKNOWN`.
- **Redact secrets, keep names.** Never output a live secret value. Preserve the variable name and its location, e.g. `STRIPE_SECRET_KEY=<REDACTED>` (`.env.example:12`).
- **Describe, don't editorialize.** Report what is, not what should be — opinions belong in the Risks section only.

## Investigation

### Step 1 — Stack & Framework Detection
Determine, from manifests and lockfiles (not just imports):
- Language(s) and versions
- Framework(s) and versions
- Runtime and version constraints
- Package manager(s) and lockfile(s) present
- Build tools / bundlers

Identify the **primary framework**, if one exists, and state your evidence (e.g., "Django 4.2 — `requirements.txt`, confirmed via `INSTALLED_APPS` in `settings.py`"). If no framework dominates, say so explicitly and treat the project as general-purpose. If this is a monorepo, detect and report each subproject's stack separately, and apply the rules below to each.

### Step 2 — Framework-Specific Coverage
**If Django:** settings (all environments/modules), installed apps, models, migrations (state vs. applied), URL configuration, views (CBV/FBV), templates, admin customizations, signals, middleware stack, context processors, custom management commands, fixtures, forms, DRF serializers/viewsets/routers (if present), Celery/background task config, cache backend, auth/permission classes, database config (engine, replicas), and every environment variable consumed by settings.

**If any other framework, or none dominates:** entrypoints, routing, state management, data models/schema, middleware/hooks/interceptors, configuration system, dependency graph, rendering strategy, API layer, background/async jobs, and deployment mechanism.

This feeds the Framework-Specific Appendix in the output — don't treat it as redundant with Step 3.

### Step 3 — Full Project Capture
Investigate and evidence all of the following:
- Project purpose and who it's for
- Domain concepts and primary user flows
- Repository structure — top-level and key nested directories
- Responsibility of each major directory/module, based on contents, not the directory name
- Key files (entrypoint, core config, primary business logic) and why each matters
- Entrypoints — how the app starts, how requests/events/jobs enter the system
- Critical end-to-end flows, traced through real code
- Business rules encoded in code: validation logic, state machines, permission checks
- Authorization/permission model and where it's enforced
- Data models, fields, relationships, constraints
- Migrations, seed data, fixtures
- Configuration and every environment variable: name, purpose, where consumed, default if any
- Dependencies and dev tooling
- Tests present (types, frameworks) and CI configuration
- Deployment and operations: how/where deployed, containerization, IaC, health checks, logging/monitoring
- Risks, gaps, dead code, TODO/FIXME markers, security concerns, performance concerns
- Small conventions that affect maintainability (naming patterns, folder conventions, enforced style)

If a category doesn't apply to this project, state that explicitly — don't omit it silently.

## Output Structure
Produce one report — save it as `PROJECT_SNAPSHOT.md` in the repo root — in this order:

1. **Metadata** — repo name, primary language/framework, snapshot date, commit hash/branch
2. **Executive Summary** — 5–10 sentences: what this is, its current state, and overall health (actively maintained vs. stagnant, test/CI signal, headline risks)
3. **Stack & Framework** — Step 1 results, with evidence
4. **Repository Map** — directory tree with per-directory responsibility, plus key files
5. **Domain & User Flows** — what the system does, for whom, and the main flows, grounded in code (models/views/routes), not assumed business context
6. **Critical Flows (Traced)** — the 3–6 most important flows (e.g. signup, auth, checkout, core feature action). For each: entry point, step-by-step trace through actual files/functions, where validation/permissions/side effects occur, with file:line citations
7. **Data Model** — table: `Model | Fields | Relationships | Constraints | Source`
8. **Configuration & Environment Variables** — table: `Variable | Purpose | Consumed In | Default | Source`
9. **Dependencies & Tooling** — table of direct dependencies: `Package | Version | Purpose | Source`. Note transitive dependency count and lockfile used — don't enumerate every transitive package
10. **Deployment & Operations** — deploy mechanism, containerization, IaC, health checks, logging/monitoring
11. **Testing & CI** — test types/frameworks present, coverage signal if tooling exists (else `UNKNOWN`), CI gates and deploy triggers
12. **Risks, Gaps & Dead Code** — grouped as Security / Performance / Dead Code & TODOs / Other Gaps, each with evidence
13. **Unknowns** — every `UNKNOWN` used in this report, consolidated, with what was checked and why it couldn't be determined
14. **Framework-Specific Appendix** — full Step 2 checklist for the detected framework. Include a routes/endpoints table where applicable: `Method | Path | Handler | Auth Required | Source`
15. **Quick-Start Commands** — table: `Task | Command | Source`, pulled from actual scripts (`package.json`, `Makefile`, `manage.py`, `docker-compose.yml`, CI config), never invented

## Verification (required before delivering)
Check every box below. Fix anything that fails and re-check — do not deliver a report that fails any of these:
- [ ] Every major claim carries a file path, line number, or command output
- [ ] Framework detection is stated with evidence, not asserted
- [ ] Every config/environment variable is documented or explicitly `UNKNOWN`
- [ ] Data models and relationships are listed individually, not summarized as "several models exist"
- [ ] At least 3 critical flows are traced through real code with citations
- [ ] Risks/gaps/dead code/TODOs section is populated, or explicitly states none were found
- [ ] Unknowns are consolidated in their own section, not silently dropped
- [ ] Secrets are redacted everywhere; variable names are preserved
