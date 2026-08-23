# 11. Runtime Variables, State & End-to-End Test Playbook

**Document Version:** 1.1  
**Status:** Current runtime-evidence baseline + test methodology  
**Updated:** 23 Aug 2026

## 1. Purpose

This document defines the current runtime model used while reverse-engineering QI Studio orchestration.

The key rule is:

> **Node execution success, node output, workflow state and final user-visible response are separate concerns.**

A downstream node must have an explicit data path to the value it needs, and the Output node must have an explicit response source.

For the authoritative current state and unresolved items, see [`13 - Current Understanding & Verification Ledger.md`](13%20-%20Current%20Understanding%20%26%20Verification%20Ledger.md).

---

# 2. Variable Areas

The Variables UI exposes five logical views:

1. Flow
2. System
3. Conversation History
4. Runtime
5. All

They should not be treated as interchangeable storage.

## 2.1 Flow Variables

Flow Variables are workflow-owned variables created through the separate Variables UI.

Observed configuration fields:

| Property | Observed |
|---|---|
| Variable Name | User-defined |
| Type | String, Number, Boolean, Array, Object |
| Scope | Flow or System |
| Description | Optional |
| Default Value | Optional |

Current rule:

> Use Flow scope for ordinary workflow state unless System-scope custom-variable behaviour is separately runtime-tested.

## 2.2 System Variables

Observed built-in runtime/context variables include:

- `system.userQuery`
- `system.sessionId`
- `system.dateTime.utcNow`
- `system.attachments`
- `system.files`
- `system.humanInput`
- `system.uiAction`

The UI evidence also showed `system.timestamp`; its exact runtime representation should be treated as environment-specific until separately tested.

## 2.3 Conversation History

`conversationHistory` represents conversational context. It is not a replacement for explicit workflow state.

The tested execution retained the original START message (`hello`) in conversation history while the Human Input response was represented through HITL/runtime state.

## 2.4 Runtime Variables

Runtime values describe execution/environment context such as workflow metadata, environment and headers. They should not be treated as business state.

---

# 3. Human Input Runtime Contract

Confirmed UI and runtime behaviour:

1. Node pauses orchestration.
2. A human is prompted.
3. The response is captured.
4. The configured target is recorded in the HITL checkpoint.
5. Execution resumes.
6. The response is available through the configured state surface.

Observed runtime structure:

```text
resumeUserInput.hitlType = human_input
resumeUserInput.message = ...
resumeUserInput.variableTarget = ...
nodes.human_input_0.input = ...
nodes.human_input_0.variableTarget = ...
nodes.human_input_0.success = true
```

### Runtime-confirmed example

```text
Human Input → system.humanInput
```

After the user entered `START_TEST`:

```text
system.humanInput = START_TEST
```

---

# 4. Output Runtime Contract

The Output node does not appear to infer a response from arbitrary execution state.

### Confirmed path

```text
Human Input
    ↓
system.humanInput = START_TEST
    ↓
Output configured as {{system.humanInput}}
    ↓
output.messages = START_TEST
    ↓
User Window = START_TEST
```

### Regression case

A previous execution completed successfully but the user-facing layer returned:

```text
No response content found in the execution result. Please try again.
```

At that time, the intended response value had not been explicitly configured as the Output response source.

This supports the current rule:

> **Output requires an explicit response source.**

The complete set of expressions accepted by Output is still being mapped.

---

# 5. Flow Variable Runtime Contract

A Human Input execution was also observed with:

```text
variableTarget = flow.startTestResponse
```

and an execution state containing:

```text
flow.startTestResponse = START_TEST
```

The current repository deliberately does **not** treat this alone as a fully isolated Flow Variable → Output proof because the later controlled System-variable test produced the same final response through an explicitly referenced System variable.

Therefore the Flow path is currently:

**Runtime Partial / controlled rerun required**

The isolated test should be:

```text
START
  ↓
Human Input → flow.testResponse
  ↓
Output → {{flow.testResponse}}
```

with no dependency on `system.humanInput`.

---

# 6. Flow Variable Design Rules

These are architecture rules for the solution, not claims that every UI state mutation has already been runtime-tested.

1. Create workflow-owned variables in the Variables UI.
2. Give each important Flow Variable one intentional producer.
3. Treat shared state as explicit data contracts between nodes.
4. Prefer Flow scope for workflow business state.
5. Keep deterministic result variables distinct from semantic recommendations.
6. Preserve provenance/confidence where inference is involved.
7. Do not use Conversation History as a hidden state store.

---

# 7. Knowledge Tool Initialization Rule

A directly supplied tool definition adds an important mandatory sequence that supersedes older wording:

```text
Knowledge-related agent invocation
        ↓
get-knowledge-workflow-instructions
        ↓
get_library_metadata
        ↓
source-specific knowledge tools
```

For data-search knowledge, the known sequence continues:

```text
get_library_metadata
        ↓
get_data_search_fields
        ↓
execute_search_query
```

If `default_filters` are supplied by the data-search schema, they are described as mandatory access-control constraints and must be preserved in later search execution.

---

# 8. General End-to-End Test Pattern

Each node should be understood at four levels.

## Level 1 - UI Contract

Capture:

- visible fields;
- input types;
- required/optional flags;
- output variables;
- state-update controls;
- routing handles;
- advanced options.

## Level 2 - Runtime Contract

Capture:

- node input;
- node output;
- success/failure;
- state changes;
- pause/resume metadata.

## Level 3 - Downstream Contract

Verify that another node can actually consume the produced value.

A value merely appearing in raw execution JSON is not enough.

## Level 4 - User-visible Contract

Verify the actual final behaviour shown to the user.

Only after Level 4 should a path be marked Runtime Confirmed.

---

# 9. High-Information Test Plan

Use eight tests rather than many repetitive tests:

### T1 - START → Output

Baseline START input, `system.userQuery`, and explicit Output mapping.

### T2 - START → Human Input(system) → Output

Runtime-confirmed HITL path using `system.humanInput`.

### T3 - START → Human Input(flow) → Output

Isolated Flow Variable path. This is the immediate next test.

### T4 - Variable lifecycle

Create → write → read → transform → output without relying on Human Input as the only writer.

### T5 - Condition / branching

Execute the same condition flow once with a true value and once with a false value.

### T6 - Approval

Approve and reject the same flow to establish decision value, routing and resume behaviour.

### T7 - Agent → Tool → State → Output

Use one deterministic tool and compare Store Tool Output vs Return Direct while observing Variable Path and state.

### T8 - Composite workflow

Knowledge initialization → retrieval → tool → HITL → export/file handling → Output.

This sequence is intentionally optimized for maximum understanding per test.

---

# 10. Evidence Recording Template

For every test record:

```text
Test ID
Objective
Graph topology
Configuration
Input
Checkpoint/HITL data
Flow state
System state
Node outputs
Downstream consumption
Final user-visible result
Status
Interpretation
Open questions
```

---

# 11. Status Rules

| Status | Meaning |
|---|---|
| PASS - Runtime Confirmed | Full intended path demonstrated end-to-end. |
| PASS - Runtime Partial | Important part works but a material linkage/edge case remains untested. |
| FAIL | Expected behaviour not achieved. |
| BLOCKED | Another required component/configuration was missing. |
| PENDING | Test designed but not executed. |
| SUPERSEDED | Earlier interpretation replaced by stronger evidence. |
| CONTRADICTED | Earlier assumption directly disproven. |

Never upgrade a UI-only observation to Runtime Confirmed.
