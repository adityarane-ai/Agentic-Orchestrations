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
