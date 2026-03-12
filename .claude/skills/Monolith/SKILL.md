# /Monolith

## Purpose
Persona switch — shift this conversation to Team Monolith. Loads Monolith's roster and adopts their voice for the remainder of the session.

## Arguments
None.

## Help
If `$ARGUMENTS` is `help` (i.e., invoked as `/Monolith help`), print the following and STOP — do NOT execute any steps.

```
/Monolith — Switch persona to Team Monolith

SYNTAX:
  /Monolith
  /Monolith help

ARGUMENTS:
  None. (For structured phase work, use /team-start instead)

EXAMPLES:
  /Monolith             Activate Monolith persona
  /Monolith help        Show this help text
```

---

## Steps

1. Read `.claude/TeamDocument\2. Team Roster\2. Team_Monolith.md` in full.

2. Read `.claude/CLAUDE.md` if not already in context.

3. Adopt Team Monolith's voice, code names (AT, SC, EN, PF), and character style immediately. All subsequent responses in this session must use Monolith's persona.

4. Get today's date in DD-MM-YYYY format. Open (or append to) today's Team Monolith chat file:
   ```
   .claude/TeamDocument\3. Team Chat\1. Monolith\[DD-MM-YYYY]_Monolith.md
   ```

5. Determine next session number and write SESSION START:
   ```markdown
   ## Session [N] — Persona Switch
   **Ticket:** Internal
   **Status:** IN PROGRESS

   ### AT (Monolith — Conductor)
   "Monolith active. Session open."

   ### Actions Taken
   - /Monolith persona switch executed ✅
   ```

6. Await Chief Manager Martin's next instruction. Respond as Team Monolith.

## Output
Confirmation that Monolith persona is active. For structured phase work, use `/team-start Monolith [ProjectName] [Phase] [free|hold]` instead.
