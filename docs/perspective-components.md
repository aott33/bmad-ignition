# Perspective Component Reference

Quick reference for Ignition Perspective components used in industrial HMI design.

## Layout Containers

### Flex Container
Primary layout component for responsive industrial HMI.

**Use when:** building responsive screens that adapt to different display sizes.

**Key properties:**
- `direction`: `row` or `column`
- `alignItems`: cross-axis alignment (`flex-start`, `center`, `stretch`)
- `justifyContent`: main-axis alignment (`flex-start`, `space-between`, `center`)
- `gap`: spacing between children
- `wrap`: allow wrapping to next line

**Pattern:** Nest flex containers to build complex layouts. Outer container = page structure; inner containers = component groups.

### Breakpoint Container
Distinct layout configurations at defined screen widths.

**Use when:** same view must work on control room monitors (1920px) AND tablets (1024px) AND mobile.

**Key properties:**
- Define breakpoints as pixel widths
- Each breakpoint has its own child layout configuration
- Children don't reflow within a breakpoint

### Coordinate Container
Fixed pixel-position layout — components stay exactly where placed.

**Use when:** P&ID process graphics, equipment layout diagrams, screens viewed on fixed-size displays only.

**Key properties:**
- `position` object on each child: `x`, `y`, `width`, `height`
- Does NOT reflow or adapt to screen size changes
- Best for process graphics requiring exact spatial relationships

### Tab Container
Switchable panels for related content categories.

**Use when:** equipment detail screens with multiple views (Status | Trends | Alarms | Configuration).

**Key properties:**
- Each tab holds a separate embedded view
- `selectedTab` property for programmatic tab switching
- Configure tab labels, icons

---

## Alarm Components (ISA-18.2)

### Alarm Status Table
Primary component for displaying active alarms.

**Standard columns to configure:**
- Priority (with color coding)
- Event Time
- Source (tag path)
- Display Path (human-readable location)
- State (Active/Acknowledged/Cleared)
- Ack button

**Placement:** persistent alarm banner visible on all screens OR dedicated Alarm Summary screen.

**Note:** Supports filtering by area, priority, state. Supports batch acknowledgment.

### Alarm Journal Table
Historical alarm event log for analysis and compliance.

**Use when:** shift handoff reports, troubleshooting, ISA-18.2 performance metrics (alarms/hour).

**Columns:** Event Time, State Change, Source, Display Path, Operator, Notes

**Note:** Audible alarms are NOT supported in Perspective — audio is at PLC/hardware level (annunciators, stack lights, horn relays).

---

## Data Display Components

### Power Chart
Interactive historical trend with multiple data pens.

**Use when:** maintenance troubleshooting, process analysis, multi-pen comparison.

**Key features:**
- Multiple pens (tags, calculated values)
- Zoom/pan interaction
- Time range picker (1h, 8h, 24h, custom)
- Data export
- Pen configuration at runtime (tag browsing)

### Sparkline
Lightweight inline mini-trend.

**Use when:** embedding recent trend context (last 8 hours) in equipment cards or overview screens.

**Key properties:**
- `tagPath`: direct tag binding
- `displayMode`: `line`, `bar`, `area`
- No interaction (view-only) — keeps screen clean

### Linear Scale
Analog value representation per ISA-101.

**Use when:** showing a value position on a graduated scale with alarm zones.

**Key properties:**
- `value`: current value binding
- `min`, `max`: scale range
- Alarm zones (high-high, high, low, low-low) with color bands
- Setpoint marker

**Why:** Operators recognize deviations from normal range faster from analog shape than from a number.

### Table
Tabular data display for lists and records.

**Use when:** equipment lists, batch records, production summaries, maintenance logs.

**Key features:**
- Static or dynamic data binding (dataset, named query result)
- Column configuration (type, format, width)
- Sorting, filtering
- Row selection (drives other component updates via view parameter)
- Cell rendering (conditional color, status icons)

---

## Equipment Symbol Components

### Standard Symbols
Pre-built symbols with state-based appearance:

| Symbol | States | Binding |
|---|---|---|
| `Motor` | Running, Stopped, Faulted, Maintenance | Integer mode tag |
| `Pump` | Running, Stopped, Faulted | Integer mode tag |
| `Valve` | Open, Closed, Throttled, Fault | Integer position tag |
| `Vessel` / `Cylindrical Tank` | Fill level 0-100% | Float level tag |
| `Sensor` | Normal, Fault | Boolean quality tag |

**ISA-101 compliance:**
- Normal state: gray (no color)
- Abnormal state: color (red for fault, yellow for warning)
- Prefer static symbols over animations

### Building Custom State-Based Symbols
Use `style` binding to map tag values to visual states:

```json
{
  "type": "expr",
  "config": {
    "expression": "if({view.params.status} == 1, '#CC0000', '#808080')"
  }
}
```

Map integer states to colors: 0=Stopped(gray), 1=Running(gray), 2=Fault(red), 3=Maintenance(yellow).

---

## Reuse and Embedding Components

### Embedded View
Reusable faceplate, banner, or widget.

**Use when:** same UI pattern appears on multiple screens (alarm banner, navigation bar, equipment faceplate).

**Pattern:**
1. Create a view with `params` contract (document required parameters)
2. Embed it with `Embedded View` component
3. Pass `equipmentId` or `tagBasePath` as parameters
4. One definition serves all equipment instances

**Parameter passing:**
```json
{
  "instanceProps": {
    "params": {
      "equipmentId": {"value": 42},
      "tagBasePath": {"value": "[default]DairyPlant/Refrigeration/Compressor1"}
    }
  }
}
```

### Flex Repeater
Dynamically generate multiple view instances from data.

**Use when:** equipment lists, alarm card grids, any repeating pattern driven by data.

**Pattern:**
1. Design a single "card" view with parameters
2. Bind Flex Repeater `instances` to a dataset or tag array
3. Map dataset columns to card view parameters
4. Repeater creates one card per row automatically

**Key properties:**
- `instances`: dataset or array binding
- `instanceCommonProps`: shared properties across all instances
- `repeatContainerPath`: view path to repeat

### View dropConfig
Enable drag-and-drop UDT binding onto a view.

**Use when:** accelerating screen building — drag UDT instance from Tag Browser onto a faceplate view to auto-bind tag paths.

Configure in `view.json` root `meta.dropConfig` to specify which parameters get populated from UDT properties.

---

## Operator Interaction Components

### Button
Operator action trigger (Start, Stop, Acknowledge, Open).

**ISA-101 rules:**
- Minimum touch target: 44×44px
- Primary actions: prominent placement and size
- Destructive/critical actions (Emergency Stop): red background, confirmation dialog
- Disabled state: clearly grayed — no ambiguity

**Event handler pattern:**
```json
{
  "events": {
    "onActionPerformed": {
      "type": "script",
      "config": {
        "script": "system.tag.writeBlocking(['[default]Equipment/StartCmd'], [True])"
      }
    }
  }
}
```

### Numeric Entry Field
Setpoint and parameter adjustment.

**Key properties:**
- `min`, `max`: validation range
- `format`: display format string (e.g. `#.## °C`)
- Always configure min/max limits
- Add confirmation dialog for critical setpoints:

```python
# In event handler script
confirmed = system.gui.confirm(
    'Change setpoint to %.1f°C?' % newValue,
    'Confirm Setpoint Change'
)
if confirmed:
    system.tag.writeBlocking(['[default]Tank1/TempSetpoint'], [newValue])
```

### Multi-State Button / Toggle Switch
Mode selection (Hand/Off/Auto, Enable/Disable, Manual/Auto).

**Key properties:**
- Shows current state and available transitions
- Bind to integer tag for equipment mode
- Label each state clearly (avoid 0/1/2 — use "Manual"/"Auto"/"Cascade")

### Dropdown
Selection from list of options.

**Use when:** recipe selection, equipment selection, alarm filter, configuration choices.

**Pattern for dynamic lists:**
- Bind `options` to a Named Query result dataset
- Bind `value` to a tag or view parameter
- Columns: `label` (display text), `value` (stored value)

---

## Navigation Components

### Menu Tree
Hierarchical navigation matching ISA-95 structure.

**Use for:** primary side navigation with Site → Area → Equipment drill-down.

**Key properties:**
- `items`: nested menu item dataset
- `onItemClicked`: navigate to target view with parameters
- Configure icons for equipment types

### Horizontal Menu
Top-bar global navigation.

**Use for:** site-level navigation (switch between plant areas, global tools).

### Link
Contextual navigation to related views.

**Use for:** "View Equipment Detail", "Open Alarm History", cross-references.

**Style consistently** — users should recognize links as navigable.

---

## Perspective View JSON Structure

Every view is a `view.json` file:

```json
{
  "custom": {},
  "params": {
    "equipmentId": {"value": 0, "type": "int"},
    "tagBasePath": {"value": "", "type": "str"}
  },
  "propConfig": {},
  "root": {
    "children": [
      {
        "children": [],
        "meta": {"name": "AlarmBanner"},
        "position": {"basis": "48px", "grow": 0, "shrink": 0},
        "props": {
          "path": "shared/AlarmBanner"
        },
        "type": "ia.display.view"
      }
    ],
    "meta": {"name": "root"},
    "props": {
      "style": {
        "backgroundColor": "#888888"
      }
    },
    "type": "ia.container.flex"
  }
}
```

Key rules:
- `params`: view's public API — document all parameters
- `root`: always a container (usually `ia.container.flex`)
- `meta.name`: required for components referenced by scripts
- `custom`: view-level custom properties
- `position`: flex layout properties (`grow`, `shrink`, `basis`)
