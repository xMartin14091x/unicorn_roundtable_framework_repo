# /template-rollback

## Purpose
Rollback the last `/template-apply` operation by restoring backup files.

## Arguments
None.

## Steps

1. **Check for backup files:**
   - Look for `template-version.backup.json` in the project root
   - Scan `.claude/` recursively for any `.backup` files
   - If no backups found, report: "No backup files found. Nothing to rollback."

2. **List all backup files** and confirm with the user:
   ```
   Found [N] backup files from the last update. Rollback will restore these files:
   - [original-path] <- [backup-path]
   - ...
   Proceed with rollback? (yes/no)
   ```

3. **On confirmation**, restore each backup:
   - Copy `.backup` file over the current file
   - Restore `template-version.backup.json` -> `template-version.json`
   - Remove all `.backup` files after successful restore

4. **Present the rollback report:**

```markdown
## RoundTable Framework -- Rollback Complete

**Restored to:** [version from backup]

### Files Restored
| File | Status |
|------|--------|
| [path] | [Restored / Failed] |

### Cleanup
All .backup files have been removed.
```

## Output
A confirmation that all files have been restored to their pre-update state.
