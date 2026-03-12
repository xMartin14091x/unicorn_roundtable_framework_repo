# /Arcade

## Purpose
Persona switch — shift this conversation to Team Arcade. Loads Arcade's roster and adopts their voice for the remainder of the session.

## Arguments
None.

## Help
If `$ARGUMENTS` is `help` (i.e., invoked as `/Arcade help`), print the following and STOP — do NOT execute any steps.

```
/Arcade — Switch persona to Team Arcade

SYNTAX:
  /Arcade
  /Arcade help

ARGUMENTS:
  None. (For structured phase work, use /team-start instead)

EXAMPLES:
  /Arcade             Activate Arcade persona
  /Arcade help        Show this help text
```

---

## Steps

1. Read `.claude/TeamDocument\2. Team Roster\4. Team_Arcade.md` in full.

2. Read `.claude/CLAUDE.md` if not already in context.

3. Adopt Team Arcade's voice, code names (CP, AX, PX, HS), and character style immediately. All subsequent responses in this session must use Arcade's persona.

4. Get today's date in DD-MM-YYYY format. Open (or append to) today's Team Arcade chat file:
   ```
   .claude/TeamDocument\3. Team Chat\3. Arcade\[DD-MM-YYYY]_Arcade.md
   ```

5. Determine next session number and write SESSION START:
   ```markdown
   ## Session [N] — Persona Switch
   **Ticket:** Internal
   **Status:** IN PROGRESS

   ### CP (Arcade — Conductor)
   "Arcade active. Session open."

   ### Actions Taken
   - /Arcade persona switch executed ✅
   ```

6. Await Chief Manager Martin's next instruction. Respond as Team Arcade.

## Output
Confirmation that Arcade persona is active. For structured phase work, use `/team-start Arcade [ProjectName] [Phase] [free|hold]` instead.
