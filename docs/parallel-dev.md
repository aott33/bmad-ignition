# Parallel Development: File Isolation Rules

When multiple agents or developers work on an Ignition project simultaneously, file conflicts are unavoidable without deliberate isolation. Ignition's JSON-heavy file format makes merge conflicts destructive — Git cannot meaningfully three-way merge JSON.

## The Core Rule

**One agent/developer per file at a time.** Assign work at the file level, not the feature level.

---

## File Isolation by Resource Type

### Perspective Views

Each view is a single `view.json` file in its own folder:
```
com.inductiveautomation.perspective/views/{ViewName}/view.json
```

**Rule:** Assign one agent per view folder. Never assign the same view to two parallel agents.

**Safe parallel assignment:**
```
Agent A → views/OverviewDashboard/
Agent B → views/TankDetail/
Agent C → views/AlarmSummary/
```

**Unsafe:**
```
Agent A → views/TankDetail/  (adding alarm banner)
Agent B → views/TankDetail/  (adding trend component)
← These will conflict on the same view.json
```

### UDT Definitions

UDT definitions are individual JSON files in the `UDTs/` folder:
```
UDTs/Tank.json
UDTs/Motor.json
UDTs/Compressor.json
```

**Rule:** Assign one agent per UDT file. Concurrent UDT edits break inheritance chains and dependent instances.

**Important:** UDT definitions should be written complete in one pass. If two agents write different parts of the same UDT definition, merging is not safe.

### Jython Scripts

Library scripts are single `.py` files:
```
ignition/script-python/project/tankUtils.py
ignition/script-python/project/alarmUtils.py
```

**Rule:** Assign one agent per script module. Jython files support line-level Git merges but shared utility functions cause conflicts.

**Shared utility pattern:** If multiple stories need shared utility functions, create the shared function in one story first, merge to main, then reference it in dependent stories.

### Tag Folders (ISA-95 Hierarchy Branches)

Tags are organized by ISA-95 hierarchy:
```
[default]Site/Area/Line/Cell/Equipment/
```

**Rule:** Assign work by ISA-95 hierarchy branch. Never assign overlapping branches to parallel agents.

**Safe parallel assignment:**
```
Agent A → [default]DairyPlant/Refrigeration/
Agent B → [default]DairyPlant/Pasteurization/
Agent C → [default]DairyPlant/Utilities/
```

**Unsafe:**
```
Agent A → [default]DairyPlant/Refrigeration/CoolingLoop1/
Agent B → [default]DairyPlant/Refrigeration/  ← overlaps with Agent A
```

### Named Queries

Named queries are folders with config files, similar to views:
```
com.inductiveautomation.perspective/queries/{QueryName}/
```

**Rule:** Assign one agent per query folder. Same isolation principle as views.

---

## Git Workflow for Parallel Development

### Branch Strategy

Use feature branches per story:

```bash
# Naming convention: {epic}-{story}-{slug}
git checkout -b 3-2-tank-detail-view
git checkout -b 3-3-alarm-summary-view
git checkout -b 4-1-motor-udt-definition
```

### Merge Promptly

Long-lived branches accumulate merge debt. Merge completed stories to `main` promptly to reduce the distance for other branches.

```bash
# After story completion and review
git checkout main
git merge 3-2-tank-detail-view
git push origin main
```

### Rebase Regularly

For long-running branches, rebase onto main regularly:

```bash
git fetch origin main
git rebase origin/main
```

### Merge Strategy by File Type

| File type | Merge strategy | Notes |
|---|---|---|
| Perspective `view.json` | **One-wins** — do not attempt JSON merge | Prevent conflicts by isolation |
| UDT definition JSON | **One-wins** | Prevent conflicts by isolation |
| Tag export XML | **Manual review required** | Assign by hierarchy branch to avoid |
| Jython `.py` scripts | **Line-level merge** | Shared functions still cause conflicts |
| Named query config | **One-wins** | Prevent conflicts by isolation |

---

## Story Assignment Checklist

Before assigning parallel work, verify:

- [ ] Each story has distinct, non-overlapping file ownership
- [ ] No two active stories reference the same `view.json` file
- [ ] No two active stories reference the same UDT definition file
- [ ] Script stories use different `.py` modules, or shared functions are created in one story first
- [ ] Tag work is assigned to non-overlapping ISA-95 hierarchy branches
- [ ] If a story creates a new shared utility, dependent stories are marked as blocked until it merges

---

## Ignition Project Structure Quick Reference

```
ignition/
└─ projects/
   └─ {ProjectName}/
      ├─ com.inductiveautomation.perspective/
      │  ├─ views/
      │  │  └─ {ViewName}/view.json          ← one file per view (assign atomically)
      │  ├─ queries/
      │  │  └─ {QueryName}/                  ← one folder per query
      │  ├─ styles/
      │  └─ page-config.json
      ├─ ignition/
      │  └─ script-python/
      │     └─ project/
      │        └─ {module}.py                ← one file per module
      └─ project.json

tags/
└─ default/
   └─ {Site}/
      └─ {Area}/                             ← assign by Area branch
         └─ {Line}/
            └─ {Cell}/
               └─ {Equipment}/              ← UDT instance

UDTs/
├─ Tank.json                                 ← one file per UDT (assign atomically)
├─ Motor.json
└─ Compressor.json
```
