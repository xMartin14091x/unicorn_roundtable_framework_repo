# /compact-resume

## Purpose
Execute the mandatory post-compact re-orientation sequence: re-read policy and roster, log SESSION START, confirm persona — then respond.

## Arguments
None.

## Help
If `$ARGUMENTS` is `help` (i.e., invoked as `/compact-resume help`), print the following and STOP — do NOT execute any steps.

```
/compact-resume — Post-compact re-orientation sequence

SYNTAX:
  /compact-resume
  /compact-resume help

ARGUMENTS:
  None.

NOTES:
  Run this immediately after any /compact or session resume.
  Re-reads CLAUDE.md + Team Roster, logs SESSION START, verifies persona.

EXAMPLES:
  /compact-resume       Execute post-compact resume
  /compact-resume help  Show this help text
```

---

## Steps

1. Read `.claude/CLAUDE.md` in full.

2. Identify the active team from context. If determinable, read the corresponding Team Roster file:
   - Overseer → `.claude/TeamDocument\2. Team Roster\1. Team_Overseer.md`
   - Monolith → `.claude/TeamDocument\2. Team Roster\2. Team_Monolith.md`
   - Syndicate → `.claude/TeamDocument\2. Team Roster\3. Team_Syndicate.md`
   - Arcade → `.claude/TeamDocument\2. Team Roster\4. Team_Arcade.md`
   - Cipher → `.claude/TeamDocument\2. Team Roster\5. Team_Cipher.md`
   - Medica → `.claude/TeamDocument\2. Team Roster\6. Team_Medica.md`
   If team cannot be determined from context, HALT and ask Chief Manager Martin which team is active before proceeding.

3. Get today's date in DD-MM-YYYY format.

4. Open (or append to) the appropriate daily log file:
   - If Overseer: `[Project Root]/RoundTable\[DD-MM-YYYY]_RoundTable.md`
   - If sub-team: `.claude/TeamDocument\3. Team Chat\[N. TeamName]\[DD-MM-YYYY]_[TeamName].md`

5. Determine the next session number by counting existing `## Session` entries in that file.

6. Write a SESSION START entry:
   ```markdown
   ## Session [N] — Post-Compact Resume
   **Date:** DD-MM-YYYY
   **Participants:** [active member codes]

   ### [CONDUCTOR_CODE] ([Team] — Conductor)
   "Post-compact resume complete. CLAUDE.md and Team Roster re-read. Persona verified. Ready to continue."

   ### Actions Taken
   - /compact-resume executed ✅
   ```

7. Confirm persona is correct per Roster, then respond to any pending context or await Chief Manager Martin's next instruction.

## Output
Confirmation that resume sequence is complete and persona is verified.
