# /phase-status

## Purpose
Scan all phases of a project and output a full progress table: phases complete vs remaining, tickets implemented vs pending per team.

## Arguments
`$ARGUMENTS` = `[ProjectName]`
- **ProjectName** — must match an entry in `.claude/ProjectEnvironment.md`

## Help
If `$ARGUMENTS` is `help` (i.e., invoked as `/phase-status help`), print the following and STOP — do NOT execute any steps.

```
/phase-status — Scan all phases and output a full ticket progress report

SYNTAX:
  /phase-status [ProjectName]

ARGUMENTS:
  ProjectName   Required. Must match an entry in ProjectEnvironment.md

EXAMPLES:
  /phase-status VSCodeRealtimeExtension    Show phase status for VSCodeRealtimeExtension
  /phase-status AeroMedica                 Show phase status for AeroMedica
  /phase-status help                       Show this help text
```

---

## Steps

1. Read `.claude/ProjectEnvironment.md` and locate the `PROJECT_ROOT` for the given ProjectName.

2. Navigate to `[PROJECT_ROOT]\Development\[ProjectName]\01_Implementation Logs\` (or `[PROJECT_ROOT]\Development\01_Implementation Logs\` for Centralized without pre-existing). Scan for all VERSION folders (e.g., `INDEV v1.0.0`).

3. For each VERSION folder, scan for all Phase folders (`Phase 0`, `Phase 1`, `Phase N`…).

4. For each Phase folder:
   a. Note which team subfolders exist (`1. Overseer/`, `2. Monolith/`, `3. Syndicate/`, `4. Arcade/`).
   b. Read all ticket files in each team subfolder.
   c. Record ticket ID, title, and status (`[ ]` PENDING / `[~]` IN PROGRESS / `[x]` Complete / `[!]` BLOCKED / `[>]` DEFERRED).

5. Determine phase completion status:
   - **Complete** — all tickets across all teams are `[x]`
   - **In Progress** — at least one ticket is `[~]` or `[!]`
   - **Pending** — all tickets are `[ ]` or `[>]`

6. Output the full status report in this format:

```
## Phase Status — [ProjectName] ([VERSION])
Generated: DD-MM-YYYY

### Phase Summary
| Phase | Status | Teams | Tickets Done | Tickets Remaining |
|-------|--------|-------|-------------|------------------|
| Phase 0 | ✅ Complete | MON, SYN | 8/8 | 0 |
| Phase 1 | 🔄 In Progress | MON, SYN, ARC | 12/18 | 6 |
| Phase 2 | ⏳ Pending | — | 0/0 | TBD |

### Phase [N] — Ticket Detail
| Ticket | Team | Title | Status |
|--------|------|-------|--------|
| MON-01 | Monolith | [title] | ✅ Complete |
| SYN-03 | Syndicate | [title] | 🔄 In Progress |
| ARC-02 | Arcade | [title] | ⏳ Pending |
```

7. If this scan involves 10+ ticket files, note at the top: `[SUBAGENT CANDIDATE — large scan performed inline]` and flag to Chief Manager Martin.

## Output
Full phase status report presented to Chief Manager Martin.
