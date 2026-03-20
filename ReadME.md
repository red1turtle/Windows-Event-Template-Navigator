# Windows Event Template Explorer — User Guide

Version covered: **Schema-aware MITRE + Helix v1.18**

## What this app does

This single-page web app lets you load Windows event template data and explore it with:

- **Windows event message templates** from JSON or single-entry ZIP files
- **MITRE ATT&CK detection strategy mappings** embedded in the message object
- **HELIX rules** from JSON
- **HELIX field taxonomy** from CSV
- **Custom field mappings** you create inside the app and export/import as JSON

It is designed for detection engineering, parsing design, ATT&CK coverage analysis, and Windows telemetry research.

---

## Quick start

1. Open the HTML file in a browser.
2. Click **Import templates JSON / ZIP** and load your Windows message template object.
3. Optionally load:
   - **Import rules JSON** for HELIX rule decoration
   - **Import taxonomy CSV** for field transform / taxonomy mapping
4. Use the **Global search** bar or the left-side filters to narrow results.
5. Click any row in the table to open the event detail drawer.
6. From the drawer, inspect:
   - schema metadata
   - MITRE mappings
   - rule matches
   - taxonomy field mapping
   - raw message text / description

---

## Main layout

The app uses a responsive multi-pane layout.

### Top row
- **Title / version**
- **Global search**
- **Import templates JSON / ZIP**

### Action row
- **Show filters / Hide filters**
- **Import taxonomy CSV**
- **Import rules JSON**
- **Clear filters**
- **Export page CSV**
- **Export mappings JSON**
- **Import mappings JSON**
- Dataset status pill

### Left pane
The left pane contains filters. It can be hidden with **Show filters / Hide filters**.

### Center pane
The center pane contains:
- dataset stats
- error summary / **View errors** button
- pagination controls
- the results table

### Right pane
The right pane is the event detail drawer. It opens when you click a table row.

### Center modal
Deep MITRE detail views open in a **centered modal** so large ATT&CK strategy content has more room.

---

## Supported imports

### 1) Templates JSON / ZIP
Use **Import templates JSON / ZIP**.

Supported inputs:
- a Windows message template JSON file
- a ZIP containing a single JSON payload

The app normalizes newer schema fields when present, including values like:
- `event_identifier`
- `qualifiers`
- `sources`
- `classic_sources`
- `keywordsdisplayNames`
- `level`
- `task`
- `opcode`
- MITRE mapping objects
- extraction or load errors
- additional schema keys

### 2) HELIX taxonomy CSV
Use **Import taxonomy CSV**.

This enables field transform / taxonomy mapping controls inside the event drawer.

### 3) HELIX rules JSON
Use **Import rules JSON**.

The app decorates rows and event details with matched CA rules and indexes rule metadata for search.

### 4) Saved mapping JSON
Use **Import mappings JSON** to restore previously exported field mappings.

---

## Global search

The **Global search** bar searches across the loaded dataset, including:

- EventId
- provider
- source / channel
- message and description text
- template field names
- newer schema fields
- extraction error text
- MITRE ATT&CK IDs, names, descriptions, URLs, mutable elements
- rule IDs, rule descriptions, tuning searches, tags, distinguishers, confidence, risk, severity, output, deleted state
- additional schema keys

### Search results
When global search is active, the table includes a **Search hit** column that explains where the match came from, for example:
- Message
- MITRE
- Rules
- Extra schema
- Template
- Provider
- Source

This makes it easier to understand *why* a row matched.

---

## Left-side filters

The filter pane contains focused filters for narrowing the event list.

### Common filters
- **Provider**
- **Source (channel)**
- **Event ID**
- **Description**
- **Any object text**

For some filters you can toggle:
- **Contains**
- **Case-sensitive**

### Template health
- **Only records with extraction / load errors**

### HELIX enrichment
- **Only templates with rule matches**

### MITRE ATT&CK
- **Only templates with MITRE mappings**

### Show / hide filters
Use the toolbar button to collapse the entire filter pane when you want more room for the results table.

---

## Results table

Each row represents one event template record.

Typical columns include:
- **EventId**
- **Source**
- **Provider**
- **Message**
- **Template fields**
- **State**
- **Search hit**
- **Rules**
- **MITRE**

### Table behavior
- click a row to open its detail drawer
- large text is wrapped so the page does not spill horizontally
- pagination is controlled from the stats bar above the table
- **Export page CSV** exports the current visible page

---

## Event detail drawer

Click any table row to open the right-side event drawer.

### Drawer header
The header shows the event identity and includes:
- **Copy details**
- **Close**

### Drawer badges / summary chips
Depending on the record, you may see badges for:
- record state
- template XML presence
- template version
- level / task / opcode
- extra fields count

### Schema summary
The drawer can show normalized event metadata such as:
- EventId
- event identifier
- qualifiers
- provider
- source
- sources
- classic sources
- version
- template hash

### Level / task / opcode cards
These cards explain normalized metadata that comes from the newer schema.

They are intended to be useful rather than just raw values. The cards may include:
- resolved label
- raw value
- resolution state
- common meaning
- why the field matters

### Additional schema keys
If the loaded object has extra top-level keys beyond the modeled structure, they are surfaced here rather than being hidden.

---

## HELIX rules in the drawer

If rules are loaded and matched, the drawer includes a **Rules (CA)** section.

For each rule, the app can show:
- rule id / revision
- title
- enabled badge
- deleted badge
- output badge(s)
- queues
- search
- tuning search
- description
- tags
- distinguishers
- confidence
- risk
- severity

All of the above are searchable through **Global search**.

---

## Field transforms / taxonomy mapping

If taxonomy CSV is loaded, the drawer includes a **Field transforms (HELIX taxonomy)** section.

You can use it to map Windows template fields to HELIX taxonomy fields.

### Typical workflow
1. Load taxonomy CSV.
2. Open an event template.
3. In the field transform section, select taxonomy targets for the event’s template fields.
4. Export those mappings using **Export mappings JSON**.
5. Re-import later with **Import mappings JSON**.

---

## MITRE ATT&CK support

This app has deep ATT&CK-aware behavior when the loaded template object includes MITRE content.

### What is surfaced
Depending on the event and your schema, the app can show:
- tactics
- techniques
- sub-techniques
- detection strategies
- analytics
- data components
- log sources
- channels
- platforms
- ATT&CK URLs
- descriptions
- mutable elements

### MITRE summary chips
At the event level, summary chips such as **Tactics**, **Techniques**, **Detection strategies**, **Analytics**, and **Data components** are clickable.

Clicking one opens a focused ATT&CK view for that layer.

### DET / analytic cards
Detection strategy rows are clickable.

The focused view can include:
- DET id and name
- match record count on the current event
- linked event count across the loaded dataset
- tactics / techniques / sub-techniques
- analytics
- analytic details / how to detect
- data components
- data component details
- log sources
- channels
- platforms
- descriptions
- reference URLs
- mutable elements
- raw MITRE match JSON

### Raw match JSON
The **Raw match JSON** block is collapsed by default. Expand it only when you need the exact source object.

### Linked events
The MITRE modal includes linked events across the loaded dataset.

This is useful for:
- seeing all Windows events associated with a detection strategy
- checking **template version** coverage
- quickly jumping from a strategy to another linked event

The linked-events section also summarizes which template versions are present so you can reason about OS or template coverage.

### Copying
Use **Copy section** in the MITRE modal to copy the currently focused ATT&CK content.

---

## Error handling

If the app detects extraction or load issues in the imported object:
- the dataset summary shows an error count
- **View errors** becomes available
- error rows can be filtered with **Only records with extraction / load errors**

### View errors
Click **View errors** to inspect the load/extraction problems.

From there you can:
- see which records failed or degraded
- review error text
- open the event drawer for the affected record

---

## Resizing and responsive behavior

The app adapts to viewport size.

### Large desktop / ultrawide
- the shell expands to use more horizontal space
- the center pane and right drawer get a better split
- the MITRE detail view opens in a larger centered modal

### Laptop-sized screens
- the layout stays tighter and more compact
- the current sizing is intentionally closer to the laptop-friendly view

### Resizable event drawer
The right event drawer can be resized from its left edge.

- drag the resize handle to widen or narrow the drawer
- the chosen width is remembered in `localStorage`

---

## Export and persistence

### Export page CSV
Exports the current table page as CSV.

### Export mappings JSON
Exports the field transform mappings you have created.

### Import mappings JSON
Restores previously saved mappings.

### Local persistence
Some UI preferences are remembered in `localStorage`, such as:
- filter pane visibility
- drawer width

---

## Recommended workflow for detection engineering

### Coverage review
1. Load templates JSON / ZIP.
2. Search for a concept such as `persistence`, `credential`, `rdp`, or `powershell`.
3. Use the **Search hit** column to understand whether the match came from the message, MITRE layer, rules, or another schema field.
4. Open the event drawer.
5. Review MITRE mappings and linked events.

### Rule engineering
1. Import rules JSON.
2. Search by rule content, tag, severity, or technique.
3. Open candidate events.
4. Compare event message structure to rule search and tuning search.
5. Use taxonomy mapping to align extracted fields to HELIX.

### ATT&CK strategy validation
1. Open an event with MITRE mappings.
2. Click a DET row or summary chip.
3. Review analytics details and data components.
4. Use linked events and template versions to evaluate coverage depth across the dataset.

---

## Troubleshooting

### “No templates loaded”
You have not yet imported a templates JSON or ZIP.

### Search returns no matches
Check whether:
- the relevant data source is loaded
- the search term is in rules, MITRE, or schema fields rather than the message text
- a left-side filter is narrowing the result set too aggressively

### Rule matches are missing
Check that:
- rules JSON was imported successfully
- the rule schema includes searchable fields such as `search` and `tuningSearch`
- the event IDs in the rule content line up with the loaded templates

### Taxonomy controls are unavailable
Import the taxonomy CSV first.

### Too little table space
Use **Hide filters** and, on large screens, widen the event drawer only as much as you need.

---

## Best practices

- Load templates first, then rules and taxonomy.
- Use **Global search** for broad discovery.
- Use the left pane for precise narrowing.
- Treat **Search hit** as the explanation layer for why a result surfaced.
- Use the MITRE linked-events view to validate coverage across related events, not just one row.
- Export mappings periodically so you do not lose field mapping work.

---

## In one sentence

This app is a browser-based workbench for exploring Windows event templates, linking them to MITRE ATT&CK detection strategy content, decorating them with HELIX rules, and mapping parser output to a HELIX field taxonomy.
