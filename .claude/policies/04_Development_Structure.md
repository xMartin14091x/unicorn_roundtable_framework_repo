# §4 — Development Structure, Project Organization & Planning Workflow

> **Policy reference file.** Loaded on-demand from `.claude/policies/`. Core rules live in CLAUDE.md.

---

## Project Organization

```
[PROJECT_ROOT]/
├── .claude/
│   ├── CLAUDE.md                        # Core policy file
│   ├── ProjectEnvironment.md            # Project path config, mode, source registry — fill on setup
│   ├── policies/                        # Detailed policy reference files (loaded on-demand)
│   ├── Team Roster/                     # Team definitions
│   │   ├── 1. Team_Overseer.md
│   │   ├── 2. Team_Monolith.md
│   │   ├── 3. Team_Syndicate.md
│   │   ├── 4. Team_Arcade.md
│   │   └── 5. Team_Cipher.md
│   ├── Team Chat/                       # Inter/intra team logs
│   │   ├── 1. Monolith/
│   │   │   ├── DD-MM-YYYY_Monolith.md
│   │   │   └── HandOver/
│   │   ├── 2. Syndicate/
│   │   │   ├── DD-MM-YYYY_Syndicate.md
│   │   │   └── HandOver/
│   │   ├── 3. Arcade/
│   │   │   ├── DD-MM-YYYY_Arcade.md
│   │   │   └── HandOver/
│   │   └── 4. OverseerReport/
│   │       └── DD-MM-YYYY_OverseerReport.md
│   └── Diagnostic Log/                 # Cipher engagement records
│       └── [NUMBER]. [TASK]_DD_MM_YYYY.md
├── RoundTable/
│   ├── _Index.md                        # Volume navigation index
│   └── DD-MM-YYYY_RoundTable.md         # Daily session logs (with Volume system)
└── Development/                         # Planning & documentation
    └── (see Project Structure Mode below)
```

---

## Project Structure Mode

Every project operates in one of two modes. The active mode is declared in `.claude/ProjectEnvironment.md → PROJECT_MODE`.

### Centralized Mode

Planning and source code share the same project root. Use for greenfield projects or projects you own end-to-end.

**Development folder shape — no pre-existing codebase:**
```
Development/
├── 01_Implementation Logs/
├── 02_FeatureDescription/
├── 03_SubFeatures Implementation/
├── 04_Modification Logs/
├── 05_BugFixesLog/
├── 06_InstallationGuide/
├── 07_TechnicalDebt/
├── Current TechStack.md
├── Main Implementation Plan.md
└── Main TechStack Logic.md
```

**Development folder shape — WITH pre-existing codebase:**
```
Development/
└── [ProjectName]/
    ├── 01_Implementation Logs/
    ├── 02_FeatureDescription/
    ├── 03_SubFeatures Implementation/
    ├── 04_Modification Logs/
    ├── 05_BugFixesLog/
    ├── 06_InstallationGuide/
    ├── 07_TechnicalDebt/
    ├── Current TechStack.md
    ├── Main Implementation Plan.md
    ├── Main TechStack Logic.md
    └── PreExisting TechStack/
        └── [ProjectName].md
```

Source code location (Centralized):
```
[PROJECT_ROOT]/
├── .claude/
├── Development/
├── RoundTable/
└── [src-folder]/                         # Source code lives here, inside the same root
```

### Decentralized Mode

Planning hub and source code are fully separated. Use for pre-existing codebases, client repos, multi-repo projects, or any codebase you cannot restructure.

**Development folder shape (always named subfolder):**
```
Development/
└── [ProjectName]/
    ├── 01_Implementation Logs/
    ├── 02_FeatureDescription/
    ├── 03_SubFeatures Implementation/
    ├── 04_Modification Logs/
    ├── 05_BugFixesLog/
    ├── 06_InstallationGuide/
    ├── 07_TechnicalDebt/
    ├── Current TechStack.md
    ├── Main Implementation Plan.md
    ├── Main TechStack Logic.md
    └── PreExisting TechStack/
        └── [ProjectName].md
```

Source code location (Decentralized):
```
[PROJECT_ROOT]/          ← Claude Code root — planning only
[EXTERNAL_PATH]/         ← Source code lives here, outside the root entirely
                           Record this path in ProjectEnvironment.md → External Source Code Registry
```

### Mode Decision Table

| Situation | Mode |
|-----------|------|
| Greenfield project, you own the repo end-to-end | Centralized |
| Pre-existing codebase you cannot restructure | Decentralized |
| Client repo or external repo | Decentralized |
| Multiple repos under one planning hub | Decentralized |
| Source code has sensitive history (no planning docs in git) | Decentralized |
| Simple solo project | Centralized |

### Pre-Existing Codebase Rule (applies to BOTH modes)

**Whenever a pre-existing codebase is involved — regardless of mode — use the named project subfolder shape.** The `Development/[ProjectName]/` layer is mandatory when a PreExisting TechStack scan file exists or will be created.

### PreExisting TechStack Path Formula

```
Development/[ProjectName]/PreExisting TechStack/[ProjectName].md
```

This formula applies in both Centralized (with pre-existing codebase) and Decentralized mode. There is no flat `Development/PreExisting TechStack/` path — the named project subfolder is always required.

---

## Planning-First Workflow (Development Folder Policy)

### CRITICAL RULE: Plan Before Implementation

When Commander ท่านผู้บัญชาการ requests ANY implementation work (fix a bug, add a feature, modify existing code), you MUST:

1. **Create a plan document FIRST** in the appropriate Development folder
2. **Wait for explicit Commander ท่านผู้บัญชาการ confirmation** ("approved", "proceed", "go ahead", "yes") before implementing
3. **Never implement without approval** — no matter how simple the task appears

### Development Folder Structure (Standard)

This is the **canonical template** for the contents of a project subfolder. The shape of `Development/` itself depends on the active mode — see **Project Structure Mode** section above.

> In Centralized mode with no pre-existing codebase: these folders sit directly inside `Development/`.
> In all other cases (Decentralized, or Centralized with pre-existing codebase): these folders sit inside `Development/[ProjectName]/`.

```
[ProjectName]/
├── 01_Implementation Logs/           # Tickets, briefings, and completed implementation logs
│   └── [VERSION]/                    # e.g. INDEV v1.0.0
│       ├── Phase 0/
│       │   ├── Monolith_Phase0_Briefing.md
│       │   ├── Syndicate_Phase0_Briefing.md
│       │   ├── Arcade_Phase0_Briefing.md
│       │   ├── 1. Overseer/
│       │   ├── 2. Monolith/
│       │   ├── 3. Syndicate/
│       │   └── 4. Arcade/
│       ├── Phase 1/
│       │   └── (same structure)
│       └── Phase N/
│           └── (same structure)
├── 02_FeatureDescription/
│   └── [FeatureName]/
│       └── [ComponentName].md
├── 03_SubFeatures Implementation/
│   └── [FeatureName]/
│       ├── [ORDER]. [STATUS]_[Name]_DD-MM-YYYY.md
│       └── [STATUS]_[Name]_DD-MM-YYYY/
│           ├── 1. Overseer/
│           ├── 2. Monolith/
│           ├── 3. Syndicate/
│           └── 4. Arcade/
├── 04_Modification Logs/
│   └── [FeatureName]/
│       ├── [ORDER]. [STATUS]_[Name]_DD-MM-YYYY.md
│       └── [STATUS]_[Name]_DD-MM-YYYY/
│           ├── 1. Overseer/
│           ├── 2. Monolith/
│           ├── 3. Syndicate/
│           └── 4. Arcade/
├── 05_BugFixesLog/
│   └── [FeatureName]/
│       ├── [ORDER]. [STATUS]_[Name]_DD-MM-YYYY.md
│       └── [STATUS]_[Name]_DD-MM-YYYY/
│           ├── 1. Overseer/
│           ├── 2. Monolith/
│           ├── 3. Syndicate/
│           └── 4. Arcade/
├── 06_InstallationGuide/             # Setup guides, installation docs
├── 07_TechnicalDebt/                 # Temporary or hacky code fixes
├── Current TechStack.md              # Living doc: all classes, methods, functions
├── Main Implementation Plan.md       # READ-ONLY core roadmap
├── Main TechStack Logic.md           # READ-ONLY tech documentation
└── PreExisting TechStack/            # (Pre-existing projects only)
    └── [ProjectName].md
```

---

### Request Type → Destination Folder

| Commander's Request | Destination Folder |
|--------------|-------------------|
| "Fix bug in X" | `05_BugFixesLog/[FeatureName]/` |
| "Add feature X" | `03_SubFeatures Implementation/` |
| "Modify/expand X" | `04_Modification Logs/[FeatureName]/` |
| "Document X" | `02_FeatureDescription/[FeatureName]/` |

### Naming Conventions

**Implementation Logs & SubFeatures:**
```
[ORDER]. [FeatureName]_DD-MM-YYYY.md
```
Example: `01. RomanceSystem_04-02-2026.md`

**SubFeature, Modification & Bug Fix Logs (Main Overview + Tickets):**

Main overview file (brief description of job scope and ticket list):
```
[ORDER]. [STATUS]_[Name]_DD-MM-YYYY.md
```

Tickets folder (detailed per-task tickets divided into 4 teams):
```
[STATUS]_[Name]_DD-MM-YYYY/
├── 1. Overseer/    # Integration + deployment tickets
├── 2. Monolith/    # Core pipeline / infrastructure tickets
├── 3. Syndicate/   # Backend / API / process management tickets
└── 4. Arcade/      # UI / prompts / formatting tickets
```

- STATUS: `PLANNED` or `IMPLEMENTED` (both file AND folder rename on completion)
- Main overview is **brief** — describes what the job is about and lists tickets
- Tickets contain **detailed** per-task specs with acceptance criteria
- Ticket naming: `[TEAM_PREFIX]-[NUMBER]_[ShortName].md` (e.g., `MON-01_ParseOverseerOutput.md`)
- Example overview: `04. PLANNED_ParallelExecution_11-02-2026.md`
- Example folder: `PLANNED_ParallelExecution_11-02-2026/`

### Main Document Rules

**Main Implementation Plan.md** and **Main TechStack Logic.md** are **READ-ONLY**.

**Allowed actions:** Update checkboxes/status only.

**If changes needed:** Create `Indev Implementation Plan.md` or `Indev TechStack.md` with proposed changes and get Commander ท่านผู้บัญชาการ approval before merging.

**Current TechStack.md** is a **living document** — document all Classes, Methods, and Functions created in code with explanation here.

### Workflow Steps

1. Identify request type and destination folder
2. Determine next ORDER number in target folder
3. Create **main overview file** with `PLANNED` status (brief description + ticket list)
4. Create **tickets folder** (`PLANNED_[Name]_DD-MM-YYYY/`) with team subfolders
5. Create **individual tickets** in team subfolders (detailed specs + acceptance criteria)
6. Present plan location to user and ask for approval
7. **WAIT** for Commander ท่านผู้บัญชาการ to approve
8. After approval: implement and rename `PLANNED` → `IMPLEMENTED` (both file and folder)
9. Log in `01_Implementation Logs/` if major feature
10. Update `Current TechStack.md` with new classes/methods/functions

### Source Code Structure

For projects with multiple sub-projects (e.g., server, extension, dashboard):

```
[ProjectName]/
├── Development/                     # Planning & documentation
├── Projects/                        # Actual source code
│   ├── [component-1]/
│   ├── [component-2]/
│   └── [shared]/
└── .stignore / .gitignore
```

| Project Type | Use Projects/ Folder? |
|--------------|----------------------|
| Single codebase (one language/framework) | No — put source at root |
| Multi-component (server + client + shared) | **Yes** |
| Monorepo with distinct apps | **Yes** |
| Simple scripts or utilities | No |

### 06_InstallationGuide Contents

Store setup and installation documentation:
- Environment setup instructions
- Dependency installation guides
- Configuration procedures
- Deployment documentation

### 07_TechnicalDebt

Store all temporary or hacky code fixes that need future cleanup.

---

*Extracted from CLAUDE.md — Project Organization, Structure Mode, Planning-First Workflow, Development Structure — 11-03-2026*
