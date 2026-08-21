# 02. Business Requirements

**Document Version:** 1.2

**Status:** Deep Agent + Human-in-the-Loop Baseline

**Parent Document:** Software Design Specification (SDS)

---

# Purpose

This document defines the business and functional requirements for the RFP Qualitative Bid Analysis Agent.

The solution uses a **Master Deep Agent** with three specialist sub-agents and a formal **Bid Understanding / Human Confirmation Gate**. Users provide the files they have; the agent discovers and explains its understanding, after which the human evaluator confirms the evaluation framework and provides/confirms knockout requirements before evaluation begins.

The internal system remains schema-driven and deterministic wherever possible.

---

# Requirement Classification

| Prefix | Description |
|---|---|
| BR | Business Requirement |
| FR | Functional Requirement |
| NFR | Non-Functional Requirement |
| CR | Conversation Requirement |
| DR | Data Requirement |
| RR | Reporting Requirement |
| ER | Error Handling Requirement |
| AR | AI Behaviour Requirement |
| AG | Agent Orchestration Requirement |
| HITL | Human-in-the-Loop Requirement |

---

# Business Requirements

## BR-001
The solution shall significantly reduce manual effort required to evaluate qualitative supplier responses.

## BR-002
The solution shall improve consistency of supplier evaluation across procurement consultants.

## BR-003
The solution shall maintain explainability for every material supplier evaluation outcome.

## BR-004
The solution shall reduce evaluation cycle time.

## BR-005
The solution shall generate standardized consultant-ready evaluation reports.

## BR-006
The solution shall preserve human oversight throughout the evaluation process.

## BR-007 — Low-Friction User Experience
A floor user shall not be required to understand the internal data model of the evaluation agent.

## BR-008 — User-Owned File Flexibility
The solution shall accept reasonably structured Excel files supplied by users without requiring prescribed filenames, sheet names, column names or workbook templates.

## BR-009 — Intelligent File Understanding
The solution shall determine likely business roles of uploaded files and sheets using semantic and structural analysis.

## BR-010 — Human-Governed Evaluation Configuration
The system shall present its current understanding before evaluation and shall obtain human confirmation of material evaluation configuration.

---

# Conversation Requirements

## CR-001
The system shall greet the user when a conversation begins.

## CR-002
The system shall explain that users can upload the Excel files available to them.

## CR-003
The system shall maintain conversational context throughout the evaluation session.

## CR-004
The system shall determine what information has already been received before asking for additional input.

## CR-005
The system shall not ask users to identify information that can be reliably discovered from uploaded files.

## CR-006
The system shall communicate detected file roles in business language.

## CR-007
The system shall support clarification when files or interpretations are materially ambiguous.

## CR-008
The system shall produce a **Bid Clarification Package** before the evaluation run is authorized.

## CR-009
The Bid Clarification Package shall show the agent's understanding of the uploaded material, including detected criteria, suppliers, scoring/weighting information, potential knockouts, ambiguities and missing information.

## CR-010
The system shall allow the human evaluator to confirm, correct or supplement the Bid Clarification Package.

## CR-011
The system shall ask the human evaluator to provide or confirm knockout questions/requirements and their acceptance conditions before evaluation.

## CR-012
The system shall not begin the evaluation run until the material evaluation configuration is confirmed.

## CR-013
The system shall support follow-up questions after evaluation without repeating extraction unnecessarily.

---

# File Intake and Discovery Requirements

## FR-001 to FR-015
The system shall accept one or more Excel files, inspect workbook/sheet structures, classify file and sheet roles, identify suppliers and evaluation criteria, retain confidence and provenance, detect material ambiguity and avoid requiring reformatting when semantic mapping is reliable.

Supported file roles:

- evaluation_criteria
- supplier_submission
- combined_evaluation_and_supplier
- supporting_document
- unknown

Supported sheet roles:

- evaluation_criteria
- supplier_response
- technical_response
- commercial_response
- company_profile
- references
- coverage
- instructions
- supporting_information
- irrelevant
- unknown

---

# Criteria Processing

## FR-020
The system shall extract every identifiable evaluation section.

## FR-021
The system shall extract every identifiable evaluation question or requirement.

## FR-022
The system shall preserve question numbering where present and create stable identifiers where absent.

## FR-023
The system shall preserve meaningful section ordering.

## FR-024
The system shall extract question weights where present.

## FR-025
The system shall extract scoring guidance and rubrics where present.

## FR-026
The system shall identify potential knockout requirements without treating them as confirmed rules.

## FR-027
The system shall preserve source provenance.

## FR-028
The system shall tolerate semantically equivalent field names and reasonable structural variations.

## FR-029
The system shall distinguish explicit source criteria from inferred interpretations.

## FR-030 — Bid Understanding
The system shall present extracted criteria and material interpretations to the human evaluator before the evaluation run.

## FR-031 — Knockout Configuration
The system shall allow the human evaluator to configure knockout requirements.

## FR-032 — Acceptance Conditions
The system shall allow the human evaluator to specify or confirm expected answers/acceptance conditions for each knockout.

## FR-033
The system shall allow modification of evaluation weights before the evaluation run is frozen.

## FR-034
The system shall allow excluding questions from evaluation where supported by the business process.

## FR-035
The finalized evaluation configuration shall be immutable for that evaluation run.

---

# Supplier Processing

## FR-040 to FR-050
The system shall process supplier responses from reasonably structured Excel workbooks without prescribed supplier templates, preserve original wording and provenance, detect unanswered questions, support multiple suppliers and incremental uploads, and detect likely duplicate supplier files.

The Supplier Specialist shall not score or rank suppliers.

---

# Validation

## FR-060
The system shall validate structural compatibility between supplier responses and normalized evaluation criteria.

## FR-061
The system shall validate meaningful section compatibility.

## FR-062
The system shall validate question mapping and numbering where available.

## FR-063
The system shall validate question coverage.

## FR-064
The system shall distinguish hard validation failures from warnings.

## FR-065
The system shall stop scoring where a material validation failure would make the result unreliable.

## FR-066
The system shall not reject a workbook solely because it differs from an internal template if its information can be mapped reliably.

---

# Canonical Mapping

## FR-080
The system shall construct a canonical evaluation model.

## FR-081
The canonical model shall contain exactly one normalized representation of every evaluation question/requirement used for scoring.

## FR-082
Each canonical question shall combine, where available:

- stable question identifier
- source question number
- section
- question text
- supplier answer
- evaluation criteria
- confirmed weight
- scoring guidance
- confirmed knockout rule
- source provenance
- mapping confidence

## FR-083
The canonical model shall preserve uncertainty rather than silently fabricating missing source information.

---

# Knockout Evaluation

## FR-100
The system shall execute mandatory knockout requirements before qualified scoring/ranking.

## FR-101
A supplier failing a confirmed mandatory knockout shall be identified before ranking.

## FR-102
Every knockout decision shall include explanation and supporting source evidence.

## FR-103
Knockout evaluation shall use human-confirmed acceptance conditions rather than generic keyword matching.

## FR-104
Ambiguous knockout interpretations shall be surfaced to the human evaluator rather than silently treated as failures.

## FR-105
If the human explicitly confirms that there are no knockout requirements, the configuration shall record `knockoutRules = []`; the agent shall not invent knockout rules.

---

# Scoring

## FR-120
The system shall perform qualitative scoring against the confirmed rubric where a valid rubric exists.

## FR-121
Every score shall include supporting reasoning and source evidence.

## FR-122
The system shall apply confirmed/configured question weights.

## FR-123
The system shall calculate section totals deterministically.

## FR-124
The system shall calculate overall supplier scores deterministically.

## FR-125
The system shall generate supplier strengths.

## FR-126
The system shall generate supplier weaknesses.

## FR-127
The system shall identify evaluation risks.

## FR-128
The system shall identify negotiation opportunities.

---

# Ranking

## FR-140
The system shall rank qualified suppliers by deterministic weighted score.

## FR-141
Ranking calculation shall be deterministic and reproducible.

## FR-142
Disqualified suppliers shall not receive a qualified rank.

## FR-143
The system shall explain supplier ranking using the underlying scores, evidence and configuration.

---

# Reporting

## RR-001
The system shall generate an Excel workbook.

## RR-002
The workbook shall contain an **Executive Summary** tab.

## RR-003
The workbook shall contain a **Supplier Profiles** tab.

## RR-004
The workbook shall contain a **Q&A Scorecard** tab.

## RR-005
The workbook shall contain a **Score Legend** tab.

## RR-006
The Executive Summary shall show supplier status/ranking, section-level comparison, overall results and recommendation.

## RR-007
Supplier Profiles shall show supplier-specific summary, strengths, weaknesses and section-level assessment.

## RR-008
Q&A Scorecard shall preserve question-level supplier responses, scores and evaluator comments.

## RR-009
Score Legend shall describe the scoring methodology actually used for the evaluation run; it shall not claim an unapproved methodology.

## RR-010
The workbook shall display knockout/qualification status prominently.

## RR-011
The report generator shall preserve the approved reference workbook's visual hierarchy and formatting logic while dynamically scaling to supplier/question counts.

## RR-012
The report shall contain sufficient source/evaluation configuration metadata for auditability.

---

# Post-Evaluation Analysis

## FR-160
The system shall answer questions regarding completed evaluations.

## FR-161
The system shall compare suppliers.

## FR-162
The system shall explain individual scores and knockout outcomes.

## FR-163
The system shall support approved changes to evaluation weights as a new scenario.

## FR-164
The system shall regenerate rankings deterministically after approved weight changes.

## FR-165
The system shall regenerate reports after approved re-evaluation.

## FR-166
Original evaluation configurations and results shall remain recoverable when a new scenario is created.

---

# AI Behaviour Requirements

## AR-001
The system shall use deterministic processing wherever deterministic solutions exist.

## AR-002
LLMs shall perform semantic interpretation and qualitative reasoning; they shall not be the authoritative arithmetic, weighting or ranking engine.

## AR-003
The system shall never fabricate supplier information.

## AR-004
The system shall never fabricate source evaluation criteria.

## AR-005
The system shall preserve extracted source information.

## AR-006
The system shall produce explainable reasoning.

## AR-007
The system shall distinguish source facts from inferred interpretations.

## AR-008
The system shall use confidence for material classification and mapping decisions.

## AR-009
The system shall prefer a targeted human clarification over an unsupported material assumption.

## AR-010
The Master Deep Agent shall not silently modify a human-confirmed evaluation configuration.

## AR-011
The Master Deep Agent shall challenge specialist outputs for evidence sufficiency and internal consistency before final synthesis.

## AR-012
Specialist agents shall return structured outputs conforming to their contracts.

---

# Agent Orchestration Requirements

## AG-001
There shall be one Master Deep Agent for the bid-analysis workflow.

## AG-002
The Master Deep Agent may delegate to at most three direct specialist sub-agents in the V1 architecture.

## AG-003
The three specialist roles shall be:

1. RFP & Evaluation Criteria Analyst
2. Supplier Response & Evidence Analyst
3. Qualitative Evaluation & Comparison Analyst

## AG-004
The Master Deep Agent shall dynamically determine which specialists are required for the user's request.

## AG-005
Independent specialist tasks may be executed in parallel where platform capabilities permit.

## AG-006
Dependent tasks shall execute only after their prerequisites are available.

## AG-007
The Master shall reconcile specialist outputs and request targeted re-analysis where evidence is insufficient or inconsistent.

## AG-008
Tool discovery shall be used only when an appropriate capability is not already known/available to the executing agent.

## AG-009
The Master shall remain responsible for final orchestration and synthesis; specialist agents shall not independently redefine the workflow.

---

# Human-in-the-Loop Requirements

## HITL-001
The system shall produce a Bid Clarification Package before the first evaluation run.

## HITL-002
The package shall include:

- identified files and roles
- identified suppliers
- evaluation sections/questions
- detected scoring scale/rubric
- detected weights
- potential knockout candidates
- proposed acceptance conditions where inferable
- material ambiguities
- missing information
- explicit vs inferred values

## HITL-003
The human evaluator shall be able to confirm the agent's understanding.

## HITL-004
The human evaluator shall be able to correct the agent's understanding.

## HITL-005
The human evaluator shall be able to add knockout requirements not discovered by the agent.

## HITL-006
The human evaluator shall be able to remove or modify proposed knockout candidates.

## HITL-007
Each confirmed knockout shall have an acceptance condition or an explicitly approved decision method.

## HITL-008
The evaluation run shall not start until material configuration is approved.

## HITL-009
Once approved, the configuration shall be versioned/frozen for that run.

---

# Data Requirements

## DR-001
All workflow modules shall communicate using defined JSON contracts.

## DR-002
No module shall directly modify another module's internal output.

## DR-003
Shared business information shall be exchanged using Flow Variables.

## DR-004
The canonical question model shall be the single source of truth for downstream evaluation.

## DR-005
Source provenance shall be retained for material extracted fields.

## DR-006
File discovery outputs shall be immutable once accepted into downstream processing, except through explicit re-discovery.

## DR-007
Human-confirmed evaluation configuration shall be stored separately from source criteria.

## DR-008
Evaluation scenarios shall preserve parent configuration/result references.

---

# Non-Functional Requirements

## NFR-001
The system shall support evaluation of at least ten supplier workbooks within one sourcing event.

## NFR-010
Every workflow stage shall perform one primary responsibility.

## NFR-011
Workflow failures shall be isolated.

## NFR-012
Errors shall be explainable in business language.

## NFR-013
File classification uncertainty shall not silently propagate into procurement decisions.

## NFR-014
The architecture shall support dynamic delegation while preserving bounded direct sub-agent count.

## NFR-020
JSON contracts shall remain stable within the version.

## NFR-030
Supplier information shall remain within the evaluation workflow.

---

# Acceptance Criteria

The project shall be considered complete when:

- A floor user can upload reasonably structured Excel files without prescribed filenames or sheet names.
- The Master Deep Agent can determine whether criteria, supplier extraction and/or evaluation analysis are required.
- The system can produce a Bid Clarification Package before evaluation.
- A human evaluator can confirm/correct the understanding and configure knockouts.
- Evaluation cannot bypass the confirmed configuration gate.
- Criteria and supplier data are normalized into the canonical model.
- Knockout and scoring logic are reproducible.
- Qualified suppliers are ranked deterministically.
- The four-tab Excel report is generated in the approved format.
- Follow-up questions can be answered from stored evaluation results.
- Re-weighting creates a controlled scenario rather than silently overwriting the original run.

---

# Requirement Traceability

Implementation artefacts shall reference the requirement identifiers defined within this document.
