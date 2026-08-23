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

### Platform evidence discipline

The repository distinguishes between:

1. **Confirmed** — explicitly shown in QI Studio UI or supplied platform documentation.
2. **Working understanding** — strong interpretation that still needs validation.
3. **Runtime confirmed** — demonstrated through an actual execution test.
4. **Pending evidence** — capability identified but not yet sufficiently evidenced.
5. **Contradicted** — a previous assumption disproven by later evidence.

### Current state-model principle

> **System and Runtime provide engine context. Flow Variables carry workflow state. Conversation History carries conversational context. Output requires an explicit response source.**
