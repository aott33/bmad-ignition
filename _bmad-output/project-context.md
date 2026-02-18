# Project Context: bmad-ignition

## What This Is

A framework and starter kit for using AI coding agents in industrial automation development — specifically Ignition by Inductive Automation. It bridges two worlds: the BMAD Method (structured agentic development workflows) and the Ignition SCADA platform (used across oil & gas, manufacturing, food & beverage, utilities, and other industrial sectors). **It's also a framework that shows when and how AI coding agents can be used in parallel to expedite the process.**

This is not a finished product. It's an evolving set of templates, agent configurations, documentation, and example artifacts that demonstrate how AI agents can be useful in a domain where mistakes have physical consequences.

## Who It's For

- Ignition developers exploring agentic workflows for SCADA/MES projects
- BMAD users who work in industrial automation and want domain-specific agents
- Controls engineers curious about integrating AI into their development process
- The developer of this repo (Andy), who is using it as both a learning vehicle and a portfolio piece bridging SCADA expertise with modern development practices

## Core Goals

### Make AI agents useful in industrial automation without being dangerous

Generic AI coding tools don't understand Ignition's platform constraints (Jython 2.7, UDT hierarchies, alarm pipelines, Perspective JSON structure). They don't know that a wrong alarm priority or hallucinated tag path has real-world consequences. This project gives agents the context, guardrails, and validation feedback loops they need to produce work that's actually usable in this domain.

### Provide a repeatable starting point for Ignition projects using BMAD

When someone starts a new Ignition project with AI agents, they shouldn't have to figure out the Docker bind-mount paths, write agent personas from scratch, or guess how BMAD's lifecycle maps to SCADA development phases. The template and module should get them to a working starting point quickly.

### Demonstrate the integration between existing tools

This project doesn't reinvent anything. It connects tools that already exist (NOTE THAT THERE MAY BE OTHER TOOLS THAT EXIST SO WE NEED TO DO A THOROUGH INVESTIGATION INTO OTHER EXAMPLES):
- **BMAD Method** for structured agent orchestration
- **ignition-lint** for automated validation of Perspective views
- **ignition.nvim** for Neovim/terminal-based Ignition development
- **Ignition Git Module** for version control of Ignition projects
- **Ignition Flint** for VS Code-based Ignition development
- **Docker** for reproducible Ignition gateway environments

The value is in showing how these fit together, not in replacing any of them.

### Keep safety and compliance front-of-mind

Industrial automation has regulatory and safety requirements that software development generally doesn't. ISA-18.2 alarm management, IEC 62443 OT cybersecurity, network segmentation between IT and OT — these aren't optional. The framework should make it natural to address them throughout the development lifecycle rather than bolting them on at the end.

### Stay practical and grounded

The dairy SCADA pilot using Ignition's built-in simulator is intentionally small. The goal is to prove the workflow works end-to-end on something concrete before scaling to real projects with physical PLCs and production consequences. Documentation should be honest about what's validated and what's aspirational.

## Current State

**JUST A REFERENCE! WE DON'T NEED TO GO DOWN THIS SPECIFIC PATH. WE JUST NEED TO MEET THE CORE GOALS**

The repo contains:
- A root README covering the framework philosophy (three pillars: BMAD, Ignition tooling, zero trust agent security)
- A comprehensive USER_MANUAL.md walking through a dairy SCADA pilot project on Omarchy Linux
- A project template with Docker Compose configured for Ignition 8.3 with bind-mounted project directories
- A custom BMAD module (`bmad-ia-module/`) with a Safety Reviewer agent, customization files for adapting BMAD's built-in agents to IA roles, and three workflow definitions (greenfield, brownfield, quick-fix)
- Example artifacts showing what completed planning docs and stories look like for the dairy pilot

## What Needs Attention

These are areas that need work. They're listed here for awareness, not as a task list — approach them as the project evolves naturally.

**Validation against real BMAD installation.** The agent YAML and customize files were written to match the v6 format based on documentation and examples. They need to be tested through an actual `npx bmad-method install` cycle to confirm they compile and activate correctly.

**The dairy pilot needs to be actually run.** The USER_MANUAL describes a first stab effort but it hasn't been executed end-to-end yet. We need to review, update and test. Running it will surface gaps in the instructions, incorrect assumptions about the simulator tag structure, and places where the agent personas need refinement.

**Docker bind-mount behavior under real usage.** The bind-mount approach (projects/ and third-party-modules/ mapped into the container) is documented but needs testing to confirm the Designer and the host filesystem stay in sync without permission issues, especially around gateway restarts and project saves.

**The .customize.yaml files may need tuning.** BMAD's customization system uses specific merge semantics (some fields replace, some append). The current customize files assume replacement behavior for persona sections — this needs to be verified against the actual compiler.

**ignition-lint integration in the agent workflow.** The Developer agent persona says "run ignition-lint on every change" but there's no automated hook or script that actually does this. It's a manual step right now.

**The USER_MANUAL references BMAD commands (like `/load analyst`) that may not match the current BMAD CLI.** BMAD is actively evolving (v6 alpha) and command syntax may have changed.

**Security model is conceptual.** The zero trust agent security section in the README describes principles but doesn't implement them. There's no actual MCP server configuration, no agent certificate setup, no RBAC enforcement. This is aspirational documentation for now.

**No CI/CD pipeline.** The template includes a placeholder for `.github/workflows/lint.yml` in the project structure diagram but the actual workflow file doesn't exist yet.

## Technical Context for Agents

- **Ignition** is a Java-based SCADA platform. Projects are stored as JSON/XML files on the gateway filesystem. The Designer is a Java Swing client that connects to the gateway.
- **Perspective** is Ignition's web-based HMI module. Views are defined in `view.json` files with a component tree, bindings, and event handlers.
- **Jython 2.7** is Ignition's scripting language — it's Python 2.7 syntax running on the JVM. Not Python 3.
- **UDTs (User Defined Types)** are Ignition's reusable tag templates with parameters, inheritance, and alarm configuration.
- **The gateway runs in Docker** with project files bind-mounted to the host at `ignition/projects/`. Changes in the Designer appear on the host filesystem immediately.
- **Third-party modules** (`.modl` files) go to `ignition/third-party-modules/` on the host, bind-mounted to `/usr/local/bin/ignition/data/local/modl` in the container.
- **BMAD installs to `_bmad/`** (underscore prefix) and manages its own directory structure. Don't create or modify files inside `_bmad/` manually — use the BMAD CLI.
- **ignition-lint** validates Perspective `view.json` files. Run it against `ignition/projects/<ProjectName>/com.inductiveautomation.perspective/views/`.
- **The target dev environment** is Omarchy Linux (Arch-based, Hyprland, Neovim/LazyVim, Ghostty terminal) but nothing in the framework is OS-specific beyond the Docker install commands.