# bmad-ignition

A starter kit and documented process for using AI coding agents on Ignition SCADA/MES projects.

## The Problem

Generic AI coding tools don't understand Ignition. They hallucinate Jython 2.7 syntax, invent UDT properties, bind to non-existent tag paths, and misconfigure alarms. In industrial automation, these aren't bugs—they're risks.

## The Solution

bmad-ignition provides domain-specific context injection through BMAD Method customize files, ISA standards alignment (ISA-101, ISA-95, ISA-88, ISA-18.2), and validation guardrails via ignition-lint and ignition.nvim.

## Status

**Planning complete.** PRD finalized. Next: Architecture, UX Design, Epic Breakdown.

See [prd.md](_bmad-output/planning-artifacts/prd.md) for full requirements.

## Quick Start

```bash
# Clone this repo
git clone https://github.com/your-username/bmad-ignition.git
cd bmad-ignition

# Install BMAD Method (customize files are pre-populated)
npx bmad-method install

# Follow the BMAD workflow
```

## Stack

| Tool | Purpose |
|------|---------|
| [BMAD Method](https://github.com/bmad-code-org/BMAD-METHOD) | Agent orchestration framework |
| [ignition.nvim](https://github.com/TheThoughtagen/ignition-nvim) | Neovim LSP for Ignition development |
| [ignition-lint](https://github.com/TheThoughtagen/ignition-lint) | Perspective view validation |
| Docker | Ignition gateway environment |
| Git | Version control |

## ISA Standards

All agent output aligns with professional industrial automation standards:

- **ISA-101** — High Performance HMI principles
- **ISA-95** — Equipment hierarchy (Enterprise → Site → Area → Line → Cell → Asset)
- **ISA-88** — Batch/procedural control patterns
- **ISA-18.2** — Alarm management (priorities, states, rationalization)
- **IEC 62443** — OT cybersecurity awareness

## Repo Structure

```
bmad-ignition/
├── _bmad/                    # BMAD installation + customize files
│   └── _config/agents/       # Pre-populated Ignition domain knowledge
├── _bmad-output/             # Planning artifacts
│   ├── brainstorming/        # Discovery sessions
│   └── planning-artifacts/   # PRD, architecture, stories
├── .claude/                  # BMAD-generated slash commands
└── README.md
```

## Target User

Lead Ignition Developer who:
- Has deep Ignition expertise (UDTs, Perspective, alarm systems)
- Comfortable with Git, Docker, Linux
- Wants AI productivity gains but can't trust generic tools in safety-critical domains

## Platform

**Linux only** for MVP. Omarchy (Arch-based) is the reference environment.

## Acknowledgments

- [Fuuz Claude Skills](https://github.com/fuuz-io) — Customize file patterns and ISA standards implementation
- [BW Design Group](https://github.com/bw-design-group/templates.ignition.postgres) — Docker/Liquibase reference templates