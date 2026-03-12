# /template-changelog

## Purpose
Display the RoundTable Framework changelog, optionally filtered to a specific version.

## Arguments
- `$ARGUMENTS` — (Optional) A version number to filter to (e.g., `1.0.0`, `1.1.0`). If omitted, show the full changelog.

## Steps

1. **Read `CHANGELOG.md`** from the project root (same level as `.claude/`).
   - If the file does not exist, report: "No CHANGELOG.md found. The changelog may have been removed or this project was not initialized from RoundTable Framework."

2. **If a version argument was provided:**
   - Search for the heading matching `## [version]` (e.g., `## [1.1.0]`)
   - Display only that version's section
   - If the version is not found, report: "Version [X] not found in changelog. Available versions: [list headings]"

3. **If no argument was provided:**
   - Display the full changelog content

4. **Append a footer:**
   ```
   Current installed version: [read from template-version.json → version]
   ```

## Output
The changelog content, formatted as-is from the CHANGELOG.md file.
