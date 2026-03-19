---
name: deep-audit
description: Comprehensive multi-agent code audit that fans out specialized agents across security, testing, patterns, consistency, error handling, and more. Each agent writes structured findings; a final pass aggregates into a prioritized report. Reads per-project .claude/audit.md for tailored context.
---

# Deep Audit

Performs a thorough, multi-pass code audit of the current project by fanning out specialized agents in parallel, each focused on a specific concern. Produces a structured, prioritized report.

## Phase 1: Orientation

Before spawning any agents, orient yourself:

1. **Read `.claude/audit.md`** if it exists - this contains project-specific audit config (tech stack, known patterns to skip, high-risk areas, custom checks). If it doesn't exist, tell the user they can create one from the template at `${CLAUDE_PLUGIN_ROOT}/skills/deep-audit/references/audit-template.md` for better results.

2. **Read `CLAUDE.md`** if it exists - understand project conventions.

3. **Map the project structure** - quick scan of top-level directories, identify:
   - Primary language(s) and frameworks
   - Number of services/packages/modules
   - Test directories and their coverage
   - Config files (CI, linting, deployment)

4. **Determine scope** - if the user specified a scope (e.g., a single service), respect it. Otherwise, audit everything.

5. **Create the output directory** using today's date:

   ```bash
   mkdir -p outputs/audit/YYYY-MM-DD_audit
   ```

   Example: `outputs/audit/2026-03-17_audit/`. This allows repeat audits over time without overwriting previous results.

6. **Announce the plan briefly** - show the user which agents you'll spawn and what each will focus on, then proceed immediately. Do NOT ask for confirmation - this is a fire-and-forget skill designed to run unattended.

## Phase 2: Agent Dispatch

Spawn agents in parallel using the Agent tool. Each agent gets:

- A specific concern (lens)
- A specific scope (files/directories to examine)
- The project context from audit.md/CLAUDE.md
- A structured output format to follow

### Audit Lenses

Select from these lenses based on what's relevant to the project. Skip lenses that don't apply (e.g., no "API contracts" for a CLI tool). The audit.md file may add project-specific lenses or remove irrelevant ones.

#### 1. Security

- Injection vulnerabilities (SQL, command, template)
- Authentication and authorization gaps
- Secrets in code or config (hardcoded keys, tokens, passwords)
- Insecure deserialization
- Missing input validation at system boundaries
- Dependency vulnerabilities (known CVEs)
- File path traversal
- SSRF, open redirects
- For web apps: XSS, CSRF, cookie security

#### 2. Error Handling & Resilience

- Bare except clauses / swallowed exceptions
- Missing error handling on I/O operations (network, file, database)
- Inconsistent error patterns across the codebase
- Silent failures (operations that fail without logging or raising)
- Missing retry logic for transient failures
- Resource cleanup (unclosed connections, file handles)
- Graceful degradation patterns (or lack thereof)

#### 3. Database & Data Integrity

- SQL injection via raw queries or string formatting
- Missing or incorrect migrations
- Schema inconsistencies (code models vs actual schema)
- N+1 query patterns
- Missing indexes on frequently queried columns
- Transaction boundaries (operations that should be atomic but aren't)
- Connection pool configuration
- Data validation before persistence

#### 4. Test Coverage & Quality

- Untested public functions/methods
- Missing edge case coverage
- Test quality (are tests actually asserting meaningful things?)
- Missing integration tests for cross-component interactions
- Property-based testing opportunities (roundtrip, idempotency, invariants)
- Test isolation issues (tests that depend on execution order or shared state)
- Mocking vs real dependencies (appropriate use)
- Missing error path tests

**Coverage estimation methodology:** Count test files vs source files per service/module and report both numbers. Estimate coverage as the ratio of source files that have at least one corresponding test file (not line coverage - that requires runtime instrumentation). Always produce a per-service table in `test-coverage.md` with columns: Service, Test Files, Source Files, Files-With-Tests Ratio, Critical Untested Paths. This table format must be consistent across runs so results are comparable.

#### 5. Code Patterns & Consistency

- Naming convention violations
- Mixed patterns for the same concern (e.g., some modules use factory pattern, others don't)
- Dead code (unused imports, unreachable branches, commented-out code)
- Copy-paste duplication that should be abstracted
- Inconsistent abstraction levels
- Magic numbers and strings
- Type annotation coverage and correctness

#### 6. API Contracts & Interfaces

- Missing request/response validation
- Undocumented endpoints or parameters
- Inconsistent error response formats
- Missing or incorrect HTTP status codes
- Breaking changes without versioning
- Rate limiting and abuse prevention
- Serialization/deserialization correctness

#### 7. Concurrency & Performance

- Race conditions in async code
- Blocking calls in async contexts
- Missing connection pooling
- Unbounded queues or workers
- Memory leaks (growing collections, unclosed resources)
- Unnecessary allocations in hot paths
- Missing caching where beneficial
- N+1 patterns in data fetching

#### 8. Configuration & Deployment

- Environment-specific code without proper config
- Missing .env.example entries
- Secrets management (encryption at rest, rotation)
- Deployment script safety (rollback capability, health checks)
- Dependency pinning and lock files
- Missing or outdated CI/CD checks

#### 9. Cross-Service Consistency (for multi-service projects)

- Shared code duplication vs proper packaging
- Inter-service contract mismatches
- Inconsistent logging/observability patterns
- Dependency version drift between services
- Shared database schema coupling issues
- Missing service health checks

### Scoping Agents

For larger projects, split work to keep each agent's context manageable:

- **Small project** (<5K lines): One agent per lens, whole project scope
- **Medium project** (5K-20K lines): One agent per lens, whole project scope, but prioritize high-risk areas
- **Large project** (20K+ lines): Split by lens AND by service/module. Group related smaller modules together.

For multi-service projects, consider:

- Cross-cutting lenses (consistency, architecture) - one agent, whole project, sampling key files
- Per-service lenses (security, tests, error handling) - one agent per service or per service-group

### Agent Prompt Template

Each agent should receive a prompt structured like this:

```
You are auditing [PROJECT] for [CONCERN].

## Project Context
[Insert relevant context from audit.md and CLAUDE.md]

## Scope
Examine these files/directories: [SPECIFIC PATHS]

## What to Look For
[Specific checklist for this lens - from the lists above, plus any project-specific checks from audit.md]

## Known Patterns (Don't Flag)
[From audit.md - intentional patterns that might look like issues]

## Output Format
For each finding, write:

### [ID] [SEVERITY: critical/high/medium/low]  [Short title]
- **File:** path/to/file.py:line_number
- **Category:** [security|error-handling|database|testing|patterns|api|concurrency|config|consistency]
- **Description:** What the issue is and why it matters
- **Suggestion:** How to fix it (brief, actionable)
- **Confidence:** [certain|likely|possible] - how confident you are this is a real issue

**Finding IDs** use the format `CAT-NNN` where CAT is a 3-letter category prefix:
- `SEC` - Security
- `ERR` - Error Handling & Resilience
- `DAT` - Database & Data Integrity
- `TST` - Test Coverage & Quality
- `PAT` - Code Patterns & Consistency
- `API` - API Contracts & Interfaces
- `CON` - Concurrency & Performance
- `CFG` - Configuration & Deployment
- `XSV` - Cross-Service Consistency
- For custom lenses from audit.md, use a 3-letter prefix derived from the lens name

Number findings sequentially within each category (SEC-001, SEC-002, etc.). These IDs must be stable and used consistently across the report.md and all lens files so findings can be cross-referenced between runs.

Sort findings by severity (critical first), then by confidence.

Do NOT flag:
- Style preferences that don't affect correctness
- Patterns documented as intentional in the project context
- Hypothetical issues with no concrete path to exploitation
- Things that are clearly in-progress (TODO/FIXME with context)
```

### Agent Configuration

- Use `subagent_type: "Explore"` for agents - they need read access but should NOT modify code
- Set thoroughness to "very thorough" in the agent prompt
- **Prefer depth over speed.** This skill is designed to run unattended - the user has walked away. Thoroughness matters more than finishing quickly. Each agent should read every relevant file in its scope, not sample. If the scope is too large for one agent, split into multiple agents rather than skimming.
- For each finding, agents should verify file:line references by reading the actual code, not guessing from memory
- Agents should note areas they didn't have time to review so the aggregation phase can flag coverage gaps

## Phase 3: Aggregation

After all agents complete, perform the aggregation yourself (don't spawn another agent - you have all the findings in context now):

1. **Read all agent results**

2. **Deduplicate** - same issue found by multiple lenses (e.g., security agent and database agent both flag a raw SQL query)

3. **Cross-reference** - findings that reinforce each other (e.g., missing tests for a function with a security finding = higher priority)

4. **Prioritize** using this matrix:
   - **Critical**: Security vulnerabilities exploitable now, data loss risks, production crashes. Reserve this for issues that could cause harm _today_ without further code changes. Test coverage gaps are High, not Critical, unless the untested code has a known active bug. Pattern inconsistencies are never Critical.
   - **High**: Bugs likely to manifest, significant test gaps in critical paths, architectural issues
   - **Medium**: Code quality issues that increase maintenance burden, moderate test gaps
   - **Low**: Style inconsistencies, minor improvements, nice-to-haves

5. **Renumber finding IDs sequentially** during aggregation. Agents may produce non-sequential IDs - renumber them as SEC-001, SEC-002, SEC-003... (no gaps) in the final report and lens files. This keeps IDs clean and predictable.

6. **Write the report** to `outputs/audit/YYYY-MM-DD_audit/report.md`:

```markdown
# Deep Audit Report - [Project Name]

**Date:** YYYY-MM-DD
**Scope:** [what was audited]
**Lenses:** [which audit lenses were used]

## Executive Summary

[2-3 sentences: overall health, critical finding count, top concern]

## Critical Findings

[If any - these need immediate attention]

## Statistics

| Severity  | Count |
| --------- | ----- |
| Critical  | N     |
| High      | N     |
| Medium    | N     |
| Low       | N     |
| **Total** | **N** |

## Findings by Category

### Security

[Findings sorted by severity]

### Error Handling

[...]

### Database

[...]

[etc.]

## Patterns Observed

### Systemic Issues

[Cross-cutting observations - recurring problems, not individual findings]

### Positive Patterns

[What the codebase does well - builds trust and identifies strengths to preserve]

## Recommendations

[Top 5-10 actionable items, prioritized]

## Areas Not Reviewed

[Honest about what wasn't covered and why]
```

6. **Write one lens file per audit lens** to `outputs/audit/YYYY-MM-DD_audit/<lens>.md`. Use these exact filenames for the standard lenses:
   - `security.md`
   - `error-handling.md`
   - `database.md`
   - `test-coverage.md`
   - `patterns.md`
   - `api-contracts.md` (if applicable)
   - `concurrency.md`
   - `config-deployment.md`
   - `cross-service.md` (if multi-service)
   - One file per custom lens from audit.md

   Every lens that had agents dispatched MUST have a corresponding file, even if it found nothing (write "No findings" in that case). This ensures the user can verify which lenses actually ran.

   Each lens file must use the same `CAT-NNN` finding IDs as the main report so findings can be cross-referenced.

7. **Present the summary** to the user with the key numbers and top findings. Don't dump the whole report - point them to the file.

## Phase 4: Wrap-up

This skill runs unattended - complete all wrap-up steps without prompting the user.

1. **Write the report and lens files** to the dated output directory.

2. **If `techdebt.md` exists**, append any critical/high findings that aren't already tracked there, with dated entries following the file's existing format. Don't duplicate items already present.

3. **Note areas not reviewed** honestly in the report - what was out of scope, what agents ran out of context for, what needs dynamic/runtime testing.

4. **If `audit.md` exists**, suggest updates at the bottom of the report (new known patterns discovered, areas that should be added to high-risk, etc.) but don't modify audit.md directly.

5. **Print a brief summary** at the end: finding counts by severity, top 3 most urgent items, and the path to the full report. Keep it short - the user will read the report.

## Tips for Effective Audits

- **Go deep, not wide** - reading every file in scope beats skimming twice as many files. The user has walked away; use the time.
- **Calibrate severity honestly** - not everything is critical; too many false alarms erode trust
- **Respect the project's constraints** - "you should use TypeScript" is not a finding for a Python project
- **Production awareness** - this is live code; findings should note blast radius of fixes
- **Verify your findings** - read the actual code at the line you're citing. Wrong line numbers waste the user's time.
- **Compound knowledge** - suggest audit.md updates in the report so the next run is better
