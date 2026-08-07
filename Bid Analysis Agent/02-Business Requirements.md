# 02. Business Requirements

**Document Version:** 0.1

**Status:** Draft

**Parent Document:** Software Design Specification (SDS)

---

# Purpose

This document defines the complete business and functional requirements for the RFP Qualitative Evaluation Agent.

The requirements documented here constitute the contractual behaviour expected from the solution.

Every architectural decision, workflow, prompt, script, report and user interaction shall satisfy one or more of these requirements.

All subsequent implementation documents shall reference requirement identifiers defined in this chapter.

---

# Requirement Classification

Requirements are classified into the following categories.

| Prefix | Description |
|----------|-------------|
| BR | Business Requirement |
| FR | Functional Requirement |
| NFR | Non-Functional Requirement |
| CR | Conversation Requirement |
| DR | Data Requirement |
| RR | Reporting Requirement |
| ER | Error Handling Requirement |
| AR | AI Behaviour Requirement |

---

# Business Requirements

## BR-001

The solution shall significantly reduce manual effort required to evaluate qualitative supplier responses.

---

## BR-002

The solution shall improve consistency of supplier evaluation across procurement consultants.

---

## BR-003

The solution shall maintain complete explainability for every supplier recommendation.

---

## BR-004

The solution shall reduce evaluation cycle time.

---

## BR-005

The solution shall generate standardized consultant-ready evaluation reports.

---

## BR-006

The solution shall preserve human oversight throughout the evaluation process.

---

# Functional Requirements

## Conversation

### FR-001

The system shall greet the user when a conversation begins.

---

### FR-002

The system shall guide the user through the complete evaluation workflow.

---

### FR-003

The system shall request the Evaluation Criteria workbook before supplier evaluation begins.

---

### FR-004

The system shall validate that an Evaluation Criteria workbook has been uploaded.

---

### FR-005

The system shall request supplier response workbooks only after successful criteria processing.

---

### FR-006

The system shall support uploading multiple supplier response workbooks within a single interaction.

---

### FR-007

The system shall maintain conversational context throughout the evaluation session.

---

### FR-008

The system shall answer follow-up questions after evaluation without repeating extraction or scoring.

---

# Evaluation Criteria Processing

### FR-020

The system shall extract every evaluation section.

---

### FR-021

The system shall extract every evaluation question.

---

### FR-022

The system shall preserve question numbering.

---

### FR-023

The system shall preserve section ordering.

---

### FR-024

The system shall extract question weights.

---

### FR-025

The system shall extract evaluation guidance.

---

### FR-026

The system shall extract knockout requirements.

---

### FR-027

The system shall preserve workbook structure.

---

# Supplier Processing

### FR-040

The system shall process one supplier workbook at a time.

---

### FR-041

The system shall extract every supplier answer.

---

### FR-042

The system shall preserve supplier formatting where practical.

---

### FR-043

The system shall preserve section ordering.

---

### FR-044

The system shall preserve question numbering.

---

### FR-045

The system shall never rewrite supplier responses.

---

### FR-046

The system shall never summarize supplier responses during extraction.

---

### FR-047

The system shall detect unanswered questions.

---

### FR-048

The system shall support evaluating multiple suppliers within the same sourcing event.

---

# Validation

### FR-060

The system shall validate structural compatibility between supplier responses and evaluation criteria.

---

### FR-061

The system shall validate section count.

---

### FR-062

The system shall validate section order.

---

### FR-063

The system shall validate question numbering.

---

### FR-064

The system shall validate question count.

---

### FR-065

The system shall stop evaluation if validation fails.

---

# Canonical Mapping

### FR-080

The system shall construct a canonical evaluation model.

---

### FR-081

The canonical model shall contain exactly one representation of every evaluation question.

---

### FR-082

Each canonical question shall combine:

- Question
- Supplier Answer
- Evaluation Criteria
- Weight
- Knockout Rules

---

# Knockout Evaluation

### FR-100

The system shall evaluate mandatory knockout requirements before scoring.

---

### FR-101

A supplier failing a mandatory knockout requirement shall be identified before ranking.

---

### FR-102

Every knockout decision shall include an explanation.

---

# Scoring

### FR-120

The system shall perform qualitative scoring.

---

### FR-121

Every score shall include supporting reasoning.

---

### FR-122

The system shall apply configured question weights.

---

### FR-123

The system shall calculate section totals.

---

### FR-124

The system shall calculate overall supplier scores.

---

### FR-125

The system shall generate supplier strengths.

---

### FR-126

The system shall generate supplier weaknesses.

---

### FR-127

The system shall identify evaluation risks.

---

### FR-128

The system shall identify negotiation opportunities.

---

# Ranking

### FR-140

The system shall rank suppliers by weighted score.

---

### FR-141

The ranking shall be deterministic.

---

### FR-142

The system shall explain supplier ranking.

---

# Reporting

### RR-001

The system shall generate an Excel workbook.

---

### RR-002

The report shall contain an executive summary.

---

### RR-003

The report shall contain detailed scoring.

---

### RR-004

The report shall contain ranking.

---

### RR-005

The report shall contain knockout results.

---

### RR-006

The report shall contain strengths and weaknesses.

---

### RR-007

The report shall contain recommendations.

---

# Post-Evaluation Analysis

### FR-160

The system shall answer questions regarding completed evaluations.

---

### FR-161

The system shall compare suppliers.

---

### FR-162

The system shall explain individual scores.

---

### FR-163

The system shall support changing evaluation weights.

---

### FR-164

The system shall regenerate rankings after weight changes.

---

### FR-165

The system shall regenerate reports after re-evaluation.

---

# AI Behaviour Requirements

### AR-001

The system shall use deterministic processing wherever deterministic solutions exist.

---

### AR-002

LLMs shall only perform semantic reasoning.

---

### AR-003

The system shall never fabricate supplier information.

---

### AR-004

The system shall never fabricate evaluation criteria.

---

### AR-005

The system shall always preserve extracted source information.

---

### AR-006

The system shall produce explainable reasoning.

---

# Data Requirements

### DR-001

All workflow modules shall communicate using defined JSON contracts.

---

### DR-002

No module shall directly modify another module's internal output.

---

### DR-003

All shared information shall be exchanged using Flow Variables.

---

### DR-004

The canonical question model shall be the single source of truth for downstream evaluation.

---

# Non-Functional Requirements

## Performance

### NFR-001

The system shall support evaluation of at least ten supplier workbooks within one sourcing event.

---

### NFR-002

The architecture shall support future horizontal scaling.

---

## Reliability

### NFR-010

Every workflow stage shall perform exactly one responsibility.

---

### NFR-011

Workflow failures shall be isolated.

---

### NFR-012

Errors shall be explainable.

---

## Maintainability

### NFR-020

Node responsibilities shall remain independent.

---

### NFR-021

JSON contracts shall remain stable.

---

### NFR-022

Prompts shall remain modular.

---

## Security

### NFR-030

Supplier information shall never be exposed outside the evaluation workflow.

---

### NFR-031

The system shall not fabricate procurement recommendations.

---

# Acceptance Criteria

The project shall be considered complete when:

- All functional requirements are implemented.
- All mandatory business requirements are satisfied.
- All acceptance tests pass.
- Evaluation reports are generated successfully.
- Supplier rankings are reproducible.
- The architecture complies with the Software Design Specification.

---

# Requirement Traceability

Every implementation artefact shall reference the requirement identifiers defined within this document.

Example:

Extract Supplier Submission

Implements:

FR-040

FR-041

FR-042

FR-043

FR-045

FR-047

This traceability ensures complete alignment between business requirements and implementation.
