---
stepsCompleted: [1, 2, 3, 4]
inputDocuments: ['project-context.md']
session_active: false
workflow_completed: true
session_topic: 'Designing bmad-ignition for parallel agent development on Ignition 8.3 projects'
session_goals: 'Parallel agent workflows (scrum-style), BMAD Method + Agent Builder integration, Ignition 8.3 domain context, safety/validation guardrails, simple and robust architecture'
selected_approach: 'ai-recommended'
techniques_used: ['First Principles Thinking', 'Morphological Analysis', 'Cross-Pollination', 'Chaos Engineering']
ideas_generated: []
context_file: '_bmad-output/project-context.md'
domain_resources:
  - 'https://docs.inductiveautomation.com/'
  - 'https://docs.inductiveautomation.com/docs/8.3/tutorials/ignition-8-deployment-best-practices'
  - 'https://docs.inductiveautomation.com/docs/8.3/tutorials/version-control-guide'
---

# Brainstorming Session Results

**Facilitator:** AndrewOtt
**Date:** 2026-02-17

## Session Overview

**Topic:** Designing bmad-ignition for parallel agent development (scrum-style) on Ignition 8.3 projects using BMAD Method + Agent Builder

**Goals:**
- Architect parallel agent workflows where multiple agents work on different stories simultaneously
- Leverage BMAD Method's existing orchestration + Agent Builder for custom IA-specific agents
- Deep Ignition 8.3 domain context (Jython 2.7, UDTs, Perspective, deployment best practices, version control patterns)
- Safety/validation guardrails appropriate for industrial automation
- Keep it simple, robust, and learnable

### Context Guidance

Key constraints from project-context.md:
- Ignition has real-world physical consequences (not just software bugs)
- Domain-specific: Jython 2.7, UDT hierarchies, Perspective JSON structure
- Existing tools to integrate: ignition-lint, ignition.nvim, Flint, Git Module, Docker
- BMAD Method v6 as orchestration layer
- This is both a learning vehicle AND portfolio piece

### Domain Resources

- Ignition 8.3 Documentation: https://docs.inductiveautomation.com/
- Deployment Best Practices: https://docs.inductiveautomation.com/docs/8.3/tutorials/ignition-8-deployment-best-practices
- Version Control Guide: https://docs.inductiveautomation.com/docs/8.3/tutorials/version-control-guide

### Session Setup

_Participant confirmed goals and domain context. Ready for technique selection._

## Technique Selection

**Approach:** AI-Recommended Techniques
**Analysis Context:** Parallel agent framework for Ignition 8.3 with focus on scrum-style workflows, BMAD integration, and safety guardrails

**Recommended Techniques:**

1. **First Principles Thinking** - Strip away assumptions to find the irreducible core of what parallel agents on Ignition actually requires
2. **Morphological Analysis** - Systematically map all parameter combinations (agent types, coordination, validation, domain knowledge injection)
3. **Cross-Pollination** - Transfer proven patterns from distributed teams, CI/CD, microservices orchestration
4. **Chaos Engineering** - Stress-test by deliberately breaking: hallucinated tags, concurrent UDT edits, validation failures

**AI Rationale:** Complex systems architecture problem requiring both divergent exploration and convergent simplification, with non-negotiable safety constraints from industrial automation domain.

---

## Technique Execution

### First Principles Thinking

**8 Core Principles Established:**

1. **Layer Dependencies:** Ignition development has natural layer order. Tags/UDTs/Data Model must exist before Views and Scripts can bind. Parallel work safe *within* layers, not *across* until dependencies satisfied.

2. **File-Level Isolation:** The parallel work boundary is the individual file/asset. Two agents can work on different view.json files simultaneously. Git handles merge conflicts if boundaries crossed.

3. **Upstream Coordination:** Coordination happens at story assignment time, not merge time. Orchestrator must ensure stories don't overlap on same files before agents start.

4. **Flexible Story Granularity:** A "story" is a unit of work assigned by orchestrator — granularity flexible (single view, feature slice, foundational model work) but must have clearly defined file boundaries.

5. **Layered Domain Knowledge:** Ignition knowledge isn't siloed to one agent — layered across all roles at different depths. PM scopes around assets, Architect knows structure, Developer knows deep implementation. Critical decisions require collaborative multi-agent analysis.

6. **Party Mode = Foundation Mode:** BMAD Party Mode for foundational work (PRD, Architecture, Asset Models, UDTs). Once foundation approved, shift to parallel story execution. Re-invoke Party Mode when foundation changes needed.

7. **Human-in-the-Loop Scaling:** Human selects stories and spins up 2-3 parallel agents. Scale up only for well-understood, low-risk tasks.

8. **Leverage BMAD Orchestration:** Use existing BMAD infrastructure: SM creates stories → Human reviews/approves → Human spins up dev agents → sprint-status.yaml tracks state.

---

### Morphological Analysis

**6 Key Parameters Mapped:**

| Parameter | Decision |
|-----------|----------|
| **1. Agent Customization** | `.customize.yaml` for existing BMAD agents + context injection for dynamic knowledge |
| **2. Domain Knowledge** | Layered: project-context.md (always) + static docs (reference) + IA MCP Module (live state) + web fetch (fallback) |
| **3. Tooling Integration** | ignition.nvim (LSP + lint) as primary, MCP Module for gateway state, Docker for environment, Git for version control |
| **4. Validation** | Defense in depth: LSP real-time → pre-commit hook → PR/CI check |
| **5. Coordination** | Human assigns stories with file boundaries, branch-per-story, git handles conflicts |
| **6. Dev Environment** | Direct file edit on bind-mount, MCP for read awareness, clear path documentation |

**Research Items Identified:**
- Define bmad-ignition MCP primitives (get-tag-structure, get-udt-definitions, list-project-resources, validate-perspective-view)
- Investigate IA MCP Module EA capabilities and limitations
- Document Ignition project file structure for agent reference

---

### Cross-Pollination

**Patterns Transferred from Other Domains:**

| Source Domain | Pattern | bmad-ignition Application |
|---------------|---------|---------------------------|
| **Scrum** | Sprint Planning | Party Mode for foundation + sprint planning |
| **Scrum** | Definition of Done | Lint passes + combined code review clean + human approval |
| **Scrum** | Code Review | Customized `code-review` workflow with IA safety checks |
| **CI/CD** | Dependency Graph | Stories declare layer dependencies, can't start until deps "done" |
| **CI/CD** | Gate Checks | Validation layers block merge if checks fail |
| **CI/CD** | Parallel Stages | Independent stories run in parallel |
| **ATC** | Flight Plans Filed | Story scope declared with file boundaries before agent starts |
| **ATC** | Positive Control | Human approves assignment and reviews PR |
| **Database** | Optimistic Locking | Branch-per-story, git detects conflicts at PR |

**Key Decision:** Combined code review (merge BMAD `code-review` workflow + IA safety checks from `ia-safety-reviewer` principles) rather than two separate reviews.

---

### Chaos Engineering

**Failure Scenarios Analyzed:**

| Category | Scenario | Severity | v1 Mitigation |
|----------|----------|----------|---------------|
| **Hallucination** | Agent binds view to non-existent tag path | HIGH | Agent discipline in project-context.md + future MCP validation |
| **Hallucination** | Wrong Jython syntax (Python 3 in Jython 2.7) | MEDIUM | ignition.nvim LSP catches |
| **Hallucination** | Invented UDT property | MEDIUM | Future MCP primitive for UDT structure |
| **Hallucination** | Bad alarm priority | MEDIUM | Code review IA checks + alarm philosophy doc |
| **Coordination** | Two agents edit same view | MEDIUM | Human assigns with boundaries |
| **Coordination** | UDT modified mid-sprint | HIGH | Hard lock: return to Party Mode for foundation changes |
| **Coordination** | Merge conflict in JSON | LOW | Accept risk, document recovery |
| **Coordination** | Story dependency violated | MEDIUM | Story declares deps, future automated enforcement |
| **Environment** | Gateway not running | LOW | Agent should detect and warn |
| **Environment** | Bind-mount desync | LOW | Document recovery steps |

**Top Risks and Mitigations:**

1. **Hallucinated tag paths** — Silent runtime failure
   - v1: Document rule "verify tag path exists before binding"
   - Future: MCP `validate-bindings` primitive

2. **UDT modification mid-sprint** — Breaks parallel work assumptions
   - v1: Hard lock. UDT/foundation changes require Party Mode, pause parallel work
   - Recovery: Stop agents → Party Mode → update foundation → resume

3. **JSON merge conflict** — Acceptable risk
   - v1: Git detects, human resolves
   - Document recovery steps in project docs

---

## Idea Organization and Prioritization

### Thematic Organization

**Theme 1: Workflow Architecture**
_Focus: How foundation and parallel execution phases work together_

- Party Mode for collaborative foundation work (PRD, Architecture, UDTs)
- Human-gated transition to parallel execution
- 2-3 parallel dev agents per sprint
- Branch-per-story with file-level isolation
- Hard lock on foundation during parallel execution

**Theme 2: Agent Configuration**
_Focus: How to give BMAD agents Ignition knowledge_

- `.customize.yaml` for existing agents (not new agents)
- Layered domain knowledge (project-context.md + static docs + MCP + web fetch)
- Combined code review (adversarial + IA safety checks)
- Agent discipline rules documented in project-context.md

**Theme 3: Tooling Integration**
_Focus: Which tools enable safe, validated development_

- ignition.nvim as primary environment (LSP + ignition-lint)
- IA MCP Module for live gateway state awareness
- Docker with bind-mounts for file access
- Git for version control and conflict detection
- Defense-in-depth validation (real-time → pre-commit → CI)

**Theme 4: Risk Mitigation**
_Focus: What could go wrong and how to prevent it_

- Hallucinated tag paths → agent discipline + future MCP validation
- UDT modification mid-sprint → hard lock, return to Party Mode
- JSON merge conflicts → acceptable risk, document recovery

### Prioritized Action Items

**Immediate (v1 Foundation):**

1. **Create `.customize.yaml` files for BMAD agents**
   - Add Ignition context to PM, Architect, SM, Dev agents
   - Merge IA safety checks into code-review workflow

2. **Document Ignition project file structure**
   - Path conventions for views, tags, UDTs, scripts
   - Bind-mount locations for Docker
   - Critical rules for agents in project-context.md

3. **Set up development environment template**
   - Docker Compose with correct bind-mounts
   - ignition.nvim configuration
   - Pre-commit hook for ignition-lint

**Research (Before Implementation):**

4. **Investigate IA MCP Module EA**
   - What primitives can be defined?
   - Can it expose tag structure? UDT definitions?
   - Latency and reliability for agent workflows

5. **Verify ignition-lint capabilities**
   - Does it validate binding targets?
   - Can it catch hallucinated tag paths?

**Future (Post-v1):**

6. **Build MCP primitives for validation**
   - `get-tag-structure`
   - `get-udt-definitions`
   - `validate-bindings`

7. **Automated story dependency enforcement**
   - Block story start until dependencies are "done"

---

## Session Summary and Insights

### Key Achievements

- **8 Core Principles** established for parallel agent development on Ignition
- **6 Key Parameters** mapped with clear decisions for v1
- **9 Patterns** transferred from proven domains (Scrum, CI/CD, ATC, Database)
- **10 Failure Scenarios** analyzed with mitigations identified
- **7 Prioritized Action Items** organized by timeline (Immediate, Research, Future)

### Architecture Summary

```
┌─────────────────────────────────────────────────────────┐
│  FOUNDATION PHASE (Party Mode)                          │
│  PRD → Architecture → Asset Models → UDTs               │
│  Collaborative: PM + Architect + Dev(s)                 │
│  Gate: Human approval                                   │
│  Rule: Foundation locked during parallel execution      │
└─────────────────────────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────────┐
│  PARALLEL EXECUTION PHASE                               │
│  Human assigns stories (2-3 agents max)                 │
│  Branch-per-story, file-level isolation                 │
│  ignition.nvim LSP + lint for real-time validation      │
│  Combined code review (adversarial + IA safety)         │
│  Human approval → Merge                                 │
└─────────────────────────────────────────────────────────┘
```

### Session Reflections

**What Worked Well:**
- First Principles thinking stripped away complexity to find core requirements
- Morphological Analysis forced explicit decisions on key parameters
- Cross-Pollination brought proven patterns from mature domains
- Chaos Engineering identified risks before implementation

**Key Insight:**
The simplest architecture that works: leverage existing BMAD orchestration, customize agents via `.customize.yaml`, use ignition.nvim for validation, and enforce a hard boundary between foundation work (Party Mode) and parallel execution (branch-per-story).

**Next Step:**
Begin with action item #1 — create `.customize.yaml` files for BMAD agents with Ignition context.
