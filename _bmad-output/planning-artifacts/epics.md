---
stepsCompleted: ['step-01-validate-prerequisites', 'step-02-design-epics', 'step-03-create-stories', 'step-04-final-validation']
status: complete
completedAt: '2026-02-19'
inputDocuments:
  - prd.md
  - architecture.md
---

# bmad-ignition - Epic Breakdown

## Overview

This document provides the complete epic and story breakdown for bmad-ignition, decomposing the requirements from the PRD and Architecture into implementable stories.

## Requirements Inventory

### Functional Requirements

**Onboarding & Setup (FR1-FR4):**
- FR1: Developer can clone the bmad-ignition repository and have a working directory structure immediately
- FR2: Developer can run a single install command (`npx bmad-method install`) to configure all agent customizations
- FR3: Developer can follow quickstart documentation to produce first workflow output within one hour
- FR4: Developer can watch a video walkthrough demonstrating the complete workflow visually

**Requirements & Analysis (FR5-FR7):**
- FR5: Analyst agent can gather industrial automation requirements using domain-specific questions
- FR6: Analyst agent can identify equipment types, alarm philosophy, and operator workflows during discovery
- FR7: Developer can generate a project brief that captures SCADA/MES scope accurately

**Planning & PRD Creation (FR8-FR11):**
- FR8: PM agent can produce PRD documents with ISA-95 equipment hierarchy
- FR9: PM agent can align requirements with ISA-101, ISA-88, and ISA-18.2 standards
- FR10: PM agent can prioritize features appropriate for Ignition platform capabilities
- FR11: Developer can iterate on PRD content through collaborative refinement

**Architecture & Data Modeling (FR12-FR16):**
- FR12: Architect agent can design ISA-95 compliant equipment hierarchies (Enterprise → Site → Area → Line → Cell → Asset)
- FR13: Architect agent can propose UDT structures aligned with Ignition best practices
- FR14: Architect agent can design database schemas for equipment metadata when applicable
- FR15: Architect agent can apply ISA-88 batch/procedural control patterns
- FR16: Developer can review and refine architectural decisions before implementation

**UX Design (FR17-FR19):**
- FR17: UX Designer agent can create screen layouts following ISA-101 High Performance HMI principles
- FR18: UX Designer agent can design navigation flows appropriate for operator workflows
- FR19: UX Designer agent can specify visual design patterns for industrial displays

**Frontend Development (FR20-FR24):**
- FR20: Frontend agent can generate Perspective view JSON definitions with correct schema
- FR21: Frontend agent can create dynamic view patterns (templates, indirect bindings)
- FR22: Frontend agent can write valid Jython 2.7 event handlers (not Python 3)
- FR23: Frontend agent can create bindings that reference valid tag paths
- FR24: Frontend agent can apply ISA-101 principles in view implementation

**Backend Development (FR25-FR29):**
- FR25: Backend agent can create UDT definitions following Ignition patterns
- FR26: Backend agent can configure tag instances with appropriate parameter mappings
- FR27: Backend agent can implement alarm configurations per ISA-18.2 (priorities, states, shelving)
- FR28: Backend agent can write gateway scripts in valid Jython 2.7
- FR29: Backend agent can design database queries for equipment data when applicable

**Validation & Quality (FR30-FR33):**
- FR30: QA agent can validate agent output against Ignition requirements
- FR31: Developer can run ignition-lint to validate Perspective views before import
- FR32: ignition.nvim can provide LSP completions and diagnostics during development
- FR33: Developer can identify and correct agent errors before deployment

**Parallel Development (FR34-FR36):**
- FR34: Multiple dev agents can work on separate Perspective views without file conflicts
- FR35: Developer can run parallel stories during the same sprint
- FR36: Version control can track changes across parallel development streams

**Documentation & Reference (FR37-FR40):**
- FR37: Developer can access ISA standards reference documentation within the project
- FR38: Developer can use project context templates that encode Ignition constraints
- FR39: Customize file documentation can explain how agent customizations work
- FR40: Dairy simulator example can demonstrate complete end-to-end workflow

**Safety & Compliance Awareness (FR41-FR43):**
- FR41: Agents can note when touching SIS configurations or safety interlocks (awareness only)
- FR42: Agents can recognize IT/OT boundary concepts in code generation
- FR43: All agent output can be reviewed by human engineering authority before deployment

### NonFunctional Requirements

**Developer Experience:**
- NFR-DX1: First workflow output achievable within 60 minutes of repository clone (including BMAD install)
- NFR-DX2: Agent output requires <50% correction time compared to generic AI tools (Copilot/Claude baseline)

**Integration:**
- NFR-I1: Agent-generated Perspective views import into Ignition 8.3+ without modification
- NFR-I2: Generated Jython scripts produce zero syntax errors in ignition.nvim diagnostics
- NFR-I3: Generated Perspective views pass ignition-lint validation on first attempt (>90% target)
- NFR-I4: Customize files load correctly with `npx bmad-method install`

**Output Quality:**
- NFR-Q1: Agent-generated tag paths reference only tags defined in project context
- NFR-Q2: Agent-generated UDT references use only UDT definitions that exist
- NFR-Q3: Agent-generated scripts contain zero undefined `system.*` function calls
- NFR-Q4: Agent-generated UDT structures follow Ignition community naming conventions and patterns

**Error Handling & Resilience:**
- NFR-E1: Agent errors recoverable without full workflow restart
- NFR-E2: Critical syntax errors identifiable within editor before Gateway import attempt

**Maintainability:**
- NFR-M1: Customize files can be updated without breaking existing projects
- NFR-M2: Documentation explicitly states supported versions; breaking changes documented in CHANGELOG
- NFR-M3: ISA standards references include citation sources
- NFR-M4: Customize file changes validated against dairy pilot output before release

### Additional Requirements

**From Architecture - Content Architecture Approach:**
- This is a content architecture (customize files + documentation), not a software application
- BMAD Method v6 is the foundation — no starter template required
- Pre-populated customize files ship with the repo

**From Architecture - Customization Strategy:**
- Use `memories` + `critical_actions` sections (append behavior)
- Preserve BMAD's base agent personas unless pilot testing shows insufficient behavior change
- 7 customize files to populate: bmm-analyst, bmm-pm, bmm-architect, bmm-ux-designer, bmm-dev, bmm-qa, bmm-sm

**From Architecture - Implementation Phases:**
- Phase 1: Foundation — Populate bmm-dev.customize.yaml (pilot), create docs/ structure, minimal ISA content
- Phase 2: Validation — Execute test story, measure behavior change, gate decision
- Phase 3: Scale — Populate remaining customize files, complete ISA docs, dairy pilot planning
- Phase 4: Example — Complete dairy pilot Ignition project, quickstart guide, video
- Phase 5: Polish — Git merge docs, final review, CHANGELOG

**From Architecture - Validation Strategy:**
- Pilot test with bmm-dev.customize.yaml first
- Gate: Does agent reference ISA standards? Avoid Python 3 syntax? Validate tag paths?
- Escalation path: If memories insufficient, selectively adjust persona

**From Architecture - Key Patterns:**
- Memory format: Rule + context, 1-3 sentences, backticks for code
- ISA references: Standard ID only (no years)
- Ignition terminology: Official capitalization (Ignition, UDT, Perspective, Designer, Gateway, Jython 2.7)
- Critical actions: Imperative commands, action-first

### FR Coverage Map

| FR | Epic | Description |
|----|------|-------------|
| FR1 | Epic 6 | Clone repo → working structure |
| FR2 | Epic 6 | Single install command |
| FR3 | Epic 6 | Quickstart → first output <60 min |
| FR4 | Epic 6 | Video walkthrough |
| FR5 | Epic 2 | Analyst gathers IA requirements |
| FR6 | Epic 2 | Analyst identifies equipment/alarms |
| FR7 | Epic 2 | Generate project brief |
| FR8 | Epic 2 | PM produces ISA-95 PRDs |
| FR9 | Epic 2 | PM aligns with ISA standards |
| FR10 | Epic 2 | PM prioritizes for Ignition |
| FR11 | Epic 2 | Iterate on PRD |
| FR12 | Epic 2 | Architect designs ISA-95 hierarchy |
| FR13 | Epic 2 | Architect proposes UDT structures |
| FR14 | Epic 2 | Architect designs database schemas |
| FR15 | Epic 2 | Architect applies ISA-88 patterns (when applicable) |
| FR16 | Epic 2 | Review architectural decisions |
| FR17 | Epic 3 | UX Designer creates ISA-101 layouts |
| FR18 | Epic 3 | UX Designer designs navigation |
| FR19 | Epic 3 | UX Designer specifies visual patterns |
| FR20 | Epic 1 | Frontend generates Perspective JSON |
| FR21 | Epic 1 | Frontend creates dynamic patterns |
| FR22 | Epic 1 | Frontend writes Jython 2.7 |
| FR23 | Epic 1 | Frontend creates valid bindings |
| FR24 | Epic 1 | Frontend applies ISA-101 |
| FR25 | Epic 1 | Backend creates UDT definitions |
| FR26 | Epic 1 | Backend configures tag instances |
| FR27 | Epic 1 | Backend implements ISA-18.2 alarms |
| FR28 | Epic 1 | Backend writes Jython scripts |
| FR29 | Epic 1 | Backend designs database queries |
| FR30 | Epic 3 | QA validates agent output |
| FR31 | Epic 1 | Run ignition-lint validation |
| FR32 | Epic 1 | ignition.nvim LSP diagnostics |
| FR33 | Epic 1 | Identify/correct agent errors |
| FR34 | Epic 3 | Multiple agents without conflicts |
| FR35 | Epic 3 | Parallel stories in sprint |
| FR36 | Epic 3 | Version control tracks parallel |
| FR37 | Epic 4 | Access ISA standards docs |
| FR38 | Epic 5 | Project context templates |
| FR39 | Epic 4 | Customize file documentation |
| FR40 | Epic 5 | Dairy simulator example |
| FR41 | Epic 1-3 | Agents note SIS configurations (cross-cutting) |
| FR42 | Epic 1-3 | Agents recognize IT/OT boundaries (cross-cutting) |
| FR43 | Epic 1-3 | Output reviewed by engineering authority (cross-cutting) |

## Epic List

### Epic 1: Foundation — Dev Agent Pilot
Validate the customize file approach by populating bmm-dev.customize.yaml with Ignition domain knowledge. Developer can use the Dev agent to generate valid Perspective views, UDT definitions, Jython 2.7 scripts, and ISA-18.2 alarm configurations. This epic proves the pattern before scaling to other agents.

**FRs covered:** FR20-FR29, FR31-FR33
**Cross-cutting:** FR41-FR43 (safety awareness encoded in customize file)
**Gate:** Does agent reference ISA standards? Avoid Python 3 syntax? Validate tag paths?

### Epic 2: Planning Agents — Analyst, PM, Architect
Developer can run the full BMAD planning workflow (Analyst → PM → Architect) with Ignition/ISA awareness. Agents produce domain-appropriate briefs, PRDs with ISA-95 hierarchy, and architecture docs with UDT patterns. ISA-88 batch patterns available when applicable.

**FRs covered:** FR5-FR16
**Cross-cutting:** FR41-FR43 (safety awareness encoded in customize files)

### Epic 3: Design & Coordination — UX Designer, SM, QA
Developer can use UX Designer for ISA-101 HMI layouts, SM for parallel execution coordination, and QA for Ignition-specific validation. Completes the full agent suite.

**FRs covered:** FR17-FR19, FR30, FR34-FR36
**Cross-cutting:** FR41-FR43 (safety awareness encoded in customize files)

### Epic 4: ISA Standards Reference Documentation
Developer can access concise ISA standards reference sheets within the project, understanding what the agents "know" and why. Includes customize file documentation explaining how customizations work.

**FRs covered:** FR37, FR39

### Epic 5: Dairy Pilot — End-to-End Validation
Developer can follow the complete workflow on a real example project (dairy simulator), proving the entire system works end-to-end. All agents working together on a real project.

**FRs covered:** FR38, FR40
**Implementation note:** Dairy pilot artifacts can be built incrementally during Epics 1-3 for faster validation feedback.

### Epic 6: Onboarding & Polish
A new developer can clone the repo, follow the quickstart, and have working AI agents within 60 minutes. Video walkthrough available. Documentation written after the system is validated.

**FRs covered:** FR1-FR4
**NFR:** NFR-DX1 (<60 min first output)

---

## Epic 1: Foundation — Dev Agent Pilot

Validate the customize file approach by populating bmm-dev.customize.yaml with Ignition domain knowledge.

### Story 1.1: Populate Dev Agent Memories — Platform Constraints

As a **Lead Ignition Developer**,
I want **the Dev agent to understand Ignition platform constraints**,
So that **generated code uses valid Jython 2.7 syntax and follows Ignition patterns**.

**Acceptance Criteria:**

**Given** the bmm-dev.customize.yaml file exists
**When** I add memories for platform constraints
**Then** the memories include Jython 2.7 syntax rules (no f-strings, no type hints, `print` without parentheses)
**And** the memories include Perspective JSON structure awareness
**And** the memories include UDT naming conventions and inheritance patterns
**And** the memories include tag path format guidance (`[default]Site/Area/Equipment/Tag`)
**And** each memory follows the rule + context format (1-3 sentences, backticks for code)

### Story 1.2: Populate Dev Agent Memories — ISA Standards

As a **Lead Ignition Developer**,
I want **the Dev agent to understand ISA standards relevant to implementation**,
So that **generated views, alarms, and tag structures align with industrial best practices**.

**Acceptance Criteria:**

**Given** the bmm-dev.customize.yaml file has platform constraint memories
**When** I add memories for ISA standards
**Then** the memories include ISA-101 High Performance HMI principles for view implementation
**And** the memories include ISA-18.2 alarm configuration patterns (priority levels 1-4, states, deadband)
**And** the memories include ISA-95 hierarchy awareness for tag path structure
**And** the memories include ISA-88 batch pattern recognition (applied when project involves batch processes)
**And** ISA references use standard ID only (no years)

### Story 1.3: Populate Dev Agent Critical Actions

As a **Lead Ignition Developer**,
I want **the Dev agent to have validation reminders and discipline rules**,
So that **the agent consistently validates output and avoids common errors**.

**Acceptance Criteria:**

**Given** the bmm-dev.customize.yaml file has memories populated
**When** I add critical_actions
**Then** the actions include "Run ignition-lint before marking any script complete"
**And** the actions include "Verify tag paths exist before creating bindings"
**And** the actions include "Use Jython 2.7 syntax — no f-strings, no type hints, no walrus operator"
**And** the actions include safety awareness notes (note when touching SIS configs, respect IT/OT boundaries)
**And** each action starts with an imperative verb (Run, Verify, Use, Check)

### Story 1.4: Populate Dev Agent Tooling Integration

As a **Lead Ignition Developer**,
I want **the Dev agent to understand how to use ignition-lint and ignition.nvim**,
So that **the agent integrates with the validation toolchain**.

**Acceptance Criteria:**

**Given** the bmm-dev.customize.yaml file has memories and critical_actions
**When** I add tooling integration guidance
**Then** the memories include ignition-lint CLI usage for Perspective view validation
**And** the memories include ignition.nvim LSP diagnostics for real-time error detection
**And** the memories include the validation workflow (edit → LSP check → lint → Designer import)
**And** the critical_actions reference these tools appropriately

### Story 1.5: Validate Dev Agent Behavior

As a **Lead Ignition Developer**,
I want **to verify the customized Dev agent produces correct output**,
So that **I can confirm the customize file approach works before scaling to other agents**.

**Acceptance Criteria:**

**Given** the bmm-dev.customize.yaml is fully populated and recompiled (`npx bmad-method install` → Recompile Agents)
**When** I execute a test task (e.g., "Create a UDT for a tank level sensor with high-high alarm")
**Then** the agent references ISA standards appropriately (ISA-18.2 for alarms, ISA-95 for hierarchy)
**And** the agent generates valid Jython 2.7 syntax (no Python 3 constructs)
**And** the agent warns about unvalidated tag paths
**And** the agent mentions ignition-lint or validation steps
**And** if behavior is insufficient, document escalation needed (persona adjustment)

---

## Epic 2: Planning Agents — Analyst, PM, Architect

Developer can run the full BMAD planning workflow (Analyst → PM → Architect) with Ignition/ISA awareness.

### Story 2.1: Populate Analyst Agent — IA Requirements Discovery

As a **Lead Ignition Developer**,
I want **the Analyst agent to ask industrial automation-specific questions during discovery**,
So that **project briefs capture SCADA/MES scope accurately from the start**.

**Acceptance Criteria:**

**Given** the bmm-analyst.customize.yaml file exists
**When** I add memories for IA requirements discovery
**Then** the memories include SCADA/MES scope patterns (monitoring, control, batch, historian)
**And** the memories include equipment type questions (tanks, motors, conveyors, pumps, valves)
**And** the memories include alarm philosophy discovery (how many alarms, priority approach, shelving needs)
**And** the memories include operator workflow questions (shift handoff, acknowledgment patterns, roles)
**And** the memories include safety awareness (note SIS scope boundaries, engineering authority)
**And** each memory follows the rule + context format (1-3 sentences)

### Story 2.2: Populate PM Agent — ISA-95 and Ignition Awareness

As a **Lead Ignition Developer**,
I want **the PM agent to structure PRDs with ISA-95 hierarchy and Ignition awareness**,
So that **requirements documents align with industrial standards and platform capabilities**.

**Acceptance Criteria:**

**Given** the bmm-pm.customize.yaml file exists
**When** I add memories for ISA-95 and Ignition awareness
**Then** the memories include ISA-95 equipment hierarchy (Enterprise → Site → Area → Line → Cell → Asset)
**And** the memories include ISA-101 references for HMI requirements
**And** the memories include ISA-18.2 references for alarm management requirements
**And** the memories include ISA-88 batch pattern recognition (when applicable to project type)
**And** the memories include Ignition platform feature prioritization (Perspective vs Vision, Tag Historian, Alarming)
**And** critical_actions include safety awareness (note SIS scope, IT/OT boundaries)

### Story 2.3: Populate Architect Agent — Data Modeling and UDT Patterns

As a **Lead Ignition Developer**,
I want **the Architect agent to design ISA-compliant hierarchies and Ignition-specific data models**,
So that **architecture documents produce implementable UDT structures and tag hierarchies**.

**Acceptance Criteria:**

**Given** the bmm-architect.customize.yaml file exists
**When** I add memories for data modeling and UDT patterns
**Then** the memories include ISA-95 hierarchy for system architecture design
**And** the memories include UDT structure best practices (inheritance, parameters, naming conventions)
**And** the memories include database schema patterns for equipment metadata (when applicable)
**And** the memories include ISA-88 batch/procedural patterns (when project involves batch processes)
**And** the memories include tag path structure guidance aligned with ISA-95
**And** critical_actions include safety awareness (note SIS configurations, engineering authority required)

### Story 2.4: Validate Planning Workflow

As a **Lead Ignition Developer**,
I want **to verify the customized planning agents produce Ignition-relevant output**,
So that **I can confirm the workflow produces usable briefs, PRDs, and architecture docs**.

**Acceptance Criteria:**

**Given** bmm-analyst, bmm-pm, and bmm-architect customize files are populated and recompiled
**When** I run the planning workflow on a test scenario (e.g., "Small dairy processing facility HMI")
**Then** the Analyst asks IA-specific questions (equipment types, alarm philosophy)
**And** the PM produces a PRD with ISA-95 hierarchy section
**And** the Architect proposes UDT structures aligned with Ignition patterns
**And** all agents reference appropriate ISA standards
**And** safety awareness notes appear where relevant

---

## Epic 3: Design & Coordination — UX Designer, SM, QA

Developer can use UX Designer for ISA-101 HMI layouts, SM for parallel execution coordination, and QA for Ignition-specific validation.

### Story 3.1: Populate UX Designer Agent — ISA-101 HMI Principles

As a **Lead Ignition Developer**,
I want **the UX Designer agent to apply ISA-101 High Performance HMI principles**,
So that **screen layouts follow industrial best practices for operator effectiveness**.

**Acceptance Criteria:**

**Given** the bmm-ux-designer.customize.yaml file exists
**When** I add memories for ISA-101 HMI principles
**Then** the memories include High Performance HMI design principles (gray backgrounds, color for abnormal only)
**And** the memories include navigation flow patterns for operators (hierarchy-based, role-based access)
**And** the memories include industrial display visual patterns (information density, alarm visualization)
**And** the memories include Perspective-specific layout guidance (views, containers, responsive design)
**And** critical_actions include safety awareness (note safety-critical displays, engineering review required)

### Story 3.2: Populate SM Agent — Parallel Execution Coordination

As a **Lead Ignition Developer**,
I want **the SM agent to coordinate parallel development without file conflicts**,
So that **multiple dev agents can work on the same project simultaneously**.

**Acceptance Criteria:**

**Given** the bmm-sm.customize.yaml file exists
**When** I add memories for parallel execution coordination
**Then** the memories include file isolation rules (one agent per view, one agent per UDT)
**And** the memories include story naming conventions (`story-NNN-slug.md`)
**And** the memories include merge strategy awareness (avoid JSON merge conflicts)
**And** the memories include Ignition project structure awareness (separate views, tags, scripts)
**And** critical_actions include "Verify no file overlap before assigning parallel stories"

### Story 3.3: Populate QA Agent — Ignition Validation Patterns

As a **Lead Ignition Developer**,
I want **the QA agent to validate Ignition-specific output**,
So that **agent-generated code passes validation before Designer import**.

**Acceptance Criteria:**

**Given** the bmm-qa.customize.yaml file exists
**When** I add memories for Ignition validation patterns
**Then** the memories include ignition-lint integration (CLI usage, expected pass rate >90%)
**And** the memories include Ignition-specific test patterns (tag binding verification, view import testing)
**And** the memories include ISA standards compliance checking (ISA-18.2 alarm config, ISA-95 hierarchy)
**And** the memories include Jython 2.7 syntax validation patterns
**And** critical_actions include "Run ignition-lint on all Perspective views before approval"

### Story 3.4: Validate Design & Coordination Agents

As a **Lead Ignition Developer**,
I want **to verify the UX Designer, SM, and QA agents work correctly**,
So that **the full agent suite is ready for end-to-end use**.

**Acceptance Criteria:**

**Given** bmm-ux-designer, bmm-sm, and bmm-qa customize files are populated and recompiled
**When** I test the UX Designer on an HMI layout task (e.g., "Design tank overview screen")
**Then** the agent applies ISA-101 principles (gray background, color for abnormal)
**When** I test the SM on parallel story setup
**Then** the agent assigns non-overlapping files to parallel stories
**When** I test the QA on a validation workflow
**Then** the agent references ignition-lint and Ignition-specific validation steps

---

## Epic 4: ISA Standards Reference Documentation

Developer can access concise ISA standards reference sheets within the project, understanding what the agents "know" and why.

### Story 4.1: Create ISA Standards Index and Overview

As a **Lead Ignition Developer**,
I want **an index explaining why ISA standards matter for Ignition projects**,
So that **I understand the context for agent customizations**.

**Acceptance Criteria:**

**Given** the docs/isa-standards/ folder exists
**When** I create index.md
**Then** the document explains why ISA standards matter for SCADA/MES projects
**And** the document lists all included standards (ISA-95, ISA-101, ISA-18.2, ISA-88, IEC 62443)
**And** the document links to individual reference sheets
**And** the document clarifies these are concise references, not comprehensive standards documentation

### Story 4.2: Create ISA-95 Equipment Hierarchy Reference

As a **Lead Ignition Developer**,
I want **a concise ISA-95 reference sheet**,
So that **I understand how equipment hierarchy maps to Ignition tag structure**.

**Acceptance Criteria:**

**Given** the docs/isa-standards/ folder exists
**When** I create isa-95-hierarchy.md
**Then** the document explains the ISA-95 hierarchy levels (Enterprise → Site → Area → Line → Cell → Asset)
**And** the document shows how hierarchy maps to Ignition tag paths
**And** the document includes a simple example (e.g., dairy facility)
**And** the document is concise (1-2 pages maximum)

### Story 4.3: Create ISA-101 HMI Reference

As a **Lead Ignition Developer**,
I want **a concise ISA-101 High Performance HMI reference sheet**,
So that **I understand HMI design principles the agents apply**.

**Acceptance Criteria:**

**Given** the docs/isa-standards/ folder exists
**When** I create isa-101-hmi.md
**Then** the document explains High Performance HMI principles (gray backgrounds, color for abnormal only)
**And** the document covers information density and visual hierarchy
**And** the document covers navigation patterns for operators
**And** the document includes Perspective-specific implementation notes
**And** the document is concise (1-2 pages maximum)

### Story 4.4: Create ISA-18.2 Alarm Management Reference

As a **Lead Ignition Developer**,
I want **a concise ISA-18.2 alarm management reference sheet**,
So that **I understand alarm configuration patterns the agents apply**.

**Acceptance Criteria:**

**Given** the docs/isa-standards/ folder exists
**When** I create isa-18-2-alarms.md
**Then** the document explains alarm states (unacknowledged, acknowledged, cleared, suppressed)
**And** the document explains priority levels (1-4 with meanings)
**And** the document covers deadband and shelving concepts
**And** the document shows how concepts map to Ignition alarm configuration
**And** the document is concise (1-2 pages maximum)

### Story 4.5: Create ISA-88 Batch and IEC 62443 Security References

As a **Lead Ignition Developer**,
I want **concise reference sheets for ISA-88 and IEC 62443**,
So that **I understand batch patterns and security awareness the agents apply**.

**Acceptance Criteria:**

**Given** the docs/isa-standards/ folder exists
**When** I create isa-88-batch.md
**Then** the document explains batch/procedural control concepts (recipes, phases, equipment modules)
**And** the document notes when ISA-88 applies (food processing, pharmaceuticals, chemicals)
**And** the document is concise (1 page maximum)
**When** I create iec-62443-security.md
**Then** the document explains OT security awareness concepts (zones, conduits, IT/OT boundaries)
**And** the document notes this is awareness only, not a security implementation guide
**And** the document is concise (1 page maximum)

### Story 4.6: Create Customize File Reference Documentation

As a **Lead Ignition Developer**,
I want **documentation explaining what each agent knows**,
So that **I understand how the customize files work**.

**Acceptance Criteria:**

**Given** the docs/ folder exists
**When** I create customize-file-reference.md
**Then** the document lists all 7 customize files (analyst, pm, architect, ux-designer, dev, qa, sm)
**And** the document summarizes key memories per agent
**And** the document summarizes critical_actions per agent
**And** the document explains how to modify customize files
**And** the document references the recompile workflow (`npx bmad-method install` → Recompile Agents)

---

## Epic 5: Dairy Pilot — End-to-End Validation

Developer can follow the complete workflow on a real example project (dairy simulator), proving the entire system works end-to-end.

### Story 5.1: Create Dairy Pilot Project Brief

As a **Lead Ignition Developer**,
I want **a complete project brief for the dairy pilot**,
So that **there is a real example of Analyst workflow output**.

**Acceptance Criteria:**

**Given** the bmm-analyst customize file is populated (Epic 2)
**When** I run the Analyst workflow for a dairy processing facility scenario
**Then** the brief captures equipment types (tanks, pasteurizers, refrigeration, conveyors)
**And** the brief captures alarm philosophy (priority approach, shelving needs)
**And** the brief captures operator workflows (shift patterns, acknowledgment roles)
**And** the brief is saved to `_bmad-output/planning-artifacts/dairy-pilot/brief.md`

### Story 5.2: Create Dairy Pilot PRD

As a **Lead Ignition Developer**,
I want **a complete PRD for the dairy pilot**,
So that **there is a real example of PM workflow output with ISA-95 hierarchy**.

**Acceptance Criteria:**

**Given** the dairy pilot brief exists (Story 5.1)
**When** I run the PM workflow using the brief
**Then** the PRD includes ISA-95 equipment hierarchy for dairy equipment
**And** the PRD references ISA-101 for HMI requirements
**And** the PRD references ISA-18.2 for alarm management
**And** the PRD references ISA-88 for batch processing (pasteurization)
**And** the PRD is saved to `_bmad-output/planning-artifacts/dairy-pilot/prd.md`

### Story 5.3: Create Dairy Pilot Architecture

As a **Lead Ignition Developer**,
I want **a complete architecture document for the dairy pilot**,
So that **there is a real example of Architect workflow output with UDT designs**.

**Acceptance Criteria:**

**Given** the dairy pilot PRD exists (Story 5.2)
**When** I run the Architect workflow using the PRD
**Then** the architecture includes UDT structures for dairy equipment (Tank, Pasteurizer, Compressor)
**And** the architecture includes tag path structure following ISA-95
**And** the architecture includes alarm configuration patterns per ISA-18.2
**And** the architecture is saved to `_bmad-output/planning-artifacts/dairy-pilot/architecture.md`

### Story 5.4: Create Dairy Pilot Project Context Template

As a **Lead Ignition Developer**,
I want **a reusable project context template for the dairy pilot**,
So that **dev agents can consume project-specific context during implementation**.

**Acceptance Criteria:**

**Given** the dairy pilot architecture exists (Story 5.3)
**When** I create the project context template
**Then** the template encodes Ignition platform constraints (Jython 2.7, Perspective JSON)
**And** the template encodes ISA standards relevant to dairy (ISA-95, ISA-101, ISA-18.2, ISA-88)
**And** the template encodes project-specific context (equipment list, tag structure, UDT definitions)
**And** the template is usable by dev agents for story implementation

### Story 5.5: Document Dairy Pilot Walkthrough

As a **Lead Ignition Developer**,
I want **a walkthrough guide for the dairy pilot example**,
So that **other developers can understand how to use and extend the example**.

**Acceptance Criteria:**

**Given** all dairy pilot artifacts exist (Stories 5.1-5.4)
**When** I create `examples/dairy-pilot/README.md`
**Then** the document explains the purpose of the dairy pilot (end-to-end validation)
**And** the document references the planning artifacts in `_bmad-output/planning-artifacts/dairy-pilot/`
**And** the document explains how the pilot validates the customize file approach
**And** the document provides guidance for extending or adapting the example

---

## Epic 6: Onboarding & Polish

A new developer can clone the repo, follow the quickstart, and have working AI agents within 60 minutes.

### Story 6.1: Create Quickstart Guide

As a **Lead Ignition Developer**,
I want **a step-by-step quickstart guide**,
So that **I can get working AI agents in under 60 minutes**.

**Acceptance Criteria:**

**Given** the docs/ folder exists
**When** I create quickstart.md
**Then** the document lists prerequisites (Node.js, Git, Neovim + ignition.nvim recommended)
**And** the document provides clone and install steps (`git clone`, `npx bmad-method install`)
**And** the document guides the user through their first workflow (Analyst → brief)
**And** the document can be completed in under 60 minutes (NFR-DX1)
**And** the document links to video walkthrough for visual learners

### Story 6.2: Create Ignition Development Lifecycle Guide

As a **Lead Ignition Developer**,
I want **a reference guide for the BMAD + Ignition workflow**,
So that **I understand when to use each agent**.

**Acceptance Criteria:**

**Given** the docs/ folder exists
**When** I create ignition-development-lifecycle.md
**Then** the document explains the agent flow (Analyst → PM → Architect → UX → Dev → QA)
**And** the document explains when each agent is used
**And** the document covers parallel development with SM agent
**And** the document references the customize files that power each agent

### Story 6.3: Create Git Merge Strategies Guide

As a **Lead Ignition Developer**,
I want **guidance for handling customize file conflicts**,
So that **I can update bmad-ignition without losing my modifications**.

**Acceptance Criteria:**

**Given** the docs/ folder exists
**When** I create git-merge-strategies.md
**Then** the document explains common conflict scenarios (customize file updates)
**And** the document provides stash/pull/pop workflow
**And** the document provides manual merge guidance
**And** the document explains the recompile workflow after resolving conflicts

### Story 6.4: Polish README and Repository Structure

As a **Lead Ignition Developer**,
I want **a polished repository ready for public use**,
So that **the project makes a good first impression**.

**Acceptance Criteria:**

**Given** all other epics are complete
**When** I polish the repository
**Then** README.md links to quickstart.md as the primary entry point
**And** README.md includes a brief project description and value proposition
**And** .gitignore excludes appropriate files (.pyc, Designer cache, IDE files)
**And** CHANGELOG.md documents v1.0 release with all features
**And** LICENSE file exists with appropriate license

### Story 6.5: Record Video Walkthrough

As a **Lead Ignition Developer**,
I want **a video walkthrough demonstrating the complete workflow**,
So that **visual learners can quickly understand how to use bmad-ignition**.

**Acceptance Criteria:**

**Given** all documentation and examples are complete
**When** I record the video walkthrough
**Then** the video is 10-15 minutes long
**And** the video demonstrates clone → install → first workflow
**And** the video shows agent customizations in action (ISA standards references, Jython 2.7)
**And** the video is linked from README.md and quickstart.md
