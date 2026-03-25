# claude-ignition-skills

A starter kit for using Claude Code on Ignition SCADA/MES projects. No framework installs — just clone, open in Claude Code, and start building.

## The Problem

Generic AI tools don't understand Ignition. They hallucinate Jython 2.7 syntax, invent UDT properties, bind to non-existent tag paths, and misconfigure alarms. In industrial automation, these aren't just bugs — they're risks.

## How It Works

This repo provides **domain-aware context** through two mechanisms:

1. **`CLAUDE.md`** — Auto-loaded every session. Gives Claude the critical Ignition constraints (Jython 2.7 rules, safety flagging, validation requirements) without you doing anything.

2. **Slash commands** — Role-specific skills you invoke when you need deeper context for a particular task.

No npm. No framework. Just clone and go.

## Quick Start

```bash
git clone https://github.com/your-username/claude-ignition-skills.git
cd claude-ignition-skills

# Open with Claude Code
claude .
```

That's it. `CLAUDE.md` is automatically loaded. Claude now knows this is an Ignition project and will enforce Jython 2.7 constraints, safety flagging, and validation requirements in every session.

## Skills (Slash Commands)

Invoke these when you need deeper role-specific context. Type `/` in Claude Code to see them.

### `/ignition-dev`
**Use when:** writing Jython scripts, Perspective views, UDT definitions, tag configurations.

Enforces Jython 2.7 constraints, Perspective JSON structure, UDT patterns, ISA standards in code, and the full validation workflow.

```
/ignition-dev

Write a UDT definition for a centrifugal pump with ISA-18.2 alarm config
```

### `/ignition-architect`
**Use when:** designing system architecture, UDT hierarchy, database schema, integration patterns.

Covers ISA-95 equipment hierarchy, database-driven master data model, UDT inheritance design, implementation sequencing, ISA-88 batch architecture.

```
/ignition-architect

Design the UDT hierarchy and database schema for a dairy pasteurization system
```

### `/ignition-ui`
**Use when:** designing Perspective screens, navigation flows, operator UX.

Covers ISA-101 High Performance HMI principles, the full Perspective component catalog, reusable view patterns, parameter-driven faceplates.

```
/ignition-ui

Design a tank farm overview screen following ISA-101 principles
```

### `/ignition-plan`
**Use when:** writing PRDs, conducting discovery, scoping epics, planning sprints.

Covers industrial requirements discovery questions, PRD structure for Ignition projects, epic ordering (aligned with Ignition's implementation sequence), alarm philosophy, safety scoping.

```
/ignition-plan

Help me write the alarm management section of the PRD for a cheese production facility
```

### `/ignition-review`
**Use when:** reviewing Jython scripts, Perspective views, or architecture docs for correctness.

Runs structured review against Jython 2.7 compliance, Perspective structure, ISA standards, and validation evidence. Returns APPROVE or RETURN with specific findings.

```
/ignition-review

Review this gateway event script for Jython 2.7 compliance and ISA-95 tag structure
```

## Reference Docs

Use `@` to pull these into context when you need detailed reference:

| Doc | Use for |
|---|---|
| `@docs/jython-constraints.md` | Jython 2.7 syntax rules and patterns |
| `@docs/isa-standards.md` | ISA-101, ISA-95, ISA-88, ISA-18.2, IEC 62443 |
| `@docs/tag-structure.md` | Tag paths, UDT patterns, ISA-95 folder structure, DB schema |
| `@docs/perspective-components.md` | Full Perspective component reference |
| `@docs/validation-workflow.md` | ignition-lint + LSP + Gateway validation steps |
| `@docs/parallel-dev.md` | File isolation rules for multi-agent parallel work |

### Example: referencing docs in a prompt

```
Using @docs/tag-structure.md, create the tag folder structure and UDT hierarchy
for a 3-area dairy facility with refrigeration, pasteurization, and packaging areas
```

## Typical Workflows

### Starting a new project

```
/ignition-plan
Help me scope a PRD for a water treatment SCADA system
```

### Designing the architecture

```
/ignition-architect
Based on this PRD, design the UDT hierarchy and implementation sequence
```

### Building screens

```
/ignition-ui
Design the equipment detail faceplate for a centrifugal pump
```

### Writing code

```
/ignition-dev
Implement the pump faceplate as a Perspective view.json with parameter-driven tag bindings
```

### Reviewing work

```
/ignition-review
Review this Jython gateway script and pump view.json for production readiness
```

## Repo Structure

```
claude-ignition-skills/
├── CLAUDE.md                        # Auto-loaded: baseline Ignition context
├── .claude/
│   └── commands/
│       ├── ignition-dev.md          # /ignition-dev skill
│       ├── ignition-architect.md    # /ignition-architect skill
│       ├── ignition-ui.md           # /ignition-ui skill
│       ├── ignition-plan.md         # /ignition-plan skill
│       └── ignition-review.md       # /ignition-review skill
├── docs/
│   ├── jython-constraints.md        # Jython 2.7 reference
│   ├── isa-standards.md             # ISA-101, 95, 88, 18.2, IEC 62443
│   ├── tag-structure.md             # Tag paths, UDT patterns, DB schema
│   ├── perspective-components.md    # Perspective component reference
│   ├── validation-workflow.md       # ignition-lint + LSP workflow
│   └── parallel-dev.md              # Multi-agent file isolation
└── README.md
```

## ISA Standards Covered

| Standard | Coverage |
|---|---|
| **ISA-101** | High Performance HMI — color discipline, information density, navigation |
| **ISA-95** | Equipment hierarchy — tag folder structure, UDT design, DB schema |
| **ISA-88** | Batch control — procedural model, phase state machine, recipe management |
| **ISA-18.2** | Alarm management — priorities, states, deadband, shelving, rationalization |
| **IEC 62443** | OT cybersecurity — network zones, IT/OT boundaries, SIS scope |

## Target User

Lead Ignition Developer who:
- Has deep Ignition expertise (UDTs, Perspective, alarms, historian)
- Is comfortable with Git and Claude Code
- Wants AI productivity gains without trusting generic tools in safety-critical environments

## Acknowledgments

- [Fuuz Claude Skills](https://github.com/fuuz-io) — customize file patterns and ISA standards implementation
- [BW Design Group](https://github.com/bw-design-group/templates.ignition.postgres) — Docker/Liquibase reference templates
