# /team-start

## Purpose
Formal team session kickoff — loads roster, reads phase briefing, logs SESSION START in Team Chat, begins implementation with Early Advance authorization explicitly set.

## Arguments
`$ARGUMENTS` = `[TeamName] [ProjectName] [Phase] [free|hold]`
- **TeamName** — `Monolith`, `Syndicate`, or `Arcade`
- **ProjectName** — must match an entry in `.claude/ProjectEnvironment.md`
- **Phase** — phase number (e.g., `2`)
- **free|hold** — `free` = team may advance to Phase N+1 on completion without waiting. `hold` = team must wait for Chief Manager Martin's explicit authorization before advancing.

## Help
If `$ARGUMENTS` is `help` (i.e., invoked as `/team-start help`), print the following and STOP — do NOT execute any steps.

```
/team-start — Formal team session kickoff — loads roster, reads briefing, begins phase work

SYNTAX:
  /team-start [TeamName] [ProjectName] [Phase] [free|hold]

ARGUMENTS:
  TeamName      Monolith, Syndicate, or Arcade
  ProjectName   Required. Must match an entry in ProjectEnvironment.md
  Phase         Phase number (e.g., 2)
  free|hold     Early Advance authorization: free = may advance on completion, hold = must wait for Chief Manager Martin

EXAMPLES:
  /team-start Monolith VSCodeRealtimeExtension 2 hold    Kick off Monolith Phase 2, hold advance
  /team-start Arcade AeroMedica 1 free                   Kick off Arcade Phase 1, free advance
  /team-start help                                        Show this help text
```

---

## Dual-Use
This skill may be invoked by Chief Manager Martin directly (Mode B — separate sessions) OR by KP internally when orchestrating sub-teams in Mode A. Both usages follow the same steps below.

## Steps

1. Read the Team Roster file for the given TeamName:
   - Monolith → `.claude/TeamDocument\2. Team Roster\2. Team_Monolith.md`
   - Syndicate → `.claude/TeamDocument\2. Team Roster\3. Team_Syndicate.md`
   - Arcade → `.claude/TeamDocument\2. Team Roster\4. Team_Arcade.md`
   Adopt this team's voice, code names, and coding style immediately.

2. Read `.claude/CLAUDE.md` if not already in context.

3. Read `.claude/ProjectEnvironment.md` and locate `PROJECT_ROOT` for the given ProjectName.

4. Read the Phase Briefing:
   ```
   [PROJECT_ROOT]\Development\[ProjectName]\01_Implementation Logs\INDEV v1.0.0\Phase [N]\[TeamName]_Phase[N]_Briefing.md
   ```
   If the briefing does not exist, HALT and notify Chief Manager Martin before proceeding.

5. Get today's date in DD-MM-YYYY format. Open (or append to) today's Team Chat file:
   ```
   .claude/TeamDocument\3. Team Chat\[N. TeamName]\[DD-MM-YYYY]_[TeamName].md
   ```

6. Determine the next session number and write the SESSION START entry:
   ```markdown
   ## Session [N] — Phase [X] Kickoff — [ProjectName]
   **Ticket:** Phase [X] — all tickets
   **Status:** IN PROGRESS
   **Early Advance:** [free — may advance on completion | hold — must wait for Chief Manager Martin]

   ### [CONDUCTOR_CODE] ([Team] — Conductor)
   "Roster loaded. Briefing read. Phase [X] work begins now. Early advance status: [free/hold]."

   ### Actions Taken
   - Roster loaded ✅
   - Briefing read ✅
   - Team Chat session opened ✅
   ```

7. Run the Subagent Pre-Flight Check (§8):
   - Count independent tickets in the briefing.
   - If 3+: declare subagent delegation plan in Team Chat. Provide kickoff messages for each subagent.
   - If under threshold: declare "Single-session execution" with reason.

8. Begin work per the briefing's ticket order and parallel execution instructions.

## Output
Confirmation of session start, Early Advance status, and subagent pre-flight declaration. Then begin implementation.
