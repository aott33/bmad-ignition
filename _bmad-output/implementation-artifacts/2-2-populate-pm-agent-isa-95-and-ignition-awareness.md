# Story 2.2: Populate PM Agent — ISA-95 and Ignition Awareness

Status: done

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a **Lead Ignition Developer**,
I want **the PM agent to structure PRDs with ISA-95 hierarchy and Ignition awareness**,
So that **requirements documents align with industrial standards and platform capabilities**.

## Acceptance Criteria

1. **Given** the bmm-pm.customize.yaml file exists
   **When** I add memories for ISA-95 and Ignition awareness
   **Then** the memories include ISA-95 equipment hierarchy (Enterprise → Site → Area → Line → Cell → Equipment Module)

2. **And** the memories include ISA-101 references for HMI requirements

3. **And** the memories include ISA-18.2 references for alarm management requirements

4. **And** the memories include ISA-88 batch pattern recognition (when applicable to project type)

5. **And** the memories include Ignition platform feature prioritization (Perspective vs Vision, Tag Historian, Alarming)

6. **And** critical_actions include safety awareness (note SIS scope, IT/OT boundaries)

7. **And** each memory follows the rule + context format (1-3 sentences)

## Tasks / Subtasks

- [x] Task 1: Review Current Customize File State (AC: #1-7)
  - [x] 1.1 Read bmm-pm.customize.yaml and confirm it's empty/template
  - [x] 1.2 Review Architecture doc for memory format patterns
  - [x] 1.3 Review bmm-dev.customize.yaml for proven memory examples (Epic 1)
  - [x] 1.4 Review bmm-analyst.customize.yaml for Story 2.1 patterns (parallel story)

- [x] Task 2: Add ISA-95 Equipment Hierarchy Memories (AC: #1, #7)
  - [x] 2.1 Add memory for ISA-95 hierarchy levels (Enterprise → Site → Area → Line → Cell → Asset)
  - [x] 2.2 Add memory for mapping ISA-95 to Ignition UDT hierarchy structure
  - [x] 2.3 Add memory for ISA-95 hierarchy application in PRD structure
  - [x] 2.4 Add memory for equipment naming conventions and consistency

- [x] Task 3: Add ISA-101 HMI Requirements Memories (AC: #2, #7)
  - [x] 3.1 Add memory for ISA-101 High Performance HMI principles in requirements
  - [x] 3.2 Add memory for ISA-101 information density considerations
  - [x] 3.3 Add memory for ISA-101 navigation and hierarchy requirements
  - [x] 3.4 Add memory for ISA-101 color philosophy (gray backgrounds, abnormal-only color)

- [x] Task 4: Add ISA-18.2 Alarm Management Memories (AC: #3, #7)
  - [x] 4.1 Add memory for ISA-18.2 alarm priority requirements in PRD
  - [x] 4.2 Add memory for ISA-18.2 alarm state definitions
  - [x] 4.3 Add memory for ISA-18.2 shelving and rationalization requirements
  - [x] 4.4 Add memory for alarm management as a PRD section requirement

- [x] Task 5: Add ISA-88 Batch Pattern Memories (AC: #4, #7)
  - [x] 5.1 Add memory for ISA-88 batch process identification
  - [x] 5.2 Add memory for ISA-88 procedural model (Recipe → Procedure → Phase)
  - [x] 5.3 Add memory for ISA-88 equipment model mapping
  - [x] 5.4 Add memory for when to apply ISA-88 (food & beverage, pharma, chemicals)

- [x] Task 6: Add Ignition Platform Feature Memories (AC: #5, #7)
  - [x] 6.1 Add memory for Perspective vs Vision decision criteria
  - [x] 6.2 Add memory for Tag Historian configuration requirements
  - [x] 6.3 Add memory for Ignition Alarming module capabilities
  - [x] 6.4 Add memory for Ignition module prioritization (core vs optional)
  - [x] 6.5 Add memory for Ignition Gateway architecture considerations

- [x] Task 7: Add Safety Awareness Critical Actions (AC: #6, #7)
  - [x] 7.1 Add critical action for SIS scope documentation in PRD
  - [x] 7.2 Add critical action for IT/OT boundary identification
  - [x] 7.3 Add critical action for engineering authority requirements
  - [x] 7.4 Add reinforcement memories for critical actions (Epic 1 learning)

- [x] Task 8: Validate Customize File (AC: #1-7)
  - [x] 8.1 Run Architecture Compliance Checklist validation
  - [x] 8.2 Validate YAML syntax
  - [x] 8.3 Verify memory count and categorization
  - [x] 8.4 Verify critical actions have companion reinforcement memories

## Dev Notes

### Story Purpose

This is the second story in Epic 2 (Planning Agents — Analyst, PM, Architect). It populates the PM agent's customize file with ISA standards awareness and Ignition platform knowledge so the agent structures PRDs with industrial standards alignment and platform-appropriate feature prioritization.

**Why This Matters:**
- Generic PMs structure requirements by generic software categories (features, user stories, integrations)
- IA-aware PMs structure requirements by equipment hierarchy (ISA-95), alarm philosophy (ISA-18.2), and HMI standards (ISA-101)
- Ignition-aware PMs prioritize features based on platform capabilities (Perspective vs Vision, available modules)
- Proper PRD structure enables better downstream work by Architect and Dev agents

### Previous Story Intelligence (Epic 1 + Story 2.1 Learnings)

**Memory Format Pattern (PROVEN):**
- Rule + context format, 1-3 sentences per memory
- Code references use backticks: `[default]Site/Area/Line/Equipment/Tag`
- ISA standards use ID-only: ISA-95 (not ISA-95:2010)
- Ignition terms use official capitalization: Ignition, UDT, Perspective, Vision, Designer, Gateway, Tag Historian
- ~3-4 items per logical category works well

**Critical Action Pattern (PROVEN):**
- Start with imperative verb (Include, Document, Identify, Verify)
- 1-2 sentences maximum
- **Critical actions need companion memories for proactive application** (Epic 1 key learning)

**From Epic 1 Retrospective:**
- Add reinforcement memories alongside critical actions
- Self-review against compliance checklist before code review
- Real validation in Ignition Gateway is gold standard
- Track memory counts for clear progression

**From Story 2.1 (Analyst Agent):**
- Analyst focuses on discovery QUESTIONS (gathering requirements)
- PM focuses on STRUCTURING requirements into PRD format
- PM works downstream from Analyst output
- ISA standards apply differently: Analyst asks about them, PM structures PRD sections around them

### Architecture Compliance Checklist

Before finalizing, verify:
- [x] All memories follow rule + context format (1-3 sentences)
- [x] Code references use backticks (`example`)
- [x] ISA standards use ID-only format (no years)
- [x] Ignition terms use official capitalization
- [x] Critical actions start with imperative verbs
- [x] Tag path examples are realistic (`[default]Site/Area/Line/Equipment/Tag`) — N/A for PM (PRD focus)
- [x] YAML syntax validates
- [x] Critical actions have companion reinforcement memories

### Memory Categories for PM Agent

| Category | Target Count | Description |
|----------|--------------|-------------|
| ISA-95 Hierarchy | 4 memories | Hierarchy levels, UDT mapping, PRD structure, naming |
| ISA-101 HMI | 4 memories | HMI principles, density, navigation, color |
| ISA-18.2 Alarms | 4 memories | Priorities, states, shelving, PRD section |
| ISA-88 Batch | 4 memories | Identification, procedural model, equipment, applicability |
| Ignition Platform | 5 memories | Perspective/Vision, Historian, Alarming, modules, Gateway |
| Safety Awareness | 3 memories | SIS, IT/OT, engineering authority (reinforcement) |
| **Total Memories** | **~24** | |

| Category | Target Count | Description |
|----------|--------------|-------------|
| SIS Scope | 1 action | Document SIS boundaries in PRD |
| IT/OT Boundaries | 1 action | Identify network segmentation |
| Engineering Authority | 1 action | Note approval requirements |
| **Total Critical Actions** | **~3** | |

### Key ISA Standards for PM Agent

| Standard | Relevance to PM |
|----------|-----------------|
| ISA-95 | PRD structure mirrors equipment hierarchy; requirements organized by area/line/cell |
| ISA-18.2 | PRD includes alarm management section with priority scheme, rationalization approach |
| ISA-101 | PRD includes HMI requirements section with ISA-101 principles |
| ISA-88 | PRD includes batch control section when applicable (pharma, food, chemicals) |
| IEC 62443 | PRD includes security requirements section with OT considerations |

### PM Agent vs Other Agents

| Agent | Focus | ISA Standards Application |
|-------|-------|--------------------------|
| **Analyst** | Discovery questions | Asks about ISA compliance needs |
| **PM** | PRD structure | Organizes PRD sections by ISA patterns |
| **Architect** | Technical design | Implements ISA patterns in UDTs/data models |
| **Dev** | Implementation | Follows ISA patterns in code/views |

### Ignition Platform Knowledge Areas

**Perspective vs Vision Decision:**
- Perspective: Modern web-based, mobile-first, responsive design
- Vision: Legacy Java Swing, desktop-focused, existing customer base
- Decision criteria: New projects = Perspective; Legacy support = Vision

**Tag Historian:**
- Store tag data for trending, analysis, reporting
- Configuration: sample rate, storage method, retention
- PRD should specify history requirements

**Alarming Module:**
- Built-in alarming with ISA-18.2 support
- Priority levels, shelving, acknowledgment, notification
- PRD should specify alarm requirements

**Gateway Architecture:**
- Central server processing all client requests
- Licensing model affects capacity planning
- Module-based architecture affects feature availability

### Sample Memory Formats

**Good Examples (PM-specific):**
```yaml
memories:
  - "Structure PRD equipment sections using ISA-95 hierarchy: Enterprise → Site → Area → Line → Cell → Asset. This creates natural alignment between requirements and downstream UDT/tag hierarchy design."

  - "Include an Alarm Management section in PRDs for industrial projects. Define the ISA-18.2 priority scheme (4 levels recommended), expected alarm count targets, and rationalization approach."

  - "When the project involves batch processing (food & beverage, pharmaceuticals, chemicals), include an ISA-88 section covering the procedural model (recipes, procedures, phases) and equipment model (process cells, units)."

  - "Recommend Perspective for new projects requiring mobile access, responsive design, or web-based deployment. Recommend Vision only for legacy integration or when existing Vision screens must be maintained."
```

**Bad Examples (avoid):**
```yaml
memories:
  - "Use ISA standards"  # Too vague
  - "ISA-95 is a standard for enterprise integration that defines how manufacturing systems communicate with enterprise systems and includes a hierarchy model with multiple levels..."  # Too long, not actionable
```

### Ignition PRD Section Patterns

**Equipment Hierarchy Section (ISA-95):**
- Document equipment structure: Site → Area → Line → Cell
- List major equipment categories per area
- Define naming conventions
- Map to expected tag folder structure

**HMI Requirements Section (ISA-101):**
- Navigation hierarchy matching equipment structure
- Screen types: Overview, Area, Equipment Detail, Alarm Summary, Trends
- ISA-101 High Performance principles: gray background, abnormal-only color
- Information density requirements

**Alarm Management Section (ISA-18.2):**
- Priority scheme (recommend 4 levels per ISA-18.2)
- Target alarm rate (alarms per 10 minutes per operator)
- Shelving policy for maintenance/known conditions
- Alarm rationalization requirements

**Batch Control Section (ISA-88, when applicable):**
- Process cell structure
- Recipe types (master, control, general)
- Phase definitions (discrete actions)
- Equipment module mapping

### Project Structure Notes

- **Customize file location:** `_bmad/_config/agents/bmm-pm.customize.yaml`
- **Recompile command:** `npx bmad-method install` → "Recompile Agents"
- **Compiled agent:** `_bmad/bmm/agents/pm.md`
- Alignment with unified project structure confirmed

### Git Intelligence (Recent Commits)

Recent commits show:
- `sprint retro epic 1` - Epic 1 retrospective completed
- `story 1.5`, `story 1.4`, `story 1.3`, `story 1.2` - Dev agent stories completed
- Pattern established: commit per story completion

Story 2.1 (Analyst agent) is in parallel - can reference if needed after that story completes.

### Web Research Notes

**ISA-95 Current Best Practices:**
- 6-level hierarchy is the standard: Enterprise → Site → Area → Line → Cell → Equipment Module
- Aligns naturally with Ignition's tag folder structure
- Equipment Module level maps to UDT instances

**Ignition 8.3+ Features:**
- Perspective is the primary HMI framework for new development
- Tag Historian 2.0 with improved storage efficiency
- Built-in alarming with ISA-18.2 compliance support
- Gateway Network for distributed architectures

### References

- [Source: _bmad-output/planning-artifacts/epics.md#Story 2.2]
- [Source: _bmad-output/planning-artifacts/architecture.md#Memory Formatting]
- [Source: _bmad-output/planning-artifacts/architecture.md#Customize Files Scope (MVP)]
- [Source: _bmad-output/planning-artifacts/prd.md#FR8-FR11]
- [Source: _bmad-output/planning-artifacts/prd.md#Planning & PRD Creation]
- [Source: _bmad-output/implementation-artifacts/epic-1-retro-2026-02-19.md#Key Insights and Learnings]
- [Source: _bmad/_config/agents/bmm-dev.customize.yaml] (proven memory patterns)
- [Source: _bmad-output/project-context.md#Core Goals]

## Dev Agent Record

### Agent Model Used

Claude Opus 4.5 (claude-opus-4-5-20251101)

### Debug Log References

N/A - No errors encountered during implementation.

### Completion Notes List

- Populated bmm-pm.customize.yaml with 24 memories across 5 ISA/Ignition categories plus 3 safety reinforcement memories:
  - ISA-95 Equipment Hierarchy (4 memories): PRD structure, Ignition mapping, equipment hierarchy section, naming conventions
  - ISA-101 HMI Requirements (4 memories): High Performance principles, navigation hierarchy, screen types, color philosophy
  - ISA-18.2 Alarm Management (4 memories): Priority scheme, alarm states, shelving, rationalization
  - ISA-88 Batch Patterns (4 memories): Process identification, procedural model, equipment model, applicability
  - Ignition Platform Features (5 memories): Perspective vs Vision, Tag Historian, Alarming, modules, Gateway architecture
  - Safety Awareness Reinforcement (3 memories): Companion memories for critical actions — SIS scope, IT/OT boundaries, engineering authority

- Added 3 critical actions with companion reinforcement memories (per Epic 1 learning):
  - Document SIS scope boundaries
  - Identify IT/OT network boundaries
  - Include engineering authority requirements

- All memories follow proven format: rule + context, 1-3 sentences, backticks for code, ISA ID-only format, official Ignition capitalization

- YAML syntax validated successfully

- PM agent focus: STRUCTURING requirements into PRD format (vs Analyst who ASKS questions during discovery)

### File List

| Action | File Path |
|--------|-----------|
| Modified | `_bmad/_config/agents/bmm-pm.customize.yaml` — Populated with 24 memories and 3 critical actions |
| Modified | `_bmad/bmm/agents/pm.md` — Compiled agent with customizations applied |

## Change Log

| Date | Change |
|------|--------|
| 2026-02-19 | Initial implementation — Populated PM agent customize file with 24 memories and 3 critical actions for ISA-95/Ignition PRD structuring |
| 2026-02-19 | Code review fixes — Updated AC #1 terminology to match Architecture doc (Equipment Module not Asset), documented compiled pm.md in File List, clarified memory categorization in Completion Notes |
