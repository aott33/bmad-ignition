# Ignition SCADA Project Context

This is an **Ignition SCADA/MES project**. All code and configuration must respect Ignition platform constraints.

## Critical: Jython 2.7

Ignition scripts run on **Jython 2.7**, not Python 3. Violations cause silent runtime failures.

| Forbidden (Python 3) | Use instead |
|---|---|
| `f'Value: {x}'` | `'Value: %s' % x` or `'Value: {}'.format(x)` |
| `def foo(x: int) -> str:` | `def foo(x):` (docstring for docs) |
| `if (x := getValue()):` | `x = getValue()` then `if x:` |
| `print('a', 'b')` | `print 'a', 'b'` or `system.util.getLogger()` |
| `str` as unicode | `u'string'` prefix for unicode literals |

## Ignition Scripting Scope

- **`system` is pre-scoped** in all Ignition script contexts (Gateway, Perspective, tag events). Never write `import system` - it is unnecessary and wrong.
- **`java.lang` must be imported explicitly** when catching Java exceptions: `import java.lang` before using `java.lang.Throwable`.
- Always catch both Java and Python exceptions for any `system.db.*`, `system.tag.*`, or external resource call:

```python
import java.lang

try:
    system.db.runNamedQuery('myQuery', params)
except java.lang.Throwable as ex:
    system.util.getLogger('Module').error('Failed: {}'.format(ex))
except Exception as ex:
    system.util.getLogger('Module').error('Failed: {}'.format(ex))
```

## String Formatting Rules

- **No em-dashes** (`-`) in scripts, log messages, labels, or docs. Use ` - ` (space-hyphen-space) instead. Em-dashes do not render correctly in Gateway logs or Perspective labels on all locales.

## Safety-Critical Requirements

- **Flag SIS scope**: Note any Safety Instrumented System configurations and require engineering review
- **IT/OT boundaries**: Do not create connections across network zones without explicit authorization
- **Alarm changes**: Flag all alarm priority changes and interlock modifications for human engineering review
- **MOC**: Safety-critical architecture changes require Management of Change documentation

## Validation Before Completion

Any Perspective view or Jython script is not complete until validated:
1. `ignition.nvim` LSP - zero errors
2. `ignition-lint path/to/view.json` - pass rate > 90%
3. Gateway auto-detects file changes
4. Designer verification with live tags

## Key Reference Docs

- `docs/jython-constraints.md` - full Jython 2.7 reference, java.lang.Throwable patterns
- `docs/tag-structure.md` - tag paths, UDT patterns, ISA-95 folders
- `docs/isa-standards.md` - ISA-101, ISA-95, ISA-88, ISA-18.2 summary
- `docs/system-architectures.md` - Gateway architecture patterns and decision guide
- `docs/perspective-components.md` - Perspective component reference
- `docs/perspective-styles.md` - style classes, themes, CSS variables
- `docs/validation-workflow.md` - ignition-lint + LSP workflow
- `docs/parallel-dev.md` - file isolation for parallel agent work

## Skills

| Command | Use when... |
|---|---|
| `/ignition-dev` | Writing scripts, views, UDTs, tag configs |
| `/ignition-architect` | Designing system architecture, UDT hierarchy, DB model |
| `/ignition-ui` | Designing Perspective screens and UX flows |
| `/ignition-plan` | Writing PRDs, discovery, requirements, epics |
| `/ignition-review` | Reviewing code, views, or architecture for correctness |
