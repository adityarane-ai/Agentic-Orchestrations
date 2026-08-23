# 10. QI Studio Nodes, Agent Tools & Verification Register

**Document Version:** 1.2  
**Status:** Current UI/tool-contract evidence register  
**Updated:** 23 Aug 2026

This document records platform behaviour that has been explicitly shown in supplied QI Studio screenshots, platform documentation or directly supplied tool definitions. Runtime claims belong in `12 - End-to-End Node Test Log.md`.

For the canonical current understanding and unresolved questions, see `13 - Current Understanding & Verification Ledger.md`.

---

# 1. Evidence Status Model

| Status | Meaning |
|---|---|
| Confirmed | Directly shown in UI/platform documentation or tool definition. |
| Runtime Confirmed | Demonstrated by an actual execution; recorded in the runtime test log. |
| Working Understanding | Strong interpretation supported by evidence but not fully proven. |
| Pending Verification | Needs a targeted runtime test or additional evidence. |
| Contradicted | Older assumption disproven by stronger evidence. |
| Superseded | Historical statement replaced by newer evidence. |

---

# 2. Orchestration Nodes

## 2.1 Decision Tree

**Platform status:** Experimental / Alpha based on supplied UI.

A Decision Tree is a mini flow-builder inside one node. It holds an internal state-driven graph and executes steps based on state.

### Captured internal steps

| Step | Purpose |
|---|---|
| Start | Entry point for the internal tree. |
| Ask user | Ask a person and capture the reply into state. |
| Tool call | Invoke a backend tool and write its result into state. |
| Compute | Derive/transform state without user interaction. |
| Condition | Branch based on state. |
| Done | End the internal flow. |

### Captured cross-step controls

- Produces keys
- State / Memory Keys
- Extra Conditions
- Message Template
- Notes for the LLM
- Require reflection before this step fires
- Fit / Auto-layout
- Issues validation

### Captured condition operators

- Equals
- Not Equals
- Greater Than or Equal To
- Less Than or Equal To
- Greater Than
- Less Than
- Contains
- Is Empty
- Is Not Empty

### Current status

**Confirmed UI/platform documentation.** Runtime state schema, cycle behaviour, persistence and limits remain unverified.

---

## 2.2 Approval

Purpose: pause orchestration and wait for human approval/rejection before continuing.

### Captured configuration

| Setting | Evidence |
|---|---|
| Approval Message | User-facing approval request. |
| Approve Button | Custom label; default shown as `Approve`. |
| Reject Button | Custom label; default shown as `Reject`. |
| Approved route | Outgoing path for approval. |
| Rejected route | Outgoing path for rejection. |
| State Update | Available. |
| Output Variables | Available. |
| Decision | Captured as `decision` string in the supplied UI. |

Current status: **Confirmed UI; runtime pending.**

---

## 2.3 Human Input

Purpose: pause, ask a person for free-text input, save it to a configured target and continue.

### Captured configuration

| Setting | Evidence |
|---|---|
| Question | Prompt shown to the user. |
| Save Response As | Target variable, e.g. `system.humanInput`. |
| State Update | Available. |
| Output Variables | Includes `input` and `variableTarget` in the displayed node output. |

Runtime-confirmed exact path is recorded in the test log:

```text
Human Input → system.humanInput → explicit Output → User Window
```

---

# 3. Agent Node Tool-Management UI

The supplied Agent-node screenshots confirm the following per-tool controls/capabilities:

| Capability | Status |
|---|---|
| Tool Name | UI Confirmed |
| Description | UI Confirmed |
| Visual Editor / JSON Schema | UI Confirmed |
| Typed parameters | UI Confirmed |
| Required flags | UI Confirmed |
| Configure Values | UI Confirmed |
| State Update | UI Confirmed |
| Variable Path | UI Confirmed |
| Include Thoughts | UI Confirmed |
| Human-in-the-Loop | UI Confirmed |
| Response Filtering | UI Confirmed |
| Store Tool Output | UI Confirmed |
| Return Direct | UI Confirmed |
| Handoff Target Node | UI Confirmed for handoff tools |

These are configuration capabilities, not universal runtime semantics. Runtime interactions remain under test.

---

# 4. System Tool Inventory Captured

The supplied System-tab screenshots showed:

### Handoff

1. `Handoff`

### Knowledge

2. `Get Library Metadata`  
3. `Get Data Search Fields`  
4. `Search Knowledge`  
5. `Get Reference File`  
6. `Get Table Schema`  
7. `Resolve Field Value`  
8. `Search Table Data`  
9. `get-knowledge-workflow-instructions`

### Memory

10. `Recall Memory`  
11. `Save Memory`  
12. `Update Memory`  
13. `Set In Memory`  
14. `Get From Memory`

### Export / document

15. `Export PowerPoint V2`  
16. `Export Excel V2`  
17. `Export PDF V2`  
18. `Export Word V2`  
19. `Export HTML V2`  
20. `Extract Document to Markdown`  
21. `Export File`  
22. `Export to CSV`  
23. `Export to Excel`  
24. `Export to PowerPoint`  
25. `Export to Word`

### Web / communication / files

26. `Web Search`  
27. `Send Email`  
28. `Conversation Attachment`

### System tool discovery / execution

29. `Search System Tools`  
30. `Get System Tool Schema`  
31. `Execute System Tool`

**Important:** Shared, MCP and Quantum MCP tabs are visible but their complete inventories have not been captured. Do not infer them.

---

# 5. Detailed Tool Contracts Supplied

## 5.1 get-knowledge-workflow-instructions

No parameters.

Mandatory rule supplied by the tool:

> Call this tool **FIRST** before any knowledge-related tool, at the start of every agent invocation involving knowledge sources.

This supersedes older wording that treated `get_library_metadata` as the first knowledge call.

---

## 5.2 BraveWebSearch / Web Search

**Tool name:** `BraveWebSearch`

### Inputs

| Parameter | Type | Required |
|---|---|---|
| `query` | string | Yes |
| `searchFromDate` | date-time or null | Yes |

### Output

A list of search results with:

- `Name`
- `Link`
- `Value`

The supplied contract states that if search results are used in the final answer, their links must also be included through the tool's `citations` path.

Status: **Tool contract confirmed; runtime pending.**

---

## 5.3 Store / Set In Memory

**Tool name:** `Store`

### Inputs

```text
key: string
value: any
```

Both required.

The tool description explicitly restricts use to cases where there are clear instructions to store data in memory.

Status: **Tool contract confirmed; runtime pending.**

---

## 5.4 Retrieve / Get From Memory

**Tool name:** `Retrieve`

### Input

```text
key: string
```

Required.

Returns the value associated with the key if found.

The description explicitly restricts use to cases where there are clear instructions to retrieve data from memory.

Status: **Tool contract confirmed; runtime pending.**

---

## 5.5 SendEmail

Required:

```text
tos
subject
emailBody
```

`emailBody` must be complete valid HTML with inline styles. The contract requires closing the email with:

```text
Regards, GEP Quantum
```

Optional:

- `ccs`
- `bccs`
- `replyTo`
- `sender`
- `attachments`

Recipients contain an `email` plus optional contact metadata. Attachments use:

```text
{name, id}
```

Status: **Tool contract confirmed; runtime pending.**

---

## 5.6 ConversationAttachment

Input:

```text
fileId: string
```

Required.

Returns uploaded session-file content as a string. The supplied description says to call it separately for each relevant file when multiple attachments need to be read.

The supplied instructions also say that image source references in returned content should be preserved exactly when used downstream.

Status: **Tool contract confirmed; runtime pending.**

---

## 5.7 ExportBlob / Export File

Input:

```text
fileId: string
```

Required.

Output:

- `Name`: blob file name
- `Id`: blob file identifier/path

The description states that the returned object should be included in the final response `attachments` array.

The supplied configuration also appends the node output to `system.files`.

Status: **Tool contract confirmed; runtime file-flow pending.**

---

# 6. Export Tool Contracts

## 6.1 Export Excel V2

Required top-level inputs:

```text
filename
sheets
```

Each sheet requires:

```text
name
 data
```

Supported cell values:

- string
- number
- boolean
- ISO date string
- Excel formula beginning with `=`
- null

Sheet features captured:

- headers
- freeze panes
- autofilter
- tab color
- column widths
- header formatting
- column-scoped number formats
- conditional formatting
- bar/column/line/pie charts

The supplied tool definition appends the export result to `system.files`.

Status: **Tool contract confirmed; runtime file handling pending.**

## 6.2 Export PowerPoint V2

Required:

```text
title
slides
```

Optional:

```text
template
```

Captured templates:

- default
- gep
- business_blue_black

Captured slide layouts include title slide, section header, title/content combinations, two/three content, picture and chart variants. Supported layouts depend on the selected template; unsupported layouts must not be used.

Status: **Tool contract confirmed; runtime pending.**

## 6.3 Export PDF V2

Required:

```text
title
sections
```

Supports structured sections including:

- cover
- TOC
- headings
- paragraphs
- bullet/numbered lists
- tables
- images
- charts
- spacers
- page breaks
- key/value blocks
- callouts
- endnotes
- horizontal rules

Also supports page size, orientation, margins, cover page, headers/footers, author and style overrides.

Status: **Tool contract confirmed; runtime pending.**

## 6.4 Export Word V2

Required:

```text
title
sections
```

Supports structured sections including cover, TOC, headings, paragraphs, lists, tables, images, editable native charts, hyperlinks, page/section breaks, bookmarks, cross-references, footnotes and custom styles.

Also supports document metadata and TOC field updating on open.

Status: **Tool contract confirmed; runtime pending.**

## 6.5 Export HTML V2

Required:

```text
title
sections
```

Captured section types:

- hero
- section
- text
- bullet list
- stat cards
- table
- chart
- callout
- code block
- timeline
- two-column
- image
- divider

Themes:

- light
- dark
- corporate

Supports `accent_color` override.

The tool description explicitly says it may be used proactively for structured dashboards/reports/visual pages.

Status: **Tool contract confirmed; runtime pending.**

---

# 7. Extract Document to Markdown

Input:

```text
fileId: string
fileName: string
bpc: optional string
sessionId: optional string
```

Supported formats captured:

- PDF
- DOCX
- PPTX
- DOC
- PPT
- PNG/JPG/JPEG/GIF/WEBP/BMP/TIF/TIFF

The supplied PDF pipeline includes:

1. MarkItDown text extraction.
2. PyMuPDF image extraction.
3. Header/footer/watermark filtering.
4. Composite image stitching.
5. OCR for scanned pages.
6. Vision-LLM image descriptions.
7. Reading-order merge of text and image descriptions.
8. Upload of the final Markdown/text result.

Status: **Tool contract confirmed; runtime pending.**

---

# 8. System Tool Discovery / Execution

The supplied system tools establish a strict three-step pattern:

```text
SearchSystemTools
      ↓
GetSystemToolSchema
      ↓
ExecuteSystemTool
```

## SearchSystemTools

Input:

```text
intent: string
```

Purpose: discover relevant system tools. The description explicitly says not to guess tool names.

## GetSystemToolSchema

Input:

```text
toolNames: array<string>
```

Purpose: retrieve full parameter schema after discovery and before execution.

## ExecuteSystemTool

Inputs:

```text
tool_name: string
arguments: object
```

The tool name must exactly match the discovered tool name and arguments must match the retrieved schema.

Status: **Tool contract confirmed; runtime pending.**

---

# 9. Knowledge Tools - Current Evidence

The following knowledge tools are captured:

- `get_library_metadata`
- `get_data_search_fields`
- `search_knowledge`
- `get_reference_file`
- `get_table_schema`
- `resolve_field_value`
- `search_table_data`
- `get-knowledge-workflow-instructions`

The strongest current workflow rule is:

```text
get-knowledge-workflow-instructions
        ↓
get_library_metadata
        ↓
source-specific tools
```

For data-search sources:

```text
get_library_metadata
        ↓
get_data_search_fields
        ↓
execute_search_query
```

When the data-search schema contains `default_filters`, the supplied instructions describe them as mandatory access-control filters that must be retained for subsequent search execution.

`search_knowledge` is described as performing semantic retrieval, relevance ranking and possible reranking. Exact runtime ranking/reranking behaviour remains unverified.

---

# 10. Detailed Tool Runtime Queue

The following require targeted runtime tests rather than further UI description:

- Agent output contract.
- Handoff execution.
- Approval decision/routing/resume.
- Decision Tree internal state.
- Flow Variable write/read/transform.
- Variable Path.
- Store Tool Output.
- Return Direct.
- Response Filtering.
- Tool-level Human-in-the-Loop.
- System tool discovery/execution.
- Memory store/retrieve.
- Web Search citation behaviour.
- Email HTML/attachment flow.
- Conversation attachment reading.
- Export file result and `system.files` propagation.
- Downstream consumption of exported files.
- Knowledge initialization + metadata + source-specific execution.

Do not create separate low-information tests for every checkbox. Use the eight high-information end-to-end tests defined in the runtime test log.

---

# 11. Documentation Rule

Never infer a runtime contract from a tool name alone.

For each newly captured node/tool, record:

1. exact name;
2. exact description;
3. inputs/types;
4. required/optional fields;
5. State Update;
6. Variable Path;
7. Output Variables;
8. advanced settings;
9. routing/continuation behaviour;
10. hard rules from the supplied tool description;
11. runtime evidence;
12. unresolved questions.
