# Jython 2.7 Constraints for Ignition Development

Ignition scripting runs on **Jython 2.7.x**, a Java implementation of Python 2.7. This is not Python 3. These constraints apply to all scripts: Gateway Event scripts, project library scripts, Perspective event handlers, and Tag Event scripts.

## Hard Constraints (Silent Runtime Failures)

### 1. No f-strings

```python
# WRONG — SyntaxError in Jython 2.7
name = f'Tank: {tankId}'
msg = f'Level is {level:.2f}%'

# CORRECT
name = 'Tank: %s' % tankId
msg = 'Level is %.2f%%' % level

# Also correct (.format style)
name = 'Tank: {}'.format(tankId)
msg = 'Level is {:.2f}%'.format(level)
```

### 2. No Type Hints

```python
# WRONG — SyntaxError in Jython 2.7
def getTagValue(tagPath: str, defaultVal: float = 0.0) -> float:
    pass

# CORRECT
def getTagValue(tagPath, defaultVal=0.0):
    """
    Read a tag value from the Gateway.

    Args:
        tagPath (str): Full tag path including provider, e.g. [default]Site/Area/Tag
        defaultVal (float): Value to return if tag read fails
    Returns:
        float: Current tag value or defaultVal on error
    """
    pass
```

### 3. No Walrus Operator

```python
# WRONG — SyntaxError in Jython 2.7
if (value := system.tag.readBlocking(['[default]Tank/Level'])[0].value) > 80:
    triggerAlarm()

# CORRECT
results = system.tag.readBlocking(['[default]Tank/Level'])
value = results[0].value
if value > 80:
    triggerAlarm()
```

### 4. print Is a Statement, Not a Function

```python
# WRONG — print() as function prints a tuple in Jython 2.7
print('Pump started:', pumpId)    # outputs: ('Pump started:', 'P001')

# CORRECT — print statement
print 'Pump started:', pumpId     # outputs: Pump started: P001

# BEST for production — use Ignition logger
logger = system.util.getLogger('MyModule')
logger.info('Pump started: %s' % pumpId)
logger.warn('High temperature: %.1f' % temp)
logger.error('Tag read failed: %s' % tagPath)
```

### 5. Integer Division

```python
# Jython 2.7 — integer / integer = integer (truncated)
5 / 2      # = 2 (not 2.5!)
10 / 3     # = 3 (not 3.333...)

# Use float division when decimal result needed
5.0 / 2    # = 2.5
float(x) / y
x / 2.0
```

### 6. Unicode Handling

```python
# str and unicode are separate types in Jython 2.7
type('hello')    # <type 'str'>    (bytes)
type(u'hello')   # <type 'unicode'>

# Use u'' prefix for non-ASCII text
label = u'Température'
unit = u'°C'

# Avoid mixing str and unicode in concatenation — use explicit encoding
```

## Things That Don't Exist in Jython 2.7

| Python 3 feature | Status |
|---|---|
| `async` / `await` | Not available |
| `yield from` | Not available |
| `@` matrix multiply operator | Not available |
| `from __future__ import annotations` | Not available |
| `dataclasses` | Not available |
| `pathlib` | Not available |
| `f'...'` strings | Not available |
| `{**dict1, **dict2}` dict unpacking | Not available |
| `[*list1, *list2]` list unpacking | Not available |
| `nonlocal` keyword | Not available |

## Things That Work Fine

```python
# List comprehensions
active_tags = [t for t in tags if t['enabled']]

# Dict comprehensions
tag_map = {t['name']: t['path'] for t in tag_list}

# Lambda
sorter = lambda x: x['priority']

# Try/except/finally
try:
    val = system.tag.readBlocking([path])[0].value
except Exception, e:           # Jython 2.7 syntax
    logger.error('Read failed: %s' % str(e))
    val = default

# Note: Python 3 uses "except Exception as e:" — both work in Jython 2.7
```

## Ignition Scripting Patterns

### Reading Tags

```python
# Read single tag
path = '[default]Dairy/CoolingSystem/Compressor1/Status'
qval = system.tag.readBlocking([path])[0]
value = qval.value
quality = qval.quality

# Read multiple tags
paths = [
    '[default]Area1/Motor1/Speed',
    '[default]Area1/Motor1/Current',
]
results = system.tag.readBlocking(paths)
speed = results[0].value
current = results[1].value
```

### Writing Tags

```python
# Write single tag
system.tag.writeBlocking(['[default]Tank1/Setpoint'], [75.0])

# Write multiple tags
system.tag.writeBlocking(
    ['[default]Tank1/Setpoint', '[default]Tank1/AlarmEnable'],
    [75.0, True]
)
```

### Logging Best Practices

```python
# Get logger once at module level
logger = system.util.getLogger('ProjectName.ModuleName')

# Log levels
logger.trace('Entering function: %s' % funcName)
logger.debug('Tag value: %s = %s' % (tagPath, value))
logger.info('Batch started: %s' % batchId)
logger.warn('High temperature warning: %.1f C' % temp)
logger.error('Tag write failed: %s' % tagPath)
```

### Database Queries

```python
# Named query — ALWAYS preferred. Parameterized, no SQL injection risk.
params = {'areaId': 'Refrigeration', 'status': 1}
results = system.db.runNamedQuery('getEquipmentByArea', params)

# Direct query — AVOID. Never use string formatting to build queries.
# The following is SQL injection vulnerable and MUST NOT be used:
# query = "SELECT * FROM Equipment WHERE area = '%s'" % area  # WRONG
```

### Error Handling — Java Exceptions

Ignition runs on the JVM. SQL and JDBC operations throw **Java exceptions** (`java.lang.Throwable`) that Jython's `Exception` will NOT catch. Always catch both:

```python
import java.lang

logger = system.util.getLogger('MyModule')

try:
    countDs = system.db.runNamedQuery('getFailedCount', {'tagProvider': tagProvider})
    resetCount = countDs.getValueAt(0, 'FailedCount')

    system.db.runNamedQuery('resetFailed', {'tagProvider': tagProvider})

    message = 'Reset {} failed chunks for {}'.format(resetCount, tagProvider)
    logger.info(message)
    return {'message': message, 'resetCount': resetCount}

except java.lang.Throwable as ex:
    # Catches Java/JDBC/SQL exceptions — e.g. connection failures, constraint violations
    message = 'Reset failed: {}'.format(ex)
    logger.error(message)
    return {'message': message, 'resetCount': 0}

except Exception as ex:
    # Catches Jython/Python exceptions — e.g. NullPointerException wrappers, logic errors
    message = 'Reset failed: {}'.format(ex)
    logger.error(message)
    return {'message': message, 'resetCount': 0}
```

**Why both?** `system.db.*` calls invoke JDBC drivers written in Java. Java exceptions inherit from `java.lang.Throwable`, not from Python's `BaseException`. A bare `except Exception` will silently miss JDBC errors — the try block fails and control falls through without logging.

**Rule:** Any code calling `system.db.*`, `system.tag.*`, OPC functions, or any Ignition system function that touches external resources must catch `java.lang.Throwable` in addition to `Exception`.

## Validation Tools

- **ignition.nvim** — LSP in Neovim catches Jython 2.7 violations in real time
- **ignition-lint** — validates Perspective `view.json` structure and bindings
- Run LSP check before lint, lint before Gateway import
