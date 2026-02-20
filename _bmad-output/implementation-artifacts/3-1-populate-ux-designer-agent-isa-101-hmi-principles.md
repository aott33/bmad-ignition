# Story 3.1: Populate UX Designer Agent — ISA-101 HMI Principles

Status: done

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a **Lead Ignition Developer**,
I want **the UX Designer agent to apply ISA-101 High Performance HMI principles**,
So that **screen layouts follow industrial best practices for operator effectiveness**.

## Acceptance Criteria

1. **Given** the bmm-ux-designer.customize.yaml file exists
   **When** I add memories for ISA-101 HMI principles
   **Then** the memories include High Performance HMI design principles (gray backgrounds, color for abnormal only)

2. **And** the memories include navigation flow patterns for operators (hierarchy-based, role-based access)

3. **And** the memories include industrial display visual patterns (information density, alarm visualization)

4. **And** the memories include Perspective-specific layout guidance (views, containers, responsive design)

5. **And** critical_actions include safety awareness (note safety-critical displays, engineering review required)

## Tasks / Subtasks

- [x] Task 1: Review Current Customize File State (AC: #1-5)
  - [x] 1.1 Read bmm-ux-designer.customize.yaml and confirm it's empty/template
  - [x] 1.2 Review Architecture doc for memory format patterns
  - [x] 1.3 Review Epic 2 agent memories for established patterns (Analyst, PM, Architect)
  - [x] 1.4 Review Dev agent memories for ISA-101 implementation overlap detection

- [x] Task 2: Add ISA-101 High Performance HMI Memories (AC: #1)
  - [x] 2.1 Add memory for gray/neutral background principle (reduce operator fatigue)
  - [x] 2.2 Add memory for color usage (color indicates abnormal conditions ONLY)
  - [x] 2.3 Add memory for analog display guidelines (bar graphs, trends over digital values)
  - [x] 2.4 Add memory for alarm color conventions (red=critical, yellow=warning, no green for normal)

- [x] Task 3: Add Navigation Flow Pattern Memories (AC: #2)
  - [x] 3.1 Add memory for hierarchy-based navigation (ISA-95 alignment: Site → Area → Equipment)
  - [x] 3.2 Add memory for role-based access patterns (operator vs supervisor vs engineer views)
  - [x] 3.3 Add memory for consistent navigation placement (top/side bars, breadcrumbs)
  - [x] 3.4 Add memory for drill-down patterns (overview → area → equipment detail)

- [x] Task 4: Add Industrial Display Visual Pattern Memories (AC: #3)
  - [x] 4.1 Add memory for information density guidelines (meaningful data only, no decorative elements)
  - [x] 4.2 Add memory for alarm visualization patterns (banner placement, acknowledgment flow)
  - [x] 4.3 Add memory for trend display standards (time scales, data density, context)
  - [x] 4.4 Add memory for equipment status representation (state-based symbols, not animated)

- [x] Task 5: Add Perspective-Specific Layout Memories (AC: #4)
  - [x] 5.1 Add memory for Perspective view hierarchy (views, containers, coordinate/flex containers)
  - [x] 5.2 Add memory for responsive design patterns (breakpoints, mobile-aware layouts)
  - [x] 5.3 Add memory for view templates and embedded views for reuse
  - [x] 5.4 Add memory for property binding patterns for dynamic layouts

- [x] Task 6: Add Critical Actions for UX Designer Agent (AC: #5)
  - [x] 6.1 Add critical action for safety-critical display notation
  - [x] 6.2 Add critical action for engineering review requirements
  - [x] 6.3 Add critical action for ISA-101 compliance verification

- [x] Task 7: Validate Customize File (AC: #1-5)
  - [x] 7.1 Run Architecture Compliance Checklist validation
  - [x] 7.2 Validate YAML syntax
  - [x] 7.3 Verify memory count and categorization
  - [x] 7.4 Compare against Epic 2 agent memory patterns for consistency

## Dev Notes

### Story Purpose

This is Story 1 of Epic 3 (Design & Coordination — UX Designer, SM, QA). It populates the UX Designer agent's customize file with ISA-101 High Performance HMI principles so the agent designs screen layouts that follow industrial best practices for operator effectiveness.

**Why This Matters:**
- Generic UX designers create consumer-style interfaces (colorful, animated, decorative)
- Industrial UX must follow ISA-101 High Performance HMI principles (gray backgrounds, color for abnormal only, information density)
- Operators work 12-hour shifts monitoring processes — visual fatigue is a safety concern
- Proper HMI design prevents alarm floods, reduces operator error, improves situational awareness

**Relationship to Other Stories:**
- **Story 2.2 (PM):** PM references ISA-101 in PRD HMI requirements → UX Designer elaborates with specific layout guidance
- **Story 2.3 (Architect):** Architect designs ISA-95 hierarchy → UX Designer creates navigation reflecting that hierarchy
- **Story 3.2 (SM):** SM will coordinate parallel view development → UX Designer establishes consistent patterns
- **Story 3.4 (Validation):** Will test full workflow including UX Designer output

### Previous Story Intelligence (Epic 1 + Epic 2 Learnings)

**Memory Format Pattern (PROVEN in Epic 1 and Epic 2):**
- Rule + context format, 1-3 sentences per memory
- Code references use backticks: `Perspective`, `flex container`, `[default]Site/Area/Equipment/Tag`
- ISA standards use ID-only: ISA-101, ISA-95 (not ISA-101:2015)
- Ignition terms use official capitalization: Ignition, UDT, Perspective, Designer, Gateway, Jython 2.7
- ~3-4 items per logical category works well

**Critical Action Pattern (PROVEN):**
- Start with imperative verb (Design, Verify, Document, Flag, Note)
- 1-2 sentences maximum
- Critical actions need companion memories for proactive application

**From Epic 1 Retrospective:**
- Add reinforcement memories alongside critical actions
- Self-review against compliance checklist before code review
- Real validation in Ignition Gateway is gold standard

**From Epic 2 Stories (2.1-2.3):**
- Memory count target: ~15-20 memories with clear categorization
- Categories should map to acceptance criteria
- Include sample memory formats with good/bad examples
- Reference ISA standards relevance table

**From Story 2.4 Validation:**
- Static validation (verifying customize files are populated and compile correctly) is acceptable
- Full behavioral testing will occur in Story 3.4
- Gate decision based on memories present, not live testing

### Architecture Compliance Checklist

Before finalizing, verify:
- [x] All memories follow rule + context format (1-3 sentences)
- [x] Code references use backticks (`example`)
- [x] ISA standards use ID-only format (no years)
- [x] Ignition terms use official capitalization (Perspective, Designer, Gateway)
- [x] Critical actions start with imperative verbs
- [x] No Python 3 syntax in examples (N/A - UX Designer has no code examples)
- [x] YAML syntax validates (verified via `npx bmad-method install` → Recompile Agents)

### Memory Categories for UX Designer Agent

| Category | Target Count | Description |
|----------|--------------|-------------|
| ISA-101 High Performance HMI | 4 memories | Gray backgrounds, color for abnormal, analog displays, alarm colors |
| Navigation Flow Patterns | 4 memories | Hierarchy-based, role-based, consistent placement, drill-down |
| Industrial Display Visuals | 4 memories | Information density, alarm visualization, trends, status symbols |
| Perspective-Specific Layout | 4 memories | View hierarchy, responsive design, templates, property binding |
| **Total Memories** | **~16** | |

| Category | Target Count | Description |
|----------|--------------|-------------|
| Safety Awareness | 2 actions | Safety-critical displays, engineering review |
| Design Validation | 1 action | ISA-101 compliance verification |
| **Total Critical Actions** | **~3** | |

### Key ISA-101 Principles for UX Designer

| Principle | Implementation |
|-----------|----------------|
| **Gray/Neutral Backgrounds** | Use gray (#808080 range) or neutral colors; white is acceptable for contrast areas |
| **Color for Abnormal Only** | Red = critical alarm, Yellow = warning, NO green for normal state (use gray) |
| **Analog Over Digital** | Bar graphs, sparklines, trends over numeric displays for process values |
| **Information Density** | Every element must serve a purpose; no decorative graphics |
| **Consistent Navigation** | ISA-95 hierarchy-based navigation; same patterns across all screens |
| **Alarm Visualization** | Dedicated alarm banner/zone; clear acknowledgment workflow; priority-based color |

### Relationship to Dev Agent Memories

The UX Designer defines **what** screens should look like; the Dev agent implements **how** to build them:

| UX Designer Designs | Dev Implements |
|---------------------|----------------|
| Screen layouts and wireframes | Perspective view JSON |
| Navigation structure | View routing and params |
| Color scheme and visual patterns | Component styles and themes |
| Alarm visualization patterns | Alarm status bindings |
| Responsive breakpoints | Flex container configurations |

**Avoid Overlap:** UX Designer memories focus on DESIGN principles and layout decisions. Implementation details (Perspective JSON structure, view.json properties, style classes) belong in Dev agent memories.

### Project Structure Notes

- **Customize file location:** `_bmad/_config/agents/bmm-ux-designer.customize.yaml`
- **Recompile command:** `npx bmad-method install` → "Recompile Agents"
- **Compiled agent:** `_bmad/bmm/agents/ux-designer.md`
- Alignment with unified project structure confirmed

### Sample Memory Formats

**Good Examples (UX Designer-specific):**
```yaml
memories:
  - "Apply ISA-101 High Performance HMI principles: use gray or neutral backgrounds to reduce visual fatigue during extended operator shifts. Reserve color exclusively for abnormal conditions and alarms."

  - "Design navigation following ISA-95 equipment hierarchy: Site overview → Area selection → Line/Cell views → Equipment detail. This matches how operators think about the process and reduces clicks to reach relevant screens."

  - "For alarm visualization, dedicate a consistent screen zone (typically top banner or right panel) for active alarms. Use priority-based color coding: red for critical (immediate action), yellow for warning (attention needed), gray for acknowledged/cleared."

  - "When designing Perspective layouts, use `flex container` for responsive designs that adapt to screen sizes. Use `coordinate container` only for fixed-position elements like P&ID-style process graphics."
```

**Bad Examples (avoid):**
```yaml
memories:
  - "Use good colors"  # Too vague
  - "ISA-101 is about HMI"  # No actionable guidance
  - "The ISA-101 standard published in 2015 establishes principles for High Performance HMI design which originated from research conducted by the ASM Consortium showing that traditional HMIs with colorful graphics and animations contribute to operator fatigue and..."  # Too long
```

### References

- [Source: _bmad-output/planning-artifacts/epics.md#Story 3.1]
- [Source: _bmad-output/planning-artifacts/architecture.md#Memory Formatting]
- [Source: _bmad-output/planning-artifacts/architecture.md#Customize Files Scope (MVP)]
- [Source: _bmad-output/planning-artifacts/architecture.md#ISA Content Strategy]
- [Source: _bmad-output/planning-artifacts/prd.md#FR17-FR19]
- [Source: _bmad-output/implementation-artifacts/2-2-populate-pm-agent-isa-95-and-ignition-awareness.md]
- [Source: _bmad-output/implementation-artifacts/2-3-populate-architect-agent-data-modeling-and-udt-patterns.md]
- [Source: _bmad/_config/agents/bmm-ux-designer.customize.yaml]

## Dev Agent Record

### Agent Model Used

Claude Opus 4.5 (claude-opus-4-5-20251101)

### Debug Log References

N/A - No debug issues encountered during implementation.

### Completion Notes List

- Populated bmm-ux-designer.customize.yaml with 16 memories across 4 categories
- Added 3 critical actions for safety awareness and ISA-101 compliance
- Validated against Architecture Compliance Checklist - all checks pass
- YAML syntax validated successfully via `npx bmad-method install` → Recompile Agents
- Compiled agent verified: `_bmad/bmm/agents/ux-designer.md` contains all 16 memories and 3 critical actions
- Memory format consistent with Epic 2 agent patterns (total memories+actions: Analyst: 22, PM: 27, Architect: 23, UX Designer: 19)
- UX Designer focuses on DESIGN principles; implementation details remain in Dev agent memories (no overlap)

### File List

- `_bmad/_config/agents/bmm-ux-designer.customize.yaml` - Modified (populated with ISA-101 HMI memories and critical actions)

## Change Log

| Date | Author | Change |
|------|--------|--------|
| 2026-02-19 | Dev Agent | Initial implementation - populated customize file with 16 memories and 3 critical actions |
| 2026-02-19 | Code Review (AI) | Verified all ACs implemented, checked Architecture Compliance Checklist, updated status to done |
