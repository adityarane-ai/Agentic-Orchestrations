# 01. Executive Summary

**Document Version:** 1.2

**Status:** Updated for Deep Agent Architecture + Human-in-the-Loop Configuration

**Parent Document:** Software Design Specification (SDS)

---

# Executive Summary

The RFP Qualitative Evaluation Agent is an enterprise-grade AI application designed to automate the qualitative evaluation of supplier responses during strategic sourcing and Request for Proposal (RFP) events.

The solution is developed using **GEP Quantum Intelligence Studio (QI Studio)** and uses a **Master Deep Agent with three specialist sub-agents** to provide adaptive planning, delegation, evidence reconciliation and procurement analysis while preserving deterministic decision logic.

Version 1.2 introduces a controlled **Bid Understanding & Human Confirmation Gate**. The agent first interprets the uploaded RFP/evaluation material and supplier submissions, creates a structured clarification package for the evaluator, and waits for human confirmation before the evaluation configuration is frozen. Knockout requirements are proposed by the agent where detectable, but the human evaluator confirms, modifies or adds the knockout requirements and acceptance conditions before evaluation begins.

The solution therefore combines:

```text
Flexible User Inputs
        ↓
Master Deep Agent Discovery & Planning
        ↓
Specialist Parallel / Sequential Analysis
        ↓
Bid Understanding + Clarification Package
        ↓
Human Confirmation + Knockout Configuration
        ↓
Frozen Evaluation Configuration
        ↓
Deterministic Validation / Rules / Scoring / Ranking
        ↓
Master Challenge + Procurement Synthesis
        ↓
Consultant-Ready Excel Report
        ↓
Post-Evaluation Q&A / Re-weighting / Re-ranking
```

The system is explicitly designed so that:

- LLMs handle semantic interpretation, evidence analysis and qualitative judgement.
- Deterministic processing owns validation, knockout rule execution, arithmetic, weighting and ranking.
- Human evaluators confirm material business rules before the system makes a procurement evaluation.
- Source evidence and provenance remain traceable throughout the workflow.

The solution augments procurement consultants rather than replacing human judgement. Final sourcing decisions remain with the evaluation committee.

---

# Problem Statement

Large strategic sourcing events frequently involve:

- Multiple suppliers
- Hundreds of qualitative questions
- Thousands of pages of supplier responses
- Multiple evaluation criteria
- Cross-functional evaluation teams
- Time-sensitive decision making

Current qualitative evaluation processes are largely manual.

In addition, forcing users to prepare files in a rigid format before automation can begin creates another operational burden and limits adoption on the procurement floor.

The solution therefore addresses both problems:

1. Reduce manual evaluation effort.
2. Remove unnecessary formatting and file-preparation burden from users.
3. Preserve human control over material evaluation rules, especially knockout requirements.

---

# Business Objective

The objective is to create an intelligent evaluation assistant capable of producing consultant-quality qualitative supplier evaluations while significantly reducing manual effort and minimizing user input constraints.

The system should:

- Reduce evaluation time
- Improve scoring consistency
- Preserve explainability
- Improve auditability
- Accept practical real-world Excel inputs
- Generate standardized reports
- Support interactive consultant workflows
- Make material evaluation configuration explicit and human-approved

---

# Project Goals

## Functional Goals

- Accept available user-provided Excel files without prescribed filenames.
- Discover workbook and sheet structures.
- Identify evaluation criteria and supplier submissions automatically where possible.
- Normalize evaluation criteria.
- Normalize supplier responses.
- Generate a bid understanding / clarification package before evaluation.
- Capture human confirmation and corrections to the discovered evaluation framework.
- Capture human-confirmed knockout requirements and acceptance conditions.
- Freeze the evaluation configuration for an evaluation run.
- Validate structural compatibility.
- Build a unified canonical evaluation model.
- Apply deterministic knockout evaluation.
- Perform explainable qualitative scoring.
- Calculate weighted supplier scores.
- Rank suppliers deterministically.
- Generate a standardized consultant-ready Excel report.
- Support post-evaluation question answering.
- Support approved re-weighting and re-ranking scenarios without overwriting the original evaluation configuration.

## Technical Goals

The solution shall be:

- Production ready
- Modular
- Explainable
- Deterministic wherever possible
- Scalable
- Easy to debug
- Easy to extend
- Maintainable
- Observable
- Token efficient
- Robust to reasonable Excel structural variation
- Controlled against unsupported autonomous rule changes

---

# Scope

Version 1.2 focuses on qualitative RFP evaluation using reasonably structured Excel inputs.

Supported capabilities include:

- Flexible file intake
- File and sheet classification
- Evaluation criteria extraction
- Supplier response extraction
- Bid understanding generation
- Human confirmation / correction
- Knockout configuration
- Structural validation
- Canonical mapping
- Knockout evaluation
- Qualitative scoring
- Weighted ranking
- Excel report generation
- Post-evaluation question answering
- Approved re-weighting and re-ranking
- Evidence-based procurement synthesis

---

# Out of Scope

The following remain intentionally excluded unless separately approved:

- OCR
- PDF extraction
- Image-based supplier submissions
- Multi-language evaluation
- Live ERP integration
- Supplier portals
- Automatic sourcing event creation
- Autonomous award execution
- Contract generation
- Purchase order creation

The system is flexible with respect to Excel structure, but this does not imply unrestricted support for arbitrary unstructured documents.

---

# Target Users

Primary users include:

- Procurement Consultants
- Strategic Sourcing Managers
- Category Managers
- Procurement Transformation Teams
- Client Evaluation Committees
- Procurement users on the floor who may not know the system's internal file model

Secondary users include:

- Project Managers
- AI Engineers
- Solution Architects
- Technical Support Teams

---

# Design Philosophy

## 1. Human-Centred Evaluation

AI assists consultants but does not replace professional judgement.

## 2. Low-Friction Input

Users provide available files rather than conforming to internal templates.

## 3. Progressive Disclosure

The system first discovers and presents what it understands. Human clarification is concentrated into a configuration gate before evaluation rather than scattered across the workflow.

## 4. Deep Agent Orchestration

The Master Deep Agent plans work, selects specialist sub-agents, exploits parallelism where tasks are independent, manages dependencies, performs reconciliation and requests targeted re-analysis when evidence is insufficient.

## 5. Deterministic Processing

Validation, knockout rule execution, arithmetic, weighting and ranking remain deterministic.

## 6. Human-Confirmed Business Rules

The agent may identify candidate knockout requirements and scoring assumptions, but the evaluation configuration is not frozen until the human evaluator confirms or modifies the material business rules.

## 7. Explainable AI

Every material evaluation decision is traceable to source evidence, evaluation configuration and deterministic outcomes.

## 8. Modular Architecture

The Deep Agent owns orchestration; specialist agents own semantic sub-tasks; deterministic processing owns business rules and calculations; the report layer owns presentation only.

## 9. Conversation-Driven Workflow

The system maintains continuity before, during and after evaluation.

---

# Output Report Contract

The standard Excel report shall contain four primary tabs based on the approved reference workbook design:

1. **Executive Summary** — decision-maker view, supplier ranking/status, section-level comparison and recommendation.
2. **Supplier Profiles** — supplier-by-supplier summary, strengths, weaknesses and section-level score breakdown.
3. **Q&A Scorecard** — detailed question-by-question supplier responses, scores and evaluator comments, preserving source-response traceability.
4. **Score Legend** — scoring methodology and definitions actually used for the evaluation run.

Formatting shall follow the approved report reference design, including its visual hierarchy, merged section bands, supplier headers, wrapped long-form text, score presentation, strengths/weaknesses layout and print-friendly structure.

The report generator shall dynamically scale the layout to the number of suppliers while preserving the reference design logic.

The report shall display knockout / qualification status prominently and shall not state that an LLM-domain benchmark was used unless that methodology was actually approved for the run.

---

# Success Criteria

The project will be considered successful when it can:

✓ Accept reasonably structured Excel files without prescribed filenames or sheet names.

✓ Identify evaluation criteria and supplier submissions in common workbook variations.

✓ Produce a clear bid understanding package before evaluation.

✓ Obtain human confirmation of material evaluation configuration.

✓ Capture and apply human-confirmed knockout requirements.

✓ Ask targeted clarification questions only when required.

✓ Extract evaluation criteria accurately.

✓ Extract supplier responses accurately.

✓ Validate questionnaire consistency.

✓ Detect knockout outcomes correctly using configured acceptance conditions.

✓ Produce explainable qualitative scores.

✓ Generate accurate deterministic weighted rankings.

✓ Produce the four-tab consultant-ready Excel report.

✓ Support conversational post-evaluation analysis and approved scenario re-ranking.

✓ Preserve auditability across source evidence, human-confirmed configuration and deterministic results.

---

# Expected Business Benefits

Implementation is expected to deliver:

- Reduced evaluation effort
- Reduced user preparation effort
- Improved scoring consistency
- Faster sourcing cycles
- Increased transparency
- Better auditability
- Standardized evaluation methodology
- Improved consultant productivity
- Higher adoption by floor users
- Higher confidence in supplier evaluation decisions
- Clearer governance over mandatory supplier requirements

---

# Relationship to the Remaining Specification

This chapter establishes the updated business context for the solution.

Subsequent chapters define the requirements, architecture, state machine, data flow, QI Studio implementation, Flow Variables and JSON contracts required to deliver the Version 1.2 experience.
