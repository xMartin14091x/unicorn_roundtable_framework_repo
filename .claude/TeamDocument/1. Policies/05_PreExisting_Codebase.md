# §5 — Pre-Existing Codebase Standards

> **Policy reference file.** Loaded on-demand from `.claude/policies/`. Core rules live in CLAUDE.md.

**Applies to:** ALL teams (Overseer, Monolith, Syndicate, Arcade, Cipher) — any project that has a pre-existing codebase, regardless of whether the work is a fork, modification, rebuild, or investigation. There are no exceptions.

---

## 5a. Tiered Scan Protocol (MANDATORY — Global)

**Trigger:** `Development/[ProjectName]/PreExisting TechStack/[ProjectName].md` MUST be initialized at the **start of any Discovery or Scan session** — before any implementation, before any modification, before any ticket is opened. The document is NOT deferred until "first touch". First contact with the codebase IS the trigger.

**This applies to every team, every session, every project with pre-existing code.**

---

**Scan Tiers — Execute in order, never skip a tier:**

**L1 — Broad Scan (MANDATORY — always first)**
- Scan directory tree to depth 3
- Identify entry point files (`Program.cs`, `main.py`, `index.ts`, `package.json`, `*.sln`, `docker-compose.yml`, `project.godot`, etc.)
- Identify package manifests and tech stack markers
- Classify codebase size (see Size Gate below)
- Output: subsystem list, tech stack detected, size classification (S / M / L)
- **Action:** Initialize `Development/[ProjectName]/PreExisting TechStack/[ProjectName].md` immediately with L1 findings. Do not wait for L2 or L3.

**L2 — Specific Scan (default follow-up after L1)**
- Read key files per subsystem: entry points, config files, main service/controller files, router files
- Map architecture per subsystem, data flow, key classes and functions
- Output: architecture diagram per subsystem, Key Functions / Classes table populated
- **Action:** Populate each subsystem section in `Development/[ProjectName]/PreExisting TechStack/[ProjectName].md` as sections are scanned.

**L3 — Full Code Scan (Chief Manager Martin authorization required)**
- Read all source files in depth across the entire codebase
- Gate: only proceed if **Chief Manager Martin explicitly authorizes** — regardless of codebase size
- If L3 is requested on a Large (L) codebase, flag the scope to Chief Manager Martin before starting
- **Action:** Complete all subsystem sections in `Development/[ProjectName]/PreExisting TechStack/[ProjectName].md` to full depth.
- **Verification required:** After scan, Conductor MUST complete all five checks in §5b (File Manifest, Service-URL Tracing, Conductor Cross-Check, Cross-Layer Subsystem Tracing, External Dependency & Hidden Service Audit) before the L3 is considered complete and before any output is presented to Chief Manager Martin.

---

**Size Gate (used to inform Chief Manager Martin — does NOT block any scan tier):**

| Class | Source File Count | Default Recommendation |
|-------|------------------|----------------------|
| **S — Small** | < 50 source files | L2 default, suggest L3 to Chief Manager Martin |
| **M — Medium** | 50–200 source files | L2 default, L3 available on Chief Manager Martin request |
| **L — Large** | > 200 source files | L2 default, flag scope before L3, L3 requires explicit approval |

> Size classification is reported to Chief Manager Martin after L1. Chief Manager Martin decides whether to authorize L3.

---

**File location and naming:**
- Check `.claude/ProjectEnvironment.md → ACTIVE_PROJECTS` to confirm the correct `[ProjectName]` subfolder
- Full path formula: `Development/[ProjectName]/PreExisting TechStack/[ProjectName].md`

**File section format (one section per subsystem):**

```markdown
## [Subsystem Name]
**Source files:** `src/path/to/module`, `src/path/to/other`
**Scan tier completed:** L1 | L2 | L3
**Last reviewed:** DD-MM-YYYY

### Purpose
[What this subsystem does — 2–4 sentences.]

### Architecture / Data Flow
[ASCII diagram or numbered pipeline describing how data moves through the subsystem.]

### Key Functions / Classes
| Name | File | What it does |
|------|------|-------------|
| `functionName()` | `file:L42` | [brief description] |

### Critical Invariants
1. [Thing that must never change and why]
2. [Another invariant]

### Known Quirks / Gotchas
- [Non-obvious behaviour, timing races, caching gotchas, etc.]
```

---

**Global Rules — apply to all teams without exception:**
- The PreExisting TechStack file is initialized at **first contact with the codebase** — not at first modification.
- The PreExisting TechStack file is a **living document** — never delete sections, only update them.
- The Technologist (MT/SC/AX/GL) owns this document for their team's work.
- Each section must show its current scan tier (`L1 / L2 / L3`) so any reader knows how deep the documentation goes.
- L3 (Full Code Scan) always requires **explicit Chief Manager Martin authorization** — no team may self-authorize a full scan.
- If a bug fix reveals new architecture knowledge, add it to the relevant subsystem's "Known Quirks" before the ticket is closed.
- This document is the **first thing to consult** before filing any bug report or implementing any change in a known subsystem.
- After `/compact` or session resume: re-check the PreExisting TechStack file exists. Consult `.claude/ProjectEnvironment.md` for the correct project name and path. If the file does not exist and a pre-existing codebase is present, create it immediately before any other work.

---

## 5b. L3 Completeness Verification Protocol (MANDATORY — every L3 scan)

**Applies to:** Every team, every L3 scan, every project — no exceptions.

**Root cause of this rule:** L3 scans delegated to subagents produce prose summaries that may omit individual files even when the correct directories were scanned. A summary that touches the right folders is not the same as a summary that enumerates every file in those folders. This protocol closes that gap.

**All five checks below are mandatory. L3 is not complete until all five pass. Presenting L3 output to Chief Manager Martin before all five checks pass is a protocol violation.**

---

**Check 1 — File Manifest (Completeness Verification)**

After receiving the L3 scan output, the Conductor must run a raw file listing on every scanned subsystem directory and produce a File Manifest as a mandatory section in the PreExisting TechStack file:

```markdown
## L3 File Manifest — Completeness Verification
| Directory | Files Found | Files Documented | Gap | Status |
|-----------|-------------|-----------------|-----|--------|
| src/pages/settings/ | 8 .tsx | 8 .tsx | 0 | ✓ PASS |
| src/Controllers/    | 5 .cs  | 5 .cs  | 0 | ✓ PASS |
| src/api/routers/    | 12 .py | 12 .py | 0 | ✓ PASS |
```

**Rules:**
- A row shows **PASS** only when Files Found = Files Documented.
- Any **GAP > 0** is a mandatory re-scan trigger for that subsystem — not optional.
- The Manifest must be signed off by the Verification Scholar before the L3 is presented to Chief Manager Martin.
- L3 is not complete until every row shows PASS.

---

**Check 2 — Service-URL Tracing**

For any project with a frontend layer, every API client/service file must be read during L3 and its API base URL explicitly traced to the backend service that handles it. The result must be documented in the PreExisting TechStack file:

```markdown
## Frontend → Backend Service Map
| Service File          | API Base URL                  | Resolves To                  |
|-----------------------|-------------------------------|------------------------------|
| aiAgent.service.ts    | localhost:5200/api/ai-agents  | Observer: AiAgentController  |
| knowledge.service.ts  | localhost:8000/api/v1/knowledge | Brain: knowledge router    |
```

**Rules:**
- Every service file must have a traced row — no service file left without a mapping.
- Any service file pointing to an unexpected port or host must be flagged and investigated before the L3 is filed.
- This prevents architectural misidentification where a feature appears to belong to one service but its API lives in another.

---

**Check 3 — Conductor Independence Cross-Check**

When L3 is performed by a subagent or delegated tool, the Conductor must independently run a raw file-count check per subsystem using Glob — separate from the subagent's output — before accepting the scan as complete:

```
After receiving any subagent L3 output, Conductor MUST independently run:
  Glob("**/*.tsx", path="src/pages/")    → compare count to subagent output
  Glob("**/*.cs",  path="src/Controllers/")  → compare count to subagent output
  Glob("**/*.py",  path="src/api/routers/")  → compare count to subagent output

If count does not match → flag gap → re-scan that subsystem → do not present output to Chief Manager Martin until resolved.
```

**Rules:**
- The Conductor may NOT delegate this cross-check to the same subagent that produced the original scan.
- The cross-check is mandatory regardless of how comprehensive the subagent's output appears.
- If any count mismatches, re-scan only the affected subsystem — not the entire codebase.

---

**Check 4 — Cross-Layer Subsystem Tracing (MANDATORY)**

**Root cause:** Checks 1–3 verify files are counted and service URLs are traced, but they operate within each layer independently. They do not verify that subsystems discovered in one layer are accounted for in all other layers. A database schema can define an entire subsystem that has zero application code — and if the scan only enumerates application-layer files, the database subsystem is invisible.

**This is not limited to SQL.** The same failure pattern applies to:
- **Database scripts** containing schemas/tables/SPs that have no matching repository, controller, or service in application code
- **Application code** (controllers, services, repositories) that reference database objects, external APIs, or subsystems not documented elsewhere
- **Frontend routes/pages** that call API endpoints with no documented backend handler
- **Config/infrastructure files** (Docker Compose services, Terraform resources, CI/CD pipelines) that define services not documented as subsystems
- **Dependency manifests** (package.json, .csproj, requirements.txt) that pull in major frameworks implying undocumented capabilities

**Procedure — executed by the Conductor after Checks 1–3 pass:**

**Step 1: Extract the Subsystem Registry from ALL layers.**

Scan every layer of the codebase and extract a master list of distinct subsystems/namespaces/domains.

**Step 2: Build the Cross-Layer Matrix.**

```markdown
## Cross-Layer Subsystem Matrix
| Subsystem | DB Schema | DB SPs | App Repositories | App Controllers | App Services | Frontend UI | Infra | Status |
|-----------|-----------|--------|-----------------|----------------|-------------|-------------|-------|--------|
| Auth      | auth.*    | 5 SPs  | AuthRepository  | AuthController  | JwtService  | LoginPage   | —     | FULL   |
| AI/Agents | ai.*      | 1 SP   | **NONE**        | **NONE**        | **NONE**    | **NONE**    | —     | DB-ONLY |
```

**Step 3: Flag and document every gap.**

Any subsystem that exists at one layer but NOT at another must be explicitly documented with:
- What exists (which layer, what objects)
- What does NOT exist (which layers are missing)
- Whether this is intentional or an oversight

**Step 4: Cross-reference against comparison targets.**

When scanning two related codebases (e.g., original + refactor), the matrix must be built for BOTH and compared side-by-side.

**Rules:**
- This check applies to EVERY L3 scan, not just multi-codebase comparisons.
- A subsystem documented only at one layer is not a failure — but it MUST be explicitly stated.
- The Cross-Layer Matrix is a mandatory section in the PreExisting TechStack file for every L3 scan.
- The Conductor owns this check. It may NOT be delegated to the scan subagent.

---

**Check 5 — External Dependency & Hidden Service Audit (MANDATORY)**

**Root cause:** Checks 1–4 trace subsystems that declare themselves through code structure. But many critical system components hide inside configuration files, dependency manifests, entry-point registrations, and environment variables. This check forces the Conductor to read those files and extract every external integration, background service, and capability-implying package.

**This check has six mandatory sub-audits. All six must be completed.**

---

**5a. External Service & Integration Audit**

Scan all configuration files for outbound URLs, API keys, connection strings, and service references:

```
Files to scan:
  → appsettings.json, appsettings.*.json
  → .env, .env.*, docker.env
  → docker-compose.yml, docker-compose.*.yml
  → Any *config*.json, *config*.yaml at project root
  → project.godot, export_presets.cfg (for Godot projects)

Extract and document:
  → Every URL, hostname, port, or connection string
  → Every API key reference or secret vault path
  → Every named external service (Ollama, SendGrid, Azure, AWS, etc.)
```

Produce an **External Dependency Registry**:

```markdown
## External Dependency Registry
| Service | Type | Reference Location | URL/Endpoint | Credentials | Status |
|---------|------|-------------------|-------------|-------------|--------|
| Ollama  | Local LLM | ai_config.json | localhost:11434 | None | ACTIVE |
| PostgreSQL | Database | docker-compose.yml | localhost:5432 | .env credentials | ACTIVE |
```

**Rules:**
- Every external URL found in config must have a row — no silent dependencies.
- Credentials found in plaintext must be flagged as a security concern.

---

**5b. Background Worker & Hosted Service Audit**

Scan entry-point files for registered background services, hosted workers, and scheduled tasks:

```
Files to scan:
  → Program.cs / Startup.cs / Host configuration files
  → Any file matching *Worker*, *HostedService*, *BackgroundService*, *Job*, *Scheduler*
  → Python: main.py, celery configs, APScheduler configs
  → Godot: autoloads registered in project.godot
```

Produce a **Background Services Registry**:

```markdown
## Background Services Registry
| Service | Type | Registered In | What It Does | Triggers |
|---------|------|--------------|-------------|----------|
| TelemetryCollector | Autoload | project.godot | Collects gameplay events | Always-on singleton |
```

**Rules:**
- Any registered background service or autoload must be documented.
- If none exist, explicitly state "No background services found."

---

**5c. Middleware & Pipeline Component Audit**

Scan entry-point files for all middleware registrations and pipeline components:

```
Extract from Program.cs / Startup.cs / project.godot:
  → Every app.Use*() call
  → Every app.Map*() call
  → Autoload order (for Godot — order matters)
  → Document the exact pipeline/autoload sequence
```

Produce a **Middleware / Autoload Pipeline Registry**:

```markdown
## Middleware Pipeline (in execution order)
| Order | Middleware / Autoload | File | What It Does | Dependencies |
|-------|----------------------|------|-------------|--------------|
| 1 | GameManager | game_manager.gd | Global state, scene changes | None |
| 2 | TelemetryCollector | telemetry_collector.gd | Event collection | None |
```

**Rules:**
- Pipeline/autoload order must be documented — it affects initialization correctness.
- Any component that references an external service must note that dependency.

---

**5d. Dependency Manifest Audit (Package-Implied Capabilities)**

Scan all dependency manifests for major frameworks and libraries that imply undocumented capabilities:

```
Files to scan:
  → *.csproj (NuGet packages)
  → package.json (npm packages)
  → requirements.txt, pyproject.toml (Python packages)
  → project.godot addons section (Godot plugins)

Flag any package that implies a capability not documented as a subsystem:
  → Microsoft.SemanticKernel → AI orchestration capability
  → SignalR → Real-time communication
  → Hangfire/Quartz → Job scheduling
  → StackExchange.Redis → Caching layer
  → langchain/llama-index → RAG/LLM pipeline
  → GodotSteam → Steam platform integration
```

Produce a **Capability-Implying Packages** table:

```markdown
## Capability-Implying Packages
| Package | Found In | Implied Capability | Documented as Subsystem? | Actually Used in Code? |
|---------|----------|-------------------|-------------------------|----------------------|
| HTTPRequest (Godot built-in) | ollama_review_client.gd | HTTP client for Ollama | Yes — AI Review subsystem | Yes |
```

---

**5e. Inter-Service Communication Audit**

Scan application code for any service-to-service HTTP calls, message bus interactions, or internal API consumption:

```
Search patterns:
  → Grep("HttpClient|IHttpClientFactory|RestClient", path="src/")
  → Grep("HTTPRequest", path="src/") — for Godot projects
  → Grep("localhost:\d+|127\.0\.0\.1:\d+", path="src/")
  → Grep("fetch\(|axios\.|\.post\(|\.get\(", path="src/", glob="*.ts")
```

Produce an **Inter-Service Communication Map**:

```markdown
## Inter-Service Communication Map
| Caller | Target | Protocol | URL/Channel | Purpose |
|--------|--------|---------|-------------|---------|
| OllamaReviewClient | Ollama | HTTP POST | :11434/api/generate | AI performance review |
```

**Rules:**
- Every internal service-to-service call must be documented.
- If none exist, explicitly state "No inter-service communication found."
- Any hardcoded localhost URL in source code must be flagged.

---

**5f. Environment Configuration Audit**

Scan all environment-specific configuration files and produce a topology map per environment:

```
Files to compare:
  → appsettings.json vs appsettings.Development.json vs appsettings.Production.json
  → .env vs .env.production vs .env.test
  → user://ai_config.json vs res://user_data/ai_config.json (for Godot projects)
```

Produce an **Environment Topology Map**:

```markdown
## Environment Topology
| Setting | Development | Testing | Production | Notes |
|---------|------------|---------|------------|-------|
| Ollama URL | auto-discover | localhost:11434 | configured | LAN IP scan on startup |
| AI model | llama3.1:8b | llama3.1:8b | configurable | Via ai_config.json |
```

**Rules:**
- Any setting that differs across environments must be documented.
- Security-sensitive settings relaxed in dev must be flagged with a warning for production.

---

**Check 5 Output Requirements:**

All six sub-audits produce their respective registries/tables as mandatory sections in the PreExisting TechStack file:

1. External Dependency Registry
2. Background Services Registry
3. Middleware / Autoload Pipeline Registry
4. Capability-Implying Packages table
5. Inter-Service Communication Map
6. Environment Topology Map

**Rules:**
- All six sub-audits are mandatory for every L3 scan. If a sub-audit finds nothing, it must explicitly state "None found."
- The Conductor owns this check. It may NOT be delegated to the scan subagent.
- Check 5 runs AFTER Check 4 (Cross-Layer Matrix).
- Config files from ALL environments must be read, not just the default.

---

**Failure consequence:** Presenting an L3 output to Chief Manager Martin without completing all five checks is a protocol violation — equivalent to filing an incomplete ticket. The output must be retracted and a corrected version resubmitted.

---

*Adopted from ClaudeTemplate — 11-03-2026. Full L1/L2/L3 Tiered Scan Protocol + L3 Completeness Verification (5 checks, 6 sub-audits in Check 5). Adapted for RoundTable: KP/Martin naming. Added Godot-specific examples (project.godot, autoloads, HTTPRequest, ai_config.json) to reflect active project context.*
