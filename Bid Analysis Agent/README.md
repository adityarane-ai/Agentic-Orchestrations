# RFP Qualitative Evaluation Agent

## Software Design Specification (SDS)

**Project Codename:** Project Athena

**Version:** 1.1

**Status:** Architecture Baseline Updated

**Platform:** GEP Quantum Intelligence Studio (QI Studio)

---

## Purpose

This repository contains the design, architecture, implementation specifications, prompts, scripts, workflows and technical documentation for the **RFP Qualitative Evaluation Agent**.

The system automates qualitative supplier evaluation during strategic sourcing and RFP events while preserving transparency, explainability, determinism and consultant-level decision quality.

The repository is the architectural source of truth for the implementation.

---

## Version 1.1 Product Principle

The agent is designed for deployment to procurement users on the floor.

### User expectation

The user should be able to:

> **Upload the Excel files they already have and let the agent figure out how to process them.**

Users should not be required to:

- create a prescribed workbook
- rename files
- use prescribed sheet names
- use prescribed column names
- manually classify files
- reformat data that the system can reasonably interpret

This does **not** mean the internal architecture becomes unstructured. The system introduces a File Intake and Discovery layer that translates flexible inputs into strict canonical contracts.

---

## Core Architecture

```text
                         USER
                           ↓
                      SUPERVISOR
                           ↓
                FILE INTAKE & DISCOVERY
                           ↓
                INPUT COMPLETENESS CHECK
                           ↓
             ┌─────────────┴─────────────┐
             ↓                           ↓
      CRITERIA PROCESSING        SUPPLIER PROCESSING
             ↓                           ↓
      EVALUATION CONFIGURATION           ↓
             └─────────────┬─────────────┘
                           ↓
                   EVALUATION ENGINE
                           ↓
        VALIDATE → MAP → KNOCKOUT → SCORE
                           ↓
             WEIGHT → RANK → RECOMMEND
                           ↓
                    REPORT GENERATOR
                           ↓
                    POST-EVALUATION Q&A
```

---

## Design Principles

1. **Low-friction input** — users provide available files rather than system templates.
2. **File intelligence** — the system discovers file and sheet roles automatically where possible.
3. **Progressive disclosure** — ask only for information that cannot be reliably inferred.
4. **Single responsibility** — every module has one business responsibility.
5. **Deterministic first** — scripts handle validation, arithmetic, weighting and ranking.
6. **LLM for semantics** — LLMs interpret business meaning but do not own deterministic calculations.
7. **Canonical data** — downstream evaluation operates on normalized contracts.
8. **Explainability** — decisions retain source evidence and reasoning.
9. **Immutable source data** — extracted source information is preserved.
10. **Single producer** — every Flow Variable has one owner.

---

## Major Modules

| Module | Responsibility |
|---|---|
| Supervisor | Conversation and workflow state |
| File Intake & Discovery | Understand uploaded files and sheets |
| Criteria Processing | Normalize evaluation criteria |
| Evaluation Configuration | Configure approved evaluation rules |
| Supplier Processing | Normalize supplier responses |
| Evaluation Engine | Validate, map, evaluate, calculate and rank |
| Report Generator | Generate consultant-ready outputs |
| Post-Evaluation Q&A | Analyze completed evaluations |

---

## Evaluation Pipeline

```text
Raw Excel Files
↓
File Discovery
↓
Normalized Criteria + Suppliers
↓
Validation
↓
Canonical Question Map
↓
Knockout Evaluation
↓
Qualitative Scoring
↓
Weighted Score Calculation
↓
Supplier Ranking
↓
Recommendation
↓
Report
```

---

## Repository Structure

Key specification documents:

```text
01-Executive-Summary.md
02-Business Requirements.md
03-Solution-Overview.md
04-System-Architecture.md
05-Conversation-State-Machine.md
05A-Data-Flow-Architecture.md
06-Overall-Orchestration.md
07-QI Studio Orchestration Blueprint.md
08 – Flow Variables.md
09-JSON Schemas.md
```

The Version 1.1 documents collectively define the new floor-user experience and its QI Studio implementation contract.

---

## Source of Truth Rule

Implementation in QI Studio shall follow the latest architecture and contract documents in this repository.

If QI Studio limitations require a deviation, the deviation should be documented and tested rather than silently introduced into the workflow.

---

## Scope

Version 1.1 supports reasonably structured Excel inputs, including variations in:

- filenames
- sheet names
- column names
- workbook organization
- supplier-file grouping

The system does not claim unrestricted understanding of arbitrary unstructured documents.

---

## Intended Outcome

The final experience should feel simple to the floor user:

```text
Upload files
    ↓
Agent understands them
    ↓
Agent asks only necessary questions
    ↓
Evaluation runs
    ↓
Results + report
```

The complexity remains inside the architecture rather than being transferred to the user.
