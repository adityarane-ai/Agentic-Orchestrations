# 03. Solution Overview

**Document Version:** 0.2

**Status:** Updated for Floor-User UX

**Parent Document:** Software Design Specification (SDS)

---

# Purpose

This chapter provides the high-level architectural overview of the RFP Qualitative Evaluation Agent, including the new **File Intake and Discovery** capability required for deployment to users on the procurement floor.

The user experience is intentionally schema-agnostic. Users may upload the Excel files available to them without knowing the agent's internal workbook templates, filenames, sheet names or column conventions.

The architecture remains internally schema-driven, canonical and deterministic wherever possible.

---

# Solution Summary

The RFP Qualitative Evaluation Agent is an enterprise-grade conversational AI application designed to automate qualitative supplier evaluation while preserving consultant-level analytical quality.

The solution is composed of independent modules coordinated by a central **Supervisor Agent**.

A new **File Intake and Discovery** capability sits between the conversational layer and business processing modules. It interprets uploaded workbooks and identifies their likely business roles before downstream processing begins.

The Supervisor manages:

- conversation
- workflow state
- user guidance
- file collection
- routing
- handoffs
- session lifecycle

The File Intake and Discovery layer manages:

- workbook discovery
- file classification
- sheet classification
- supplier identification
- evaluation-framework identification
- structural discovery
- confidence assessment
- ambiguity detection

The actual procurement evaluation remains delegated to specialized processing and evaluation modules.

---

# Architectural Philosophy

## Principle 1 — Separation of Concerns

Conversation management, file interpretation and procurement evaluation remain separate responsibilities.

The Supervisor manages the user journey.

File Intake and Discovery determines what the uploaded information represents.

The Evaluation Engine evaluates normalized procurement data.

---

## Principle 2 — Zero-Template User Experience

Users should provide data rather than understand the system's internal data model.

The system shall not require users to:

- rename files
- use prescribed sheet names
- use prescribed column names
- create a specific workbook template
- split information into specific files when it can be interpreted without doing so

The internal canonical model remains strict even though the external input experience is flexible.

---

## Principle 3 — Progressive Disclosure

The system should infer information whenever it can do so reliably.

The user should only be asked for clarification when ambiguity is material to the evaluation outcome.

---

## Principle 4 — Deterministic First

Where deterministic algorithms exist, deterministic algorithms shall be preferred.

LLMs are used for semantic interpretation such as identifying business meaning from varied workbook structures.

Scripts remain authoritative for validation, calculations, ranking and other deterministic business rules.

---

## Principle 5 — Canonical Data

Every downstream evaluation component consumes the same normalized canonical representation.

```text
Raw Files
   ↓
File Discovery
   ↓
Semantic Extraction
   ↓
Normalized Objects
   ↓
Validation
   ↓
Canonical Mapping
   ↓
Evaluation
   ↓
Reporting
```

---

## Principle 6 — Explainability

Every material procurement decision must remain traceable to source evidence and evaluation rules.

File interpretation itself must also retain provenance and confidence so that downstream decisions do not depend on invisible assumptions.

---

## Principle 7 — Production-Oriented Engineering

Priority is given to:

- reliability
- maintainability
- scalability
- observability
- fault isolation
- testability
- auditability

The architecture is not optimized merely for minimum node count.

---

# High-Level Architecture

```text
                         ┌──────────────────────────┐
                         │      Supervisor Agent     │
                         │ Conversation + State     │
                         └────────────┬─────────────┘
                                      │
                                      ▼
                         ┌──────────────────────────┐
                         │ File Intake & Discovery  │
                         │                          │
                         │ • classify files         │
                         │ • inspect sheets         │
                         │ • identify suppliers     │
                         │ • identify criteria      │
                         │ • assess confidence      │
                         └────────────┬─────────────┘
                                      │
                    ┌─────────────────┼─────────────────┐
                    │                 │                 │
                    ▼                 ▼                 ▼
            Criteria Processing  Supplier Processing  Clarification
                    │                 │                 │
                    ▼                 │                 │
          Evaluation Configuration   │                 │
                    │                 │                 │
                    └────────┬────────┴─────────────────┘
                             ▼
                    ┌─────────────────────┐
                    │  Evaluation Engine  │
                    │                     │
                    │ Validation          │
                    │ Canonical Mapping   │
                    │ Knockout            │
                    │ Qualitative Score   │
                    │ Weighted Calculation│
                    │ Ranking             │
                    │ Recommendation      │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │  Report Generator   │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Post-Evaluation Q&A │
                    └─────────────────────┘
```

---

# File Intake and Discovery

This is a new first-class architectural capability.

## Responsibilities

The File Intake and Discovery layer shall:

1. Accept one or more uploaded files.
2. Inspect workbook metadata and available sheets.
3. Determine likely file roles.
4. Determine likely sheet roles.
5. Identify supplier names where possible.
6. Detect evaluation criteria and scoring frameworks.
7. Detect combined workbooks containing multiple business roles.
8. Record confidence for material classifications.
9. Identify material ambiguity.
10. Route confidently classified information downstream.

## File Role Classification

At minimum:

- `evaluation_criteria`
- `supplier_submission`
- `combined_evaluation_and_supplier`
- `supporting_document`
- `unknown`

## Sheet Role Classification

At minimum:

- `evaluation_criteria`
- `supplier_response`
- `technical_response`
- `commercial_response`
- `company_profile`
- `references`
- `coverage`
- `instructions`
- `supporting_information`
- `irrelevant`
- `unknown`

## Confidence Handling

The system shall use confidence to determine whether it can proceed automatically.

Conceptually:

```text
High confidence
    ↓
Proceed automatically

Medium confidence
    ↓
Proceed with a visible assumption or targeted confirmation,
where the decision is material

Low confidence
    ↓
Ask a targeted clarification question
```

Exact numeric thresholds are implementation parameters and shall be validated during QI Studio testing rather than hard-coded at architecture level.

---

# Module Responsibilities

## 1. Supervisor

Responsible for:

- greeting users
- explaining the upload experience
- maintaining workflow state
- collecting files
- invoking File Intake and Discovery
- determining when clarification is required
- invoking downstream modules
- handling follow-up requests
- managing conversation continuity

The Supervisor never performs supplier evaluation itself.

---

## 2. File Intake and Discovery

Responsible for understanding uploaded files and preparing them for downstream modules.

It does not score suppliers or make procurement recommendations.

Outputs include:

```text
flow.fileIntake
```

containing classified files, sheets, detected entities, provenance and confidence.

---

## 3. Criteria Processing

Consumes discovered criteria sources and produces normalized evaluation criteria.

Responsibilities include:

- extracting sections
- extracting questions/requirements
- extracting weights
- extracting guidance
- extracting scoring rubrics
- identifying knockout candidates
- preserving provenance

Outputs:

```text
flow.criteria
```

---

## 4. Evaluation Configuration

Consumes normalized criteria and produces business-approved evaluation rules.

Outputs:

```text
flow.evaluationConfiguration
```

---

## 5. Supplier Processing

Consumes discovered supplier sources and produces normalized supplier response objects.

Responsibilities include:

- extracting supplier responses
- preserving source wording
- preserving provenance
- detecting unanswered questions
- identifying supplier identity

Outputs:

```text
flow.suppliers
```

---

## 6. Evaluation Engine

Performs all procurement analysis after the input has been normalized.

Internal stages:

1. Structure Validation
2. Canonical Mapping
3. Knockout Evaluation
4. Qualitative Scoring
5. Weighted Score Calculation
6. Supplier Ranking
7. Recommendation Generation

The Evaluation Engine shall not inspect raw workbook layout directly.

---

## 7. Report Generator

Consumes:

```text
flow.evaluationResult
```

and produces consultant-ready deliverables.

---

## 8. Post-Evaluation Q&A

Consumes stored evaluation results and reports to support follow-up analysis without unnecessary reprocessing.

---

# Overall Workflow

```text
User
 ↓
Supervisor
 ↓
Upload available files
 ↓
File Intake & Discovery
 ↓
Assess completeness / ambiguity
 ↓
Criteria Processing + Supplier Processing
 ↓
Evaluation Configuration
 ↓
Evaluation Engine
 ↓
Report Generation
 ↓
Results Delivered
 ↓
Post-Evaluation Q&A
```

The user is not required to understand or explicitly declare the role of every file.

---

# Human Intervention Model

Human intervention is preserved for decisions that require business judgement.

Examples:

- Two files appear equally likely to be the evaluation framework.
- A knockout requirement is materially ambiguous.
- A scoring rubric is absent and cannot be safely inferred.
- A supplier identity cannot be reliably determined.

The system should not ask users to resolve purely technical formatting questions when the information can be inferred.

---

# Architectural Constraints

The architecture assumes flexible real-world Excel inputs but a strict internal evaluation model.

The system shall therefore:

- tolerate reasonable workbook variation
- preserve source data
- retain provenance
- retain confidence
- avoid unsupported inference
- normalize before evaluation
- keep deterministic evaluation logic isolated

---

# Architecture Summary

The solution remains a modular conversational procurement evaluation platform, but the user-facing boundary is now intentionally schema-agnostic.

The Supervisor manages the conversation.

File Intake and Discovery translates user-provided files into structured business inputs.

Criteria and Supplier Processing normalize those inputs.

The Evaluation Engine performs deterministic, explainable procurement analysis.

The Report Generator produces client-ready outputs.

Post-Evaluation Q&A provides continued analytical interaction.

This architecture supports deployment to floor users without transferring internal data-model constraints to them.
