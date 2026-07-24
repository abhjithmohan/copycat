You are helping a developer turn a vague request into a precise agent instruction.
User goal:
Create a prompt that instructs an agent to comprehensively snapshot the current state of a software project, including tiny details, framework usage, architecture, data, configuration, behavior, and project health.
When generating the improved instruction:
1. Make the goal explicit
   - The agent must create a complete, factual snapshot of the project as it exists now.
   - The snapshot must be comprehensive enough for another engineer to understand the project without manually exploring everything.
2. Force safe, evidence-based analysis
   - Instruct the agent to work read-only.
   - Instruct it not to guess.
   - Require UNKNOWN labels when evidence is missing.
   - Require file paths or command output as evidence for important claims.
   - Require secret values to be redacted while preserving variable names.
3. Require stack and framework detection
   - Detect languages, frameworks, runtimes, package managers, lockfiles, and versions.
   - Identify the primary framework if one exists.
   - Adapt the snapshot to the detected framework.
4. Add framework-specific adaptation rules
   - If Django is detected, require coverage of:
     settings, apps, models, migrations, URLs, views, templates, admin, signals,
     middleware, context processors, management commands, fixtures, forms,
     DRF/APIs, Celery/background tasks, cache, auth/permissions, database config,
     and environment variables.
   - If another framework is detected, require equivalent coverage of:
     entrypoints, routing, state, data models, middleware/hooks, config,
     dependencies, rendering, API layer, background jobs, and deployment.
5. Require full project capture
   The instruction must tell the agent to capture:
   - project purpose
   - domain and user flows
   - repository structure
   - directory/module responsibilities
   - key files
   - entrypoints
   - critical end-to-end flows
   - business rules
   - permissions and validation
   - data models and relationships
   - migrations/seeds/fixtures
   - configuration and environment variables
   - dependencies and tooling
   - tests and CI
   - deployment and operations
   - risks, gaps, dead code, TODOs, security issues, and performance concerns
   - small conventions that affect maintainability
6. Require useful output structure
   - Produce a structured report.
   - Include metadata, executive summary, stack, repo map, domain, flows,
     data model, config, dependencies, deployment, testing, risks, unknowns,
     framework-specific appendix, and quick-start commands.
   - Prefer tables for models, dependencies, environment variables, routes,
     and commands.
7. Keep the instruction direct and efficient
   - Avoid fluffy wording.
   - Avoid unnecessary abstractions.
   - Use imperative language.
   - Make the prompt usable as-is.
8. Add a verification step
   - Before finishing, the agent must verify that every major claim has evidence.
   - It must confirm that framework detection, configuration, data models,
     critical flows, risks, and unknowns are all covered.
Output requirement:
Return a single improved prompt in a code block, ready to paste into an agent.
