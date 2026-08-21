# 01. Executive Summary

**Document Version:** 1.3

**Status:** Deep Agent + GEP Knowledge + Human-in-the-Loop Architecture

## Executive Summary

The RFP Qualitative Evaluation Agent is an enterprise-grade AI application designed to automate qualitative evaluation of supplier responses during strategic sourcing and RFP events.

The solution is developed using **GEP Quantum Intelligence Studio (QI Studio)** and uses one **Master Deep Agent with three direct specialist sub-agents**, GEP internal knowledge accessed through Knowledge Library tools, a formal human confirmation gate and deterministic processing for business rules and calculations.

The operating model is:

```text
Flexible User Inputs
        ↓
Master Deep Agent Discovery & Planning
        ↓
Criteria + Supplier Specialists
        ↓
Relevant GEP Knowledge Enrichment
        ↓
Bid Understanding + Clarification Package
        ↓
Human Confirmation + Knockout Configuration
        ↓
Frozen Evaluation Configuration
        ↓
Semantic Qualitative Evaluation
        ↓
Deterministic Validation / Knockouts / Scoring / Ranking
        ↓
Master Challenge + Procurement Synthesis
        ↓
Consultant-Ready Four-Tab Excel Report
        ↓
Post-Evaluation Q&A / Scenarios
```

The architecture deliberately separates:

- **Source truth:** RFP and supplier evidence.
- **GEP domain knowledge:** category toolkits, methodologies, benchmarks and guidance used as contextual evidence.
- **Human-confirmed evaluation truth:** the approved configuration for the sourcing event.
- **Deterministic computational truth:** validated rules, calculations and rankings.

GEP knowledge cannot silently override the RFP or human-confirmed evaluation configuration.

## Business Objective

Create an intelligent evaluation assistant capable of producing consultant-quality qualitative supplier evaluations while reducing manual effort, preserving auditability and keeping material procurement judgement under human control.

## Functional Goals

- Accept practical user-owned Excel files without prescribed filenames/templates.
- Discover evaluation criteria and supplier submissions.
- Retrieve relevant GEP category/internal knowledge through Knowledge Library tools.
- Generate a Bid Understanding / Clarification Package.
- Capture human confirmation/corrections and knockout requirements.
- Freeze the evaluation configuration for a run.
- Validate and canonicalize the evaluation dataset.
- Apply confirmed knockout rules deterministically.
- Perform evidence-backed qualitative scoring using the approved rubric and relevant GEP context.
- Calculate weighted scores deterministically.
- Rank qualified suppliers deterministically.
- Generate the standardized four-tab Excel report.
- Support post-evaluation explanations and controlled scenario re-evaluation.

## Deterministic Boundary

Semantic qualitative assessment is AI reasoning and therefore is not mathematically deterministic. Once the human-confirmed configuration and structured score outputs are fixed, validation, confirmed knockout execution, score arithmetic, weighting and ranking are deterministic and reproducible.

## Knowledge Governance

GEP knowledge is a contextual capability layer, not a fourth specialist agent. It can provide category terminology, best-practice guidance, benchmarks, methodologies and negotiation context.

Knowledge-derived recommendations that would materially change an evaluation criterion, knockout, weight or acceptance condition require human confirmation.

Supplier extraction remains source-first; GEP knowledge must not be used to fabricate or rewrite supplier evidence.

## Scope

Version 1.3 focuses on qualitative RFP evaluation using reasonably structured Excel inputs and approved GEP internal knowledge sources.

## Out of Scope

Unless separately approved:

- OCR
- PDF/image extraction
- Multi-language evaluation
- Live ERP integration
- Supplier portals
- Automatic sourcing event creation
- Autonomous award execution
- Contract generation
- Purchase order creation

## Design Philosophy

1. Human-centred evaluation.
2. Low-friction file intake.
3. Progressive disclosure through the clarification package.
4. Deep Agent adaptive orchestration.
5. GEP knowledge as contextual domain intelligence.
6. Deterministic business-rule execution.
7. Explainable and traceable AI.
8. Bounded specialist responsibilities.
9. Scenario lineage and recoverability.

## Output Report Contract

The standard Excel report contains:

1. **Executive Summary** — supplier qualification/status, ranking, section comparison, critical findings and recommendation.
2. **Supplier Profiles** — supplier summaries, strengths, weaknesses, risks, section scores and recommendation.
3. **Q&A Scorecard** — question-level supplier responses, scores and evaluator comments with source traceability.
4. **Score Legend** — actual scoring methodology used in the run.

Formatting follows the approved reference workbook design.

## Success Criteria

The project is successful when it can:

- accept flexible Excel inputs
- discover criteria and suppliers
- retrieve relevant GEP knowledge
- produce and obtain confirmation of a Bid Clarification Package
- capture human-confirmed knockouts
- perform evidence-backed qualitative evaluation
- execute deterministic qualification/scoring/ranking
- generate the four-tab report
- support follow-up questions and controlled re-ranking scenarios
- preserve source, configuration, knowledge and computational auditability

## Expected Business Benefits

- Reduced evaluation effort
- Reduced user preparation effort
- Improved consistency
- Faster sourcing cycles
- Better transparency and auditability
- Standardized evaluation methodology
- Improved consultant productivity
- Better use of GEP category knowledge
- Clearer governance over mandatory supplier requirements
