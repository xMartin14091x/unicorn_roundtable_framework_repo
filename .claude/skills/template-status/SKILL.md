# /template-status

## Purpose
Display the current RoundTable Framework version, installation state, and customization summary.

## Arguments
None.

## Steps

1. **Read `template-version.json`** from the project root (same level as `.claude/`).
   - If the file does not exist, report: "No template-version.json found. This project may not be using RoundTable Framework, or the file was removed."
   - Extract: `name`, `version`, `released`.

2. **Scan `.claude/` structure** and report:
   - CLAUDE.md exists: YES/NO
   - TeamDocument/ exists: YES/NO
   - Number of policy files in `TeamDocument/1. Policies/`
   - Number of team roster files in `TeamDocument/2. Team Roster/`
   - Number of skills in `skills/`
   - ProjectEnvironment.md exists: YES/NO

3. **Check for local customizations** by scanning:
   - CLAUDE.md: does it reference the default "KP" / "Chief Manager Martin" naming, or has it been customized?
   - Team Roster: list which rosters exist vs the default 6 (Overseer, Monolith, Syndicate, Arcade, Cipher, Medica)
   - Skills: list any skills NOT in the default template set

4. **Present the status report** to the user:

```markdown
## RoundTable Framework — Status

| Property | Value |
|----------|-------|
| **Version** | [version from template-version.json] |
| **Released** | [released date] |
| **CLAUDE.md** | [Present / Missing] |
| **Policies** | [N] files |
| **Team Rosters** | [N] files |
| **Skills** | [N] total ([M] default + [K] custom) |
| **ProjectEnvironment** | [Present / Missing] |
| **Naming** | [Default (KP/Martin) / Customized] |

### Custom Additions
- [list any non-default skills, rosters, or policies]

### Missing Defaults
- [list any expected default files that are absent]
```

## Output
A formatted status table showing the current template installation state, version, and customizations.
