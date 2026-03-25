# Validation Workflow

Every Perspective view and Jython script must pass through this validation sequence before being marked complete. Fix errors at the earliest stage — catching problems earlier is faster and safer.

```
Edit file → [1] LSP → [2] ignition-lint → [3] Gateway → [4] Designer
```

---

## Stage 1: ignition.nvim LSP (Jython scripts)

**What it catches:** Jython 2.7 syntax errors, Python 3 syntax used in Jython context, undefined variables, type mismatches, basic code quality issues.

**When to run:** Continuously while editing in Neovim — real-time feedback.

**Pass criteria:** Zero errors in LSP diagnostics panel.

```
:lua vim.diagnostic.setloclist()    -- view all diagnostics
:lopen                              -- open diagnostics list
```

**Common LSP errors to fix:**
- f-string syntax (`f'...'`)
- Type hint syntax (`def foo(x: int):`)
- Undefined Ignition system functions (ensure ignition.nvim is configured with Ignition stubs)
- `print()` as function instead of statement

**Do not proceed to Stage 2 until LSP shows zero errors.**

---

## Stage 2: ignition-lint (Perspective views)

**What it catches:** Perspective `view.json` structure errors, malformed bindings, missing required properties, deprecated component patterns.

**When to run:** After editing any `view.json` file, before Gateway pickup.

### Running ignition-lint

```bash
# Validate a single view
ignition-lint path/to/view.json

# Validate all views in a project
ignition-lint ignition/projects/MyProject/com.inductiveautomation.perspective/views/

# Output with error detail
ignition-lint --verbose path/to/view.json
```

### Severity Levels

| Severity | Action |
|---|---|
| Critical | **Block** — fix before proceeding (invalid JSON, broken binding, missing required property) |
| High | **Block** — fix before proceeding (malformed component config, invalid property type) |
| Medium | Document justification — may proceed with approval |
| Low | Advisory — note but do not block |

**Pass criteria:** Zero Critical and High errors. Pass rate > 90%.

### Common ignition-lint Errors

| Error | Fix |
|---|---|
| Invalid JSON syntax | Check for trailing commas, missing brackets, unescaped quotes |
| Malformed binding | Verify binding structure: `{"type": "tag", "config": {"path": "..."}}` |
| Unknown component type | Check component type string against Perspective component library |
| Missing required property | Add the required `props` field for that component type |
| Invalid position config | Ensure `position` object has valid flex properties |

**Do not proceed to Stage 3 until lint passes.**

---

## Stage 3: Gateway Auto-Detection

Ignition Gateway continuously monitors the filesystem for changes. When a file is saved, the Gateway automatically picks it up — **no manual import needed** for file-based resources.

**File-based resources (auto-detected):**
- Perspective views (`view.json`)
- Project library scripts (`.py` files)
- Named queries
- Page configs
- Styles

**NOT file-based (require explicit action):**
- Tags (stored in SQLite DB — use `system.tag.exportTags()` or ignition-git-module)
- Images (stored in SQLite DB)
- UDT definitions (stored in SQLite DB)
- Gateway scripts configured in the Gateway web UI

### Verifying Gateway Pickup

```bash
# Check Gateway logs for file detection
# In Gateway web UI: Status → Diagnostics → Logs
# Filter: "Project"

# Or watch for file change detection message:
# INFO  [IgnitionGateway            ] [14:23:01]: Project 'MyProject' changed, reloading...
```

If the Gateway doesn't pick up changes within 5-10 seconds, check:
1. File was saved (not just in-editor buffer)
2. File path is within the correct project directory
3. Gateway has read permissions on the project directory

---

## Stage 4: Designer Verification

Open Ignition Designer to verify the view renders correctly and bindings resolve with live tag data. Designer verification catches runtime issues that earlier stages cannot detect.

**What Designer catches that lint misses:**
- Binding errors with real tag data (tag exists but wrong data type)
- Component rendering issues (properties interact unexpectedly at runtime)
- Script runtime errors (Jython syntax passed but logic fails with real values)
- Navigation flow issues (parameterized views receive unexpected values)
- Performance issues (too many bindings, expensive queries)

### Designer Verification Checklist

For Perspective views:
- [ ] View opens without errors in Designer
- [ ] All components render visibly (no blank/invisible components)
- [ ] Tag bindings show live values (not `null` or quality error)
- [ ] Event handlers execute without error (test button clicks, etc.)
- [ ] Parameters pass correctly between embedded views
- [ ] View displays correctly at target screen size/resolution

For Jython scripts:
- [ ] Script compiles without error (Designer shows green checkmark)
- [ ] Script executes without runtime exception in test context
- [ ] Output / side effects are correct (tag writes succeed, queries return expected data)

---

## Project File Structure

Understanding where files live helps with the validation workflow:

```
ignition/
└─ projects/
   └─ {ProjectName}/
      ├─ com.inductiveautomation.perspective/
      │  ├─ views/
      │  │  └─ {ViewName}/
      │  │     └─ view.json              ← edit and lint here
      │  ├─ queries/
      │  │  └─ {QueryName}/
      │  │     └─ query.json
      │  └─ page-config.json
      ├─ ignition/
      │  └─ script-python/
      │     └─ project/
      │        └─ {module}.py            ← LSP validates here
      └─ project.json
```

---

## Version Control Integration

### File-Based Resources
Perspective views, scripts, named queries are plain files — commit directly to Git:

```bash
git add ignition/projects/MyProject/com.inductiveautomation.perspective/views/TankDetail/view.json
git add ignition/projects/MyProject/ignition/script-python/project/tankUtils.py
git commit -m "Add tank detail view and utility scripts"
```

### Tag Export (Required for Tags)
Tags are in Ignition's SQLite database. Export before committing:

```python
# Jython script to export tags
system.tag.exportTags(
    filePath='/path/to/project/ignition/tags/default.json',
    tagPaths=['[default]'],
    recursive=True,
    exportType='json'
)
```

Or use the **ignition-git-module** which:
- Adds a commit popup in Designer on save
- Provides push/pull toolbar buttons
- Automates tag export on Designer save

### What Goes in Git

| Resource | Storage | Git approach |
|---|---|---|
| Perspective views | Filesystem | Direct commit |
| Library scripts | Filesystem | Direct commit |
| Named queries | Filesystem | Direct commit |
| Tags | SQLite DB | Export to JSON, commit JSON |
| Images | SQLite DB | Export, commit |
| UDT definitions | SQLite DB | Export to JSON, commit JSON |
| Gateway config | Gateway SQLite | Use ignition-git-module or manual export |
