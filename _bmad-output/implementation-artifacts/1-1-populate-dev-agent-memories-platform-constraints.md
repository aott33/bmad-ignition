# Story 1.1: Populate Dev Agent Memories — Platform Constraints

Status: done

## Story

As a **Lead Ignition Developer**,
I want **the Dev agent to understand Ignition platform constraints**,
So that **generated code uses valid Jython 2.7 syntax and follows Ignition patterns**.

## Acceptance Criteria

1. **Given** the bmm-dev.customize.yaml file exists
   **When** I add memories for platform constraints
   **Then** the memories include Jython 2.7 syntax rules (no f-strings, no type hints, `print` without parentheses)

2. **And** the memories include Perspective JSON structure awareness

3. **And** the memories include UDT naming conventions and inheritance patterns

4. **And** the memories include tag path format guidance (`[default]Site/Area/Equipment/Tag`)

5. **And** each memory follows the rule + context format (1-3 sentences, backticks for code)

## Tasks / Subtasks

- [x] Task 1: Open and review bmm-dev.customize.yaml (AC: #1-5)
  - [x] 1.1 Read the existing customize file structure
  - [x] 1.2 Understand the `memories` section format (YAML list)

- [x] Task 2: Add Jython 2.7 syntax constraint memories (AC: #1)
  - [x] 2.1 Add memory for no f-strings rule
  - [x] 2.2 Add memory for no type hints rule
  - [x] 2.3 Add memory for print statement syntax (no parentheses in Jython)
  - [x] 2.4 Add memory for no walrus operator (:=)
  - [x] 2.5 Add memory for Python 2 unicode handling awareness

- [x] Task 3: Add Perspective JSON structure memories (AC: #2)
  - [x] 3.1 Add memory for Perspective view JSON structure (view.json components)
  - [x] 3.2 Add memory for binding structure and indirect bindings
  - [x] 3.3 Add memory for event handler structure in Perspective

- [x] Task 4: Add UDT pattern memories (AC: #3)
  - [x] 4.1 Add memory for UDT naming conventions (PascalCase for definitions)
  - [x] 4.2 Add memory for UDT inheritance patterns
  - [x] 4.3 Add memory for UDT parameters and how instances consume them

- [x] Task 5: Add tag path format memories (AC: #4)
  - [x] 5.1 Add memory for tag path format (`[provider]path/to/tag`)
  - [x] 5.2 Add memory for ISA-95 hierarchy in tag paths (Site/Area/Line/Cell/Equipment)
  - [x] 5.3 Add memory for parameter binding in UDT tag paths

- [x] Task 6: Validate memory formatting (AC: #5)
  - [x] 6.1 Ensure all memories follow rule + context format (1-3 sentences)
  - [x] 6.2 Ensure backticks used for all code references
  - [x] 6.3 Run validation against Architecture patterns checklist

## Dev Notes

### Target File
**Location:** `_bmad/_config/agents/bmm-dev.customize.yaml`

### Memory Format Requirements (from Architecture)

Each memory entry MUST follow this pattern:
- **Format:** Rule + context, 1-3 sentences per memory
- **Code:** Use backticks for code references (`system.tag.read()`, `UDT`, `Perspective`)
- **Avoid:** Vague statements like "Follow ISA standards"
- **Avoid:** Entries exceeding 3 sentences

**Good Example:**
```yaml
memories:
  - "Ignition uses Jython 2.7, not Python 3. Always use `print` statements without parentheses and avoid f-strings."
```

**Bad Example (too vague):**
```yaml
memories:
  - "Follow Python 2 rules"
```

### Platform Constraints to Encode

#### Jython 2.7 Constraints (PRD: FR22, FR28)
- No f-strings (use `%` formatting or `.format()`)
- No type hints
- No walrus operator (`:=`)
- `print` is a statement, not a function (though `print()` works as a tuple)
- Unicode handling differs from Python 3
- Some standard library modules unavailable

#### Perspective JSON Structure (PRD: FR20, FR21)
- Views stored as `view.json` files
- Component tree structure with `root` element
- Bindings as separate property within components
- Event handlers in `events` array
- Props vs. custom properties distinction

#### UDT Patterns (PRD: FR13, FR25-FR26)
- Naming: PascalCase for UDT definitions (e.g., `Tank`, `Motor`, `Compressor`)
- Inheritance: UDTs can extend other UDTs
- Parameters: Defined at UDT level, bound at instance level
- Properties: Static values vs. bound values
- Alarm configuration embedded in UDT definitions

#### Tag Path Structure (PRD: FR23, NFR-Q1)
- Format: `[provider]path/to/tag` (e.g., `[default]Dairy/CoolingSystem/Compressor1/Status`)
- Provider is typically `default` but can be custom
- Path follows ISA-95 hierarchy for organization
- Tag browsers and indirect bindings use these paths

### ISA-95 Hierarchy Reference (for tag paths)
```
Enterprise → Site → Area → Line → Cell → Equipment Module
```
Map to Ignition tag structure:
```
[default]SiteName/AreaName/LineName/EquipmentName/TagName
```

### Architecture Compliance Checklist (from architecture.md)
Before finalizing the customize file, verify:
- [x] All memories follow rule + context format (1-3 sentences)
- [x] Code references use backticks (`example`)
- [x] ISA standards use ID-only format (no years)
- [x] Ignition terms use official capitalization (Ignition, UDT, Perspective, Designer, Gateway, Jython 2.7)
- [x] No Python 3 syntax in examples
- [x] Tag path examples are realistic (`[default]Site/Area/Line/Equipment/Tag`)

### Project Structure Notes

- **Customize file location:** `_bmad/_config/agents/bmm-dev.customize.yaml`
- **Merge behavior:** `memories` section APPENDS to base agent (does not replace)
- **Recompile required:** After editing, run `npx bmad-method install` → "Recompile Agents"

### References

- [Source: _bmad-output/planning-artifacts/architecture.md#Memory Formatting]
- [Source: _bmad-output/planning-artifacts/architecture.md#Implementation Patterns]
- [Source: _bmad-output/planning-artifacts/prd.md#Technical Constraints]
- [Source: _bmad-output/planning-artifacts/prd.md#FR20-FR29]
- [Source: _bmad-output/planning-artifacts/epics.md#Story 1.1]
- [Source: _bmad-output/project-context.md#Technical Context for Agents]

## Dev Agent Record

### Agent Model Used

Claude Opus 4.5 (claude-opus-4-5-20251101)

### Debug Log References

- YAML validation performed using Python yaml.safe_load() - confirmed valid syntax
- Memory count validated: 15 memories total (5 Jython + 4 Perspective + 3 UDT + 3 Tag Path)

### Completion Notes List

- Reviewed existing bmm-dev.customize.yaml structure - found empty memories array ready for population
- Added 5 Jython 2.7 syntax constraint memories covering f-strings, type hints, print syntax, walrus operator, and unicode handling
- Added 3 Perspective JSON structure memories covering view.json structure, bindings, and event handlers
- Added 3 UDT pattern memories covering naming conventions, inheritance, and parameters
- Added 3 tag path format memories covering provider format, ISA-95 hierarchy, and parameter bindings
- Validated all 14 memories against Architecture compliance checklist:
  - ✅ Rule + context format (1-3 sentences per memory)
  - ✅ Backticks for all code references
  - ✅ ISA standards use ID-only format (no years)
  - ✅ Ignition terms correctly capitalized
  - ✅ No Python 3 syntax in examples
  - ✅ Realistic tag path examples

### Change Log

- 2026-02-19: Story implementation complete - added 14 platform constraint memories to bmm-dev.customize.yaml
- 2026-02-19: Code review fixes applied:
  - Fixed incorrect print statement memory (tuple claim was wrong)
  - Fixed UDT inheritance explanation (typeId vs extends clarification)
  - Added missing Perspective position/meta/custom memory
  - Removed leftover template comments
  - Updated Architecture Compliance Checklist to checked status

### File List

- `_bmad/_config/agents/bmm-dev.customize.yaml` (MODIFIED) - Added 15 memories for Ignition platform constraints (originally 14, +1 from code review)
