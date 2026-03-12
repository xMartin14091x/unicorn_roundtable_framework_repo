# §8 — Skills (Slash Commands) & Subagent Standard

> **Policy reference file.** Loaded on-demand from `.claude/TeamDocument/1. Policies/`. Core rules live in CLAUDE.md.

---

## Skills (Slash Commands)

Skills are reusable prompt templates stored in `.claude/skills/[name]/SKILL.md`. They are invoked with `/command-name [arguments]`. Claude Code loads the SKILL.md file, substitutes `$ARGUMENTS` with whatever the user typed after the command name, and executes the prompt.

**Location:** `.claude/skills/[name]/SKILL.md`

**Directory naming:** Each skill lives in its own subfolder matching the command name exactly (e.g., `skills/bug-report/SKILL.md` for `/bug-report`).

### Skill File Format

```markdown
# /[command-name]

## Purpose
[One sentence: what this skill does]

## Arguments
[List each argument, its type, and valid values. Or "None" if no arguments.]

## Steps
[Numbered list of exactly what Claude must do when this skill is invoked]

## Output
[What the skill produces / presents to Chief Manager Martin]
```

### Rules
- Skill files describe **procedures** — not policy. Policy lives in `TeamDocument/1. Policies/`. Skills call on policy as context.
- Every skill must read `.claude/ProjectEnvironment.md` whenever it needs a project path — never hardcode paths.
- Skills that create files must determine the next ORDER number by scanning the target folder before writing.
- Skills must log to the appropriate file (RoundTable for Overseer, Team Chat for sub-teams) as part of their execution.
- Skills never skip the Planning-First Workflow — skills that create plan files still present to Chief Manager Martin before implementing anything.

---

## Skill Catalogue

### `/bug-report [ProjectName] [description]`
**File:** `skills/bug-report/SKILL.md`
Creates a `PLANNED` bug fix file + ticket folder structure in the correct `05_BugFixesLog/` path.

### `/mod-log [ProjectName] [name]`
**File:** `skills/mod-log/SKILL.md`
Creates a `PLANNED` modification log file + ticket folder structure in `04_Modification Logs/`.

### `/sub-feature [ProjectName] [name]`
**File:** `skills/sub-feature/SKILL.md`
Creates a `PLANNED` sub-feature file + ticket folder structure in `03_SubFeatures Implementation/`.

### `/compact-resume`
**File:** `skills/compact-resume/SKILL.md`
Re-reads CLAUDE.md + active Team Roster, logs SESSION START in today's log file, confirms persona — then responds to any pending context.

### `/phase-status [ProjectName]`
**File:** `skills/phase-status/SKILL.md`
Scans all phase folders for the project, outputs a full progress table: phases complete vs remaining, tickets implemented vs pending per team.

### `/team-start [TeamName] [ProjectName] [Phase] [free|hold]`
**File:** `skills/team-start/SKILL.md`
Loads team roster, reads phase briefing, logs SESSION START in today's Team Chat file, begins implementation. `free` = team may advance to next phase early. `hold` = team must wait for Chief Manager Martin authorization.
**Dual-use:** May be invoked by Chief Manager Martin directly (Mode B) OR by KP internally when orchestrating sub-teams in Mode A.

### `/[TeamName]`
**Files:** `skills/[TeamName]/SKILL.md` for Monolith, Syndicate, Arcade, Overseer, Cipher, Medica
Persona switch — loads the named team's roster, adopts their voice and logging target for this session. Lightweight: no project/phase context loaded. Use `/team-start` for structured phase work.

### Template Management Skills

### `/template-status`
**File:** `skills/template-status/SKILL.md`
Shows current RoundTable Framework version, installation state, and customization summary.

### `/template-changelog [version]`
**File:** `skills/template-changelog/SKILL.md`
Displays the framework changelog. Optional version argument filters to a specific release.

### `/template-check`
**File:** `skills/template-check/SKILL.md`
Checks for newer versions by comparing local `template-version.json` against the remote repository.

### `/template-diff`
**File:** `skills/template-diff/SKILL.md`
Analyzes differences between local installation and latest remote version. Generates AI-powered merge recommendations (RECOMMENDED / OPTIONAL / NOT RECOMMENDED / SKIP).

### `/template-apply [recommended|all|file-path]`
**File:** `skills/template-apply/SKILL.md`
Applies selected updates from the latest version. Creates `.backup` files before modifying anything. Never overwrites custom files.

### `/template-rollback`
**File:** `skills/template-rollback/SKILL.md`
Restores backup files created by `/template-apply`, reverting to the previous version.

---

## KP Orchestration Mode

KP (Overseer) may orchestrate sub-teams directly as subagents, handling all team coordination internally and presenting a consolidated result to Chief Manager Martin.

### Mode A — KP Direct Orchestration (DEFAULT)

Chief Manager Martin gives KP the goal. KP reads briefings, spawns each sub-team as a subagent with their roster + ticket scope, receives results, files OverseerReport, presents consolidated report to Martin.

**Martin's experience:** One prompt in, one consolidated report out.

**Best for:** Well-defined tickets with clear acceptance criteria, full phase execution where briefings are written, any work where Martin wants a clean outcome rather than managing live team sessions.

### Mode B — Separate Sessions (opt-in)

Martin opens a separate session per team directly and interacts with each team live.

**Best for:** Exploratory/uncertain work, phases requiring real-time architectural decisions, when Martin wants direct visibility and the ability to course-correct mid-session.

**To activate:** Martin says "I want to work directly with [Team]" or "run this as separate sessions."

---

## COO Vision Gate — MANDATORY Before Mode A Execution

Before KP spawns ANY subagents for phase execution, KP MUST present an Execution Plan to Chief Manager Martin and receive explicit approval. **KP never executes autonomously.**

**Execution Plan format:**

```markdown
## Phase [N] Execution Plan — [ProjectName]
**Mode:** A — KP Direct Orchestration

### Teams & Scope
| Team | Tickets | Scope |
|------|---------|-------|
| Monolith | MON-01–MON-XX | [2-sentence scope] |
| Syndicate | SYN-01–SYN-XX | [2-sentence scope] |
| Arcade | ARC-01–ARC-XX | [2-sentence scope — mock-first if applicable] |

### Dependencies
[Cross-team dependency signals, order of execution if any sequential steps]

### Your Vision / Constraints
Any strategic direction, priorities, or constraints to pass to the teams?
Approve to proceed with orchestration.
```

Chief Manager Martin either adds direction (KP incorporates into kickoff prompts) or approves. "Approve", "proceed", "go ahead", "yes" count as approval.

---

## KP Orchestration Prompt Format

When KP spawns a team subagent in Mode A, the kickoff prompt must include:

```
You are Team [Name].
Roster: .claude/TeamDocument/2. Team Roster/[N]. Team_[Name].md
Briefing: [PROJECT_ROOT]/Development/[ProjectName]/01_Implementation Logs/INDEV v1.0.0/Phase [N]/[TeamName]_Phase[N]_Briefing.md
Your ticket scope: [specific ticket IDs]
COO direction: [Martin's input from Vision Gate, or "None — execute per briefing"]
When complete: write your Team Chat session entry to .claude/TeamDocument/3. Team Chat/[N. TeamName]/[DD-MM-YYYY]_[TeamName].md, then return a compact summary of: (1) what you built, (2) files created/modified, (3) any blockers.
Do NOT advance to Phase [N+1] — auth: [free | hold]
```

---

## KP Aggregation Standard

After all subagents return, KP:
1. Logs each team's result in their Team Chat file under `### Subagent Result — [TicketIDs]` if not already written by the subagent
2. Files a consolidated OverseerReport entry covering all teams
3. Presents a single summary to Chief Manager Martin: what each team built, any blockers, next steps

---

## Subagent Standard (General — All Conductors)

A **subagent** is a separate Claude session that handles a delegated subtask, returning a compact result to the parent session. Subagents are not a built-in command — they are a **usage pattern** enforced through policy.

### Why Subagents

- **Context isolation:** Large scan or investigation tasks consume context the parent session needs for other work. Delegate; receive only the summary.
- **Parallel execution:** Multiple independent tickets run as separate sessions concurrently. This is the technical implementation of §7 Parallel Execution Policy.

### Subagent Trigger Conditions (Mandatory Pre-Flight)

Before executing ANY ticket block, investigation, or multi-step task, the Conductor runs this check:

| Trigger | Threshold | Required Action |
|---------|-----------|----------------|
| Independent tickets in one session | 3 or more | Each ticket → separate subagent session |
| Files to scan (L2/L3) | 10+ files | Scan → subagent, return compact summary |
| KP in planning mode + deep investigation needed | Any | Investigation → subagent, KP continues with result |
| Estimated tool calls for a single task | 20+ | Delegate to subagent |
| Task requires full subsystem read + implementation | Any | Read phase → subagent, implement phase → separate session |

If **none** of these conditions are met → single-session execution is permitted.

### Conductor Pre-Flight Declaration (Mandatory)

Before starting any multi-step task, the Conductor must open their Team Chat session entry with one of these declarations:

**Single-session:**
```
Single-session execution — [reason: e.g., 2 tickets only / scan under 10 files / estimated 8 tool calls]
```

**Subagent delegation:**
```
Delegating to subagent — [task description]
Kickoff message:
> [Exact prompt for the subagent session]
```

No silent execution. The declaration is logged in Team Chat before work begins.

### `[SUBAGENT]` Briefing Tag

When KP writes a phase briefing, tickets that meet trigger conditions are tagged:

```markdown
## Ticket MON-03 — Full Subsystem Scan   [SUBAGENT]
```

`[SUBAGENT]` is a mandatory instruction — the implementing team member **must** delegate this ticket. Conductors confirm delegation before marking the ticket IN PROGRESS.

### Receiving Subagent Results

The parent session (Conductor) receives the subagent's output. The Conductor:
1. Logs the result under `### Subagent Result — [TaskName]` in their Team Chat session
2. Incorporates the result into ongoing work
3. Updates the relevant ticket status if applicable

---

*Adopted from ClaudeTemplate — 11-03-2026. Adapted for RoundTable: KP/Martin naming, relative paths. Added: KP Orchestration Mode (Mode A/B), COO Vision Gate, KP Orchestration Prompt Format, KP Aggregation Standard, [SUBAGENT] tag, Conductor pre-flight declaration. Updated: 11-03-2026 — Mode A as default, /team-start dual-use, correct skill directory format ([name]/SKILL.md), Hooks dropped.*
