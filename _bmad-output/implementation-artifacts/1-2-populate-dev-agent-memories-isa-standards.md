# Story 1.2: Populate Dev Agent Memories — ISA Standards

Status: done

## Story

As a **Lead Ignition Developer**,
I want **the Dev agent to understand ISA standards relevant to implementation**,
So that **generated views, alarms, and tag structures align with industrial best practices**.

## Acceptance Criteria

1. **Given** the bmm-dev.customize.yaml file has platform constraint memories
   **When** I add memories for ISA standards
   **Then** the memories include ISA-101 High Performance HMI principles for view implementation

2. **And** the memories include ISA-18.2 alarm configuration patterns (priority levels 1-4, states, deadband)

3. **And** the memories include ISA-95 hierarchy awareness for tag path structure

4. **And** the memories include ISA-88 batch pattern recognition (applied when project involves batch processes)

5. **And** ISA references use standard ID only (no years)

## Tasks / Subtasks

- [x] Task 1: Review existing bmm-dev.customize.yaml (AC: #1-5)
  - [x] 1.1 Read the current customize file and verify platform constraint memories exist (from Story 1.1)
  - [x] 1.2 Identify the insertion point for ISA standards memories (append after platform constraints)

- [x] Task 2: Add ISA-101 High Performance HMI memories (AC: #1, #5)
  - [x] 2.1 Add memory for ISA-101 gray background principle (color for abnormal only)
  - [x] 2.2 Add memory for ISA-101 information density and visual hierarchy
  - [x] 2.3 Add memory for ISA-101 navigation patterns for operators
  - [x] 2.4 Add memory for ISA-101 alarm visualization principles

- [x] Task 3: Add ISA-18.2 alarm configuration memories (AC: #2, #5)
  - [x] 3.1 Add memory for ISA-18.2 priority levels (1=Critical, 2=High, 3=Medium, 4=Low)
  - [x] 3.2 Add memory for ISA-18.2 alarm states (Active, Acknowledged, Cleared, Suppressed)
  - [x] 3.3 Add memory for ISA-18.2 deadband configuration
  - [x] 3.4 Add memory for ISA-18.2 shelving and rationalization concepts

- [x] Task 4: Add ISA-95 hierarchy memories (AC: #3, #5)
  - [x] 4.1 Add memory for ISA-95 equipment hierarchy levels (Enterprise → Site → Area → Line → Cell → Equipment Module)
  - [x] 4.2 Add memory for ISA-95 application to Ignition tag organization
  - [x] 4.3 Add memory for ISA-95 hierarchy in UDT design patterns

- [x] Task 5: Add ISA-88 batch pattern memories (AC: #4, #5)
  - [x] 5.1 Add memory for ISA-88 procedural control concepts (recipes, phases, operations)
  - [x] 5.2 Add memory for ISA-88 equipment module patterns
  - [x] 5.3 Add memory for when ISA-88 applies (batch processes: food processing, pharma, chemicals)

- [x] Task 6: Validate all ISA memories (AC: #5)
  - [x] 6.1 Verify all ISA references use ID-only format (ISA-101, not ISA-101:2015)
  - [x] 6.2 Ensure all memories follow rule + context format (1-3 sentences)
  - [x] 6.3 Ensure backticks used for all code references
  - [x] 6.4 Run Architecture compliance checklist validation

## Dev Notes

### Target File
**Location:** `_bmad/_config/agents/bmm-dev.customize.yaml`

### Pre-Existing Content (Story 1.1 Completed)
The customize file already contains 15 platform constraint memories organized into sections:
- Jython 2.7 Syntax Constraints (5 memories)
- Perspective JSON Structure (4 memories)
- UDT Patterns (3 memories)
- Tag Path Structure (3 memories)

**Add ISA standards memories AFTER the existing platform constraint memories.**

### Memory Format Requirements (from Architecture)

Each memory entry MUST follow this pattern:
- **Format:** Rule + context, 1-3 sentences per memory
- **Code:** Use backticks for code references
- **ISA references:** Use standard ID only, NO years (ISA-101, not ISA-101:2015)
- **Avoid:** Vague statements like "Follow ISA standards"
- **Avoid:** Entries exceeding 3 sentences

**Good Example:**
```yaml
memories:
  - "Apply ISA-101 High Performance HMI principles: use neutral gray backgrounds and reserve bright colors only for abnormal conditions. This reduces operator fatigue and makes alarms immediately visible."
```

**Bad Example (includes year, too vague):**
```yaml
memories:
  - "Follow ISA-101:2015 HMI guidelines"
```

### ISA Standards Content to Encode

#### ISA-101: High Performance HMI (PRD: FR17-FR19, FR24)
- Gray/neutral backgrounds reduce operator fatigue
- Color reserved for abnormal conditions only (alarms, faults)
- Information density: show only what operators need
- Visual hierarchy: critical information prominently displayed
- Navigation: hierarchy-based, role-appropriate access
- Trending and historical context readily accessible

#### ISA-18.2: Alarm Management (PRD: FR27)
- Priority levels:
  - Priority 1 (Critical): Immediate response required, safety impact
  - Priority 2 (High): Urgent response within minutes
  - Priority 3 (Medium): Response within shift
  - Priority 4 (Low): Awareness, no immediate action
- Alarm states: Active (unacknowledged), Acknowledged, Cleared, Suppressed
- Deadband: prevents nuisance alarms on noisy signals
- Shelving: temporary suppression during known conditions
- Rationalization: documented justification for each alarm

#### ISA-95: Equipment Hierarchy (PRD: FR12, Architecture)
- Levels: Enterprise → Site → Area → Line → Cell → Equipment Module
- Maps directly to Ignition tag paths: `[default]Site/Area/Line/Cell/Equipment/Tag`
- UDT design should reflect this hierarchy
- Enables filtering, reporting, and alarm management by area

#### ISA-88: Batch Control (PRD: FR15)
- Applies to batch processes: food & beverage, pharmaceuticals, chemicals
- Procedural model: Recipe → Procedure → Unit Procedure → Operation → Phase
- Equipment model: Process Cell → Unit → Equipment Module → Control Module
- State machine patterns for phases (Idle, Running, Paused, Complete, Aborted)
- Recipe parameters drive equipment behavior

### Previous Story Intelligence (Story 1.1)

**Key Learnings:**
- Memory count validation performed: 15 memories total in Story 1.1
- YAML validation with `yaml.safe_load()` confirmed valid syntax
- All memories validated against Architecture compliance checklist
- Code review identified issues with print statement explanation (corrected)
- Code review identified issues with UDT inheritance explanation (corrected)
- Template comments must be removed before finalizing

**Patterns Established:**
- Group related memories with YAML comments (e.g., `# Jython 2.7 Syntax Constraints`)
- 3-5 memories per logical category is the sweet spot
- Each memory should be actionable, not just informational

**Files Modified:**
- `_bmad/_config/agents/bmm-dev.customize.yaml`

### Architecture Compliance Checklist

Before finalizing the customize file, verify:
- [x] All memories follow rule + context format (1-3 sentences)
- [x] Code references use backticks (`example`)
- [x] ISA standards use ID-only format (NO years)
- [x] Ignition terms use official capitalization (Ignition, UDT, Perspective, Designer, Gateway, Jython 2.7)
- [x] No Python 3 syntax in examples
- [x] Tag path examples are realistic (`[default]Site/Area/Line/Equipment/Tag`)

### Project Structure Notes

- **Customize file location:** `_bmad/_config/agents/bmm-dev.customize.yaml`
- **Merge behavior:** `memories` section APPENDS to base agent (does not replace)
- **Recompile required:** After editing, run `npx bmad-method install` -> "Recompile Agents"

### Technical Stack Reference

- **Platform:** Ignition 8.3+
- **Scripting:** Jython 2.7 (NOT Python 3)
- **UI Framework:** Perspective (JSON-based views)
- **Validation:** ignition-lint for Perspective views
- **Editor:** Neovim + ignition.nvim for LSP support

### References

- [Source: _bmad-output/planning-artifacts/architecture.md#Memory Formatting]
- [Source: _bmad-output/planning-artifacts/architecture.md#ISA Content Strategy]
- [Source: _bmad-output/planning-artifacts/architecture.md#Implementation Patterns]
- [Source: _bmad-output/planning-artifacts/prd.md#Domain-Specific Requirements]
- [Source: _bmad-output/planning-artifacts/prd.md#ISA Standards Implementation]
- [Source: _bmad-output/planning-artifacts/epics.md#Story 1.2]
- [Source: _bmad-output/implementation-artifacts/1-1-populate-dev-agent-memories-platform-constraints.md#Completion Notes List]

## Dev Agent Record

### Agent Model Used

Claude Opus 4.5 (claude-opus-4-5-20251101)

### Debug Log References

None - implementation proceeded without errors.

### Completion Notes List

- Verified existing platform constraint memories (15) from Story 1.1 in `bmm-dev.customize.yaml`
- Added 15 new ISA standards memories organized into 4 sections:
  - **ISA-101 High Performance HMI** (5 memories): gray backgrounds, information density, visual hierarchy, navigation patterns, alarm visualization
  - **ISA-18.2 Alarm Management** (4 memories): priority levels 1-4, alarm states, deadband configuration, shelving/rationalization
  - **ISA-95 Equipment Hierarchy** (3 memories): hierarchy levels, tag organization, UDT design patterns
  - **ISA-88 Batch Control** (3 memories): procedural model, equipment model, phase state machine pattern
- Total memories in customize file: 30 (15 platform + 15 ISA)
- YAML syntax validated with `yaml.safe_load()`
- All ISA references use ID-only format (no year suffixes)
- All memories follow rule + context format (1-3 sentences)
- All code references use backticks
- Architecture compliance checklist passed

### Change Log

- 2026-02-19: Added 15 ISA standards memories to `bmm-dev.customize.yaml` (ISA-101, ISA-18.2, ISA-95, ISA-88)
- 2026-02-19: Code Review - Fixed 4 MEDIUM issues: added dev.md to File List, checked off Architecture Compliance Checklist, differentiated ISA-95 memories from Tag Path section, clarified ISA-88 conditional application

### File List

- `_bmad/_config/agents/bmm-dev.customize.yaml` (modified)
- `_bmad/bmm/agents/dev.md` (auto-compiled from customize.yaml)
