# Story 1.3: Populate Dev Agent Critical Actions

Status: done

## Story

As a **Lead Ignition Developer**,
I want **the Dev agent to have validation reminders and discipline rules**,
So that **the agent consistently validates output and avoids common errors**.

## Acceptance Criteria

1. **Given** the bmm-dev.customize.yaml file has memories populated
   **When** I add critical_actions
   **Then** the actions include "Run ignition-lint before marking any script complete"

2. **And** the actions include "Verify tag paths exist before creating bindings"

3. **And** the actions include "Use Jython 2.7 syntax — no f-strings, no type hints, no walrus operator"

4. **And** the actions include safety awareness notes (note when touching SIS configs, respect IT/OT boundaries)

5. **And** each action starts with an imperative verb (Run, Verify, Use, Check)

## Tasks / Subtasks

- [x] Task 1: Review existing bmm-dev.customize.yaml (AC: #1-5)
  - [x] 1.1 Read the current customize file and verify memories exist (from Stories 1.1 and 1.2)
  - [x] 1.2 Confirm the `critical_actions: []` section is present and empty
  - [x] 1.3 Plan the critical actions to add based on acceptance criteria

- [x] Task 2: Add validation critical actions (AC: #1, #2, #5)
  - [x] 2.1 Add ignition-lint validation action (must run before marking complete)
  - [x] 2.2 Add tag path verification action (verify before creating bindings)
  - [x] 2.3 Add Perspective view validation action (validate JSON structure)

- [x] Task 3: Add syntax discipline critical actions (AC: #3, #5)
  - [x] 3.1 Add Jython 2.7 syntax enforcement action (no f-strings, no type hints, no walrus operator)
  - [x] 3.2 Add print statement format action (use `print 'message'` not `print()`)
  - [x] 3.3 Add unicode handling action (use `u'string'` prefix when needed)

- [x] Task 4: Add safety awareness critical actions (AC: #4, #5)
  - [x] 4.1 Add SIS configuration warning action (note when touching safety-related configs)
  - [x] 4.2 Add IT/OT boundary awareness action (respect network segmentation)
  - [x] 4.3 Add engineering authority action (flag configs requiring human review)

- [x] Task 5: Validate all critical actions (AC: #5)
  - [x] 5.1 Verify all actions start with imperative verbs (Run, Verify, Use, Check, Note, Flag)
  - [x] 5.2 Ensure actions are concise and actionable (1-2 sentences max)
  - [x] 5.3 Run YAML syntax validation with `yaml.safe_load()`
  - [x] 5.4 Run Architecture compliance checklist validation

## Dev Notes

### Target File
**Location:** `_bmad/_config/agents/bmm-dev.customize.yaml`

### Pre-Existing Content (Stories 1.1 and 1.2 Completed)
The customize file already contains 30 memories organized into sections:
- **Platform Constraints (Story 1.1):** 15 memories
  - Jython 2.7 Syntax Constraints (5)
  - Perspective JSON Structure (4)
  - UDT Patterns (3)
  - Tag Path Structure (3)
- **ISA Standards (Story 1.2):** 15 memories
  - ISA-101 High Performance HMI (5)
  - ISA-18.2 Alarm Management (4)
  - ISA-95 Equipment Hierarchy (3)
  - ISA-88 Batch Control (3)

**The `critical_actions` section is currently an empty array `[]`.**

### Critical Actions Format Requirements (from Architecture)

Each critical action entry MUST follow this pattern:
- **Start with:** Imperative verb (Run, Verify, Use, Check, Ensure, Validate, Note, Flag)
- **Avoid:** Passive voice (should be, might be, try to)
- **Length:** 1-2 sentences, concise and actionable
- **Focus:** What the agent MUST DO

**Good Examples:**
```yaml
critical_actions:
  - "Run ignition-lint before marking any script complete"
  - "Verify tag paths exist in the Gateway before creating bindings"
  - "Use Jython 2.7 syntax — no f-strings, no type hints, no walrus operator"
```

**Bad Examples (passive or vague):**
```yaml
critical_actions:
  - "ignition-lint should be run"
  - "Tag paths are important to verify"
  - "Python 3 features should be avoided"
```

### Critical Actions Content to Add

#### Validation Actions (FR30-FR33)
1. **ignition-lint validation:** Run ignition-lint on Perspective views before marking any view or script complete
2. **Tag path verification:** Verify tag paths exist in the Gateway before creating bindings
3. **Perspective JSON validation:** Validate Perspective JSON structure matches schema before export

#### Syntax Discipline Actions (FR22, NFR-Q1-Q3)
4. **Jython 2.7 syntax:** Use Jython 2.7 syntax — no f-strings, no type hints, no walrus operator
5. **Print statement format:** Use `print 'message'` statement syntax; avoid `print()` which outputs tuples
6. **Unicode handling:** Use `u'string'` prefix for unicode literals when handling non-ASCII text

#### Safety Awareness Actions (FR41-FR43)
7. **SIS awareness:** Note when touching Safety Instrumented System (SIS) configurations — flag for engineering review
8. **IT/OT boundaries:** Respect IT/OT network boundaries — do not create connections between zones without explicit authorization
9. **Engineering authority:** Flag all alarm priority changes, interlock modifications, and safety-critical configurations for human engineering review

### Previous Story Intelligence (Story 1.2)

**Key Learnings:**
- YAML validation with `yaml.safe_load()` confirmed valid syntax
- All memories validated against Architecture compliance checklist
- Code review identified issues that were corrected
- Template comments must be removed before finalizing
- 15 memories per story has been the pattern

**Patterns Established:**
- Group related items with YAML comments (e.g., `# Validation Actions`)
- 3-4 items per logical category
- Each item should be actionable, not just informational

**Files Modified:**
- `_bmad/_config/agents/bmm-dev.customize.yaml`

### Architecture Compliance Checklist

Before finalizing the customize file, verify:
- [x] All critical actions start with imperative verbs (Run, Verify, Use, Check, Note, Flag)
- [x] All critical actions are concise (1-2 sentences)
- [x] Code references use backticks (`example`)
- [x] Ignition terms use official capitalization (Ignition, UDT, Perspective, Designer, Gateway, Jython 2.7)
- [x] No Python 3 syntax in examples
- [x] YAML syntax is valid (passes `yaml.safe_load()`)

### Project Structure Notes

- **Customize file location:** `_bmad/_config/agents/bmm-dev.customize.yaml`
- **Merge behavior:** `critical_actions` section APPENDS to base agent (does not replace)
- **Recompile required:** After editing, run `npx bmad-method install` -> "Recompile Agents"

### Technical Stack Reference

- **Platform:** Ignition 8.3+
- **Scripting:** Jython 2.7 (NOT Python 3)
- **UI Framework:** Perspective (JSON-based views)
- **Validation:** ignition-lint for Perspective views
- **Editor:** Neovim + ignition.nvim for LSP support

### Tooling Integration Context

The Dev agent needs to understand how to use these tools:

**ignition-lint:**
- CLI tool for validating Perspective view.json files
- Run: `ignition-lint path/to/view.json`
- Validates: JSON structure, binding syntax, component hierarchy
- Expected pass rate: >90% on first attempt (NFR-I3)

**ignition.nvim:**
- Neovim plugin providing LSP diagnostics for Ignition development
- Real-time error detection for Jython scripts
- Workflow: edit → LSP check → lint → Designer import

### Safety Awareness Context

**SIS (Safety Instrumented Systems):**
- Separate from process control systems
- Covered by IEC 61511 functional safety standards
- Any configuration touching SIS requires engineering authority review
- Examples: Emergency stop logic, safety interlocks, SIL-rated devices

**IT/OT Boundaries (IEC 62443):**
- OT networks are isolated from IT networks
- DMZ zones control data flow between IT and OT
- Agents should not assume connectivity between zones
- Flag any code that attempts cross-zone communication

### References

- [Source: _bmad-output/planning-artifacts/architecture.md#Critical Actions Format]
- [Source: _bmad-output/planning-artifacts/architecture.md#Implementation Patterns]
- [Source: _bmad-output/planning-artifacts/architecture.md#Knowledge Categories]
- [Source: _bmad-output/planning-artifacts/prd.md#Safety & Compliance Awareness FR41-FR43]
- [Source: _bmad-output/planning-artifacts/prd.md#Validation & Quality FR30-FR33]
- [Source: _bmad-output/planning-artifacts/epics.md#Story 1.3]
- [Source: _bmad-output/implementation-artifacts/1-2-populate-dev-agent-memories-isa-standards.md#Completion Notes List]
- [Source: _bmad-output/project-context.md#Technical Context for Agents]

## Dev Agent Record

### Agent Model Used

Claude Opus 4.5 (claude-opus-4-5-20251101)

### Debug Log References

None required - implementation straightforward.

### Completion Notes List

- **Task 1 Complete:** Reviewed bmm-dev.customize.yaml - confirmed 30 memories from Stories 1.1/1.2 present, `critical_actions: []` section ready for population
- **Tasks 2-4 Complete:** Added 9 critical actions organized into 3 categories:
  - **Validation Actions (3):** ignition-lint execution, tag path verification, Perspective JSON validation
  - **Syntax Discipline Actions (3):** Jython 2.7 enforcement, print statement syntax, unicode handling
  - **Safety Awareness Actions (3):** SIS configuration warnings, IT/OT boundary respect, engineering authority flagging
- **Task 5 Complete:** All validations passed:
  - All 9 actions start with imperative verbs (Run, Verify, Validate, Use, Note, Respect, Flag)
  - All actions are 1-2 sentences, concise and actionable
  - YAML syntax validated with `yaml.safe_load()` - passed
  - Architecture compliance checklist - all items checked
- **AC Verification:**
  - AC #1 ✅ "Run ignition-lint before marking any script complete" - added as action #1
  - AC #2 ✅ "Verify tag paths exist before creating bindings" - added as action #2
  - AC #3 ✅ "Use Jython 2.7 syntax — no f-strings, no type hints, no walrus operator" - added as action #4
  - AC #4 ✅ Safety awareness notes (SIS configs, IT/OT boundaries) - added as actions #7, #8, #9
  - AC #5 ✅ All actions start with imperative verbs - verified

### File List

- `_bmad/_config/agents/bmm-dev.customize.yaml` (modified) - Added 9 critical actions in 3 categories
- `_bmad-output/implementation-artifacts/sprint-status.yaml` (modified) - Updated story status to review

### Change Log

- 2026-02-19: Story 1.3 implementation complete - added 9 critical actions to Dev agent customize file
- 2026-02-19: Code review fixes - removed trailing periods from critical actions (architecture compliance), updated checklist, documented sprint-status.yaml change

