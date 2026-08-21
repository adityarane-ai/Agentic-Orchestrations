# 05. Conversation State Machine

**Document Version:** 1.3

**Status:** Deep Agent + GEP Knowledge + Human Confirmation Baseline

## Purpose

The Master Deep Agent dynamically orchestrates specialist work and relevant GEP Knowledge Library retrieval. The user journey contains a mandatory human-governed configuration gate before the first evaluation run.

## State Machine

```mermaid
stateDiagram-v2
    [*] --> INITIAL
    INITIAL --> WAITING_FOR_FILES
    WAITING_FOR_FILES --> DISCOVERING : files received
    DISCOVERING --> PLANNING
    PLANNING --> DISCOVERY_SPECIALISTS

    state DISCOVERY_SPECIALISTS {
        [*] --> CRITERIA_DISCOVERY
        [*] --> SUPPLIER_DISCOVERY
        CRITERIA_DISCOVERY --> DISCOVERY_RECONCILIATION
        SUPPLIER_DISCOVERY --> DISCOVERY_RECONCILIATION
        DISCOVERY_RECONCILIATION --> [*]
    }

    PLANNING --> KNOWLEDGE_RETRIEVAL : relevant GEP context required
    KNOWLEDGE_RETRIEVAL --> DISCOVERY_SPECIALISTS
    KNOWLEDGE_RETRIEVAL --> BUILDING_UNDERSTANDING

    DISCOVERY_SPECIALISTS --> BUILDING_UNDERSTANDING
    BUILDING_UNDERSTANDING --> CLARIFICATION_PACKAGE
    CLARIFICATION_PACKAGE --> HUMAN_CONFIRMATION

    HUMAN_CONFIRMATION --> CORRECTION_REQUIRED : incorrect / incomplete
    CORRECTION_REQUIRED --> PLANNING
    HUMAN_CONFIRMATION --> KNOCKOUT_CONFIGURATION : confirmed
    KNOCKOUT_CONFIGURATION --> CONFIGURATION_VALIDATION
    CONFIGURATION_VALIDATION --> HUMAN_CONFIRMATION : invalid / incomplete
    CONFIGURATION_VALIDATION --> EVALUATION_READY : approved

    EVALUATION_READY --> EVALUATING
    EVALUATING --> MASTER_QC
    MASTER_QC --> TARGETED_REANALYSIS : insufficient / inconsistent
    TARGETED_REANALYSIS --> EVALUATING
    MASTER_QC --> DETERMINISTIC_PROCESSING : accepted

    DETERMINISTIC_PROCESSING --> HUMAN_EXCEPTION : ambiguous knockout / material issue
    HUMAN_EXCEPTION --> KNOCKOUT_CONFIGURATION
    DETERMINISTIC_PROCESSING --> SYNTHESIS : valid

    SYNTHESIS --> GENERATING_REPORT
    GENERATING_REPORT --> COMPLETED
    COMPLETED --> POST_EVALUATION

    POST_EVALUATION --> POST_EVALUATION : explain / compare / regenerate
    POST_EVALUATION --> SCENARIO_REEVALUATION : approved rule / weight change
    SCENARIO_REEVALUATION --> CONFIGURATION_VALIDATION

    DISCOVERING --> DISCOVERY_ERROR : source problem
    KNOWLEDGE_RETRIEVAL --> KNOWLEDGE_ERROR : material retrieval failure
    KNOWLEDGE_ERROR --> HUMAN_CONFIRMATION : resolve with human
    EVALUATING --> EVALUATION_ERROR : unrecoverable specialist error
    EVALUATION_ERROR --> TARGETED_REANALYSIS
    GENERATING_REPORT --> REPORT_ERROR : export failure
    REPORT_ERROR --> GENERATING_REPORT
```

## Key States

**DISCOVERING:** inspect user files and identify likely roles.

**PLANNING:** Master determines specialists, tools, knowledge and dependencies.

**KNOWLEDGE_RETRIEVAL:** retrieve relevant GEP category/internal knowledge through approved Knowledge Library tools when it can materially improve interpretation or evaluation.

**DISCOVERY_SPECIALISTS:** Criteria and Supplier specialists may run independently/parallel where possible.

**BUILDING_UNDERSTANDING:** reconcile source facts, supplier identities, criteria and relevant knowledge context.

**CLARIFICATION_PACKAGE:** generate the human-facing Bid Clarification Package.

**HUMAN_CONFIRMATION:** human confirms/corrects understanding and material evaluation configuration.

**KNOCKOUT_CONFIGURATION:** human confirms/removes/adds knockout requirements and acceptance conditions, including explicit confirmation of no knockouts where applicable.

**CONFIGURATION_VALIDATION:** deterministically validate the confirmed configuration.

**EVALUATION_READY:** configuration is approved/frozen.

**EVALUATING:** Evaluation Specialist performs semantic assessment using confirmed criteria, supplier evidence and relevant GEP context.

**MASTER_QC:** Master challenges evidence sufficiency and consistency.

**DETERMINISTIC_PROCESSING:** validation, canonical mapping, confirmed knockout execution, score arithmetic, weighting and ranking.

**POST_EVALUATION:** answer questions from stored state.

**SCENARIO_REEVALUATION:** create a new configuration/result scenario for an approved change.

## Knowledge Governance

GEP knowledge is contextual. It may inform interpretation, benchmarking and rationale but cannot silently become an evaluation rule.

If retrieved knowledge conflicts with the RFP or confirmed configuration:

1. Preserve the source requirement.
2. Surface the conflict.
3. Ask for human confirmation if material.
4. Do not silently change the rule.

## State Transition Rules

1. Evaluation never begins before human confirmation of material configuration.
2. Candidate knockouts are not authoritative until confirmed.
3. No-knockout confirmation produces an empty rule set.
4. Confirmed configuration is frozen for the run.
5. GEP knowledge cannot silently modify configuration.
6. Material knowledge-retrieval failure returns to human clarification when required.
7. Ambiguous knockouts return to human configuration.
8. Follow-ups use stored state unless a new scenario is explicitly requested.
9. Report generation occurs after deterministic processing and final QC.
10. Source evidence remains immutable.
