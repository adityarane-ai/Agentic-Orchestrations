# 12. End-to-End Node Test Log

**Document Version:** 1.3  
**Status:** Active runtime evidence record  
**Started:** 23 Aug 2026  
**Updated:** 23 Aug 2026

This file contains actual runtime executions. It is the historical evidence record. Current platform interpretation belongs in [`13 - Current Understanding & Verification Ledger.md`](13%20-%20Current%20Understanding%20%26%20Verification%20Ledger.md).

---

# 1. Test Status Model

| Status | Meaning |
|---|---|
| PASS - Runtime Confirmed | Intended data path demonstrated through downstream/user-visible behaviour. |
| PASS - Runtime Partial | Core execution worked but a material linkage or edge condition remains unproven. |
| FAIL | Expected behaviour not achieved. |
| BLOCKED | Test could not be completed because a dependency/configuration was missing. |
| PENDING | Test designed but not yet executed. |
| SUPERSEDED | Older interpretation replaced by stronger evidence. |
| CONTRADICTED | Older assumption disproven by later evidence. |

---

# 2. TEST-START-001 - START → Human Input(flow) → Output(flow)

## Objective

Test Human Input targeting a user-created Flow Variable and then use that Flow Variable as the Output response source.

## Configuration

START:

```text
message = hello
```

Human Input:

```text
Question = Please enter "START_TEST"
Save Response As = flow.startTestResponse
```

Variables UI:

```text
Name = startTestResponse
Type = String
Scope = Flow
```

Output:

```text
{{flow.startTestResponse}}
```

## Runtime evidence

The execution completed successfully.

Observed state included:

```text
system.userQuery = hello
flow.startTestResponse = START_TEST
nodes.human_input_0.input = START_TEST
nodes.human_input_0.variableTarget = flow.startTestResponse
nodes.human_input_0.success = true
```

The execution also demonstrated that the Human Input response was written to the configured Flow Variable.

### Status

**PASS - Runtime Partial**

### Why not Runtime Confirmed?

A later controlled run changed only the Human Input target and Output expression to use the built-in System variable `system.humanInput`. That run also produced `START_TEST` successfully while `flow.startTestResponse` remained empty.

Therefore the current evidence does not isolate the Flow Variable → Output linkage strongly enough to make this test the sole authoritative proof of that linkage.

### Required controlled rerun

```text
Human Input → flow.testResponse
Output → {{flow.testResponse}}
```

Do not reference `system.humanInput` anywhere in the test.

---

# 3. TEST-START-002 - START → Human Input(system) → Output(system)

## Objective

Verify whether the built-in System variable `system.humanInput` can be explicitly consumed by Output after a HITL pause/resume.

## Controlled change

Everything was kept identical to TEST-START-001 except:

Human Input:

```text
Save Response As = system.humanInput
```

Output:

```text
{{system.humanInput}}
```

## Runtime evidence

Execution time:

```text
2026-08-23 14:17:54 UTC
```

Input:

```text
hello
```

Human response:

```text
START_TEST
```

Key runtime state:

```text
flow.startTestResponse = ""
system.humanInput = "START_TEST"
```

HITL checkpoint:

```text
hitlType = human_input
message = Please enter "START_TEST"
variableTarget = system.humanInput
```

Node state:

```text
nodes.start.success = true
nodes.human_input_0.input = START_TEST
nodes.human_input_0.variableTarget = system.humanInput
nodes.human_input_0.success = true
nodes.output_0.success = true
nodes.output_0.output.messages = START_TEST
status = completed
```

User Window:

```text
START_TEST
```

### Status

**PASS - Runtime Confirmed**

## What this proves

1. Human Input can write to built-in `system.humanInput`.
2. The value survives HITL resume.
3. Output can explicitly consume `{{system.humanInput}}`.
4. Output produces `output.messages = START_TEST`.
5. The User Window displays `START_TEST`.

## What this does not prove

It does not prove that Output automatically searches System variables, nor that user-created System-scope variables behave exactly like built-in System variables.

---

# 4. TEST-OUTPUT-REG-001 - Missing Explicit Response Source

## Objective

Retain the earlier failure as a regression test showing why Output configuration matters.

## Observed earlier behaviour

The workflow had:

```text
START success = true
Human Input success = true
system.humanInput = START_TEST
status = completed
```

but the user-facing layer returned:

```text
No response content found in the execution result. Please try again.
```

### Interpretation

The intended response existed in execution state, but the Output node had not been given an explicit response source.

### Status

**SUPERSEDED as a design assumption; retained as regression evidence.**

The current runtime rule is not that Output always fails in this configuration under every possible implementation. The evidence supports the narrower statement that **in this observed run, the absence of an explicit response source produced the fallback error despite successful upstream execution**.

---

# 5. TEST-START-003 - Isolated Flow Variable → Output

**Status:** PENDING

## Objective

Prove the Flow Variable → Output contract without relying on the built-in System Human Input variable.

## Graph

```text
START
  ↓
Human Input
  ↓
Output
```

## Configuration

Human Input:

```text
Save Response As = flow.testResponse
```

Output:

```text
{{flow.testResponse}}
```

Important:

- do not configure `system.humanInput` as the response source;
- do not add a second state copy;
- use a fresh Flow Variable name;
- capture the full runtime envelope after resume.

## Pass criteria

```text
flow.testResponse = START_TEST
nodes.output_0.output.messages = START_TEST
User Window = START_TEST
```

If this succeeds, upgrade the Flow Variable → Output linkage to Runtime Confirmed.

---

# 6. TEST-FLOW-004 - Variable Lifecycle

**Status:** PENDING

## Objective

Understand Flow Variable create/write/read/transform semantics independently of Human Input.

## Graph

```text
START
  ↓
State Update / Compute
  ↓
State Update / Compute
  ↓
Output
```

Create in Variables UI:

```text
flow.testNumber = 10
flow.testMessage = TEST
```

Then transform:

```text
flow.testNumber = flow.testNumber + 5
```

Output:

```text
{{flow.testMessage}} | {{flow.testNumber}}
```

Expected:

```text
TEST | 15
```

Questions answered:

- write semantics;
- read semantics;
- transform semantics;
- persistence across nodes;
- missing/unset variable behaviour.

---

# 7. TEST-CONDITION-005 - Condition / Branching

**Status:** PENDING

## Graph

```text
START
  ↓
Human Input → flow.choice
  ↓
Condition
  ├── YES → Output A
  └── NO  → Output B
```

Run the same workflow twice:

1. input `YES`;
2. input `NO`.

Capture:

- condition expression;
- selected route;
- skipped route;
- state available on each path;
- output.

---

# 8. TEST-APPROVAL-006 - Approval

**Status:** PENDING

## Graph

```text
START
  ↓
APPROVAL
  ├── Approved → Output A
  └── Rejected → Output B
```

Capture:

- approval checkpoint metadata;
- decision value;
- resume state;
- route selected;
- downstream output.

Run once with Approve and once with Reject.

---

# 9. TEST-AGENT-007 - Agent → Tool → State → Output

**Status:** PENDING

Use one deterministic system tool with an obvious result.

## Test objective

Understand the Agent tool-result contract and compare:

- Variable Path;
- Store Tool Output;
- Return Direct;
- State Update;
- Response Filtering.

Do not test many tools at once. Use one tool and change only one control between runs.

Capture:

```text
nodes.agent
nodes.<agent>...toolResults
flow state
system.files where applicable
final agent response
output.messages
```

---

# 10. TEST-COMPOSITE-008 - Composite Orchestration

**Status:** PENDING

Use one realistic workflow only after the primitive contracts above are known.

Recommended shape:

```text
START
  ↓
Agent
  ↓
Knowledge initialization
  ↓
Knowledge retrieval
  ↓
Tool
  ↓
Human approval/input
  ↓
Export
  ↓
Output / file handling
```

Questions:

- does state survive tool calls?
- does state survive HITL resume?
- do exported files appear in `system.files`?
- can a downstream tool consume an exported file?
- does Output return file content/result correctly?

---

# 11. Current Runtime Architecture

The current evidence supports this more precise model:

```text
START
  ↓
System / Runtime context
  ↓
Node execution
  ↓
Addressable state
  ├── Flow Variables
  ├── System variables
  └── other node/runtime surfaces
  ↓
Explicit downstream consumption
  ↓
Output with explicit response source
  ↓
User Window
```

Conversation History is parallel context rather than a substitute for workflow state.

---

# 12. Test Discipline

For each test:

1. keep the graph as small as possible;
2. change one meaningful variable at a time;
3. record the complete runtime envelope;
4. distinguish node success from final output success;
5. use isolated variable names for controlled comparisons;
6. mark results as Confirmed / Partial / Pending rather than filling gaps with inference;
7. update the Current Understanding & Verification Ledger after material findings.
