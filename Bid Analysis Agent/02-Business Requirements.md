# 02. Business Requirements

**Document Version:** 1.3

**Status:** Deep Agent + GEP Knowledge + Human-in-the-Loop Baseline

## Purpose

Define the business and functional requirements for the RFP Qualitative Bid Analysis Agent using one Master Deep Agent, three specialist sub-agents, GEP Knowledge Library tools, human-confirmed evaluation configuration and deterministic processing.

## Requirement Classification

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
| KR | Knowledge Requirement |

## Business Requirements

### BR-001 to BR-010
The solution shall reduce manual qualitative evaluation effort, improve consistency, preserve explainability, reduce cycle time, generate standardized reports, preserve human oversight, accept user-owned files without rigid templates and obtain human confirmation of material evaluation configuration before the first evaluation run.

## Conversation Requirements

- Users may upload the Excel files they have.
- The system shall not require users to know internal filenames, sheet names, column names or templates.
- The system shall maintain conversation state.
- The system shall produce a Bid Clarification Package before evaluation.
- The package shall show detected files, suppliers, criteria, scoring/weights, candidate knockouts, relevant knowledge context, ambiguities and missing information.
- The human evaluator shall confirm/correct the understanding.
- The human shall provide/confirm knockout requirements and acceptance conditions.
- Evaluation shall not begin until material configuration is confirmed.
- Follow-up questions shall use stored evaluation state where possible.

## File Intake Requirements

- Accept one or more reasonably structured Excel files.
- Inspect workbook and sheet structure.
- Classify likely file and sheet roles semantically.
- Identify suppliers and evaluation criteria.
- Preserve provenance and confidence.
- Detect material ambiguity.
- Avoid rejecting a file merely because it differs from an internal template when reliable semantic mapping is possible.

## Criteria Processing Requirements

- Extract evaluation sections and questions.
- Preserve question numbering where present.
- Create stable IDs where absent.
- Extract weights and scoring guidance where available.
- Identify candidate knockout requirements without making them authoritative.
- Preserve source provenance.
- Distinguish explicit source criteria from inferred interpretation.
- Use relevant GEP knowledge for terminology/category context when helpful.
- Never convert knowledge-only recommendations into authoritative criteria without human confirmation.

## Supplier Processing Requirements

- Identify suppliers and response boundaries.
- Extract original supplier responses.
- Preserve supplier wording and provenance.
- Detect unanswered questions and mapping confidence.
- Support multiple suppliers.
- Supplier extraction is source-first and must not fabricate supplier evidence using GEP knowledge.

## GEP Knowledge Requirements

### KR-001
The system shall support GEP internal category toolkits and other approved internal knowledge through Knowledge Library tools.

### KR-002
Relevant knowledge may include category toolkits, playbooks, methodologies, benchmarks, evaluation guidance, negotiation guidance and internal terminology.

### KR-003
The Master and relevant specialists shall retrieve knowledge only when it is relevant to the task.

### KR-004
Knowledge retrieval shall follow the platform's prescribed knowledge-tool workflow.

### KR-005
Knowledge shall be treated as contextual guidance/benchmark unless explicitly incorporated into the human-confirmed evaluation configuration.

### KR-006
GEP knowledge shall not silently override RFP requirements, supplier evidence or confirmed evaluation rules.

### KR-007
Material knowledge-derived changes to criteria, weights, knockouts or acceptance conditions shall require human confirmation.

### KR-008
Material knowledge references used in evaluation rationale shall remain traceable.

## Human-in-the-Loop Requirements

- Produce Bid Clarification Package.
- Allow confirmation/correction of file roles, suppliers, criteria, scoring, weights and assumptions.
- Allow confirmation/removal/modification/addition of knockout requirements.
- Require acceptance conditions for confirmed knockouts unless another approved decision method exists.
- Allow explicit confirmation of no knockouts.
- Freeze approved configuration for the run.
- Create a new scenario/version for post-evaluation rule/weight changes.

## Validation Requirements

- Validate structural compatibility.
- Validate supplier/question coverage.
- Validate score ranges and weight integrity.
- Validate knockout-rule completeness.
- Stop deterministic evaluation when material validation errors remain.

## Canonical Mapping Requirements

The system shall construct one normalized downstream representation containing stable question ID, section, question text, supplier response, evidence, confirmed weight, rubric, confirmed knockout rule and provenance/confidence.

## Knockout Requirements

- Execute confirmed mandatory requirements before ranking.
- Use human-confirmed acceptance conditions.
- Never use generic keyword matching as the authoritative knockout decision.
- Surface ambiguous outcomes to the human.
- If no knockouts are confirmed, use an explicit empty knockout rule set.

## Scoring Requirements

- Semantic qualitative scoring shall use the confirmed rubric and supplier evidence.
- Relevant GEP knowledge may provide category context, benchmarks and best-practice guidance.
- Every score recommendation shall include rationale, evidence and confidence.
- Score arithmetic, section totals and overall weighted scores shall be deterministic.

## Ranking Requirements

- Rank qualified suppliers by deterministic weighted score.
- Disqualified suppliers shall not receive a qualified rank.
- Ranking shall be reproducible from the same configuration and structured score outputs.

## Reporting Requirements

The workbook shall contain exactly four primary tabs:

1. Executive Summary
2. Supplier Profiles
3. Q&A Scorecard
4. Score Legend

The workbook shall preserve the approved reference formatting logic and display qualification/knockout status prominently.

The Score Legend shall describe the methodology actually used; it shall not invent an LLM-based methodology.

## Post-Evaluation Requirements

The system shall answer questions about completed evaluations, compare suppliers, explain scores and knockout outcomes, regenerate reports and support approved re-weighting as a new scenario without overwriting the original run.

## AI Behaviour Requirements

1. Use deterministic processing wherever deterministic solutions exist.
2. Use LLMs for semantic interpretation and qualitative reasoning.
3. Never fabricate supplier information or source criteria.
4. Preserve source responses.
5. Distinguish source facts from inference.
6. Retain confidence/provenance for material interpretation.
7. Prefer targeted human clarification over unsupported material assumptions.
8. Never silently modify human-confirmed configuration.
9. Challenge specialist outputs for evidence sufficiency and consistency.
10. Treat GEP knowledge as contextual unless explicitly confirmed as part of the evaluation configuration.

## Agent Orchestration Requirements

- One Master Deep Agent.
- Maximum three direct specialist sub-agents.
- Specialists: Criteria, Supplier Evidence, Qualitative Evaluation/Comparison.
- Dynamic specialist selection.
- Parallel execution where tasks are independent and platform capabilities permit.
- Sequential execution where dependencies require it.
- Targeted re-analysis rather than full restart where possible.
- Conditional system-tool discovery when an unknown capability is genuinely required.
- Knowledge Library retrieval when relevant.

## Deterministic Boundary

```text
Semantic interpretation → AI
Human business-rule confirmation → Human
Confirmed rule execution → Deterministic
Arithmetic / weighting / ranking → Deterministic
Procurement synthesis → AI
```

## Data Requirements

- Shared workflow objects use defined contracts.
- Every shared object has one producer.
- Consumers treat upstream objects as read-only.
- Human-confirmed configuration is separate from source criteria.
- GEP knowledge context is separate from supplier evidence.
- Provenance is retained.
- Scenario lineage is retained.

## Non-Functional Requirements

- Production ready.
- Modular.
- Explainable.
- Scalable to at least ten suppliers per sourcing event.
- Maintainable.
- Observable.
- Robust to reasonable Excel structural variation.
- Stable JSON contracts within the version.
- Deterministic outputs reproducible from the same inputs/configuration.

## Acceptance Criteria

The solution is complete when:

- users can upload practical Excel files without prescribed templates;
- the Master can dynamically select specialist work;
- relevant GEP knowledge can be retrieved through Knowledge Library tools;
- the system produces a Bid Clarification Package;
- the human can confirm/correct understanding and configure knockouts;
- evaluation cannot bypass the confirmation gate;
- semantic evaluation is evidence-backed and knowledge-informed where relevant;
- deterministic validation, knockout execution, scoring, weighting and ranking are reproducible;
- the four-tab Excel report is generated in the approved format;
- post-evaluation scenarios preserve original results.
