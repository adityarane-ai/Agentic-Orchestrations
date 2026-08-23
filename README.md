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

The implementation investigation is maintained separately from the business architecture so that platform facts, runtime observations and open questions are not mixed with the frozen solution design.

Key documents:

- [`08 – Flow Variables.md`](Bid%20Analysis%20Agent/08%20%E2%80%93%20Flow%20Variables.md) — workflow state inventory, ownership rules and data contracts.
- [`10 - QI Studio Nodes, Agent Tools & Verification Register.md`](Bid%20Analysis%20Agent/10%20-%20QI%20Studio%20Nodes%2C%20Agent%20Tools%20%26%20Verification%20Register.md) — node/tool evidence register and verification status.
- [`11 - Runtime Variables, State & End-to-End Test Playbook.md`](Bid%20Analysis%20Agent/11%20-%20Runtime%20Variables%2C%20State%20%26%20End-to-End%20Test%20Playbook.md) — current variable-scope model, START/Human Input findings, output contract lessons and repeatable runtime testing method.
- [`12 - End-to-End Node Test Log.md`](Bid%20Analysis%20Agent/12%20-%20End-to-End%20Node%20Test%20Log.md) — actual runtime executions, pass/fail evidence, regression cases and the sequence of upcoming node tests.

### Latest runtime finding

The first completed end-to-end test confirms this data path:

```text
START
  ↓
HUMAN INPUT
  ↓
Flow Variable
  ↓
OUTPUT
```

Specifically, the Human Input response `START_TEST` was written to `flow.startTestResponse`, the workflow completed successfully, and the Output node returned `START_TEST` when explicitly configured to consume that Flow Variable.

An earlier run also established the failure mode where the workflow completes but the Output node returns `No response content found in the execution result. Please try again.` because no explicit response source was configured.

### Platform evidence discipline

The repository distinguishes between:

1. **Confirmed** — explicitly shown in QI Studio UI or supplied platform documentation.
2. **Working understanding** — strong interpretation that still needs validation.
3. **Runtime confirmed** — demonstrated through an actual execution test.
4. **Pending evidence** — capability identified but not yet sufficiently evidenced.
5. **Contradicted** — a previous assumption disproven by later evidence.
6. **Superseded** — an earlier test/design remains historically useful but has been replaced by stronger evidence.

### Current state-model principle

> **System and Runtime provide engine context. Flow Variables carry workflow state. Conversation History carries conversational context. Node execution, node output and final response are separate concerns. Output requires an explicit response source.**
