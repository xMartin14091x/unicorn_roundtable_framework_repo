# ProjectEnvironment

> Active project registry. Check this file before constructing any Development folder path or PreExisting TechStack path.
> Maintained by AM (Overseer). Update when a project is added, put on hold, or its source path changes.

---

## Field Reference

| Field | Description |
|-------|-------------|
| **PROJECT_MODE** | `Centralized` = planning docs and source code share the same root (greenfield/solo projects). `Decentralized` = planning hub and source code are in separate locations (pre-existing codebases, client repos). |
| **PROJECT_ROOT** | The top-level directory of your project. All Development folders and RoundTable logs are created relative to this path. |
| **SOURCE_ROOT** | Where the actual source code lives. Same as PROJECT_ROOT in Centralized mode. Different in Decentralized mode (e.g., a separate repo or subfolder). |
| **ACTIVE** | `true` = currently being worked on. `false` = on hold (do not delete entries, just set to false). |
| **Notes** | Free-text field for tracking significant project state changes. |

---

## Active Projects

<!-- EXAMPLE (remove or replace with your actual project):

### MyWebApp
**PROJECT_MODE:** Centralized
**PROJECT_ROOT:** `d:/Projects/MyWebApp`
**SOURCE_ROOT:** `d:/Projects/MyWebApp`
**ACTIVE:** true
**Notes:** Main web application. Started 01-03-2026.

### ClientPortal
**PROJECT_MODE:** Decentralized
**PROJECT_ROOT:** `d:/Projects/ClientPortal`
**SOURCE_ROOT:** `d:/ClientRepo/portal-frontend`
**ACTIVE:** true
**Notes:** Client's existing React app. Planning docs in PROJECT_ROOT, source code in client's repo.
-->

### [Your Project Name]
**PROJECT_MODE:** Centralized
**PROJECT_ROOT:** `[full path to your project root]`
**SOURCE_ROOT:** `[full path to your source code]`
**ACTIVE:** true
**Notes:** Replace this entry with your actual project details.

---

## Rules

- Check this file before constructing any Development folder path or PreExisting TechStack path
- If a project is not listed here, add it before beginning work
- `SOURCE_ROOT` and `PROJECT_ROOT` are the same in Centralized mode with no sub-components; different in Decentralized mode
- Set `ACTIVE: false` for projects that are on hold — do not delete entries
- Update the Notes field when significant project state changes (new deployment, mode change, source path change)
