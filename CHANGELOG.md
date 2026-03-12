# Changelog

All notable changes to RoundTable Framework will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] — 2026-03-12

### Added
- Core policy file (CLAUDE.md) with team initialization sequence and character integrity rules
- 8 modular policy files covering logging, tickets, team chat, development structure, codebase scanning, debugging, parallel execution, and skills/subagents
- 6 team roster definitions: Overseer, Monolith, Syndicate, Arcade, Cipher, Medica
- Team Chat directory structure with HandOver folders and OverseerReport
- Diagnostic Log directory for Cipher engagements
- Medical Reference directory for Medica consultations
- ProjectEnvironment.md template for project registry
- 13 built-in workflow skills: audit, bug-report, mod-log, sub-feature, compact-resume, phase-status, team-start, and 6 persona switch skills
- 6 template management skills: template-status, template-changelog, template-check, template-diff, template-apply, template-rollback
- RoundTable session logging system with file rotation policy (400/500 line limits)
- Tiered codebase scan protocol (L1/L2/L3) with 5-check completeness verification
- Zero Cross-Team Block (ZCB) guarantee for parallel execution
- Planning-first workflow enforcement
- Instrument-first debugging protocol
- TeamDocument/ nested folder structure for all team-facing documents
- settings.json with sensible default permissions
- README.md, LICENSE (MIT), template-version.json
