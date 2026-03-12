# /template-apply

## Purpose
Apply selected updates from the latest RoundTable Framework version to the local installation.

## Arguments
- `$ARGUMENTS` -- One of:
  - `recommended` -- apply only RECOMMENDED changes (safest)
  - `all` -- apply RECOMMENDED + OPTIONAL changes
  - `[file-path]` -- apply a specific file update (e.g., `TeamDocument/1. Policies/06_Debugging_Protocol.md`)

## Steps

1. **Run `/template-diff` internally** to get the current change set and recommendations.

2. **Determine scope** based on argument:
   - `recommended` -> only files marked RECOMMENDED
   - `all` -> files marked RECOMMENDED + OPTIONAL
   - Specific path -> only that file (must be RECOMMENDED or OPTIONAL; refuse NOT RECOMMENDED without explicit override)

3. **Create a backup snapshot** before applying any changes:
   - Copy `template-version.json` to `template-version.backup.json`
   - For each file to be modified, create a `.backup` copy in the same directory
   - Log the backup manifest

4. **Apply changes:**
   - For NEW files: create the file with remote content
   - For MODIFIED files: replace local content with remote content
   - For REMOVED files: delete the local file (only if it matches default content -- never delete customized files)
   - **NEVER touch files marked CUSTOM or NOT RECOMMENDED** (unless user explicitly passed the file path)

5. **Update `template-version.json`** to reflect the new version.

6. **Present the apply report:**

```markdown
## RoundTable Framework -- Update Applied

**Updated from:** [old version] -> [new version]

### Changes Applied
| File | Action | Status |
|------|--------|--------|
| [path] | [Created / Updated / Removed] | [OK / FAILED] |

### Backup Location
Backup files created with `.backup` extension alongside originals.
To rollback: run `/template-rollback`
```

7. **Remind the user** to review any OPTIONAL changes they skipped and any NOT RECOMMENDED items that may need manual merging.

## Output
A report of all changes applied, with backup confirmation and rollback instructions.
