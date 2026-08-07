# 03. Solution Overview

**Document Version:** 0.1

**Status:** Draft

**Parent Document:** Software Design Specification (SDS)

---

# Purpose

This document provides a high-level architectural overview of the RFP Qualitative Evaluation Agent.

It explains how the overall solution is structured, how individual modules interact, how information flows through the system, and why specific architectural decisions were made.

This chapter intentionally avoids implementation details. Those are covered in later chapters.

---

# Solution Summary

The RFP Qualitative Evaluation Agent is an enterprise-grade conversational AI application designed to automate qualitative supplier evaluation while preserving consultant-level analytical quality.

The solution is composed of several independent modules coordinated by a central **Supervisor Agent**.

Unlike traditional workflow automation systems, the Supervisor does not perform evaluation itself.

Instead, it manages:

- conversation
- workflow state
- user guidance
- orchestration
- handoffs
- session lifecycle

The actual evaluation work is delegated to specialized processing modules.

---

# Architectural Philosophy

The architecture follows six fundamental principles.

## Principle 1 — Separation of Concerns

Conversation management is intentionally separated from procurement evaluation.

The Supervisor is responsible only for:

- interacting with the user
- collecting inputs
- determining workflow state
- routing work

The Evaluation Engine is responsible only for supplier evaluation.

This separation minimizes coupling between business logic and conversational behaviour.

---

## Principle 2 — Modular Design

Each module performs exactly one business responsibility.

Modules communicate only through well-defined data contracts.

No module should directly manipulate another module's internal logic.

---

## Principle 3 — Canonical Data

Every downstream component consumes the same canonical representation of supplier evaluation data.

Raw supplier responses are never evaluated directly.

Instead:

Raw Supplier Data

↓

Validation

↓

Canonical Mapping

↓

Evaluation

↓

Reporting

This guarantees consistency throughout the workflow.

---

## Principle 4 — Deterministic First

Where deterministic algorithms exist, deterministic algorithms shall always be preferred over LLM reasoning.

Examples include:

- validation
- ranking
- weighting
- report generation
- sorting
- aggregation

LLMs are reserved for semantic interpretation only.

---

## Principle 5 — Explainable AI

Every recommendation produced by the system must be traceable.

Users must always be able to answer:

- Why did Supplier A score higher?
- Why was Supplier B eliminated?
- Which response contributed to the final score?
- Which evidence was considered?

The system should never behave as an opaque decision engine.

---

## Principle 6 — Production-Oriented Engineering

The architecture is designed for production deployment.

Priority is given to:

- maintainability
- scalability
- observability
- fault isolation
- deterministic behaviour

rather than minimizing node count.

---

# High-Level Architecture

The solution consists of six primary modules.

```

                ┌────────────────────────────┐
                │       Supervisor Agent      │
                └─────────────┬──────────────┘
                              │
         ┌────────────────────┼────────────────────┐
         │                    │                    │
         ▼                    ▼                    ▼
 Criteria Processing   Supplier Processing   Conversation Management
         │                    │
         └──────────────┬─────┘
                        ▼
              Evaluation Engine
                        │
                        ▼
               Report Generator
                        │
                        ▼
             Post Evaluation Q&A

```

---

# Module Responsibilities

## 1. Supervisor

The Supervisor Agent is the central orchestration layer.

Responsibilities include:

- greeting users
- collecting required files
- maintaining workflow state
- invoking downstream modules
- handling follow-up requests
- managing conversation continuity

The Supervisor never performs supplier evaluation itself.

---

## 2. Criteria Processing Module

Responsible for:

- reading Evaluation Criteria workbooks
- extracting scoring guidance
- extracting weights
- extracting knockout rules
- storing structured criteria

Outputs:

```

criteria

```

---
## 2.a. Evaluation Configuration becomes a first-class module.

Responsibilities

- Review extracted criteria
- Configure knockout rules
- Configure expected answers
- Configure weights
- Exclude questions
- Finalize evaluation settings

Outputs:

```

evaluationConfiguration

```

## 3. Supplier Processing Module

Responsible for:

- reading supplier workbooks
- extracting supplier responses
- preserving workbook structure
- storing supplier objects

Outputs:

```

suppliers[]

```

One object is produced per supplier workbook.

---

## 4. Evaluation Engine

The Evaluation Engine performs all procurement analysis.

Internal stages include:

1. Structure Validation
2. Canonical Mapping
3. Knockout Evaluation
4. Qualitative Scoring
5. Weighted Score Calculation
6. Supplier Ranking
7. Recommendation Generation

The Evaluation Engine produces a complete evaluation result.

---

## 5. Report Generator

Consumes:

```

evaluationResult

```

Produces:

- Excel workbook
- Executive Summary
- Detailed Scorecards
- Comparative Analysis
- Procurement Recommendations

---

## 6. Post-Evaluation Q&A

Allows consultants to continue interacting with completed evaluation results.

Example questions:

- Compare Supplier A and Supplier B.
- Explain Question 4 scoring.
- Show knockout failures.
- Regenerate rankings.
- Change evaluation weights.

This module never repeats extraction.

It operates exclusively on stored evaluation data.

---

# Overall Workflow

The complete business workflow is illustrated below.

```

User

↓

Supervisor

↓

Upload Evaluation Criteria

↓

Criteria Processing

↓

Supervisor

↓

Upload Supplier Responses

↓

Supplier Processing

↓

Evaluation Engine

↓

Report Generation

↓

Results Delivered

↓

Post-Evaluation Q&A

```

---

# Internal Evaluation Workflow

Once evaluation begins, the Evaluation Engine executes the following pipeline.

```

Validate Questionnaire Structure

↓

Build Canonical Question Map

↓

Knockout Evaluation

↓

Qualitative Scoring

↓

Weighted Score Calculation

↓

Supplier Ranking

↓

Recommendation Generation

```

This pipeline is deterministic except for qualitative scoring.

---

# Module Communication

Modules communicate exclusively through Flow Variables.

Modules never access another module's internal implementation.

Example:

```

Criteria Processing

↓

flow.criteria

↓

Evaluation Engine

```

not

```

Evaluation Engine

↓

Read internal Criteria node

```

This design reduces coupling and improves maintainability.

---

# Design Decisions

## Why use a Supervisor?

The evaluation process is conversational.

Users upload files incrementally.

The system must remember previous actions and determine what should happen next.

A Supervisor Agent simplifies this orchestration.

---

## Why separate Criteria and Supplier processing?

These workbooks have different structures and responsibilities.

Independent processing allows each extractor to be optimized separately.

---

## Why build a Canonical Question Map?

Without canonical mapping, every downstream module would repeatedly search two independent datasets.

Canonical mapping produces one authoritative representation of every evaluation question.

This significantly simplifies later stages.

---

## Why deterministic validation?

Validation should never depend on AI interpretation.

Structural mismatches must always produce identical outcomes.

---

## Why isolate scoring?

Scoring is the only stage requiring semantic interpretation.

By isolating it, the architecture minimizes probabilistic behaviour while maximizing explainability.

---

# Architectural Constraints

The architecture assumes:

- standardized Excel workbooks
- one Evaluation Criteria workbook per sourcing event
- multiple supplier workbooks
- one canonical evaluation model
- deterministic downstream processing
- conversational interaction through the Supervisor

---

# Architecture Summary

The solution is intentionally designed as a modular conversational system rather than a monolithic workflow.

The Supervisor orchestrates the conversation.

Specialized modules perform independent responsibilities.

The Evaluation Engine provides deterministic, explainable procurement analysis.

The resulting architecture is scalable, maintainable, production-ready and suitable for enterprise deployment.
