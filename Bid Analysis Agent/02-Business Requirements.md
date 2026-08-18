# 02. Business Requirements

**Document Version:** 0.2

**Status:** Updated for Floor-User UX

**Parent Document:** Software Design Specification (SDS)

---

# Purpose

This document defines the business and functional requirements for the RFP Qualitative Evaluation Agent, including the new **zero-template / low-friction floor-user experience**.

The solution shall allow procurement users to upload the Excel files available to them without requiring knowledge of the agent's internal workbook templates, sheet names, column names or file naming conventions.

The system remains internally schema-driven and deterministic wherever possible; complexity is absorbed by the File Intake and Discovery layer rather than imposed on the user.

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

---

# Business Requirements

## BR-001

The solution shall significantly reduce manual effort required to evaluate qualitative supplier responses.

## BR-002

The solution shall improve consistency of supplier evaluation across procurement consultants.

## BR-003

The solution shall maintain complete explainability for every supplier recommendation.

## BR-004

The solution shall reduce evaluation cycle time.

## BR-005

The solution shall generate standardized consultant-ready evaluation reports.

## BR-006

The solution shall preserve human oversight throughout the evaluation process.

## BR-007 — Low-Friction User Experience

A floor user shall not be required to understand the internal data model of the evaluation agent in order to use it.

## BR-008 — User-Owned File Flexibility

The solution shall accept reasonably structured Excel files supplied by users without requiring a prescribed filename, sheet name, column name or workbook template.

## BR-009 — Intelligent File Understanding

The solution shall determine the likely business role of uploaded files and sheets using semantic and structural analysis.

## BR-010 — Minimal Clarification

The solution shall ask the user for clarification only when the available files and context are insufficient to proceed with acceptable confidence.

---

# Conversation Requirements

## CR-001

The system shall greet the user when a conversation begins.

## CR-002

The system shall explain that the user can upload the Excel files available to them rather than requiring a prescribed template.

## CR-003

The system shall maintain conversational context throughout the evaluation session.

## CR-004

The system shall determine what information has already been received before asking for additional input.

## CR-005

The system shall avoid asking the user to identify information that can be reliably inferred from uploaded files.

## CR-006

The system shall communicate detected file roles in business language when useful, for example: "I identified one evaluation framework and three supplier responses."

## CR-007

The system shall support clarification when multiple files or interpretations are materially ambiguous.

## CR-008

The system shall answer follow-up questions after evaluation without repeating extraction or scoring unnecessarily.

---

# File Intake and Discovery Requirements

## FR-001

The system shall accept one or more uploaded Excel files in a sourcing session.

## FR-002

The system shall not require users to rename files to a prescribed naming convention.

## FR-003

The system shall not require users to use prescribed sheet names.

## FR-004

The system shall not require users to use prescribed column names.

## FR-005

The system shall inspect workbook metadata and sheet structure before downstream processing.

## FR-006

The system shall classify uploaded files into business roles where sufficient evidence exists.

Supported classifications shall include at minimum:

- evaluation_criteria
- supplier_submission
- combined_evaluation_and_supplier
- supporting_document
- unknown

## FR-007

The system shall classify relevant workbook sheets where sufficient evidence exists.

Possible classifications shall include:

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

## FR-008

The system shall identify likely supplier names from workbook content, metadata or contextual evidence.

## FR-009

The system shall detect when multiple supplier submissions are present within a workbook where practical.

## FR-010

The system shall detect when a single workbook contains both evaluation criteria and supplier response information.

## FR-011

The system shall assign a confidence level to material file and sheet classifications.

## FR-012

The system shall proceed automatically when classification confidence is sufficient and no material ambiguity exists.

## FR-013

The system shall request clarification when classification confidence is insufficient for a material downstream decision.

## FR-014

The system shall distinguish between missing information and merely unfamiliar formatting.

## FR-015

The system shall never ask a user to reformat a workbook when the existing information can be interpreted reliably without reformatting.

---

# Evaluation Criteria Processing

## FR-020

The system shall extract every identifiable evaluation section.

## FR-021

The system shall extract every identifiable evaluation question or requirement.

## FR-022

The system shall preserve question numbering where present and generate stable internal identifiers where numbering is absent.

## FR-023

The system shall preserve section ordering where meaningful.

## FR-024

The system shall extract question weights where present.

## FR-025

The system shall extract evaluation guidance and scoring rubrics where present.

## FR-026

The system shall identify potential knockout requirements where present.

## FR-027

The system shall preserve source workbook structure and provenance information.

## FR-028

The system shall tolerate semantically equivalent field names and reasonable structural variations.

## FR-029

The system shall distinguish explicit source criteria from inferred interpretations.

## FR-030

The system shall allow review of extracted evaluation criteria before supplier evaluation when material configuration decisions are required.

## FR-031

The system shall allow configuration of knockout questions.

## FR-032

The system shall allow specification of expected answers or acceptance conditions for configured knockout questions.

## FR-033

The system shall allow modification of evaluation weights before supplier evaluation.

## FR-034

The system shall allow excluding questions from evaluation where supported by the business process.

## FR-035

The finalized evaluation configuration shall be used by the Evaluation Engine.

---

# Supplier Processing

## FR-040

The system shall process supplier responses from reasonably structured Excel workbooks without requiring a prescribed supplier template.

## FR-041

The system shall extract every identifiable supplier answer.

## FR-042

The system shall preserve source response wording and provenance.

## FR-043

The system shall preserve meaningful section ordering.

## FR-044

The system shall preserve question numbering where present.

## FR-045

The system shall never rewrite supplier responses during extraction.

## FR-046

The system shall never summarize supplier responses during raw extraction.

## FR-047

The system shall detect unanswered questions where practical.

## FR-048

The system shall support evaluating multiple suppliers within the same sourcing event.

## FR-049

The system shall support incremental supplier uploads within the same session.

## FR-050

The system shall detect likely duplicate supplier files and avoid silently creating duplicate supplier records.

---

# Validation

## FR-060

The system shall validate structural compatibility between supplier responses and the normalized evaluation criteria.

## FR-061

The system shall validate section compatibility where meaningful.

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

The canonical model shall contain exactly one normalized representation of every evaluation question or requirement used for scoring.

## FR-082

Each canonical question shall combine, where available:

- stable question identifier
- source question number
- section
- question text
- supplier answer
- evaluation criteria
- weight
- scoring guidance
- knockout rules
- source provenance
- mapping confidence

## FR-083

The canonical model shall preserve uncertainty rather than silently fabricating missing source information.

---

# Knockout Evaluation

## FR-100

The system shall evaluate mandatory knockout requirements before scoring.

## FR-101

A supplier failing a mandatory knockout requirement shall be identified before ranking.

## FR-102

Every knockout decision shall include an explanation and supporting source evidence.

## FR-103

Knockout evaluation shall use configured acceptance conditions rather than simplistic keyword matching.

## FR-104

Ambiguous knockout interpretations shall be surfaced for human confirmation rather than silently treated as failures.

---

# Scoring

## FR-120

The system shall perform qualitative scoring where a valid scoring rubric exists.

## FR-121

Every score shall include supporting reasoning and source evidence.

## FR-122

The system shall apply configured question weights.

## FR-123

The system shall calculate section totals.

## FR-124

The system shall calculate overall supplier scores.

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

The system shall rank qualified suppliers by weighted score.

## FR-141

The ranking calculation shall be deterministic.

## FR-142

Disqualified suppliers shall not receive a qualified rank.

## FR-143

The system shall explain supplier ranking.

---

# Reporting

## RR-001

The system shall generate an Excel workbook.

## RR-002

The report shall contain an executive summary.

## RR-003

The report shall contain detailed scoring.

## RR-004

The report shall contain ranking.

## RR-005

The report shall contain knockout results.

## RR-006

The report shall contain strengths and weaknesses.

## RR-007

The report shall contain recommendations.

## RR-008

The report shall identify source files and evaluation configuration metadata needed for auditability.

---

# Post-Evaluation Analysis

## FR-160

The system shall answer questions regarding completed evaluations.

## FR-161

The system shall compare suppliers.

## FR-162

The system shall explain individual scores.

## FR-163

The system shall support changing evaluation weights where the business process permits it.

## FR-164

The system shall regenerate rankings after approved weight changes.

## FR-165

The system shall regenerate reports after approved re-evaluation.

---

# AI Behaviour Requirements

## AR-001

The system shall use deterministic processing wherever deterministic solutions exist.

## AR-002

LLMs shall perform semantic interpretation only; they shall not be the authoritative arithmetic or ranking engine.

## AR-003

The system shall never fabricate supplier information.

## AR-004

The system shall never fabricate evaluation criteria.

## AR-005

The system shall always preserve extracted source information.

## AR-006

The system shall produce explainable reasoning.

## AR-007

The system shall distinguish source facts from inferred interpretations.

## AR-008

The system shall use confidence thresholds for material classification and mapping decisions.

## AR-009

The system shall prefer asking one targeted clarification question over making an unsupported material assumption.

---

# Data Requirements

## DR-001

All workflow modules shall communicate using defined JSON contracts.

## DR-002

No module shall directly modify another module's internal output.

## DR-003

All shared information shall be exchanged using Flow Variables.

## DR-004

The canonical question model shall be the single source of truth for downstream evaluation.

## DR-005

Source provenance shall be retained for material extracted fields.

## DR-006

File discovery outputs shall be immutable once accepted into downstream processing, except through an explicit re-discovery action.

---

# Non-Functional Requirements

## Performance

### NFR-001

The system shall support evaluation of at least ten supplier workbooks within one sourcing event.

### NFR-002

The architecture shall support future horizontal scaling.

## Reliability

### NFR-010

Every workflow stage shall perform exactly one responsibility.

### NFR-011

Workflow failures shall be isolated.

### NFR-012

Errors shall be explainable in business language.

### NFR-013

File classification uncertainty shall not silently propagate into procurement decisions.

## Maintainability

### NFR-020

Node responsibilities shall remain independent.

### NFR-021

JSON contracts shall remain stable within the version.

### NFR-022

Prompts shall remain modular.

## Security

### NFR-030

Supplier information shall never be exposed outside the evaluation workflow.

### NFR-031

The system shall not fabricate procurement recommendations.

---

# Acceptance Criteria

The project shall be considered complete when:

- A floor user can upload reasonably structured Excel files without following a prescribed file naming convention.
- The agent can identify evaluation criteria and supplier submissions in common workbook variations.
- The agent asks for clarification only when required to resolve material ambiguity.
- Criteria and supplier data are normalized into the canonical model.
- Knockout and scoring logic are reproducible.
- Qualified suppliers are ranked deterministically.
- Evaluation reports are generated successfully.
- Follow-up questions can be answered from the stored evaluation result.
- Supplier rankings are reproducible.
- The architecture complies with the Software Design Specification.

---

# Requirement Traceability

Every implementation artefact shall reference the requirement identifiers defined within this document.

The new File Intake and Discovery component primarily implements:

- BR-007 to BR-010
- CR-002 to CR-007
- FR-001 to FR-015
- FR-028 to FR-029
- FR-050
- FR-066
- AR-007 to AR-009
- NFR-013

This traceability ensures the new low-friction user experience remains a first-class product requirement rather than an implementation detail.
