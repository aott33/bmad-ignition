---
stepsCompleted: [1, 2, 3, 4, 5, 6]
workflow_completed: true
inputDocuments:
  - brainstorming-session-2026-02-17.md
  - project-context.md
date: 2026-02-17
author: AndrewOtt
---

# Product Brief: bmad-ignition

## Executive Summary

bmad-ignition is a starter kit and reference implementation for using AI coding agents on Ignition SCADA/MES projects. It packages a working workflow — BMAD Method for orchestration, customize files for Ignition domain knowledge, ignition-lint for validation — so controls engineers and Ignition developers don't have to figure it out themselves.

The problem is straightforward: generic AI coding tools don't understand Ignition. They hallucinate Jython 2.7 syntax, invent UDT properties, bind to non-existent tag paths, and misconfigure alarms. In industrial automation, these aren't just bugs — they're risks. Most teams either avoid AI agents entirely or waste hours fixing the output.

bmad-ignition provides the missing layer: domain-specific context injection and validation guardrails that make AI agents usable in this space. v1 focuses on BMAD agent customization and ignition-lint integration. Future versions will add MCP primitives for live gateway awareness.

This is a tool, not a product. It's also a learning vehicle — the documentation reflects real problems encountered while developing the workflow. It gets better as it gets used.

---

## Core Vision

### Problem Statement

AI coding agents are transforming software development, but industrial automation has been left behind. Generic tools like Copilot and Claude Code produce code that looks plausible but fails in Ignition-specific ways — wrong scripting syntax, invalid tag paths, broken Perspective JSON structures, inappropriate alarm configurations.

Controls engineers face a choice: avoid AI agents entirely, or spend as much time fixing hallucinated output as they would have spent writing it themselves. Neither option captures the productivity gains available to other software domains.

### Problem Impact

- Teams don't benefit from AI productivity gains because they can't trust the output
- Engineers who try generic AI tools get burned and stop using them
- The gap between "modern development practices" and "industrial automation" keeps widening
- Safety concerns — valid ones — become blanket objections to any AI-assisted development

### Why Existing Solutions Fall Short

| Solution | Gap |
|----------|-----|
| Generic AI tools (Copilot, Claude) | No Ignition awareness; constant hallucinations on platform-specific patterns |
| ignition-lint, ignition.nvim | Great for validation and editing, but no multi-agent orchestration |
| BMAD Method | Excellent orchestration, but no industrial automation domain knowledge |
| Manual development | Safe but slow; doesn't leverage AI capabilities |

There's no integrated approach — until now.

### Proposed Solution

bmad-ignition assembles existing tools into a coherent, safety-first workflow:

1. **BMAD Method** for multi-agent orchestration (Analyst, PM, Architect, Developer, QA)
2. **Customize files** that inject Ignition-specific knowledge into each agent role
3. **ignition-lint** for automated validation of Perspective views and scripts
4. **ignition.nvim** for terminal-based agents with Ignition project awareness
5. **Docker template** with pre-configured bind mounts for project file access
6. **Project structure** that keeps planning artifacts and Ignition resources in one versioned repo
7. **Parallel execution model** with file-level isolation and human-gated phase transitions

### What This Provides

1. **A starting point that works** — Clone the template, install BMAD, apply the customize files, and you have AI agents that understand Ignition
2. **Safety rails for a safety-critical domain** — Validation is mandatory, not optional; agents are constrained to what they can verify
3. **Parallel agent execution model** — Foundation phase (Party Mode) followed by parallel story execution with clear file-level boundaries
4. **Documentation from real usage** — The workflow is documented because it was actually developed and tested, not theorized
5. **No lock-in** — Uses latest BMAD version; customize files adapt agents without forking anything

---

## Target Users

### Primary User: Lead Ignition Developer

**Profile:**
- Lead SCADA Developer or Automation Architect with 8+ years in industrial automation
- Deep Ignition expertise — UDT design, Perspective development, alarm configuration, system architecture
- Works at system integrators (building projects for clients) or end-user facilities (maintaining plant systems)
- Git-savvy and Docker-comfortable; familiar with modern development tooling
- Has heard the AI hype, possibly tried generic tools like Copilot, got frustrated when they didn't understand Ignition

**Problem Experience:**
- Knows AI could help with productivity but doesn't trust generic tools in this domain
- Feels the widening gap between "modern dev practices" and industrial automation workflows
- Wants to adopt better tooling but can't justify time investment for uncertain payoff
- Spends time on repetitive tasks that could theoretically be parallelized

**Success Vision:**
- Agents that help plan the project, develop the project, and review the project
- Can spin up 2-3 parallel dev agents that actually understand Ignition context
- Produces code that doesn't need complete rewriting
- Has a documented, repeatable workflow for future projects

### User Journey

**Discovery:**
- Finds bmad-ignition through LinkedIn, Ignition community Slack, or word of mouth from other developers exploring AI tooling

**Onboarding:**
- Clones the repo, follows the quickstart guide
- Runs BMAD install, builds project-context doc, follows the workflow
- First session: sees agents actually understand Ignition context without constant correction

**Core Usage:**
- Foundation phase with Party Mode for architecture and UDT design
- Parallel story execution with 2-3 dev agents working on different views/components
- Reviews PRs, runs ignition-lint validation, merges approved work

**Success Moment:**
- "Wow — I can get this to help plan the project, develop the project, AND review the project?"
- First time the full workflow produces usable output without extensive rework

**First Project:**
- Greenfield pilot project with lower stakes — internal tool, dashboard, or proof-of-concept
- Validates the workflow end-to-end before applying to client projects

---

## Success Metrics

### User Success Metrics

- **Agent output quality:** Agents produce Ignition code that doesn't require complete rewriting
- **Workflow completion:** Full workflow (plan → develop → review) completes successfully on a pilot project
- **Parallel execution:** 2-3 agents work simultaneously without file conflicts or hallucination issues
- **Context retention:** Agents demonstrate understanding of Ignition-specific patterns (Jython 2.7, UDTs, Perspective JSON) without constant correction

### Tool Objectives

Since bmad-ignition is a tool (not a commercial product), success is measured by utility and adoption readiness:

- **Documentation completeness:** Framework is documented enough that another Lead Developer could use it without hand-holding
- **End-to-end validation:** Workflow is validated on at least one real greenfield project
- **Portfolio credibility:** Serves as a credible demonstration of bridging SCADA expertise with modern agentic development practices

### Key Performance Indicators

| Metric | Target | Measurement |
|--------|--------|-------------|
| ignition-lint pass rate | >90% on first generation | Lint results on agent-generated Perspective views |
| Correction time | <50% vs. generic tools | Time spent fixing agent output compared to Copilot/Claude baseline |
| Parallel story completion | 2-3 stories/sprint | Stories completed simultaneously without merge conflicts |
| Hallucination rate | Zero on tag paths/UDT refs | Bindings that reference non-existent tags or properties |

---

## MVP Scope

### Core Features (v1)

1. **BMAD Agent Customize Files**
   - `.customize.yaml` files for Analyst, PM, Architect, Developer, Scrum Master, QA
   - Ignition-specific context injection: Jython 2.7, UDT patterns, Perspective JSON, alarm configuration
   - Safety-first principles embedded in each agent role

2. **Project Template**
   - Docker Compose with Ignition 8.3, correct bind-mounts for projects and modules
   - Directory structure: `docs/`, `stories/`, `ignition/projects/`, `ignition/third-party-modules/`
   - `.gitignore` configured for Ignition projects

3. **ignition-lint Integration**
   - Pre-commit hook configuration (or manual validation guidance)
   - Documentation on running lint against agent-generated Perspective views

4. **Project Context Template**
   - `project-context.md` template with Ignition-specific technical context sections
   - Agent discipline rules for tag path verification, UDT awareness, alarm philosophy

5. **Documentation**
   - Quickstart guide: clone → install BMAD → apply customize files → run workflow
   - Workflow walkthrough: foundation phase → parallel execution → review

### Out of Scope for v1

| Feature | Rationale | Target Version |
|---------|-----------|----------------|
| MCP primitives for live gateway awareness | Requires significant development; v1 proves workflow first | v2+ |
| Automated story dependency enforcement | Manual assignment sufficient for v1 | v2+ |
| Custom IA-specific agents | Customize files for existing BMAD agents sufficient | v2+ |
| CI/CD pipeline | Placeholder only; teams can add their own | v2+ |
| Brownfield/quick-fix workflows | Greenfield-only for v1; validate core workflow first | v2+ |
| ignition.nvim deep integration | Document usage; don't require it | v2+ |

### MVP Success Criteria

v1 is successful when:
- [ ] Full workflow (Analyst → PM → Architect → Dev → QA) completes on a pilot project
- [ ] 2-3 parallel dev agents produce usable Ignition code without extensive rework
- [ ] ignition-lint passes on >90% of agent-generated views on first attempt
- [ ] Another Lead Developer can follow the quickstart and complete a basic workflow
- [ ] Zero hallucinated tag paths or UDT references in generated bindings

### Future Vision (v2+)

**MCP Module for Live Gateway Awareness:**
- `get-tag-structure` — Query existing tags and folder hierarchy
- `get-udt-definitions` — Retrieve UDT properties and parameters
- `validate-bindings` — Check that bindings reference real tags before commit

**Expanded Workflows:**
- Brownfield enhancement workflow (analyze existing project, generate enhancement specs)
- Quick-fix workflow (targeted bug fixes and alarm tuning)

**Community Extensions:**
- Industry-specific customize files (oil & gas, food & beverage, water/wastewater)
- Pre-built UDT libraries for common equipment types
- Shared alarm philosophy templates
