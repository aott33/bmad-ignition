# Story 3.2: Populate SM Agent — Parallel Execution Coordination

Status: done

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a **Lead Ignition Developer**,
I want **the SM agent to coordinate parallel development without file conflicts**,
So that **multiple dev agents can work on the same project simultaneously**.

## Acceptance Criteria

1. **Given** the bmm-sm.customize.yaml file exists
   **When** I add memories for parallel execution coordination
   **Then** the memories include file isolation rules (one agent per view, one agent per UDT)

2. **And** the memories include story naming conventions (`story-NNN-slug.md`)

3. **And** the memories include merge strategy awareness (avoid JSON merge conflicts)

4. **And** the memories include Ignition project structure awareness (separate views, tags, scripts)

5. **And** critical_actions include "Verify no file overlap before assigning parallel stories"

## Tasks / Subtasks

- [x] Task 1: Prepare bmm-sm.customize.yaml (AC: #1-5)
  - [x] 1.1 Read current bmm-sm.customize.yaml to understand existing structure
  - [x] 1.2 Remove empty placeholder sections (agent/persona if empty)
  - [x] 1.3 Add proper header comment following established pattern from other customize files

- [x] Task 2: Add File Isolation Rule Memories (AC: #1)
  - [x] 2.1 Add memory: Perspective view file isolation (one agent per view folder)
  - [x] 2.2 Add memory: UDT definition isolation (one agent per UDT definition file)
  - [x] 2.3 Add memory: Script file isolation (one agent per script resource)
  - [x] 2.4 Add memory: Tag folder isolation (one agent per equipment hierarchy branch)

- [x] Task 3: Add Story Naming Convention Memories (AC: #2)
  - [x] 3.1 Add memory: Story file naming pattern (`{epic_num}-{story_num}-{slug}.md`)
  - [x] 3.2 Add memory: Story artifact isolation (each story produces distinct output files)
  - [x] 3.3 Add memory: Branch naming conventions for parallel work

- [x] Task 4: Add Merge Strategy Memories (AC: #3)
  - [x] 4.1 Add memory: JSON merge conflict prevention (Perspective views are JSON)
  - [x] 4.2 Add memory: Tag XML merge considerations
  - [x] 4.3 Add memory: Script merge strategies (line-level conflicts)
  - [x] 4.4 Add memory: Git workflow for parallel development

- [x] Task 5: Add Ignition Project Structure Memories (AC: #4)
  - [x] 5.1 Add memory: Perspective project folder structure (views/, resources/, etc.)
  - [x] 5.2 Add memory: Tag provider folder organization (ISA-95 aligned)
  - [x] 5.3 Add memory: Script project structure (library scripts, event scripts)
  - [x] 5.4 Add memory: Named query organization

- [x] Task 6: Add Critical Actions (AC: #5)
  - [x] 6.1 Add critical action: Verify file overlap before parallel assignment
  - [x] 6.2 Add critical action: Document file ownership in story assignments
  - [x] 6.3 Add critical action: Check for merge conflicts before story completion
  - [x] 6.4 Add critical action: Use feature branches for parallel work

- [x] Task 7: Validate and Test (AC: #1-5)
  - [x] 7.1 Verify YAML syntax is valid
  - [x] 7.2 Confirm memories follow architecture patterns (rule + context, 1-3 sentences)
  - [x] 7.3 Confirm critical actions use imperative verbs
  - [x] 7.4 Run `npx bmad-method install` → "Recompile Agents" to verify compilation

## Dev Notes

### Story Purpose

This story populates the SM (Scrum Master) agent with Ignition-specific knowledge for coordinating parallel development. The SM agent's role is to manage story assignment and execution such that multiple dev agents can work on the same Ignition project without file conflicts.

**Key Challenge:** Ignition projects consist of JSON files (Perspective views), XML files (tags, UDTs), and Jython scripts. These file formats have specific merge conflict characteristics that the SM agent must understand to coordinate parallel work safely.

### Why Parallel Execution Matters for Ignition Projects

From the Architecture doc and PRD:
- FR34: Multiple dev agents can work on separate Perspective views without file conflicts
- FR35: Developer can run parallel stories during the same sprint
- FR36: Version control can track changes across parallel development streams

**The Problem:**
- Perspective views are single JSON files — two agents editing the same view = merge nightmare
- UDT definitions are interdependent — changing a parent UDT affects all children
- Tag folders can have implicit dependencies through UDT parameters
- Scripts may share library functions — concurrent edits cause conflicts

**The Solution:**
The SM agent must understand Ignition's file structure to assign stories that operate on non-overlapping files.

### Architecture Compliance

Per [architecture.md#Memory Formatting](../_bmad-output/planning-artifacts/architecture.md):
- Memory format: Rule + context, 1-3 sentences, backticks for code
- ISA references: Standard ID only (no years)
- Ignition terminology: Ignition, UDT, Perspective, Designer, Gateway, Jython 2.7
- Critical actions: Imperative verbs, action-first

### Ignition Project File Structure

**Perspective Views:**
```
com.inductiveautomation.perspective/views/
├── MainNavigation/
│   └── view.json
├── Overview/
│   └── view.json
├── EquipmentDetail/
│   └── view.json
└── AlarmSummary/
    └── view.json
```
Each view is a **single JSON file** — assign one view folder per agent.

**Tags/UDTs:**
```
tags/
└── default/
    └── Site/
        ├── Area1/
        │   ├── Line1/
        │   │   └── Cell1/
        │   │       └── Equipment1/  # Agent A owns this branch
        │   └── Line2/               # Agent B can own this branch
        └── UDTs/
            ├── Tank.json           # One agent per UDT definition
            └── Motor.json
```
Assign tag folder branches or UDT definitions to avoid overlap.

**Scripts:**
```
ignition/script-python/
├── project/
│   ├── equipment.py      # Library script — assign to one agent
│   ├── reporting.py      # Another library script
│   └── alarms.py
└── shared/
    └── utils.py          # Shared — careful with ownership
```

### Previous Story Intelligence

**From Epic 1 (Dev Agent Pilot):**
- Dev agent has memories about Perspective JSON structure, UDT patterns, tag paths
- Validation workflow: LSP → ignition-lint → Gateway → Designer
- File-based development with Git version control

**From Epic 2 (Planning Agents):**
- ISA-95 hierarchy structure: Site → Area → Line → Cell → Equipment
- Story files use `{epic_num}-{story_num}-{slug}.md` naming
- Customize file pattern: header comment, critical_actions section, memories section

**From Story 2.4 (Validation):**
- Static validation confirmed memories and critical actions are picked up by compiled agents
- Memory formatting patterns work correctly

### File Isolation Rules Summary

| File Type | Isolation Unit | Example |
|-----------|----------------|---------|
| Perspective View | View folder | `views/TankOverview/` |
| UDT Definition | Single UDT file | `UDTs/Tank.json` |
| Tag Folder | ISA-95 hierarchy branch | `Site/Area1/Line1/` |
| Script | Single script file | `project/equipment.py` |
| Named Query | Query folder | `queries/Equipment/` |

### References

- [Source: _bmad-output/planning-artifacts/epics.md#Story 3.2]
- [Source: _bmad-output/planning-artifacts/architecture.md#Story Artifacts]
- [Source: _bmad-output/planning-artifacts/architecture.md#Memory Formatting]
- [Source: _bmad-output/planning-artifacts/prd.md#FR34-FR36]
- [Source: _bmad-output/project-context.md#Technical Context for Agents]
- [Source: _bmad/_config/agents/bmm-dev.customize.yaml] (reference for memory patterns)
- [Source: _bmad/_config/agents/bmm-pm.customize.yaml] (reference for customize file structure)

## Dev Agent Record

### Agent Model Used

Claude Opus 4.5 (claude-opus-4-5-20251101)

### Debug Log References

### Completion Notes List

- Populated SM agent with 15 memories covering file isolation rules, story naming conventions, merge strategies, and Ignition project structure
- Added 4 critical actions for parallel execution coordination
- All memories follow architecture patterns (rule + context, 1-3 sentences, backticks for code)
- All critical actions use imperative verbs
- YAML syntax validated, agent recompilation successful

### Change Log

- 2026-02-19: Implemented story 3-2 - SM agent parallel execution coordination
- 2026-02-20: Code review completed - fixed 2 medium issues (added sm.md to File List, added backticks to file paths in memories)

## Senior Developer Review (AI)

**Reviewer:** Claude Opus 4.5
**Date:** 2026-02-20
**Outcome:** ✅ APPROVED

### Review Summary

| Category | Result |
|----------|--------|
| AC#1: File isolation rules | ✅ PASS - 4 memories covering views, UDTs, scripts, tags |
| AC#2: Story naming conventions | ✅ PASS - Memory matches actual implementation pattern |
| AC#3: Merge strategy awareness | ✅ PASS - 4 memories covering JSON, XML, scripts, Git workflow |
| AC#4: Ignition project structure | ✅ PASS - 4 memories covering Perspective, tags, scripts, queries |
| AC#5: Critical actions | ✅ PASS - 4 critical actions with imperative verbs |

### Issues Found & Fixed

**Medium Issues (2):**
1. ✅ FIXED: `_bmad/bmm/agents/sm.md` modified but not in File List → Added to File List
2. ✅ FIXED: Missing backticks on `UDTs/` folder path in memory → Added backticks

**Low Issues (3) - Accepted:**
- Story naming convention differs from Architecture doc dairy-pilot examples (different context, both valid)
- Redundant memories (minor verbosity, no functional impact)
- HTML entities in compiled agent XML (expected behavior)

### Architecture Compliance

- ✅ Memory format: Rule + context, 1-3 sentences
- ✅ Code references: Backticks used appropriately
- ✅ ISA standards: ID-only format (ISA-95, not ISA-95:2013)
- ✅ Ignition terms: Official capitalization (Perspective, UDT, Jython, Gateway)
- ✅ Critical actions: Imperative verbs (Verify, Document, Check, Require)

### File List

- _bmad/_config/agents/bmm-sm.customize.yaml (modified)
- _bmad/bmm/agents/sm.md (modified - auto-generated by agent recompilation)
