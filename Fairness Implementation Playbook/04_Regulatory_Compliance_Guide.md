# Regulatory Compliance Guide 

## 1. Overview

This guide translates AI regulatory requirements (EU AI Act, GDPR Article 22, and related governance standards) into concrete engineering practices, validation criteria, and operational controls.

It ensures that AI systems are:
- Legally compliant
- Technically auditable
- Operationally governed
- Continuously monitored

This document applies across the full AI lifecycle: design → build → deploy → operate → retire.

## 2. Regulatory Mapping Framework

This section maps legal obligations into engineering actions and acceptance criteria.

### 2.1 Core Regulatory Sources

- EU AI Act (risk-based classification, high-risk obligations)
- GDPR Article 22 (automated decision-making restrictions)
- GDPR Article 35 (DPIA requirements)
- Non-discrimination & equality law (EU member state level)

### 2.2 Requirement → Engineering Translation Matrix

| Regulatory Requirement | Engineering Task | Acceptance Criteria | Evidence Artifact |
|------------------------|------------------|--------------------|------------------|
| Risk management system (AI Act Art. 9) | Implement risk register + mitigation pipeline | All identified risks logged and tracked | Risk Register |
| Data governance (Art. 10) | Dataset validation + bias checks before training | Dataset passes quality + bias thresholds | Dataset Report |
| Technical documentation (Art. 11) | Auto-generate model cards per release | Model card exists for every deployed model | Model Card |
| Human oversight (Art. 14) | Build override + escalation workflow | Human can override every decision path | Oversight Logs |
| Logging & traceability (Art. 12) | Implement immutable event logging | 100% decision traceability | Audit Logs |
| Transparency (GDPR + AI Act) | Provide explainability interface | User-facing explanation available | Explanation Records |
| DPIA (GDPR Art. 35) | Conduct DPIA for high-risk systems | DPIA approved before deployment | DPIA Document |

### 2.3 Mapping Rule

For every AI feature:
> Each regulatory requirement MUST map to at least one engineering control and one verifiable artifact.

## 3. Risk Classification System

This system determines the level of governance required for any AI application.

### 3.1 Risk Scoring Formula

```

Total Risk Score (TRS) = (P × S) + E – M

````

Where:

- P = Probability of harm (1–5)
- S = Severity of harm (1–5)
- E = Exposure (1–3)
- M = Mitigation strength (0–4)

### 3.2 Risk Tiers

| TRS Score | Risk Tier | Governance Level | Required Controls |
|-----------|----------|------------------|------------------|
| ≥ 22 | Critical (Tier 1) | External audit required | Full DPIA, external conformity audit |
| 15–21 | High (Tier 2) | Internal + peer review | Bias audit, full documentation |
| 8–14 | Medium (Tier 3) | Product governance | Model card + monitoring |
| ≤ 7 | Low (Tier 4) | Team-level oversight | Lightweight checklist |

### 3.3 Classification Rules

- Risk classification is based on **inherent risk**, not mitigated risk.
- Classification MUST be recalculated when:
  - Model changes
  - Data changes
  - User population changes
  - Regulatory updates occur

## 4. Required Documentation Templates

All AI systems MUST generate the following artifacts based on risk tier.

### 4.1 Model Card (Mandatory for Tier ≥ 3)

```yaml
model_id: ""
commit_sha: ""
dataset_version: ""

risk_tier: ""
trs_score: ""
ai_act_category: ""

intended_use: ""
out_of_scope: ""

metrics:
  overall_performance: ""
  fairness_metrics:
    - group: ""
      metric: ""
      value: ""

human_oversight:
  enabled: true
  override_process: ""

change_log:
  - date: ""
    change: ""
    reason: ""
    reviewer: ""
````

### 4.2 Dataset Documentation (Datasheet)

Must include:

* Data source provenance
* Labeling process
* Known biases
* Preprocessing steps
* Consent and legality basis

### 4.3 DPIA Summary (High-Risk Only)

Includes:

* Data categories processed
* Purpose of processing
* Risk assessment
* Mitigation measures
* Approval signature (DPO)

### 4.4 Fairness Report

Must include:

* Protected attributes evaluated
* Intersectional analysis results
* Disparate impact metrics
* Mitigation actions applied

## 5. Audit Trail System Design

### 5.1 Architecture Overview

All AI system decisions MUST be logged through an immutable audit pipeline.

```
System Event
   ↓
Event Logger
   ↓
Immutable Log Store (WORM storage)
   ↓
Metrics Store
   ↓
Evidence Graph Database
```

---

### 5.2 Logged Event Types

| Event Type       | Description                      | Retention          |
| ---------------- | -------------------------------- | ------------------ |
| prediction_event | Model inference output           | 5 years            |
| risk_assessment  | TRS calculation + classification | 10 years           |
| override_event   | Human override action            | 5 years            |
| dataset_update   | Training data changes            | lifetime + 2 years |
| model_update     | New model deployment             | 10 years           |

### 5.3 Audit Log Requirements

Each log entry MUST include:

* Timestamp
* Model version
* Input reference hash
* Output decision
* User/action ID (if applicable)
* Risk score context

### 5.4 Tamper Resistance

* Logs are hash-chained (Merkle tree structure)
* Stored in append-only storage (WORM policy)
* Daily integrity verification required

## 6. Stage-Gate Governance Model

Every AI system MUST pass the following gates before production.

### G0 — Ideation

* Define use case
* Draft initial risk score
* DPO notified if TRS ≥ 8

Evidence:

* Risk Ticket

### G1 — Design

* Risk classification finalized
* Controls mapped to requirements
* Documentation templates created

Evidence:

* Architecture Review Doc

### G2 — Build

* Bias tests implemented
* Model card auto-generated
* Unit + fairness tests pass

Evidence:

* CI pipeline logs

### G3 — Validation

* Independent QA review completed
* DPIA approved (if required)
* Human oversight verified

Evidence:

* Validation report

### G4 — Deployment

* Monitoring dashboards active
* Logging pipeline live
* Incident escalation configured

Evidence:

* Deployment checklist

### G5 — Operation

* Continuous fairness monitoring
* Monthly drift analysis
* Incident tracking

Evidence:

* Monitoring reports

### G6 — Retirement

* Data deletion executed
* Model archived
* Audit logs preserved

Evidence:

* Decommission report

## 7. Operational Integration Guide

### 7.1 CI/CD Integration Rules

* No merge without:

  * Model card generated
  * Risk score validated
  * Test suite passed
  * Logging enabled

### 7.2 Definition of Done (DoD)

A feature is NOT complete unless:

* [ ] TRS calculated
* [ ] Risk tier assigned
* [ ] Required documentation generated
* [ ] Audit logging enabled
* [ ] Fairness tests executed
* [ ] Human oversight defined (if required)

### 7.3 Monitoring Requirements

All production models MUST track:

* Performance drift
* Fairness drift
* Data distribution changes
* Override frequency
* Incident rates

## 8. Governance Responsibilities (RACI)

| Role       | Responsibility                         |
| ---------- | -------------------------------------- |
| Product    | Risk classification + feature approval |
| ML Team    | Model fairness + performance           |
| Legal/DPO  | GDPR + AI Act compliance               |
| Security   | Logging + integrity                    |
| Compliance | Audit readiness                        |

## 9. Compliance KPIs

System compliance is evaluated using:

* 100% high-risk documentation coverage
* ≥ 85% classification consistency
* ≤ 2 weeks documentation update lag
* 100% audit traceability coverage
* Zero unlogged production decisions

## 10. Key Principle

> If it is not documented, it does not exist in compliance terms.

## 11. Final Notes

This guide is designed to:

* Reduce regulatory ambiguity
* Embed compliance into engineering workflows
* Ensure audit readiness by design
* Prevent last-minute legal scrambling

All systems must treat compliance as an engineering constraint, not a post-hoc activity.
