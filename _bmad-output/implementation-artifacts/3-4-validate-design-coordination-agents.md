# Story 3.4: Validate Design & Coordination Agents

Status: in-progress

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a **Lead Ignition Developer**,
I want **to verify the UX Designer, SM, and QA agents work correctly**,
So that **the full agent suite is ready for end-to-end use**.

## Acceptance Criteria

1. **Given** bmm-ux-designer, bmm-sm, and bmm-qa customize files are populated and recompiled
   **When** I test the UX Designer on an HMI layout task (e.g., "Design tank overview screen")
   **Then** the agent applies ISA-101 principles (gray background, color for abnormal)

2. **When** I test the SM on parallel story setup
   **Then** the agent assigns non-overlapping files to parallel stories

3. **When** I test the QA on a validation workflow
   **Then** the agent references ignition-lint and Ignition-specific validation steps

## Tasks / Subtasks

- [ ] Task 1: Preparation — Verify Customize Files and Recompile (AC: #1-3)
  - [ ] 1.1 Verify bmm-ux-designer.customize.yaml is properly populated (16 memories, 3 critical actions)
  - [ ] 1.2 Verify bmm-sm.customize.yaml is properly populated (15 memories, 4 critical actions)
  - [ ] 1.3 Verify bmm-qa.customize.yaml is properly populated (16 memories, 4 critical actions)
  - [ ] 1.4 Run `npx bmad-method install` → "Recompile Agents" to apply all customizations
  - [ ] 1.5 Verify compiled agents exist in `_bmad/bmm/agents/` (ux-designer.md, sm.md, qa.md)

- [ ] Task 2: Test UX Designer Agent — ISA-101 HMI Task (AC: #1)
  - [ ] 2.1 Load the UX Designer agent in a fresh LLM context
  - [ ] 2.2 Provide test task: "Design tank overview screen for a dairy processing area with 4 tanks showing level, temperature, and valve status"
  - [ ] 2.3 Evaluate output for ISA-101 compliance:
    - [ ] Gray/neutral background specified (not white or colored)
    - [ ] Color reserved for abnormal conditions only
    - [ ] Analog representations preferred (bar graphs, trends)
    - [ ] Navigation following ISA-95 hierarchy
    - [ ] Alarm visualization zone specified
  - [ ] 2.4 Document findings: Does agent reference ISA-101? Does it apply High Performance HMI principles?
  - [ ] 2.5 If behavior insufficient, document escalation needed (persona adjustment)

- [ ] Task 3: Test SM Agent — Parallel Story Setup (AC: #2)
  - [ ] 3.1 Load the SM agent in a fresh LLM context
  - [ ] 3.2 Provide test scenario: "Set up parallel stories for implementing Tank Overview View and Pasteurizer Control View, both in the same sprint"
  - [ ] 3.3 Evaluate output for file isolation awareness:
    - [ ] Assigns non-overlapping Perspective view folders
    - [ ] Documents file ownership per story
    - [ ] References feature branch strategy
    - [ ] Mentions JSON merge conflict prevention
  - [ ] 3.4 Document findings: Does agent prevent file overlap? Does it reference Ignition project structure?
  - [ ] 3.5 If behavior insufficient, document escalation needed

- [ ] Task 4: Test QA Agent — Validation Workflow (AC: #3)
  - [ ] 4.1 Load the QA agent in a fresh LLM context
  - [ ] 4.2 Provide test scenario: "Review a Perspective view submission for a Tank Overview screen"
  - [ ] 4.3 Evaluate output for validation awareness:
    - [ ] References ignition-lint and >90% pass rate requirement
    - [ ] Mentions Jython 2.7 syntax validation (rejects Python 3 constructs)
    - [ ] References ISA standards compliance checking
    - [ ] Mentions tag path existence verification
  - [ ] 4.4 Document findings: Does agent apply QA validation criteria? Does it reference acceptance thresholds?
  - [ ] 4.5 If behavior insufficient, document escalation needed

- [ ] Task 5: Document Validation Results (AC: #1-3)
  - [ ] 5.1 Create validation summary table with pass/fail for each agent
  - [ ] 5.2 Document any behavior gaps or escalation needs
  - [ ] 5.3 If all agents pass, confirm Epic 3 is ready for retrospective
  - [ ] 5.4 Update story status and sprint-status.yaml

## Dev Notes

### Story Purpose — Validation Story Pattern

This is a **VALIDATION STORY** for Epic 3, following the same pattern as Story 1.5 (Epic 1) and Story 2.4 (Epic 2). Validation stories verify that the customize file approach produces measurable behavior change in agents.

**Why This Matters:**
- Stories 3.1-3.3 populated three customize files (UX Designer, SM, QA)
- This story VALIDATES that the agents actually exhibit Ignition/ISA domain knowledge
- Without validation, we can't confirm the customization approach works for these agents
- This is the gate before Epic 3 can be marked complete

**Relationship to Other Epic 3 Stories:**
- **Story 3.1:** Populated UX Designer with ISA-101 HMI principles → This story validates ISA-101 application
- **Story 3.2:** Populated SM with parallel execution coordination → This story validates file isolation
- **Story 3.3:** Populated QA with validation patterns → This story validates QA acceptance criteria

### Critical: Testing Requires Separate LLM Context

**IMPORTANT:** Each agent test MUST be performed in a **fresh LLM context** (new chat session) to accurately measure agent behavior. Testing in the current context would pollute results with conversation history.

**Testing Process:**
1. Copy the compiled agent file content (`_bmad/bmm/agents/{role}.md`)
2. Start a new LLM conversation (ideally with the same or similar model)
3. Paste the agent content as system prompt or initial context
4. Provide the test task
5. Evaluate the agent's response against acceptance criteria
6. Document results back in this story file

**Alternative Testing Approach:**
- Use Claude Code's `/agents` command if available
- Or use BMAD's built-in agent loading mechanism
- Or manually copy the customize file content into a fresh session

### Manual Testing Instructions

Since this validation requires testing in separate LLM instances, here are the exact steps:

**Step 1: Copy Customize Files for Testing**

Copy these files to share with the testing LLM:
- `_bmad/bmm/agents/ux-designer.md` (compiled UX Designer agent)
- `_bmad/bmm/agents/sm.md` (compiled SM agent)
- `_bmad/bmm/agents/qa.md` (compiled QA agent)

**Step 2: UX Designer Test (AC: #1)**

In a fresh LLM session, provide:
```
You are a UX Designer agent for Ignition industrial automation projects.

[Paste contents of _bmad/bmm/agents/ux-designer.md here]

---

TASK: Design a tank overview screen for a dairy processing area with 4 tanks showing level, temperature, and valve status.
```

**Evaluation Checklist for UX Designer:**
- [ ] Specifies gray/neutral background (mentions `#808080` range or neutral colors)
- [ ] Color reserved for abnormal only (red for alarms, yellow for warnings, NOT green for normal)
- [ ] Uses analog representations (bar graphs, trends, not just digital numbers)
- [ ] Navigation follows ISA-95 hierarchy (Site → Area → Line → Cell → Equipment)
- [ ] Mentions alarm visualization zone placement
- [ ] References ISA-101 explicitly

**Step 3: SM Agent Test (AC: #2)**

In a fresh LLM session, provide:
```
You are a SM (Scrum Master) agent for Ignition industrial automation projects.

[Paste contents of _bmad/bmm/agents/sm.md here]

---

TASK: Set up parallel stories for implementing Tank Overview View and Pasteurizer Control View. Both need to be worked on simultaneously in the same sprint. Describe how you would assign file ownership to prevent conflicts.
```

**Evaluation Checklist for SM:**
- [ ] Assigns one view folder per agent (mentions `com.inductiveautomation.perspective/views/`)
- [ ] Documents file ownership explicitly
- [ ] References feature branch strategy
- [ ] Mentions JSON merge conflict prevention
- [ ] References Ignition project structure
- [ ] Uses story naming convention (`{epic_num}-{story_num}-{slug}.md`)

**Step 4: QA Agent Test (AC: #3)**

In a fresh LLM session, provide:
```
You are a QA agent for Ignition industrial automation projects.

[Paste contents of _bmad/bmm/agents/qa.md here]

---

TASK: Review a Perspective view submission for a Tank Overview screen. The developer claims it's ready for approval. What validation steps do you require before approving?
```

**Evaluation Checklist for QA:**
- [ ] References ignition-lint and >90% pass rate requirement
- [ ] Mentions Jython 2.7 syntax validation (rejects f-strings, type hints, walrus operator)
- [ ] References ISA standards compliance (ISA-18.2 alarms, ISA-95 hierarchy, ISA-101 HMI)
- [ ] Mentions tag path existence verification
- [ ] Applies QA validation criteria (acceptance vs rejection thresholds)
- [ ] Mentions validation sequence (LSP → lint → Gateway → Designer)

### Previous Story Intelligence (Epic 1-2 Validation Stories)

**From Story 1.5 (Dev Agent Validation):**
- Gate question format: "Does agent reference ISA standards? Avoid Python 3 syntax? Validate tag paths?"
- Document both positive findings AND escalation needs
- If behavior insufficient, note "escalation to persona adjustment required"
- Validation proves the approach before scaling

**From Story 2.4 (Planning Agents Validation):**
- Test each agent with a realistic domain task
- Evaluate against specific criteria (not vague "does it work?")
- Consistent documentation format for comparison

**Validation Success Criteria:**
- Each agent demonstrates domain knowledge from customize file
- Agents reference ISA standards where appropriate
- Agents apply Ignition-specific patterns
- No generic responses that ignore customization

### Architecture Compliance Checklist

Validation stories focus on BEHAVIORAL testing, not file changes:
- [x] Test tasks are realistic Ignition domain scenarios
- [x] Evaluation criteria are specific and measurable
- [x] Results format matches previous validation stories (1.5, 2.4)
- [x] Escalation path documented (persona adjustment if needed)

### Customize File Status (Pre-Validation)

| Customize File | Story | Memories | Critical Actions | Status |
|----------------|-------|----------|------------------|--------|
| bmm-ux-designer.customize.yaml | 3.1 | 16 | 3 | Populated |
| bmm-sm.customize.yaml | 3.2 | 15 | 4 | Populated |
| bmm-qa.customize.yaml | 3.3 | 16 | 4 | Populated |

### Compiled Agent Locations

| Agent | Source Customize File | Compiled Agent File |
|-------|----------------------|---------------------|
| UX Designer | `_bmad/_config/agents/bmm-ux-designer.customize.yaml` | `_bmad/bmm/agents/ux-designer.md` |
| SM | `_bmad/_config/agents/bmm-sm.customize.yaml` | `_bmad/bmm/agents/sm.md` |
| QA | `_bmad/_config/agents/bmm-qa.customize.yaml` | `_bmad/bmm/agents/qa.md` |

### Expected Validation Outcomes

**UX Designer (AC: #1):**
- MUST demonstrate ISA-101 High Performance HMI principles
- MUST use gray backgrounds, color for abnormal only
- SHOULD mention Perspective-specific layout guidance
- SHOULD reference safety awareness for critical displays

**SM (AC: #2):**
- MUST assign non-overlapping files
- MUST reference Ignition project structure (views, tags, scripts folders)
- SHOULD mention feature branch strategy
- SHOULD reference merge conflict prevention for JSON files

**QA (AC: #3):**
- MUST reference ignition-lint and pass rate requirements
- MUST mention Jython 2.7 syntax validation
- MUST reference ISA standards compliance checking
- SHOULD mention validation sequence and evidence requirements

### Project Structure Notes

- **Testing context:** Validation requires fresh LLM instances, not file changes
- **Results location:** Document results in Dev Agent Record section below
- **Recompile location:** `npx bmad-method install` → "Recompile Agents"
- **Sprint status update:** Update `3-4-validate-design-coordination-agents: ready-for-dev` → `done`

### References

- [Source: _bmad-output/planning-artifacts/epics.md#Story 3.4]
- [Source: _bmad-output/planning-artifacts/architecture.md#Validation Strategy]
- [Source: _bmad-output/implementation-artifacts/1-5-validate-dev-agent-behavior.md] (validation story pattern)
- [Source: _bmad-output/implementation-artifacts/2-4-validate-planning-workflow.md] (validation story pattern)
- [Source: _bmad-output/implementation-artifacts/3-1-populate-ux-designer-agent-isa-101-hmi-principles.md]
- [Source: _bmad-output/implementation-artifacts/3-2-populate-sm-agent-parallel-execution-coordination.md]
- [Source: _bmad-output/implementation-artifacts/3-3-populate-qa-agent-ignition-validation-patterns.md]
- [Source: _bmad/_config/agents/bmm-ux-designer.customize.yaml]
- [Source: _bmad/_config/agents/bmm-sm.customize.yaml]
- [Source: _bmad/_config/agents/bmm-qa.customize.yaml]

## Dev Agent Record

### Agent Model Used

Claude Opus 4.5 (claude-opus-4-5-20251101)

### Debug Log References

### Completion Notes List

### Open Questions for Sprint Retrospective

**Docker/Infrastructure Architecture:**
- Should the Architect agent include Docker Compose setup patterns for Ignition projects?
- Current Architect memories focus on data architecture (ISA-95, UDTs, database schema)
- Infrastructure patterns (Docker, Gateway config, dev environments) are not covered
- Options: Add to Architect | Keep separate | Create reference doc for SM to inject
- Decision needed: Who owns infrastructure architecture in the agent workflow?

**Deployment & Ignition Integration Gap:**
- Current architecture and epics documents describe WHAT the system should be, not HOW to deploy it
- Missing deployment guidance includes:
  - How to import the project into Ignition Designer
  - Gateway configuration steps (scan classes, alarm pipelines, identity providers)
  - OPC-UA device connection setup
  - Development environment setup for the team
  - Production deployment checklist
- Options:
  1. Add deployment memories to Architect (expand scope)
  2. Create new "DevOps" or "Infrastructure" agent role
  3. Add deployment epic to epics.md template (Epic 0: Project Setup)
  4. Create reference doc that SM injects into stories that need deployment context
- Related: The epics have Story 1.1 "Initialize project structure" but it focuses on view folders, not Gateway/Designer setup
- Decision needed: Where does deployment knowledge live in the BMAD workflow?

**Epic Ordering & Ignition Development Workflow Gap:**
- The PM agent doesn't understand Ignition's "configure once, use everywhere" UDT pattern
- **Current Problem:** Epics treat UDTs, alarms, and historian as separate concerns configured at different times:
  - Epic 2-3: Create UDT definitions with OPC bindings only
  - Epic 5.1: Go BACK to add alarm properties to existing UDTs
  - Epic 6.1-6.3: Go BACK AGAIN to configure historian scan classes on those UDTs
- **Why This Is Wrong:** In Ignition, a UDT definition should be created ONCE with ALL its properties:
  - OPC bindings (where data comes from)
  - Alarm configurations (ISA-18.2 properties, priorities, labels)
  - Historian settings (scan class assignments)
  - Parameters for instance binding
- Modifying UDT definitions after instances exist is problematic — changes propagate to instances but can cause issues
- **Correct Ignition Development Order:**
  1. Gateway setup (scan classes, alarm pipelines, identity providers, OPC devices)
  2. Database schema (equipment master data)
  3. UDT definitions (COMPLETE with OPC + alarms + historian)
  4. UDT instances (from database-driven script or manual)
  5. Perspective views (bind to fully-configured tags)
  6. Reports and dashboards (consume existing data)
- **Potential Solutions:**
  1. **PM agent memory:** Add memories to PM explaining Ignition UDT lifecycle — "Configure UDT definitions completely before instantiation"
  2. **Architecture agent enhancement:** Have Architect produce UDT specifications with ALL properties (not just structure), then PM references that spec
  3. **Epic template change:** For Ignition projects, use a different epic pattern:
     - Epic 0: Gateway & Infrastructure Setup
     - Epic 1: Data Layer (Database + Complete UDTs + Instances)
     - Epic 2+: Feature Epics (screens, controls, reports) that consume the data layer
  4. **Story dependency enforcement:** Add dev notes to PM explaining that alarm/historian config must be in same story as UDT creation
  5. **Validation workflow enhancement:** Add an "Ignition development order" check to implementation readiness that flags when UDT modification happens after initial creation
- **Root Cause:** Generic BMAD PM doesn't know that Ignition UDTs are "define once" artifacts where you can't easily add properties later
- Decision needed: How do we teach the PM agent about platform-specific development workflows?

---

## ⚠️ VALIDATION BLOCKED — Course Correction Required

**Date:** 2026-02-21
**Decision:** Pause Story 3.4 validation. Address foundational gaps before continuing.

### Issues Identified During Validation

Three significant gaps were discovered that affect the viability of the BMAD workflow for Ignition projects:

1. **No deployment/infrastructure guidance** — Architecture and epics describe WHAT to build, not HOW to set up the Ignition environment
2. **Epic ordering violates Ignition development patterns** — UDTs are modified in multiple epics instead of defined completely once
3. **UDT specifications incomplete** — Alarm and historian configs are separated from UDT creation

### Recommended Course Correction

**Approach:** The Architect agent owns the Implementation Sequence. PM references the Architecture document when ordering epics. No PM memories needed — Architect provides the sequence, PM follows it.

**Step 1: Update Architect Customize File** ✅ COMPLETED

Added 4 new memories to `bmm-architect.customize.yaml`:

```yaml
memories:
  # Implementation Sequence (4 memories)
  - "Include an **Implementation Sequence** section in every architecture document that defines the correct order of development: 1) Gateway Configuration, 2) Project/Designer Configuration, 3) Database Schema, 4) Complete UDT Definitions, 5) UDT Instance Creation, 6) Perspective Views, 7) Reports and Dashboards. The PM agent uses this sequence to order epics correctly."

  - "**Gateway Configuration** (Step 1) includes: OPC device connections (addresses, poll rates), identity providers/user sources (local users, Active Directory integration), database connections, Gateway Network configuration, and alarm notification profiles (email, SMS, voice). These are configured in the Ignition Gateway web interface before opening Designer."

  - "**Project/Designer Configuration** (Step 2) includes: scan class definitions (names, rates, execution modes), alarm pipelines (notification routing, escalation, acknowledgment requirements), Gateway Event scripts, project library scripts, and named queries. These are configured in Ignition Designer at the project level before creating tags or views."

  - "When specifying UDT definitions, include ALL properties in a single specification — OPC binding patterns, alarm configurations (setpoints, priorities, labels, consequence text per ISA-18.2), historian scan class assignments, and parameter definitions. UDT definitions should be created COMPLETE; modifying them after instances exist causes propagation issues. The Architecture document is the source of truth for complete UDT specs."
```

**Step 2: Recompile Agents**

Run `npx bmad-method install` in the bmad-ignition repo to recompile agents with new memories.

**Step 3: Re-run Architect**

Re-run Architect agent on ignition-dairy-hmi to produce updated architecture.md with:
- **Implementation Sequence** section (7 steps)
- **Gateway Configuration** section (notification profiles, OPC devices, identity providers, database connections)
- **Project/Designer Configuration** section (scan classes, alarm pipelines, Gateway Events, scripts)
- Complete UDT specifications (OPC + alarms + historian in one spec)

**Step 4: Re-run PM**

Re-run PM agent on ignition-dairy-hmi. PM will read the Architecture document and follow the Implementation Sequence to order epics correctly:
- Epic 0: Gateway & Infrastructure Setup
- Epic 1: Project/Designer Configuration
- Epic 2: Data Layer (Database + Complete UDTs + Instances)
- Epic 3+: Feature epics (screens, controls, reports)

**Step 5: Resume Story 3.4 Validation**

After course correction, resume SM and QA agent testing.

### Impact on Epic 3 Completion

- Stories 3.1, 3.2, 3.3 (customize file population) are still valid
- Story 3.4 (validation) is BLOCKED pending course correction
- Epic 3 cannot be marked complete until validation passes
- This may require a Story 3.5 or extended 3.4 for re-validation after corrections

### File List