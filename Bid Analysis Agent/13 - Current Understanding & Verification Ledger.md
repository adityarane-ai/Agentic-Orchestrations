# 13. Current Understanding & Verification Ledger

**Document Version:** 1.0  
**Status:** Canonical reverse-engineering control document  
**Updated:** 23 Aug 2026

## Purpose

This file is the single control point for the **current platform understanding** while QI Studio is being reverse-engineered.

It prevents three common problems:

1. old assumptions being mistaken for current truth;
2. UI-only observations being presented as runtime-confirmed behaviour;
3. pending questions being duplicated across multiple documents.

The detailed evidence remains in the node/tool register and runtime test log. This ledger states what is currently believed and what is still unresolved.

---

# 1. Evidence Status Model

| Status | Meaning |
|---|---|
| Confirmed | Directly shown in supplied UI/platform documentation. |
| Runtime Confirmed | Demonstrated by an actual end-to-end execution, including downstream/user-visible behaviour where applicable. |
| Working Understanding | Strong interpretation supported by evidence but not fully demonstrated. |
| Pending Verification | Known capability/question that still needs a targeted test or stronger evidence. |
| Contradicted | A prior statement was disproven by later evidence. |
| Superseded | An earlier design/test remains historically useful but is no longer the current explanation. |

**Rule:** UI evidence never upgrades a capability to Runtime Confirmed by itself.

---

# 2. Canonical Current Understanding

## 2.1 Variable areas

QI Studio exposes distinct logical areas for:

- **Flow** - workflow-owned state created through the Variables UI.
- **System** - platform-managed runtime/context variables.
- **Conversation History** - conversation messages/context.
- **Runtime** - execution/environment metadata.
- **All** - aggregate view.

The areas are not interchangeable.

## 2.2 Flow Variables

Flow Variables are created explicitly through the Variables UI with a name, type, scope and optional description/default value.

Observed types:

- String
- Number
- Boolean
- Array
- Object

Current implementation rule:

> Use Flow scope for normal workflow-owned state unless a System-scope custom-variable behaviour has been explicitly runtime-tested.

## 2.3 Built-in System variables

Runtime evidence confirms at least these built-in surfaces:

- `system.userQuery`
- `system.sessionId`
- `system.dateTime.utcNow`
- `system.attachments`
- `system.files`
- `system.humanInput`
- `system.uiAction`

The supplied execution also showed `system.timestamp` in the Variables UI model. Exact runtime representation should not be assumed beyond the observed environment.

## 2.4 Output contract

The strongest current rule is:

> **Output requires an explicit response source.**

A value existing in `nodes`, `flow`, or `system` does not automatically become the user-visible response.

Runtime-confirmed example:

```text
Human Input
    ↓
system.humanInput
    ↓
Output configured as {{system.humanInput}}
    ↓
output.messages
    ↓
User Window
```

`output.messages = START_TEST` was observed successfully.

## 2.5 Human Input

Human Input pauses the workflow, collects a human response, and writes it to the configured target.

Runtime-confirmed target:

```text
system.humanInput
```

Runtime evidence showed:

```text
resumeUserInput.hitlType = human_input
resumeUserInput.variableTarget = system.humanInput
nodes.human_input_0.input = START_TEST
system.humanInput = START_TEST
```

## 2.6 Flow-variable target

A Human Input can be configured with a Flow Variable target, and a successful execution was observed with `flow.startTestResponse = START_TEST` in an earlier run.

However, the current evidence set deliberately keeps the **Flow Variable → Output linkage** as a controlled comparison item rather than treating it as fully isolated runtime proof. The exact output-resolution behaviour for the Flow path must be confirmed independently from the System-variable test.

## 2.7 Conversation History

Conversation History is conversational context, not a substitute for explicit workflow state.

The tested execution retained the original START message in root conversation history, while the Human Input answer was represented through runtime/HITL state and its configured variable.

## 2.8 Runtime metadata

Runtime variables describe execution context such as environment, workflow metadata and authorization headers. They are not business-state storage.

---

# 3. Confirmed Node-Level Understanding

## START

Confirmed:

- accepts the initial user input;
- executes successfully in the tested flow;
- exposes the incoming message through `system.userQuery` in the observed runtime.

Runtime test coverage is still intentionally minimal.

## Human Input

Confirmed UI + Runtime:

- pauses execution;
- resumes after human response;
- writes to an explicit target variable;
- exposes node-level `input` and `variableTarget`;
- at least `system.humanInput` can be consumed directly by Output when explicitly referenced.

## Approval

Confirmed UI:

- Approval Message;
- Approve and Reject button labels;
- Approved and Rejected routing;
- State Update and Output Variables sections;
- `decision` string shown in the captured configuration.

Runtime semantics remain pending.

## Decision Tree

Confirmed UI/platform documentation:

- internal Start;
- Ask user;
- Tool call;
- Compute;
- Condition;
- Done;
- memory/state keys;
- produced-key gating;
- validation issues;
- path conditions including an else path.

Runtime semantics remain pending.

---

# 4. Agent Node: Current Understanding

The Agent tool-management UI exposes per-tool configuration including:

- tool name and description;
- typed parameters and required flags;
- State Update;
- Variable Path;
- Include Thoughts;
- Human-in-the-Loop;
- Response Filtering;
- Store Tool Output;
- Return Direct;
- Handoff Target Node for handoff tools.

These are **UI-confirmed capabilities**. Their runtime interactions are not yet generally proven.

### Important distinction

`Variable Path`, `Store Tool Output`, `Return Direct`, Response Filtering and State Update are configuration capabilities, not yet a proven universal runtime contract across all tools.

---

# 5. Knowledge Workflow: Current Rule

A supplied platform tool definition explicitly states:

> `get-knowledge-workflow-instructions` must be called FIRST before any knowledge-related tool, at the start of every agent invocation involving knowledge sources.

Therefore the canonical sequence is:

```text
Knowledge-related agent invocation
        ↓
get-knowledge-workflow-instructions
        ↓
get_library_metadata
        ↓
source-specific knowledge tools
```

For a `data-search` knowledge source:

```text
get_library_metadata
        ↓
get_data_search_fields
        ↓
execute_search_query
```

When `default_filters` are supplied by the schema tool, those filters are described as mandatory access-control constraints and must be retained in subsequent search execution.

### Conflict resolution

Any older document saying that `get_library_metadata` is the **first** knowledge call is superseded by this newer evidence. The mandatory first step is now `get-knowledge-workflow-instructions`.

---

# 6. Captured System Tool Families

The following tool definitions have now been supplied directly and should be treated as tool-contract evidence:

### Web

`BraveWebSearch`

- query: string, required
- searchFromDate: date-time or null, required
- returns search results with Name, Link and Value
- if used in a final answer, links must be represented in the tool's `citations` output contract

### Memory

`Store`

- key: string, required
- value: any, required
- explicitly limited to cases where there is clear instruction to store data

`Retrieve`

- key: string, required
- retrieves a value by memory key
- explicitly limited to cases where there is clear instruction to retrieve data

### Communication

`SendEmail`

Required:

- `tos`
- `subject`
- `emailBody`

The email body must be fully generated HTML with inline formatting. The configured contract requires the email to close with `Regards, GEP Quantum`.

Optional:

- cc
- bcc
- replyTo
- sender
- attachments

Attachments use file `name` + `id` objects.

### Conversation files

`ConversationAttachment`

- fileId: string, required
- returns uploaded session-file content as string
- should be called for each relevant attachment when multiple files are present

### Blob file export

`ExportBlob`

- fileId: string, required
- returns file `Name` and `Id`
- returned object is intended for inclusion in final-response `attachments`
- also appends its result to `system.files` through the variable update shown in the tool definition

### System tool discovery/execution

`SearchSystemTools`

- intent: string, required
- must be called first before executing a system action; do not guess system tool names

`GetSystemToolSchema`

- toolNames: array<string>, required
- called after SearchSystemTools and before ExecuteSystemTool

`ExecuteSystemTool`

- tool_name: string, required
- arguments: object, required
- must use the exact tool name/schema obtained from the preceding discovery steps

### Knowledge initialization

`get-knowledge-workflow-instructions`

- no parameters
- mandatory first call for any knowledge-related agent invocation

---

# 7. Export / Document Tool Contracts Captured

The following are now directly documented and should be treated as **tool-contract evidence**, not runtime-confirmed behaviour:

- Export Excel V2
- Export PowerPoint V2
- Export PDF V2
- Export Word V2
- Export HTML V2
- Extract Document to Markdown

Common current understanding:

- export tools create files;
- the generated file result is appended to `system.files` through the documented variable update;
- exports have explicit structured input contracts;
- exact runtime file-object shape and downstream consumption still need testing.

### Export Excel V2

Supports:

- multiple sheets;
- strings, numbers, booleans, ISO dates, formulas and nulls;
- header styling;
- freeze panes;
- autofilter;
- tab color;
- explicit/automatic column widths;
- column-scoped number formats;
- conditional formatting;
- bar/column/line/pie charts.

### Export PowerPoint V2

Supports structured slide layouts and templates including default, GEP and business blue/black. Layout support depends on the chosen template and unsupported layouts must not be selected.

### Export PDF V2

Supports structured sections including cover, TOC, headings, paragraphs, lists, tables, images, charts, callouts, endnotes, page configuration, headers/footers and style overrides.

### Export Word V2

Supports structured sections including cover, TOC, headings, paragraphs, lists, tables, images, native editable charts, hyperlinks, page/section breaks, bookmarks, cross-references, footnotes, headers/footers and custom styles.

### Export HTML V2

Supports hero sections, text, bullets, stat cards, tables, charts, callouts, code blocks, timelines, two-column panels, images and themes. It can be used proactively when a visual dashboard/report/page would be more useful than plain text.

### Extract Document to Markdown

Supports PDF, DOCX, PPTX, legacy DOC/PPT and common image formats. PDF processing combines structured text extraction, image extraction/vision, OCR for scanned pages and coordinate-aware merge into Markdown.

---

# 8. Runtime Test Baseline

## Test A - Explicit System response

```text
START
  ↓
Human Input → system.humanInput
  ↓
Output → {{system.humanInput}}
```

Result: **Runtime Confirmed**

Observed final response: `START_TEST`.

## Test B - Flow-variable target

```text
START
  ↓
Human Input → flow.startTestResponse
  ↓
Output → {{flow.startTestResponse}}
```

Earlier execution showed `flow.startTestResponse = START_TEST` and a successful final response. However, because a subsequent controlled System-variable test produced the same final value without populating the Flow Variable, this path remains a controlled comparison item until independently rerun with the exact Flow-target configuration and isolated output path.

Status: **Runtime Partial / needs isolated confirmation**.

## Earlier regression

When no explicit Output response source was configured, the workflow completed but the user-facing layer returned:

```text
No response content found in the execution result. Please try again.
```

This remains valid regression evidence for the rule that Output mapping is explicit.

---

# 9. High-Information End-to-End Test Set

Only the following eight tests are needed as the next compact learning path:

1. **T1 START → Output** - baseline START and Output contract.
2. **T2 START → Human Input(system) → Output** - HITL pause/resume and built-in system variable path.
3. **T3 START → Human Input(flow) → Output** - isolated Flow Variable path.
4. **T4 Variable lifecycle** - create/write/read/transform state without relying on Human Input as writer.
5. **T5 Condition / branching** - positive and negative execution using the same flow.
6. **T6 Approval** - approve/reject routing and resume semantics.
7. **T7 Agent → Tool → State → Output** - tool result, Variable Path, Store Tool Output and Return Direct comparison.
8. **T8 Composite workflow** - knowledge + tool + HITL + export + output/file handling.

The purpose is to maximize learning per test rather than create one test per UI checkbox.

---

# 10. Pending Verification Queue

### Nodes

- Flow Variable lifecycle and State Update semantics.
- Approval runtime contract.
- Decision Tree runtime contract.
- Output source resolution rules beyond tested examples.

### Agent

- Agent output contract.
- Agent state propagation.
- Tool invocation/result contract.
- Variable Path runtime semantics.
- Store Tool Output vs Return Direct.
- Response Filtering.
- Tool-level Human-in-the-Loop.
- Handoff runtime semantics.

### Knowledge

- Exact runtime enforcement of `get-knowledge-workflow-instructions` first-call rule.
- Metadata/source selection behaviour.
- Search Knowledge runtime ranking/reranking.
- Data-search mandatory-filter enforcement in a live run.

### Files / exports

- Exact export result object.
- `system.files` runtime shape.
- Passing exported files to downstream tools.
- Final-response attachment behaviour.

### Remaining tool catalogue

The current System-tab inventory is documented, but Shared, MCP and Quantum MCP inventories are incomplete and must not be inferred.

---

# 11. Pending-Item Lifecycle Rule

This ledger is intentionally temporary for unresolved understanding.

When a pending item is verified:

1. record the actual evidence in the relevant evidence document/test log;
2. upgrade the item here to Confirmed or Runtime Confirmed;
3. remove the item from the Pending Verification Queue on the next documentation cleanup;
4. update any architectural statement that depended on the old uncertainty.

When evidence disproves an assumption:

1. retain the historical observation in the test log;
2. mark the old statement Contradicted or Superseded;
3. update the current understanding here;
4. eliminate contradictory wording from primary design documents.

This keeps the repository internally consistent without destroying historical evidence.
