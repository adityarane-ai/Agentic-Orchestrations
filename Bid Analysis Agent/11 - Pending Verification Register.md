# 11. Pending Verification Register

**Document Version:** 1.0  
**Status:** Living verification backlog  
**Scope:** All currently unresolved QI Studio / Agent-node behaviours, tool semantics, configuration details and runtime assumptions identified during the current design investigation.

## Purpose

This file is the working backlog for anything that is understood well enough to document but **not yet sufficiently evidenced to be treated as confirmed platform behaviour**.

The rule is simple:

```text
Observation / working understanding
        ↓
Pending Verification Register
        ↓
Direct UI evidence or runtime test
        ↓
Confirmed evidence document
        ↓
Remove from this register
```

Pending items must not be promoted into the implementation contract as facts unless the evidence required by the item has actually been captured.

---

# 1. Verification Status Model

| Status | Meaning |
|---|---|
| Pending Evidence | The capability or behaviour is known to exist or has been mentioned, but sufficient direct evidence has not been captured. |
| Runtime Validation | UI/configuration is visible, but actual execution behaviour must be tested. |
| Partially Verified | Some aspects are evidenced, but important details remain unresolved. |
| Blocked | Verification depends on access, environment, data, or a platform capability that is not currently available. |

**Completion rule:** An item is removed from this file only when the requested evidence has been captured and incorporated into the appropriate confirmed documentation.

---

# 2. Orchestration Nodes

## 2.1 Decision Tree

- Exact runtime schema for Decision Tree state / memory keys.
- Whether all internal step types use identical state read/write semantics.
- Exact persistence / serialization format for an internal Decision Tree.
- Runtime behaviour for cycles or circular paths.
- Runtime behaviour when `Produces Keys` are missing, duplicated, or inconsistent.
- Exact limits for tree depth, number of steps, branches, and paths.
- Exact semantics of `Extra conditions` in relation to normal graph routing.
- Exact behaviour of `Require reflection before this step fires`.
- Whether an internal `Done` step returns a structured result and, if so, its schema.
- How internal tool-call errors propagate to the containing Decision Tree and outer orchestration.

**Evidence needed:** UI evidence for configuration plus controlled runtime tests.

## 2.2 Approval Node

- Exact output schema beyond the displayed `decision: string`.
- Whether approval actor / user identity is automatically exposed.
- Whether approval timestamp is automatically exposed.
- Whether comments / rejection reasons can be captured.
- Timeout behaviour when no response is received.
- Retry / resume behaviour after timeout or interrupted execution.
- Whether approval state survives a resumed orchestration without extra configuration.
- Multiple approver support.
- Sequential approval support.
- Parallel approval support.
- What happens if an approval request is reopened or a user changes the decision.
- Exact persistence location of approval state.

## 2.3 Human Input Node

- Exact structure of the response object.
- Exact meaning and runtime type of the `input` output variable.
- Exact structure and behaviour of `variableTarget`.
- Whether the saved response contains only the answer or additional metadata.
- Attachment handling through Human Input.
- Structured responses or non-text response support.
- Timeout behaviour.
- Cancellation behaviour.
- Resume behaviour after a paused conversation.
- Whether the response variable automatically persists across resumed executions.
- Whether Human Input can safely collect data required by downstream typed tools.

---

# 3. Agent Node Tool Configuration

- Exact runtime semantics of `State Update` for tools.
- Whether a tool result and a state update are written atomically.
- Behaviour when a configured state path does not exist.
- Behaviour when multiple tools write to the same variable path.
- Exact scope/lifetime of tool result variable paths.
- Whether tool result paths remain available after handoff.
- Exact semantics of `Include Thoughts`.
- Whether included thoughts are ever exposed to downstream nodes or only internal execution traces.
- Exact semantics of tool-level `Human-in-the-Loop`.
- Interaction between tool-level HITL and orchestration-level Approval / Human Input nodes.
- Exact behaviour of response filtering for nested objects and arrays.
- Include vs exclude field precedence when both are configured.
- Whether filtered fields are removed before `Store Tool Output`.
- Whether `Store Tool Output` stores the raw response or the filtered response.
- Lifetime and retrieval location of stored tool output.
- Exact semantics of `Return Direct` when the tool also has state updates or output variables.
- Whether `Return Direct` terminates the agent turn, node, or entire orchestration.
- Exact error handling and retry behaviour for tool failures.
- Tool timeout limits and configurable timeout settings.

---

# 4. System Tool Inventory and Discovery

## 4.1 System Tool Catalogue

- Full inventory of the **Shared** tab has not yet been captured.
- Full inventory of the **MCP** tab has not yet been captured.
- Full inventory of the **Quantum MCP** tab has not yet been captured.
- Need to determine whether tool availability differs by agent, environment, role, or deployment.
- Need to determine whether the displayed System inventory is static or dynamically provisioned.

## 4.2 SearchSystemTools

**Observed contract:** accepts `intent` and returns matching system tools.

Pending:

- Whether the tool must actually be called before every system action or whether this is only an instruction.
- Whether the returned tool list is exhaustive or ranked / truncated.
- Exact response schema.
- Whether tool descriptions returned are authoritative and complete enough for execution.
- Whether aliases / display names are returned alongside exact tool names.
- Behaviour when no matching system tool exists.
- Behaviour for ambiguous intents matching multiple tools.
- Whether tool availability is permission-aware.

## 4.3 GetSystemToolSchema

**Observed contract:** accepts exact tool names returned by `SearchSystemTools`.

Pending:

- Exact response schema.
- Whether multiple tool schemas are returned in a stable one-to-one mapping.
- Whether schema includes descriptions, defaults, enum values, required fields and nested constraints in full.
- Whether the schema returned here exactly matches the executable runtime schema.
- Behaviour when a tool name is stale, disabled, or unavailable.

## 4.4 ExecuteSystemTool

**Observed contract:** accepts exact `tool_name` plus `arguments`.

Pending:

- Exact response schema for successful execution.
- Exact response schema for failures.
- Whether validation is performed against the schema returned by `GetSystemToolSchema`.
- Behaviour for missing required arguments.
- Behaviour for extra / unknown arguments.
- Behaviour for malformed nested objects.
- Retry semantics.
- Timeout semantics.
- Whether tool execution errors are returned as structured data or surfaced as agent errors.
- Whether execution is audited or logged separately.
- Whether the tool can execute exports that populate `system.files`.
- Exact interaction between discovery, schema lookup, execution and final response composition.

---

# 5. Knowledge Workflow

## 5.1 get-knowledge-workflow-instructions

Pending verification:

- Exact text / structure of the instructions returned at runtime.
- Whether the instruction tool is automatically executed by the platform or only available to the agent.
- Whether calling it is actually enforced before knowledge tools.
- Whether the instruction set can vary by environment, agent, library, or session.
- Whether returned instructions are versioned.
- How conflicts between this instruction set and individual knowledge-tool descriptions are resolved.

## 5.2 Knowledge Tool Ordering

The existing working understanding says:

```text
get-knowledge-workflow-instructions
        ↓
get_library_metadata
        ↓
source-specific knowledge workflow
```

Pending verification:

- Whether this ordering is technically enforced.
- Whether the knowledge workflow can be entered through `Handoff` without re-running initialization.
- Whether each agent invocation has to initialize independently.
- Whether knowledge instructions persist across node handoffs.
- Whether knowledge state is session-scoped or invocation-scoped.

---

# 6. Export Tools

## 6.1 Export Excel V2

The supplied tool definition is sufficiently detailed to document the input contract, but the following remain to be tested:

- Exact generated workbook structure beyond the documented contract.
- Formula calculation timing and whether formulas are recalculated on open.
- Behaviour of formulas with invalid references.
- Exact handling of ISO date strings and timezone semantics.
- Exact behaviour of `null` cells.
- Whether `headers: false` fully suppresses header styling and filter behaviour.
- Exact behaviour of `column_widths: auto` with long text and formulas.
- Conditional-format stacking order when multiple rules overlap.
- Exact chart rendering and supported combinations of series / chart types.
- Whether chart references include the final total row by default.
- Exact output file identifier and relationship to `system.files`.
- Whether the returned file object can be passed directly to `Send Email` attachments.

## 6.2 Export PowerPoint V2

Pending runtime verification:

- Exact output file object schema.
- Final slide rendering for each supported template/layout combination.
- Behaviour when unsupported layouts are supplied.
- Whether unsupported layouts always fallback or can lose content.
- Exact chart rendering.
- Exact image URL / local path handling.
- Behaviour with malformed image paths.
- Font availability and substitution behaviour.
- Whether `gep` and `default` are visually identical in all cases.
- Output file identifier compatibility with `Send Email` attachments.

## 6.3 Export PDF V2

Pending runtime verification:

- Exact output file object schema.
- Automatic TOC population behaviour.
- Endnote ordering and numbering behaviour.
- Page-break behaviour around tables and images.
- Landscape rendering for wide tables.
- Exact image fetch failures and error presentation.
- Font handling for extended Unicode.
- Chart rendering consistency.
- Output file identifier compatibility with `Send Email` attachments.

## 6.4 Export Word V2

Pending runtime verification:

- Exact output file object schema.
- TOC field update behaviour in Word clients.
- Bookmark / cross-reference update behaviour.
- Native chart editability.
- Footnote rendering.
- Section-break orientation and margin changes.
- Custom-style inheritance behaviour.
- Image sizing rules for values below/above 100.
- Output file identifier compatibility with `Send Email` attachments.

## 6.5 Export HTML V2

Pending runtime verification:

- Exact output file object schema.
- Whether generated pages are fully self-contained except for documented Google Fonts and Chart.js CDN dependencies.
- Offline behaviour when CDN assets are unavailable.
- Exact rendering across browsers.
- Interactive Chart.js behaviour after export.
- Image URL / data URI handling.
- Output file identifier compatibility with `Send Email` attachments.

## 6.6 ExportBlob / Export File

Observed contract:

- Input: `fileId`.
- Output: `Name` and `Id`.
- Returned object must be included in final `attachments`.

Pending:

- Exact meaning of `Id` versus file IDs returned by export tools.
- Whether `Id` is always directly consumable by `Send Email.attachments[].id`.
- Whether `Name` is always the attachment filename or may be a blob path.
- Behaviour for missing / invalid / expired file IDs.
- Whether file content is copied or merely referenced.
- Whether exported files require `ExportBlob` before they can be attached to email.

---

# 7. Extract Document to Markdown

Pending runtime / end-to-end verification:

- Exact `fileId` format required by the tool.
- Exact required/optional semantics of `bpc` and `sessionId`.
- Whether the output text file is always created successfully for every supported format.
- OCR activation threshold for scanned PDFs.
- OCR accuracy / failure behaviour.
- Vision-LLM description failure behaviour.
- Image grouping / stitching edge cases.
- Coordinate ordering accuracy for complex PDFs.
- Header/footer/watermark filtering false positives.
- Exact output envelope and whether `filePath` can be consumed directly by later tools.
- Exact relationship between extracted markdown image references and BlobProxy files.
- Behaviour on encrypted, corrupted, password-protected, or very large files.
- Maximum file size / page count beyond documented image limits.

---

# 8. Web Search

**Tool:** `BraveWebSearch`

Observed input contract:

- `query: string`
- `searchFromDate: string | null`, format `date-time`

Observed result fields:

- `Name`
- `Link`
- `Value`

Pending:

- Exact result count and ranking behaviour.
- Whether `searchFromDate` is strictly enforced as a discovery date filter.
- Timezone semantics for `searchFromDate`.
- Behaviour when `searchFromDate` is null.
- Duplicate-result handling.
- Domain / source diversity guarantees.
- Search freshness guarantees.
- Exact citation object expected by the final response when search results are used.
- Whether citations are automatically generated or must be constructed manually.
- Error and rate-limit behaviour.

---

# 9. Memory Tools

## 9.1 Set In Memory / Store

Observed:

- `key: string`
- `value: any`

Pending:

- Exact memory scope: session, conversation, user, workflow, or execution.
- Lifetime and expiration behaviour.
- Whether writes overwrite existing values or merge.
- Maximum key/value size.
- Supported complex value types.
- Concurrency behaviour for simultaneous writes.
- Security / isolation guarantees between workflows.
- Retrieval after node handoff.

## 9.2 Get From Memory / Retrieve

Observed:

- `key: string`

Pending:

- Exact not-found response.
- Type fidelity of retrieved values.
- Behaviour when stored values are malformed or unavailable.
- Whether retrieval is strongly consistent immediately after a write.
- Scope restrictions.

## 9.3 Recall / Save / Update Memory

These tools are visible in the System inventory but their detailed tool definitions have not yet been captured in the current evidence set.

Pending evidence:

- Exact parameters.
- Exact output schemas.
- Scope and persistence semantics.
- Difference between Save, Update, Set In Memory, and any other memory operations.
- Conflict resolution behaviour.
- Deletion semantics, if supported.

---

# 10. Send Email

Observed contract includes:

- `tos` required.
- `subject` required.
- `emailBody` required and must be full HTML with inline styles.
- Optional CC, BCC, reply-to, sender and attachments.
- Attachments use `{name, id}`.
- Email must close with `Regards, GEP Quantum`.

Pending:

- Exact recipient validation behaviour.
- Behaviour when optional contact metadata is omitted.
- Sender permission rules.
- HTML sanitization behaviour.
- Maximum email size.
- Maximum number and size of attachments.
- Attachment identifier compatibility with each export tool.
- Delivery failure response schema.
- Retry semantics.
- Duplicate-send prevention.
- Whether the tool returns a message ID / delivery ID.
- Whether BCC recipients are handled independently of To / CC failures.

---

# 11. Conversation Attachment

Observed:

- Requires `fileId`.
- Returns uploaded session-file content as a string.
- Should be called once per file when multiple attachments exist.

Pending:

- Exact size limits.
- Binary vs text behaviour for unsupported file types.
- Whether content is lossless for large documents.
- Encoding behaviour.
- Exact error response for inaccessible / expired file IDs.
- Whether the returned content can contain raw image references and how those should be preserved downstream.
- Whether attachments remain available after handoff or resumed execution.

---

# 12. Cross-Tool File and Attachment Lifecycle

This is a major unresolved area because multiple tools use file IDs, blob IDs, paths and attachment objects.

Pending end-to-end verification:

```text
User upload
   ↓
Conversation Attachment / Extract Document to Markdown
   ↓
Analysis / transformation
   ↓
Export Excel / Word / PDF / PowerPoint / HTML
   ↓
system.files
   ↓
Export File / ExportBlob (where required)
   ↓
Send Email.attachments
   ↓
Final response attachments
```

Specific unresolved questions:

- Which file identifier is canonical across tools.
- Difference between conversation file IDs and blob file IDs.
- Difference between `filePath`, `Id`, and generated export IDs.
- When `system.files` is populated automatically.
- Whether `system.files` contains the raw export response or normalized file objects.
- Whether a file must pass through `ExportBlob` before final attachment or email use.
- Whether file objects survive handoff and resumed orchestration.
- File retention period.
- File deletion semantics.

---

# 13. Knowledge Source Inventory

The System screenshots establish that knowledge-related tools exist, but several source-specific tools have not yet been captured in detail in the current working evidence set.

Pending detailed evidence for:

- Get Reference File
- Get Table Schema
- Resolve Field Value
- Search Table Data
- Get Library Metadata runtime response
- Get Data Search Fields runtime response
- Search Knowledge runtime response beyond the documented contract

For each, capture:

1. exact input schema,
2. exact output schema,
3. configuration shown in Agent node,
4. state/output variable behaviour,
5. response filtering behaviour,
6. Store Tool Output behaviour,
7. Return Direct behaviour,
8. Human-in-the-Loop behaviour,
9. runtime error behaviour,
10. dependency ordering.

---

# 14. Shared / MCP / Quantum MCP Tool Tabs

The Agent-node screenshots visibly distinguish:

- System
- Shared
- MCP
- Quantum MCP

The System tab has a partially documented inventory. The remaining tabs require systematic capture.

Pending:

- Complete Shared inventory.
- Complete MCP inventory.
- Complete Quantum MCP inventory.
- Tool-by-tool schemas for relevant tools.
- Differences between tools with similar display names across tabs.
- Permission / availability differences between tabs.
- Which tab should be preferred when multiple equivalent tools exist.

---

# 15. Handoff and Agent-to-Agent Behaviour

Pending runtime verification:

- Whether a handoff transfers the complete conversation state.
- Whether it transfers tool outputs and flow variables.
- Whether memory survives handoff.
- Whether knowledge initialization must be repeated after handoff.
- Whether the originating agent resumes after the target completes.
- Exact response contract of the handoff operation.
- Failure behaviour when target node does not exist.
- Failure behaviour when target node errors.
- Whether a handoff can be chained repeatedly.
- Maximum handoff depth / cycle protection.

---

# 16. Overall Orchestration Runtime

Pending end-to-end validation of the architecture baseline:

- Dynamic delegation behaviour.
- Dependency ordering between specialist agents.
- Whether the Master can execute specialists in parallel where dependencies permit.
- Reconciliation behaviour after multiple specialist outputs.
- Human confirmation gate placement and resume behaviour.
- Frozen configuration semantics after confirmation.
- Whether run-time state can accidentally modify the confirmed evaluation configuration.
- Master challenge / QC re-analysis loop behaviour.
- Deterministic processing boundary enforcement.
- Scenario lineage and preservation of the original result.
- Failure recovery and partial-run resume semantics.
- Idempotency of re-running a failed stage.

---

# 17. Deterministic Processing Boundary

The architecture assumes semantic reasoning ends before deterministic processing begins.

Pending implementation verification:

- Exact structured score output contract from the Evaluation Specialist.
- Validation rules for score ranges.
- Validation rules for missing scores.
- Knockout execution contract.
- Weight calculation contract.
- Ranking tie-break rules.
- Handling of equal scores.
- Handling of incomplete supplier evidence.
- Handling of disqualified suppliers.
- Handling of explicit empty knockout configuration.
- Whether deterministic logic is implemented in an actual Rule / Compute / code-capable node rather than delegated to an LLM.

---

# 18. Reporting Contract

The current architecture states that the final workbook has exactly four primary tabs:

1. Executive Summary
2. Supplier Profiles
3. Q&A Scorecard
4. Score Legend

Pending verification:

- Exact workbook layout and cell-level contract.
- Exact formulas and whether calculations are formula-based or precomputed.
- Exact styling and conditional formatting rules.
- Supplier ordering rules.
- Ranking presentation rules.
- Knockout presentation rules.
- Legend values and colour semantics.
- Whether the reporting layer can accidentally modify business logic.
- Exact export tool configuration required to reproduce the approved workbook.

---

# 19. Evidence Hygiene and Documentation Governance

Pending verification / process decisions:

- Define a single canonical location for each confirmed platform fact.
- Define how conflicting screenshots are reconciled.
- Define evidence naming convention for screenshots and runtime traces.
- Define how tool-definition changes are versioned.
- Define whether each verified item requires a dated evidence reference.
- Define whether runtime test cases should be kept in a separate test register.
- Define the procedure for reverting a previously confirmed item if a later platform release invalidates it.

---

# 20. Immediate Next Verification Queue

Priority should be given to items that affect the architecture or can cause incorrect execution:

1. Cross-tool file / attachment lifecycle.
2. System tool discovery → schema → execution chain.
3. Knowledge workflow initialization enforcement.
4. Agent tool `State Update`, `Store Tool Output`, and `Return Direct` semantics.
5. Human Input and Approval resume / timeout semantics.
6. Handoff state-transfer semantics.
7. Export output object compatibility with `Send Email`.
8. Deterministic scoring / ranking boundary.
9. Shared / MCP / Quantum MCP inventories.
10. Remaining knowledge-tool contracts.

---

# 21. Promotion Rule

When an item is verified:

1. Capture the evidence.
2. Add the confirmed behaviour to the relevant evidence / specification document.
3. Record the evidence source or test reference.
4. Remove the item from this file.
5. Update the document version if the promoted fact changes an implementation contract.

This file should therefore **shrink as verification progresses**. It is not a permanent second specification.
