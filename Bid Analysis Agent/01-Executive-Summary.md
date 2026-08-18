# 01. Executive Summary

**Document Version:** 0.2

**Status:** Updated for Floor-User UX

**Parent Document:** Software Design Specification (SDS)

---

# Executive Summary

The RFP Qualitative Evaluation Agent is an enterprise-grade AI application designed to automate the qualitative evaluation of supplier responses during strategic sourcing and Request for Proposal (RFP) events.

The solution is being developed using **GEP Quantum Intelligence Studio (QI Studio)** and follows a modular, explainable, production-oriented architecture.

A key Version 1.1 enhancement is a **low-friction, zero-template user experience**. Floor users should be able to upload the Excel files they already have without being required to understand internal workbook templates, filenames, sheet names or column conventions.

The system uses a File Intake and Discovery layer to interpret uploaded workbooks, identify evaluation criteria and supplier submissions, inspect workbook structures, detect ambiguity and normalize the information into strict internal contracts.

The system therefore combines:

```text
Flexible User Inputs
        ↓
File Intelligence
        ↓
Structured Normalization
        ↓
Deterministic Evaluation
        ↓
Explainable Results
```

Rather than generating simple summaries, the system extracts supplier responses, aligns them against evaluation criteria, applies deterministic business rules, performs explainable qualitative scoring, ranks suppliers, generates consultant-ready reports and continues supporting procurement teams through conversational post-evaluation analysis.

The system augments procurement consultants rather than replacing human judgement. Final sourcing decisions remain with the evaluation committee.

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

---

# Project Goals

## Functional Goals

- Accept available user-provided Excel files without prescribed filenames.
- Discover workbook and sheet structures.
- Identify evaluation criteria and supplier submissions automatically where possible.
- Normalize evaluation criteria.
- Normalize supplier responses.
- Validate structural compatibility.
- Build a unified canonical evaluation model.
- Apply deterministic knockout evaluation.
- Perform explainable qualitative scoring.
- Calculate weighted supplier scores.
- Rank suppliers.
- Generate consultant-ready Excel reports.
- Support post-evaluation conversational analysis.

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

---

# Scope

Version 1.1 focuses on qualitative RFP evaluation using reasonably structured Excel inputs.

Supported capabilities include:

- Flexible file intake
- File and sheet classification
- Evaluation criteria extraction
- Supplier response extraction
- Structural validation
- Canonical mapping
- Knockout evaluation
- Qualitative scoring
- Weighted ranking
- Excel report generation
- Post-evaluation question answering
- Re-weighting and re-ranking

---

# Out of Scope

The following remain intentionally excluded from Version 1.1 unless separately approved:

- OCR
- PDF extraction
- Image-based supplier submissions
- Multi-language evaluation
- Live ERP integration
- Supplier portals
- Automatic sourcing event creation
- Autonomous award recommendations
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

The system infers information where reliable and asks only for material clarification.

## 4. Deterministic Processing

Validation, weighting, arithmetic and ranking remain deterministic.

## 5. Explainable AI

Every material evaluation decision is traceable to evidence and rules.

## 6. Modular Architecture

Each component performs one responsibility and communicates through explicit contracts.

## 7. Conversation-Driven Workflow

The Supervisor maintains continuity before, during and after evaluation.

---

# Success Criteria

The project will be considered successful when it can:

✓ Accept reasonably structured Excel files without prescribed filenames or sheet names.

✓ Identify evaluation criteria and supplier submissions in common workbook variations.

✓ Ask targeted clarification questions only when required.

✓ Extract evaluation criteria accurately.

✓ Extract supplier responses accurately.

✓ Validate questionnaire consistency.

✓ Detect knockout failures correctly.

✓ Produce explainable qualitative scores.

✓ Generate accurate weighted rankings.

✓ Produce consultant-ready Excel reports.

✓ Support conversational post-evaluation analysis.

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

---

# Relationship to the Remaining Specification

This chapter establishes the updated business context for the solution.

Subsequent chapters define the requirements, architecture, state machine, data flow, QI Studio implementation, Flow Variables and JSON contracts required to deliver the Version 1.1 experience.
