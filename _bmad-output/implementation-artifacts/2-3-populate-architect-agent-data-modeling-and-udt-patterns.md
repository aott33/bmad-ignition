# Story 2.3: Populate Architect Agent — Data Modeling and UDT Patterns

Status: done

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a **Lead Ignition Developer**,
I want **the Architect agent to design ISA-compliant hierarchies and Ignition-specific data models**,
So that **architecture documents produce implementable UDT structures and tag hierarchies**.

## Acceptance Criteria

1. **Given** the bmm-architect.customize.yaml file exists
   **When** I add memories for data modeling and UDT patterns
   **Then** the memories include ISA-95 hierarchy for system architecture design

2. **And** the memories include UDT structure best practices (inheritance, parameters, naming conventions)

3. **And** the memories include database schema patterns for equipment metadata (when applicable)

4. **And** the memories include ISA-88 batch/procedural patterns (when project involves batch processes)

5. **And** the memories include tag path structure guidance aligned with ISA-95

6. **And** critical_actions include safety awareness (note SIS configurations, engineering authority required)

## Tasks / Subtasks

- [x] Task 1: Review Current Customize File State (AC: #1-6)
  - [x] 1.1 Read bmm-architect.customize.yaml and confirm it's empty/template
  - [x] 1.2 Review Architecture doc for memory format patterns
  - [x] 1.3 Review Epic 1 Dev agent memories for established patterns
  - [x] 1.4 Review Story 2.1 Analyst agent structure for Epic 2 patterns

- [x] Task 2: Add ISA-95 Hierarchy Memories (AC: #1, #5)
  - [x] 2.1 Add memory for ISA-95 six-level equipment hierarchy model
  - [x] 2.2 Add memory for mapping ISA-95 to Ignition folder structure
  - [x] 2.3 Add memory for hierarchy-based alarm and reporting design
  - [x] 2.4 Add memory for enterprise-to-asset navigation patterns

- [x] Task 3: Add UDT Structure Best Practices Memories (AC: #2)
  - [x] 3.1 Add memory for UDT naming conventions (PascalCase definitions, camelCase instances)
  - [x] 3.2 Add memory for UDT inheritance patterns and when to use extends
  - [x] 3.3 Add memory for UDT parameters and dynamic binding patterns
  - [x] 3.4 Add memory for UDT composition vs inheritance decisions

- [x] Task 4: Add Tag Path Structure Memories (AC: #5)
  - [x] 4.1 Add memory for tag path format and provider syntax
  - [x] 4.2 Add memory for ISA-95-aligned tag folder organization
  - [x] 4.3 Add memory for UDT parameter binding in tag paths
  - [x] 4.4 Add memory for tag path verification requirements

- [x] Task 5: Add Database Schema Pattern Memories (AC: #3)
  - [x] 5.1 Add memory for equipment metadata table design
  - [x] 5.2 Add memory for historian configuration patterns
  - [x] 5.3 Add memory for equipment-to-database relationship modeling

- [x] Task 6: Add ISA-88 Batch Pattern Memories (AC: #4)
  - [x] 6.1 Add memory for ISA-88 procedural model (Recipe → Procedure → Unit Procedure → Operation → Phase)
  - [x] 6.2 Add memory for ISA-88 equipment model (Process Cell → Unit → Equipment Module → Control Module)
  - [x] 6.3 Add memory for phase state machine design
  - [x] 6.4 Add memory for applicability criteria (batch industries)

- [x] Task 7: Add Critical Actions for Architect Agent (AC: #6)
  - [x] 7.1 Add critical action for SIS configuration flagging
  - [x] 7.2 Add critical action for engineering authority documentation
  - [x] 7.3 Add critical action for IT/OT boundary awareness
  - [x] 7.4 Add critical action for UDT hierarchy validation

- [x] Task 8: Validate Customize File (AC: #1-6)
  - [x] 8.1 Run Architecture Compliance Checklist validation
  - [x] 8.2 Validate YAML syntax
  - [x] 8.3 Verify memory count and categorization
  - [x] 8.4 Compare against Dev agent memory patterns for consistency

## Dev Notes

### Story Purpose

This is Story 3 of Epic 2 (Planning Agents — Analyst, PM, Architect). It populates the Architect agent's customize file with data modeling domain knowledge so the agent designs architecture documents that translate directly into implementable Ignition UDT structures and tag hierarchies.

**Why This Matters:**
- Generic architects design generic data models (ER diagrams, class hierarchies)
- IA architects design ISA-compliant hierarchies (ISA-95 equipment model, ISA-88 batch model)
- Proper architecture prevents downstream UDT rework and tag reorganization
- The Architect agent output feeds directly into Dev agent implementation

**Relationship to Other Stories:**
- **Story 2.1 (Analyst):** Analyst discovers requirements → Architect designs structures
- **Story 2.2 (PM):** PM creates PRD with ISA-95 hierarchy → Architect elaborates into UDT designs
- **Story 2.4 (Validation):** Will test full Analyst → PM → Architect workflow

### Previous Story Intelligence (Epic 1 + Story 2.1 Learnings)

**Memory Format Pattern (PROVEN in Epic 1):**
- Rule + context format, 1-3 sentences per memory
- Code references use backticks: `UDT`, `[default]Site/Area/Equipment/Tag`
- ISA standards use ID-only: ISA-95, ISA-88 (not ISA-95:2010)
- Ignition terms use official capitalization: Ignition, UDT, Perspective, Designer, Gateway, Jython 2.7
- ~3-4 items per logical category works well

**Critical Action Pattern (PROVEN):**
- Start with imperative verb (Design, Verify, Document, Flag)
- 1-2 sentences maximum
- Critical actions need companion memories for proactive application

**From Epic 1 Retrospective:**
- Add reinforcement memories alongside critical actions
- Self-review against compliance checklist before code review
- Real validation in Ignition Gateway is gold standard

**From Story 2.1 Structure:**
- Memory categories table format works well for planning
- Target ~3-4 memories per category
- Include sample memory formats with good/bad examples
- Reference ISA standards relevance table

### Architecture Compliance Checklist

Before finalizing, verify:
- [x] All memories follow rule + context format (1-3 sentences)
- [x] Code references use backticks (`example`)
- [x] ISA standards use ID-only format (no years)
- [x] Ignition terms use official capitalization
- [x] Critical actions start with imperative verbs
- [x] Tag path examples are realistic (`[default]Site/Area/Line/Equipment/Tag`)
- [x] YAML syntax validates

### Memory Categories for Architect Agent

| Category | Target Count | Description |
|----------|--------------|-------------|
| ISA-95 Hierarchy | 4 memories | Six-level model, folder mapping, alarm grouping, navigation |
| UDT Best Practices | 4 memories | Naming, inheritance, parameters, composition |
| Tag Path Structure | 4 memories | Format, ISA-95 alignment, parameter binding, verification |
| Database Patterns | 3 memories | Equipment metadata, historian, relationships |
| ISA-88 Batch | 4 memories | Procedural model, equipment model, state machine, applicability |
| **Total Memories** | **~19** | |

| Category | Target Count | Description |
|----------|--------------|-------------|
| Safety Awareness | 2 actions | SIS flagging, engineering authority |
| Architecture Validation | 2 actions | UDT hierarchy, IT/OT boundaries |
| **Total Critical Actions** | **~4** | |

### Key ISA Standards for Architect

| Standard | Relevance to Architect |
|----------|------------------------|
| ISA-95 | Equipment hierarchy (Enterprise → Site → Area → Line → Cell → Equipment Module) — core architecture structure |
| ISA-88 | Batch/procedural control (Recipe → Procedure → Operation → Phase) — applicable for batch industries |
| ISA-18.2 | Alarm architecture design (priority structure, state transitions) — referenced but Dev agent owns implementation details |
| IEC 62443 | OT security architecture (zones, conduits, boundaries) — safety awareness |

### Sample Memory Formats

**Good Examples (Architect-specific):**
```yaml
memories:
  - "Design UDT hierarchies reflecting ISA-95 equipment levels: create a base `EquipmentModule` UDT, then extend it for specific equipment types (`Tank`, `Motor`, `Valve`). This enables consistent tag structure and inheritance-based alarm configuration."

  - "When designing tag path structure, organize folders following ISA-95: `[provider]Site/Area/Line/Cell/Equipment/Tag`. This enables area-based alarm filtering, hierarchical navigation, and production reporting by organizational unit."

  - "For batch process projects (food processing, pharmaceuticals, specialty chemicals), apply ISA-88 procedural model to architecture. Define Recipe → Unit Procedure → Operation → Phase hierarchy in the architecture document with corresponding UDT structures."
```

**Bad Examples (avoid):**
```yaml
memories:
  - "Use UDTs"  # Too vague
  - "ISA-95 has six levels"  # No actionable guidance
  - "The ISA-95 standard defines Enterprise as the top level which contains Sites which contain Areas which contain Lines which contain Cells which contain Equipment Modules and each level has specific responsibilities..."  # Too long
```

### Architect-Specific Content Focus

**ISA-95 Equipment Hierarchy (Core Focus):**
- Six levels: Enterprise → Site → Area → Line → Cell → Equipment Module
- Mapping to Ignition folder structure
- Impact on alarm grouping and reporting
- UDT instantiation at appropriate levels

**UDT Design Patterns:**
- Base UDT definitions vs. specialized extensions
- Parameter design for dynamic binding
- Inheritance vs. composition decisions
- Naming conventions (PascalCase definitions, descriptive instances)

**Tag Path Architecture:**
- Provider syntax and default provider
- ISA-95-aligned folder organization
- UDT parameter binding patterns `{BasePath}/{EquipmentName}/Tag`
- Verification requirements before binding creation

**Database Schema (When Applicable):**
- Equipment metadata tables (equipment_id, equipment_type, parent_path)
- Historian configuration (tag to table mapping)
- Relational design for equipment hierarchies

**ISA-88 Batch Control (When Applicable):**
- Applicability criteria (batch industries: food & beverage, pharma, chemicals)
- Procedural model mapping to UDT structures
- Phase state machine architecture
- Equipment module design for batch operations

### Relationship to Dev Agent Memories

The Architect agent defines **what** structures should exist; the Dev agent implements **how** they work:

| Architect Designs | Dev Implements |
|-------------------|----------------|
| ISA-95 folder structure | Actual tag folder creation |
| UDT hierarchy definitions | UDT JSON with parameters and alarms |
| Tag path conventions | Perspective bindings to tag paths |
| Database schema | SQL queries in Jython scripts |
| ISA-88 state machine design | Jython state machine implementation |

**Avoid Overlap:** Architect memories should focus on structural/design decisions. Implementation details (Jython syntax, Perspective JSON, ignition-lint) belong in Dev agent memories.

### Project Structure Notes

- **Customize file location:** `_bmad/_config/agents/bmm-architect.customize.yaml`
- **Recompile command:** `npx bmad-method install` → "Recompile Agents"
- **Compiled agent:** `_bmad/bmm/agents/architect.md`
- Alignment with unified project structure confirmed

### References

- [Source: _bmad-output/planning-artifacts/epics.md#Story 2.3]
- [Source: _bmad-output/planning-artifacts/architecture.md#Memory Formatting]
- [Source: _bmad-output/planning-artifacts/architecture.md#Customize Files Scope (MVP)]
- [Source: _bmad-output/planning-artifacts/architecture.md#ISA Content Strategy]
- [Source: _bmad-output/planning-artifacts/prd.md#FR12-FR16]
- [Source: _bmad-output/implementation-artifacts/2-1-populate-analyst-agent-ia-requirements-discovery.md]
- [Source: _bmad/_config/agents/bmm-dev.customize.yaml#ISA-95 Equipment Hierarchy]
- [Source: _bmad/_config/agents/bmm-dev.customize.yaml#UDT Patterns]

## Dev Agent Record

### Agent Model Used

Claude Opus 4.5 (claude-opus-4-5-20251101)

### Debug Log References

N/A - No errors or issues encountered during implementation.

### Completion Notes List

1. **Task 1 - Review Current State:** Confirmed bmm-architect.customize.yaml was empty template. Reviewed Architecture doc memory format patterns (rule + context, 1-3 sentences, ISA ID-only, official capitalization). Reviewed bmm-dev.customize.yaml for proven patterns from Epic 1. Reviewed Story 2.1 Analyst agent structure.

2. **Task 2 - ISA-95 Hierarchy Memories (4 added):**
   - Six-level equipment hierarchy model with Ignition folder mapping
   - ISA-95 to Ignition tag folder structure mapping
   - Hierarchy-based alarm and reporting architecture design
   - Enterprise-to-asset navigation patterns

3. **Task 3 - UDT Structure Best Practices Memories (4 added):**
   - PascalCase definitions, descriptive instance naming conventions
   - UDT inheritance patterns with `extends` property
   - UDT parameters for dynamic binding (`BasePath`, `EquipmentName`, etc.)
   - Composition vs inheritance decision criteria

4. **Task 4 - Tag Path Structure Memories (4 added):**
   - Tag path format with provider syntax (`[provider]Path/To/Tag`)
   - ISA-95-aligned tag folder organization
   - UDT parameter binding patterns (`{BasePath}/{EquipmentName}/Tag`)
   - Tag path verification requirements before binding creation

5. **Task 5 - Database Schema Pattern Memories (3 added):**
   - Equipment metadata table design with ISA-95 alignment
   - Historian configuration patterns with tag path patterns
   - Equipment-to-database relationship modeling

6. **Task 6 - ISA-88 Batch Pattern Memories (4 added):**
   - ISA-88 procedural model (Recipe → Procedure → Unit Procedure → Operation → Phase)
   - ISA-88 equipment model (Process Cell → Unit → Equipment Module → Control Module)
   - Phase state machine design with exception states
   - Applicability criteria (batch industries: food & beverage, pharma, chemicals)

7. **Task 7 - Critical Actions (4 added):**
   - "Flag SIS configurations in architecture documents"
   - "Document engineering authority requirements for safety-critical decisions"
   - "Identify IT/OT network boundaries per IEC 62443"
   - "Validate UDT hierarchy designs against ISA-95 equipment model"

8. **Task 8 - Validation:**
   - YAML syntax validated with yaml.safe_load()
   - Memory count: 19 (matching target)
   - Critical actions count: 4 (matching target)
   - All memories 1-3 sentences
   - All critical actions start with imperative verbs
   - ISA references use ID-only format (ISA-95, ISA-88, IEC 62443)
   - Code references use backticks throughout
   - Tag path examples are realistic

**Key Design Decisions:**
- Architect memories focus on DESIGN decisions (what structures to create), not implementation details (how to implement)
- Clear separation from Dev agent: Architect designs ISA-95 hierarchy structure, Dev implements tag folders
- ISA-88 content explicitly scoped to batch industries with applicability criteria
- Safety awareness integrated through critical actions, not just memories

### File List

| Action | File Path |
|--------|-----------|
| Modified | _bmad/_config/agents/bmm-architect.customize.yaml |
| Modified | _bmad/bmm/agents/architect.md |
| Modified | _bmad-output/implementation-artifacts/sprint-status.yaml |
| Modified | _bmad-output/implementation-artifacts/2-3-populate-architect-agent-data-modeling-and-udt-patterns.md |

## Senior Developer Review (AI)

**Review Date:** 2026-02-19
**Reviewer:** Claude Opus 4.5 (Adversarial Code Review)
**Outcome:** Changes Requested → Fixed

### Issues Found and Resolved

| Severity | Issue | Resolution |
|----------|-------|------------|
| CRITICAL | ISA-95 terminology "Production Line > Work Cell" inconsistent with Architecture doc and Dev agent (which use "Line > Cell") | Fixed: Changed to `Enterprise → Site → Area → Line → Cell → Equipment Module` |
| HIGH | Tag path format used `EquipmentModule` in line 31 but `Equipment` in line 52 and Dev agent | Fixed: Standardized on `Equipment` |
| MEDIUM | Compiled agent `_bmad/bmm/agents/architect.md` missing from File List | Fixed: Added to File List |
| LOW | Arrow style `>` inconsistent with `→` used elsewhere | Fixed: Changed to `→` |

### Verification Notes

- Terminology now matches Architecture doc (lines 262, 338)
- Terminology now matches Dev agent (bmm-dev.customize.yaml line 79)
- Tag path format now consistent: `[default]Site/Area/Line/Cell/Equipment/Tag`
- All 19 memories and 4 critical_actions verified present
- YAML syntax valid

## Change Log

| Date | Change |
|------|--------|
| 2026-02-19 | Code Review: Fixed ISA-95 terminology inconsistency (Production Line/Work Cell → Line/Cell), standardized tag path format (EquipmentModule → Equipment), added compiled agent to File List |
| 2026-02-19 | Initial implementation - Populated Architect agent customize file with 19 memories and 4 critical actions for ISA-compliant data modeling and UDT patterns |

