---
stepsCompleted: ['step-01-init', 'step-02-discovery', 'step-02b-vision', 'step-02c-executive-summary', 'step-03-success', 'step-04-journeys', 'step-05-domain', 'step-06-innovation', 'step-07-project-type', 'step-08-scoping', 'step-09-functional', 'step-10-nonfunctional', 'step-11-polish']
inputDocuments:
  - product-brief-bmad-ignition-2026-02-17.md
  - brainstorming-session-2026-02-17.md
documentCounts:
  briefs: 1
  research: 0
  brainstorming: 1
  projectDocs: 0
workflowType: 'prd'
classification:
  projectType: developer_tool
  domain: process_control
  complexity: high
  projectContext: greenfield
  keyStandards:
    - ISA-101 (High Performance HMI)
    - ISA-95 (Enterprise Integration)
    - ISA-88 (Batch Processing)
    - ISA-18.2 (Alarm Management)
    - IEC 62443 (OT Cybersecurity)
  enterpriseContext:
    - CESMII SM Profiles
    - i3X API (future integration)
---

# Product Requirements Document - bmad-ignition

**Author:** AndrewOtt
**Date:** 2026-02-18

## Executive Summary

bmad-ignition is a starter kit and documented process for using AI coding agents on Ignition SCADA/MES projects. It assembles existing tools—BMAD Method for orchestration, customize files for Ignition domain knowledge, ignition-lint for validation—into a coherent, safety-conscious workflow.

The problem: Generic AI coding tools don't understand Ignition. They hallucinate Jython 2.7 syntax, invent UDT properties, bind to non-existent tag paths, and misconfigure alarms. In industrial automation, these aren't bugs—they're risks. Most teams either avoid AI agents entirely or waste hours fixing the output.

bmad-ignition provides the missing layer: domain-specific context injection, ISA standards alignment (see Project Classification), and validation guardrails that make AI agents usable in this space. The target user is a Lead Ignition Developer who wants AI productivity gains but can't trust generic tools in a safety-critical domain.

This is a tool and learning vehicle, not a commercial product. It documents a working workflow for personal use and potential community contribution. Success means another Ignition developer can clone the repo, follow the quickstart, and have working AI agents that understand their domain—without weeks of figuring it out themselves.

### What Makes This Special

**It's documentation, not invention.** The value isn't in novel technology—it's in showing how existing tools (BMAD, ignition-lint, ignition.nvim, Docker, Git) fit together for AI-assisted Ignition development. The knowledge currently scattered across tool docs and tribal experience gets captured in one place.

**Standards-first design.** Agents don't just produce "working" code—they produce code aligned with professional IA engineering standards: ISA-101 for HMI design, ISA-95 for data model hierarchy, ISA-88 for batch processing, ISA-18.2 for alarm management. This is how industrial systems should be built.

**Honest scope.** This is a reference implementation and portfolio piece. If it only helps the author, that's acceptable. If it helps the broader Ignition community adopt AI tooling safely, that's the upside. The documented process may reveal gaps that lead to better tooling or a refined stack.

## Project Classification

| Dimension | Value |
|-----------|-------|
| **Project Type** | Developer Tool |
| **Domain** | Process Control (Industrial Automation) |
| **Complexity** | High |
| **Project Context** | Greenfield |
| **Key Standards** | ISA-101, ISA-95, ISA-88, ISA-18.2, IEC 62443 |
| **Enterprise Context** | CESMII SM Profiles, i3X API (future integration) |

## Success Criteria

### User Success

- **Time to first PRD/design output:** <1 hour for a small HMI/SCADA project scope capture
- **Subsequent documents:** Time varies by document complexity and project scale
- **Self-service documentation:** Another Ignition developer can follow the quickstart and video walkthrough without needing to ask questions
- **Iterative validation:** Documentation tested and refined through actual use

### Technical Success

- **ISA-95 structure created:** Equipment hierarchy modeled correctly (Enterprise → Site → Area → Unit → Equipment Module)
- **Database designed (where applicable):** Schema for equipment, tag metadata, or other persistent data when more efficient than tag-only approach
- **UDTs created from models:** UDT definitions derive from database models or ISA-95 structure
- **Tag instances bound:** Instances created with parameter mapping to simulator/PLC paths (manual mapping acceptable for MVP; automation TBD)
- **Screens working:** Perspective views render correctly with live data bindings
- **ISA standards alignment:** All agent output reflects ISA-101 (HMI), ISA-95 (hierarchy), ISA-88 (batch), ISA-18.2 (alarms), IEC 62443 (security)

### Business Success

- **Workflow validated end-to-end:** Full BMAD lifecycle (Analyst → PM → Architect → Dev → QA) completes on pilot project
- **Documentation complete:** Written docs + video walkthrough sufficient for public sharing
- **Portfolio demonstration:** Credibly demonstrates bridging SCADA expertise with modern agentic development

### Measurable Outcomes

| Metric | Target | Measurement |
|--------|--------|-------------|
| ignition-lint pass rate | >90% first generation | Lint results on agent-generated Perspective views |
| Hallucination rate | Zero on tag paths/UDT refs | Bindings referencing non-existent tags or properties |
| Correction time | <50% vs. generic tools | Time fixing agent output vs. Copilot/Claude baseline |
| Parallel story completion | 2-3 stories/sprint | Stories completed simultaneously without merge conflicts |

## Product Scope

This section provides a high-level overview. See **Project Scoping & Phased Development** for detailed phase breakdown and implementation strategy.

| Phase | Focus |
|-------|-------|
| **MVP** | Customize files, ISA standards alignment, dairy pilot, documentation |
| **Growth** | MCP primitives, brownfield workflows, deeper tooling integration |
| **Vision** | CESMII/i3X integration, industry-specific templates, community contributions |

## User Journeys

### Lead Ignition Developer: First Project with bmad-ignition

**Persona: Marcus Chen**
- Senior Controls Engineer at a mid-size system integrator, 10 years in industrial automation
- Deep Ignition expertise — designed UDT libraries, built alarm systems, deployed Perspective to a dozen plants
- Comfortable with Git, uses Docker for dev environments, runs Linux on his dev machine
- Tried Copilot on an Ignition project last year — spent more time fixing hallucinated code than it saved
- Heard about BMAD from a colleague, skeptical but curious

**Opening Scene: The Frustration**

Marcus has a new greenfield project — a small dairy processing facility needs an HMI refresh. Standard stuff: tank levels, compressor status, alarm management, trending. He's done this a hundred times manually.

He's seen web developers spinning up apps in hours with AI assistance. Meanwhile, he's still hand-coding Perspective views, manually building UDT hierarchies, writing the same alarm configurations for the hundredth time. There has to be a better way.

He finds bmad-ignition through a LinkedIn post. "AI agents for Ignition that actually understand the platform?" Skeptical, but the README mentions Jython 2.7 awareness and ISA standards alignment. That's specific enough to be credible.

**Rising Action: Getting Started**

Marcus clones the repo. The quickstart says: install BMAD, run Docker Compose, follow the workflow. He watches the 10-minute video walkthrough while Docker pulls the Ignition image.

First surprise: the BMAD install just works. The customize files are already there. No wrestling with configuration.

He starts with the Analyst agent, describes his dairy project requirements. The agent asks the right questions — not generic software questions, but questions about equipment types, alarm philosophy, operator workflows. It feels like talking to another controls engineer.

Within 45 minutes, he has a project brief that captures the scope better than most client kickoff meetings.

**Climax: The "Aha!" Moment**

Marcus runs the PM workflow to generate a PRD. The agent produces an ISA-95 equipment hierarchy without being told. It suggests UDT structures that align with how he'd design them himself. The alarm section references ISA-18.2 principles — prioritization, rationalization, shelving rules.

He moves to the Architect phase. The agent designs a data model, proposes database tables for equipment metadata, generates UDT definitions. When Marcus reviews the output, he realizes: *this is actually usable*. Not perfect, but 80% there. The remaining 20% is refinement, not rewriting.

He spins up two parallel dev agents to work on different Perspective views. They work on separate files. No conflicts. ignition-lint catches a binding issue before he even looks at the code.

**Resolution: The New Reality**

Marcus completes the pilot project in a fraction of the usual time. The output isn't perfect — he still reviews everything, still makes corrections. But the correction time is way down. The agents aren't replacing him; they're handling the tedious parts so he can focus on the design decisions that matter.

He starts his next project the same way. The workflow is repeatable. The documentation he wished existed when he started — he now has it, and it works.

### Journey Requirements Summary

| Capability Area | Requirements |
|-----------------|--------------|
| **Onboarding** | Quickstart guide, video walkthrough, Docker Compose that "just works" |
| **Analysis Phase** | Analyst agent with IA domain knowledge |
| **Planning Phase** | PM agent producing ISA-95 aligned PRDs |
| **Architecture Phase** | Architect agent for data models, UDT design, database schema |
| **UX Design (Optional)** | UX Designer agent for screen layouts, navigation flow, ISA-101 visual design principles |
| **Frontend Development** | Frontend agent — Perspective views, dynamic patterns, bindings, ISA-101 compliance |
| **Backend Development** | Backend agent — UDTs, tags, alarms, scripts, database integration |
| **Validation** | QA agent, ignition-lint integration |

**Agent Roles for Ignition Development:**

| Agent | Role | When Used |
|-------|------|-----------|
| **Analyst** | Requirements gathering, scope definition | Always |
| **PM** | PRD creation, ISA-95 hierarchy, feature prioritization | Always |
| **Architect** | Data model, database schema, UDT design, system architecture | Always |
| **UX Designer** | Screen layouts, navigation flow, user interaction patterns, ISA-101 visual design principles | Optional — but most projects need it |
| **Frontend Dev** | Perspective views, dynamic view patterns, bindings, Jython event handlers, ISA-101 implementation | Most projects (any project with HMI) |
| **Backend Dev** | UDTs, tag instances, alarm configuration, gateway scripts, database queries | Most projects |
| **QA** | Validation, acceptance testing, lint verification | Always |

## Domain-Specific Requirements

### Technical Constraints

| Constraint | Requirement | Enforcement |
|------------|-------------|-------------|
| **Jython 2.7** | Agents generate valid Jython syntax, not Python 3 | ignition.nvim LSP catches errors |
| **Perspective JSON** | View definitions follow Ignition schema requirements | ignition-lint validates structure |
| **Tag path validity** | Bindings reference real tags | Agent discipline + future MCP validation |
| **UDT patterns** | Follow Ignition-specific inheritance and parameter patterns | Customize file guidance |

### Required Tooling

| Tool | Purpose | MVP Status |
|------|---------|------------|
| **BMAD Method** | Agent orchestration framework | Required |
| **ignition.nvim** | Primary dev environment — LSP completions, hover docs, diagnostics, `:IgnitionDecode` | Required |
| **ignition-lint** | Validates Perspective views | Required |
| **Docker** | Ignition gateway with bind-mounted projects | Required |
| **Git** | Version control for project files | Required |

### Safety & Compliance Awareness

- **Safety-adjacent work:** Agents note when touching SIS configurations, safety interlocks, or critical alarm priorities. Notes only — no blocking.
- **Network segmentation:** Agents understand IT/OT boundary concepts. Don't generate code that violates zone boundaries. Awareness, not enforcement.
- **Engineering authority:** All agent output requires human review. Safety-related changes require engineering authority per organization policies.

### Risk Mitigations

| Risk | Severity | Mitigation |
|------|----------|------------|
| Hallucinated tag paths | High | Agent discipline rules + future MCP validation |
| Wrong Jython syntax | Medium | ignition.nvim LSP catches errors in real-time |
| Invalid `system.*` calls | Medium | ignition.nvim diagnostics flag unknown functions |
| UDT modification mid-sprint | High | Hard lock — return to Party Mode for foundation changes |
| Bad alarm priority | Medium | Code review with ISA-18.2 checks |
| JSON merge conflicts | Low | Accept risk, document recovery steps |

## Developer Tool Specific Requirements

### Platform Support

| Platform | MVP Status | Notes |
|----------|------------|-------|
| **Linux** | Primary | Omarchy (Arch-based) is the reference environment |
| **Windows** | Not documented | May work, not tested or supported |
| **macOS** | Not documented | May work, not tested or supported |

### Installation Method

```
1. Clone bmad-ignition repo
2. npx bmad-method install
3. Follow workflow guide
```

No pre-built Docker Compose — the Docker setup is an *output* of the development workflow, not a starting point.

### Development Environment

| Component | Tool | Purpose |
|-----------|------|---------|
| **Editor** | Neovim + ignition.nvim | LSP completions, diagnostics, `:IgnitionDecode` |
| **AI Agent Runtime** | Claude Code (terminal) | Agent execution environment |
| **Gateway** | Docker (generated during dev) | Ignition 8.3 with bind-mounted projects |
| **Database** | PostgreSQL (optional) | When database-backed design is needed |
| **Version Control** | Git | Project files and planning artifacts |

### Documentation Deliverables

| Document | Purpose | Format |
|----------|---------|--------|
| **Quickstart Guide** | Clone → install → first workflow | Markdown |
| **Video Walkthrough** | Visual demonstration of workflow | Video (10-15 min) |
| **Workflow Reference** | BMAD lifecycle for Ignition projects | Markdown |
| **Project Context Template** | ISA standards + Ignition constraints | Markdown template |
| **Customize File Docs** | How agent customizations work | Markdown |

### Example Project: Dairy Simulator

**Scope:** Complete, working SCADA system — end-to-end implementation, not just a demo.

**Includes:**
- Full BMAD workflow artifacts (brief, PRD, architecture, stories)
- ISA-95 equipment hierarchy (tanks, compressors, refrigeration)
- UDT definitions for dairy equipment
- Perspective views (Overview, Tank Detail, Refrigeration, Trends, Alarms)
- Alarm configuration per ISA-18.2
- Docker Compose setup (as workflow output)
- Database schema (if applicable)

**Validation:** System runs against Ignition's built-in Dairy Simulator — live data, working HMI.

### Reference Templates

Rather than recreating infrastructure, bmad-ignition references existing community templates:

| Template | Source | Use Case |
|----------|--------|----------|
| **Ignition + PostgreSQL** | [bw-design-group/templates.ignition.postgres](https://github.com/bw-design-group/templates.ignition.postgres) | Docker setup with Liquibase migrations |
| *(others TBD)* | — | Additional templates to be identified |

**Note:** Need to identify additional reference templates for:
- Basic Ignition-only Docker setup
- Ignition with different database backends
- CI/CD pipeline examples
- ignition-lint integration patterns

## Project Scoping & Phased Development

### MVP Strategy & Philosophy

**MVP Approach:** Problem-solving MVP — prove the workflow works end-to-end on a single pilot project.

**Resource Requirements:** Solo developer (AndrewOtt). MVP scope calibrated for one person.

### MVP Feature Set (Phase 1)

**Core Deliverables:**
- BMAD agent customize files with Ignition domain knowledge (pre-populated, shipped with repo)
- ISA standards reference documentation (ISA-101, ISA-95, ISA-88, ISA-18.2, IEC 62443)
- Project context template with Ignition constraints
- Quickstart documentation + video walkthrough
- End-to-end dairy simulator pilot project
- ignition-lint integration guidance
- Deeper ignition.nvim integration

**Customize File Strategy:**
- Pre-populate `_bmad/_config/agents/*.customize.yaml` files with Ignition domain knowledge
- Ship these files as part of the bmad-ignition repo
- User clones → `npx bmad-method install` → customizations applied automatically
- **Technical validation needed:** Confirm BMAD install respects existing customize file content

**ISA Standards Implementation (informed by Fuuz Claude Skills research):**

| Standard | Implementation |
|----------|----------------|
| **ISA-95** | Equipment hierarchy patterns (Enterprise → Site → Area → Line → Cell → Asset) in architect customize file |
| **ISA-101** | High Performance HMI principles in UX designer and frontend dev customize files |
| **ISA-88** | Batch/procedural control patterns in architect and backend dev customize files |
| **ISA-18.2** | Alarm management patterns (states, limit types, deadband, priority) in backend dev customize file |
| **IEC 62443** | OT cybersecurity awareness notes in all agent customize files |

**Agent Customize Files (MVP):**

| Agent | Ignition Customizations |
|-------|------------------------|
| **bmm-analyst** | IA-specific requirements questions, SCADA scope patterns |
| **bmm-pm** | ISA-95 hierarchy awareness, Ignition feature prioritization |
| **bmm-architect** | UDT design patterns, database schema, ISA-95/ISA-88 data models |
| **bmm-ux-designer** | ISA-101 HMI principles, Perspective navigation patterns |
| **bmm-dev** (frontend) | Perspective view patterns, dynamic views, bindings, Jython 2.7 |
| **bmm-dev** (backend) | UDTs, tags, alarms (ISA-18.2), gateway scripts, database queries |
| **bmm-qa** | ignition-lint integration, Ignition-specific test patterns |

### Post-MVP Features (Phase 2)

- MCP primitives for live gateway awareness (get-tag-structure, get-udt-definitions, validate-bindings)
- Automated story dependency enforcement
- Brownfield/quick-fix workflows
- CESMII SM Profile import for standardized asset modeling
- i3X API integration for cross-platform data access

### Vision Features (Phase 3)

- Industry-specific customize files (oil & gas, food & beverage, water/wastewater)
- Community contributions and shared UDT libraries

### Risk Mitigation Strategy

| Risk | Severity | Mitigation |
|------|----------|------------|
| **BMAD overwrites customize files on install** | High | Validate Option A during implementation; fallback to post-install script |
| **Customize files don't produce meaningful improvement** | High | Dairy pilot validates output quality vs. generic AI baseline |
| **ISA standards too broad for MVP** | Medium | Focus on patterns that directly improve agent output; document concepts, implement templates |
| **Solo developer bandwidth** | Medium | MVP scope deliberately constrained; iterate based on pilot results |

## Functional Requirements

### Onboarding & Setup

- FR1: Developer can clone the bmad-ignition repository and have a working directory structure immediately
- FR2: Developer can run a single install command (`npx bmad-method install`) to configure all agent customizations
- FR3: Developer can follow quickstart documentation to produce first workflow output within one hour
- FR4: Developer can watch a video walkthrough demonstrating the complete workflow visually

### Requirements & Analysis

- FR5: Analyst agent can gather industrial automation requirements using domain-specific questions
- FR6: Analyst agent can identify equipment types, alarm philosophy, and operator workflows during discovery
- FR7: Developer can generate a project brief that captures SCADA/MES scope accurately

### Planning & PRD Creation

- FR8: PM agent can produce PRD documents with ISA-95 equipment hierarchy
- FR9: PM agent can align requirements with ISA-101, ISA-88, and ISA-18.2 standards
- FR10: PM agent can prioritize features appropriate for Ignition platform capabilities
- FR11: Developer can iterate on PRD content through collaborative refinement

### Architecture & Data Modeling

- FR12: Architect agent can design ISA-95 compliant equipment hierarchies (Enterprise → Site → Area → Line → Cell → Asset)
- FR13: Architect agent can propose UDT structures aligned with Ignition best practices
- FR14: Architect agent can design database schemas for equipment metadata when applicable
- FR15: Architect agent can apply ISA-88 batch/procedural control patterns
- FR16: Developer can review and refine architectural decisions before implementation

### UX Design

- FR17: UX Designer agent can create screen layouts following ISA-101 High Performance HMI principles
- FR18: UX Designer agent can design navigation flows appropriate for operator workflows
- FR19: UX Designer agent can specify visual design patterns for industrial displays

### Frontend Development

- FR20: Frontend agent can generate Perspective view JSON definitions with correct schema
- FR21: Frontend agent can create dynamic view patterns (templates, indirect bindings)
- FR22: Frontend agent can write valid Jython 2.7 event handlers (not Python 3)
- FR23: Frontend agent can create bindings that reference valid tag paths
- FR24: Frontend agent can apply ISA-101 principles in view implementation

### Backend Development

- FR25: Backend agent can create UDT definitions following Ignition patterns
- FR26: Backend agent can configure tag instances with appropriate parameter mappings
- FR27: Backend agent can implement alarm configurations per ISA-18.2 (priorities, states, shelving)
- FR28: Backend agent can write gateway scripts in valid Jython 2.7
- FR29: Backend agent can design database queries for equipment data when applicable

### Validation & Quality

- FR30: QA agent can validate agent output against Ignition requirements
- FR31: Developer can run ignition-lint to validate Perspective views before import
- FR32: ignition.nvim can provide LSP completions and diagnostics during development
- FR33: Developer can identify and correct agent errors before deployment

### Parallel Development

- FR34: Multiple dev agents can work on separate Perspective views without file conflicts
- FR35: Developer can run parallel stories during the same sprint
- FR36: Version control can track changes across parallel development streams

### Documentation & Reference

- FR37: Developer can access ISA standards reference documentation within the project
- FR38: Developer can use project context templates that encode Ignition constraints
- FR39: Customize file documentation can explain how agent customizations work
- FR40: Dairy simulator example can demonstrate complete end-to-end workflow

### Safety & Compliance Awareness

- FR41: Agents can note when touching SIS configurations or safety interlocks (awareness only)
- FR42: Agents can recognize IT/OT boundary concepts in code generation
- FR43: All agent output can be reviewed by human engineering authority before deployment

## Non-Functional Requirements

### Developer Experience

| NFR | Requirement | Verification |
|-----|-------------|--------------|
| **NFR-DX1** | First workflow output achievable within 60 minutes of repository clone (including BMAD install) | Timed walkthrough |
| **NFR-DX2** | Agent output requires <50% correction time compared to generic AI tools (Copilot/Claude baseline) | Time tracking comparison |

### Integration

| NFR | Requirement | Verification |
|-----|-------------|--------------|
| **NFR-I1** | Agent-generated Perspective views import into Ignition 8.3+ without modification | Manual import test |
| **NFR-I2** | Generated Jython scripts produce zero syntax errors in ignition.nvim diagnostics | LSP diagnostic check |
| **NFR-I3** | Generated Perspective views pass ignition-lint validation on first attempt (>90% target) | Lint results |
| **NFR-I4** | Customize files load correctly with `npx bmad-method install` | Installation test |

### Output Quality

| NFR | Requirement | Verification |
|-----|-------------|--------------|
| **NFR-Q1** | Agent-generated tag paths reference only tags defined in project context | Manual review |
| **NFR-Q2** | Agent-generated UDT references use only UDT definitions that exist | Binding validation |
| **NFR-Q3** | Agent-generated scripts contain zero undefined `system.*` function calls | ignition.nvim diagnostics |
| **NFR-Q4** | Agent-generated UDT structures follow Ignition community naming conventions and patterns | Code review |

### Error Handling & Resilience

| NFR | Requirement | Verification |
|-----|-------------|--------------|
| **NFR-E1** | Agent errors recoverable without full workflow restart | Recovery test |
| **NFR-E2** | Critical syntax errors identifiable within editor before Gateway import attempt | LSP feedback timing |

### Maintainability

| NFR | Requirement | Verification |
|-----|-------------|--------------|
| **NFR-M1** | Customize files can be updated without breaking existing projects | Regression test |
| **NFR-M2** | Documentation explicitly states supported versions: latest stable releases only; breaking changes documented in CHANGELOG | Doc review |
| **NFR-M3** | ISA standards references include citation sources | Doc review |
| **NFR-M4** | Customize file changes validated against dairy pilot output before release | Pilot regression test |

## Acknowledgments

Customize file structure and ISA standards implementation patterns informed by [Fuuz Claude Skills](https://github.com/fuuz-io). Attribution to be included in project README.

Reference templates from [BW Design Group](https://github.com/bw-design-group/templates.ignition.postgres) for Docker/Liquibase patterns.
