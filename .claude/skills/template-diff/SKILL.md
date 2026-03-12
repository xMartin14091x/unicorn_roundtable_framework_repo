# /template-diff

## Purpose
Analyze differences between the local RoundTable Framework installation and the latest remote version, with AI-powered merge recommendations.

## Arguments
- `$ARGUMENTS` -- (Optional) `--files-only` to show only changed file list without content diff.

## Steps

1. **Read local version** from `template-version.json`. Read remote version by fetching from GitHub.

2. **If versions match**, report: "Local and remote are the same version ([version]). No diff to show." Exit.

3. **Fetch the remote file list** by reading the repository tree. Compare against local `.claude/` structure.

4. **For each file**, classify as:
   - **NEW** -- exists in remote but not locally (added in update)
   - **REMOVED** -- exists locally as a default file but not in remote (removed in update)
   - **MODIFIED** -- exists in both but content differs
   - **UNCHANGED** -- identical content
   - **CUSTOM** -- exists locally but is NOT a default template file (user addition -- never overwrite)

5. **For each MODIFIED file**, perform a content diff and classify the change:
   - **POLICY UPDATE** -- rules, standards, or protocol changes
   - **STRUCTURAL** -- file/folder reorganization
   - **NAMING** -- code name or authority title changes
   - **BUG FIX** -- corrects an error in the template
   - **ENHANCEMENT** -- adds new capability without breaking existing behavior

6. **Generate merge recommendations** for each changed file:

| Recommendation | Meaning |
|---------------|---------|
| **RECOMMENDED** | Bug fix or critical policy update -- safe to merge, no conflict risk |
| **OPTIONAL** | Enhancement or structural improvement -- review before merging |
| **NOT RECOMMENDED** | Conflicts with local customizations -- manual review required |
| **SKIP** | Custom local file -- never overwrite |

7. **Present the diff report:**

```markdown
## RoundTable Framework -- Diff Report
**Local:** [version] | **Remote:** [version]

### Summary
| Status | Count |
|--------|-------|
| New files | [N] |
| Modified | [N] |
| Removed | [N] |
| Unchanged | [N] |
| Custom (yours) | [N] |

### Changes

#### RECOMMENDED (safe to apply)
| File | Change Type | What Changed |
|------|------------|-------------|
| [path] | [type] | [1-line summary] |

#### OPTIONAL (review first)
| File | Change Type | What Changed |
|------|------------|-------------|

#### NOT RECOMMENDED (conflicts with your customizations)
| File | Change Type | Conflict |
|------|------------|----------|

#### SKIPPED (your custom files -- untouched)
| File | Reason |
|------|--------|
```

## Output
A categorized diff report with merge recommendations for each changed file.
