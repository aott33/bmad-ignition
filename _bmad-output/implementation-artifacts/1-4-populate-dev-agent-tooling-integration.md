# Story 1.4: Populate Dev Agent Tooling Integration

Status: done

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a **Lead Ignition Developer**,
I want **the Dev agent to understand how to use ignition-lint and ignition.nvim**,
So that **the agent integrates with the validation toolchain**.

## Acceptance Criteria

1. **Given** the bmm-dev.customize.yaml file has memories and critical_actions
   **When** I add tooling integration guidance
   **Then** the memories include ignition-lint CLI usage for Perspective view validation

2. **And** the memories include ignition.nvim LSP diagnostics for real-time error detection

3. **And** the memories include the validation workflow (edit → LSP check → lint → Designer import)

4. **And** the critical_actions reference these tools appropriately

## Tasks / Subtasks

- [x] Task 1: Review existing bmm-dev.customize.yaml (AC: #1-4)
  - [x] 1.1 Read the current customize file and verify it has 30 memories (from Stories 1.1 and 1.2)
  - [x] 1.2 Verify 9 critical_actions exist (from Story 1.3)
  - [x] 1.3 Plan the tooling integration memories to add based on acceptance criteria

- [x] Task 2: Add ignition-lint tooling memories (AC: #1, #3)
  - [x] 2.1 Add memory for ignition-lint CLI invocation syntax and basic usage
  - [x] 2.2 Add memory for ignition-lint validation targets (Perspective views, scripts)
  - [x] 2.3 Add memory for interpreting ignition-lint output and common error patterns

- [x] Task 3: Add ignition.nvim tooling memories (AC: #2, #3)
  - [x] 3.1 Add memory for ignition.nvim LSP diagnostics and real-time error detection
  - [x] 3.2 Add memory for ignition.nvim Jython script validation capabilities
  - [x] 3.3 Add memory for ignition.nvim integration with the development workflow

- [x] Task 4: Add validation workflow memories (AC: #3)
  - [x] 4.1 Add memory for the complete validation workflow (edit → LSP check → lint → Designer import)
  - [x] 4.2 Add memory for the pre-commit validation checklist pattern
  - [x] 4.3 Add memory for handling validation failures and error resolution

- [x] Task 5: Review and update critical_actions (AC: #4)
  - [x] 5.1 Review existing critical_actions for tooling references
  - [x] 5.2 Add or update critical_actions to reinforce tooling integration
  - [x] 5.3 Ensure critical_actions reference the validation workflow

- [x] Task 6: Validate all additions (AC: #1-4)
  - [x] 6.1 Run YAML syntax validation with `yaml.safe_load()`
  - [x] 6.2 Verify all memories follow the rule + context format (1-3 sentences)
  - [x] 6.3 Run Architecture compliance checklist validation
  - [x] 6.4 Count total memories (should be ~39 after this story: 30 existing + ~9 new)

## Dev Notes

### Target File
**Location:** `_bmad/_config/agents/bmm-dev.customize.yaml`

### Pre-Existing Content (Stories 1.1, 1.2, and 1.3 Completed)

The customize file already contains:

**Memories (30 total):**
- **Jython 2.7 Syntax Constraints (5):** f-strings, type hints, print syntax, walrus operator, unicode handling
- **Perspective JSON Structure (4):** view.json structure, bindings, event handlers, position/custom props
- **UDT Patterns (3):** naming conventions, inheritance, parameters
- **Tag Path Structure (3):** path format, ISA-95 organization, parameter bindings
- **ISA-101 High Performance HMI (5):** gray backgrounds, information density, visual hierarchy, navigation, alarm visualization
- **ISA-18.2 Alarm Management (4):** priorities 1-4, alarm states, deadband, shelving
- **ISA-95 Equipment Hierarchy (3):** six levels, reporting/filtering, UDT design
- **ISA-88 Batch Control (3):** procedural model, equipment model, phase state machine

**Critical Actions (9 total):**
- **Validation Actions (3):** ignition-lint execution, tag path verification, Perspective JSON validation
- **Syntax Discipline Actions (3):** Jython 2.7 enforcement, print statement syntax, unicode handling
- **Safety Awareness Actions (3):** SIS configuration warnings, IT/OT boundary respect, engineering authority flagging

### Tooling Integration Content to Add

This story adds a new category of memories focused on tooling integration.

#### ignition-lint Memories (AC: #1, #3)
1. **CLI invocation:** How to run ignition-lint on Perspective view.json files
2. **Validation scope:** What ignition-lint validates (JSON structure, binding syntax, component hierarchy)
3. **Error handling:** Common error patterns and how to interpret/resolve them
4. **Expected outcomes:** >90% pass rate on first attempt (NFR-I3)

#### ignition.nvim Memories (AC: #2, #3)
5. **LSP diagnostics:** Real-time error detection for Jython scripts in Neovim
6. **Script validation:** Syntax checking and Jython 2.7 compliance
7. **Workflow integration:** How LSP diagnostics fit into the development cycle

#### Validation Workflow Memories (AC: #3)
8. **Complete workflow:** edit → LSP check → lint → Designer import sequence
9. **Pre-commit checklist:** Validation steps before considering work complete

#### Version Control Integration Memories (Added during implementation)
10. **File-based projects:** Projects can be committed directly to Git; tags/images need export
11. **ignition-git-module:** Designer integration for commits, push/pull, automated export
12. **Automated tag export:** `system.tag.exportTags()` for version control of tags

### Memory Format Requirements (from Architecture)

Each memory entry MUST follow this pattern:
- **Format:** Rule + context, 1-3 sentences
- **Code references:** Use backticks (`example`)
- **Length:** 1-3 sentences (target), 4 sentences max (rare)
- **ISA references:** Standard ID only (no years)
- **Ignition terms:** Official capitalization (Ignition, UDT, Perspective, Designer, Gateway, Jython 2.7)

**Good Examples:**
```yaml
memories:
  # Good - Rule + context, uses backticks for code
  - "Run ignition-lint using `ignition-lint path/to/view.json` to validate Perspective views. The tool checks JSON structure, binding syntax, and component hierarchy."

  # Good - Actionable workflow
  - "Follow the validation workflow: edit code, check LSP diagnostics in ignition.nvim, run ignition-lint, then import to Designer. Each step catches different error types."
```

### Critical Actions Review (AC: #4)

The existing critical_actions already include:
- "Run ignition-lint on Perspective views before marking any view or script complete"
- "Validate Perspective JSON structure matches schema before export"

These may need minor updates or additions to reinforce the tooling workflow. Consider:
- Adding workflow sequence reminder
- Reinforcing LSP check step
- Clarifying when to use each tool

### Previous Story Intelligence (Story 1.3)

**Key Learnings:**
- YAML validation with `yaml.safe_load()` confirmed valid syntax
- All items validated against Architecture compliance checklist
- Template comments must be removed before finalizing
- ~9 items per story has been the pattern for critical_actions
- ~15 memories per story for Stories 1.1 and 1.2

**Patterns Established:**
- Group related items with YAML comments (e.g., `# Tooling Integration`)
- 3-4 items per logical category
- Each item should be actionable, not just informational

**Files Modified:**
- `_bmad/_config/agents/bmm-dev.customize.yaml`

### Architecture Compliance Checklist

Before finalizing the customize file, verify:
- [x] All memories follow rule + context format (1-3 sentences)
- [x] Code references use backticks (`example`)
- [x] ISA standards use ID-only format (no years)
- [x] Ignition terms use official capitalization (Ignition, UDT, Perspective, Designer, Gateway, Jython 2.7)
- [x] Critical actions start with imperative verbs (Run, Verify, Use, Check, Note, Flag)
- [x] No Python 3 syntax in examples (no f-strings, no type hints)
- [x] Tag path examples are realistic (`[default]Site/Area/Line/Equipment/Tag`)
- [x] YAML syntax is valid (passes `yaml.safe_load()`)

### Project Structure Notes

- **Customize file location:** `_bmad/_config/agents/bmm-dev.customize.yaml`
- **Merge behavior:** `memories` section APPENDS to base agent (does not replace)
- **Merge behavior:** `critical_actions` section APPENDS to base agent (does not replace)
- **Recompile required:** After editing, run `npx bmad-method install` -> "Recompile Agents"
- Alignment with unified project structure confirmed

### Technical Stack Reference

- **Platform:** Ignition 8.3+
- **Scripting:** Jython 2.7 (NOT Python 3)
- **UI Framework:** Perspective (JSON-based views)
- **Validation Tools:**
  - **ignition-lint:** CLI tool for Perspective view validation
  - **ignition.nvim:** Neovim plugin for LSP diagnostics
- **Editor:** Neovim + ignition.nvim for LSP support

### Tooling Details

**ignition-lint:**
- CLI tool for validating Perspective view.json files
- Run: `ignition-lint path/to/view.json`
- Validates: JSON structure, binding syntax, component hierarchy
- Expected pass rate: >90% on first attempt (NFR-I3)
- Catches: Invalid JSON, missing required properties, malformed bindings

**ignition.nvim:**
- Neovim plugin providing LSP diagnostics for Ignition development
- Real-time error detection for Jython scripts
- Catches: Syntax errors, undefined variables, type mismatches (where detectable)
- Integration: Works within Neovim/LazyVim environment

**Validation Workflow:**
```
1. Edit code in Neovim with ignition.nvim active
2. Check LSP diagnostics (real-time, in-editor)
3. Run ignition-lint on Perspective views
4. Import to Designer for final validation
```

### FR/NFR Coverage

**Functional Requirements:**
- FR31: Developer can run ignition-lint to validate Perspective views before import
- FR32: ignition.nvim can provide LSP completions and diagnostics during development
- FR33: Developer can identify and correct agent errors before deployment

**Non-Functional Requirements:**
- NFR-I2: Generated Jython scripts produce zero syntax errors in ignition.nvim diagnostics
- NFR-I3: Generated Perspective views pass ignition-lint validation on first attempt (>90% target)
- NFR-E2: Critical syntax errors identifiable within editor before Gateway import attempt

### References

- [Source: _bmad-output/planning-artifacts/architecture.md#Implementation Patterns & Consistency Rules]
- [Source: _bmad-output/planning-artifacts/architecture.md#Knowledge Categories]
- [Source: _bmad-output/planning-artifacts/architecture.md#Validation Checklist]
- [Source: _bmad-output/planning-artifacts/prd.md#Validation & Quality FR30-FR33]
- [Source: _bmad-output/planning-artifacts/prd.md#NFR-Integration]
- [Source: _bmad-output/planning-artifacts/epics.md#Story 1.4]
- [Source: _bmad-output/project-context.md#Technical Context for Agents]
- [Source: _bmad-output/implementation-artifacts/1-3-populate-dev-agent-critical-actions.md#Completion Notes List]

## Dev Agent Record

### Agent Model Used

Claude Opus 4.5 (claude-opus-4-5-20251101)

### Debug Log References

None - implementation proceeded without issues.

### Completion Notes List

- Added 12 new memories to bmm-dev.customize.yaml covering tooling integration
- **ignition-lint memories (3):** CLI usage, validation targets, error interpretation
- **ignition.nvim memories (3):** LSP diagnostics, Jython validation, workflow integration
- **Validation workflow memories (3):** Clarified Gateway auto-pickup vs Designer verification, pre-commit checklist, error resolution
- **Version control memories (3):** File-based projects vs SQLite (tags/images), ignition-git-module integration, automated tag export
- Added 3 new critical_actions reinforcing the tooling workflow sequence
- Total memories increased from 30 to 42
- Total critical_actions increased from 9 to 12
- All content validated against Architecture compliance checklist
- YAML syntax validated with `yaml.safe_load()` - passed
- **Post-review update:** Clarified that Gateway auto-detects filesystem changes (no manual import for projects), added ignition-git-module for tags/images version control

### File List

- `_bmad/_config/agents/bmm-dev.customize.yaml` - Modified (added 12 memories, 3 critical_actions)
- `_bmad-output/implementation-artifacts/sprint-status.yaml` - Modified (story status updated)

## Change Log

- 2026-02-19: Story 1.4 implemented - Added tooling integration memories for ignition-lint, ignition.nvim, and validation workflow
- 2026-02-19: Self-review during implementation - Clarified Gateway filesystem monitoring (auto-detects), added ignition-git-module and version control memories (3 additional memories)
- 2026-02-19: Code review fixes - Checked Architecture Compliance Checklist, corrected File List memory count (9→12), documented scope expansion (version control memories), added sprint-status.yaml to File List, aligned terminology (auto-pickup→auto-detects)

