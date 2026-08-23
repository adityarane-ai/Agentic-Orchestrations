# Agentic-Orchestrations

Agentic orchestration resource hub.

## Bid Analysis Agent

The repository contains the **RFP Qualitative Bid Analysis Agent — V1.3** architecture for GEP Quantum Intelligence Studio.

The frozen design uses:

- One Master Deep Agent
- Three direct specialist sub-agents maximum
- Dynamic planning, delegation and targeted re-analysis
- Human-in-the-loop Bid Understanding / Evaluation Configuration gate
- Human-confirmed knockout requirements and acceptance conditions
- GEP internal category toolkits and knowledge accessed through Knowledge Library tools
- Explicit knowledge governance: GEP knowledge informs context/benchmarking but cannot silently override the RFP or human-confirmed configuration
- Deterministic validation, knockout execution, scoring arithmetic, weighting and ranking
- Four-tab Excel output: Executive Summary, Supplier Profiles, Q&A Scorecard, Score Legend
- Post-evaluation scenario lineage for approved rule/weight changes

### Core principle

> **AI interprets supplier evidence using the confirmed framework and relevant GEP knowledge. Human confirms material evaluation rules. Deterministic logic executes the decision. AI explains the outcome.**

See [`Bid Analysis Agent/README.md`](Bid%20Analysis%20Agent/README.md) for the implementation baseline.

## Current platform reverse-engineering documentation

The platform investigation is maintained separately from the business architecture so that platform facts, runtime observations, test evidence and open questions are not mixed with the frozen solution design.

Key documents:

- [`08 – Flow Variables.md`](Bid%20Analysis%20Agent/08%20%E2%80%93%20Flow%20Variables.md) — workflow-state inventory and business data contracts.
- [`10 - QI Studio Nodes, Agent Tools & Verification Register.md`](Bid%20Analysis%20Agent/10%20-%20QI%20Studio%20Nodes%2C%20Agent%20Tools%20%26%20Verification%20Register.md) — node/tool UI evidence and supplied tool contracts.
- [`11 - Runtime Variables, State & End-to-End Test Playbook.md`](Bid%20Analysis%20Agent/11%20-%20Runtime%20Variables%2C%20State%20%26%20End-to-End%20Test%20Playbook.md) — current state model and reusable test methodology.
- [`12 - End-to-End Node Test Log.md`](Bid%20Analysis%20Agent/12%20-%20End-to-End%20Node%20Test%20Log.md) — actual execution evidence and regression tests.
- [`13 - Current Understanding & Verification Ledger.md`](Bid%20Analysis%20Agent/13%20-%20Current%20Understanding%20%26%20Verification%20Ledger.md) — **canonical current understanding, conflicts, and pending verification queue**.

## Current runtime findings

The strongest runtime evidence currently confirms this explicit response path:

```text
START
  ↓
HUMAN INPUT → system.humanInput
  ↓
OUTPUT → {{system.humanInput}}
  ↓
User Window
```

The tested response `START_TEST` survived the HITL pause/resume, was present in `system.humanInput`, was returned as `output.messages`, and was displayed to the user.

A separate earlier execution showed a Flow Variable target containing `START_TEST`, but the Flow Variable → Output linkage remains under controlled verification rather than being treated as conclusively isolated. Do not infer that path from the System-variable test.

An earlier run also established the regression case where the workflow completed but the Output layer returned `No response content found in the execution result. Please try again.` because no explicit response source had been configured.

## Knowledge workflow correction

A later supplied platform tool definition explicitly requires:

```text
get-knowledge-workflow-instructions
        ↓
get_library_metadata
        ↓
source-specific knowledge tools
```

Therefore any older wording that describes `get_library_metadata` as the first knowledge call is superseded.

## Evidence discipline

The repository distinguishes between:

1. **Confirmed** — explicitly shown in supplied QI Studio UI or platform documentation.
2. **Runtime Confirmed** — demonstrated in an actual end-to-end execution.
3. **Working Understanding** — strong interpretation that still needs validation.
4. **Pending Verification** — capability or question without sufficient evidence.
5. **Contradicted** — a previous assumption disproven by later evidence.
6. **Superseded** — an earlier statement retained for history but replaced by stronger evidence.

Never upgrade a capability to Runtime Confirmed solely because a UI option exists, a node returns `success: true`, or a value appears somewhere in execution JSON.

## Current state-model principle

> **System and Runtime provide engine context. Flow Variables carry workflow-owned state. Conversation History carries conversational context. Nodes can expose runtime outputs, but downstream data paths should be explicit. Output requires an explicit response source.**

For the current consolidated model and pending work, use the [Current Understanding & Verification Ledger](Bid%20Analysis%20Agent/13%20-%20Current%20Understanding%20%26%20Verification%20Ledger.md).