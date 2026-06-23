# Adaptability Guide

## Objective
Enable engineers to apply the fairness intervention playbook across different domains without rebuilding the methodology from scratch.

Fairness problems change by context but the causal structure and intervention logic remain consistent.

## Core Idea

Different industries have different data, risks, and constraints.

But they all share the same underlying pattern:

> Decisions are made using data → data reflects historical bias → models amplify or encode that bias

This guide shows how to adapt the same fairness workflow across domains.

## Step 1: Identify the Domain Context

Before applying any fairness tools, clearly define the environment.

### Engineer Action:
Document:
- Industry (e.g., finance, healthcare, hiring, insurance, education)
- Decision type (e.g., approval, ranking, prediction, scoring)
- Stakeholders affected (who is impacted by the model?)

## Step 2: Redefine the Outcome Variable

Each domain has a different definition of “success.”

### Examples:

- **Loan approval:** Default risk prediction
- **Hiring:** Candidate selection probability
- **Healthcare:** Diagnosis or risk score
- **Insurance:** Claim approval or pricing

### Engineer Action:
- Clearly define `Y` (outcome variable)
- Ensure it reflects real-world decision impact, not just model output

## Step 3: Map Domain-Specific Sensitive Attributes

Sensitive attributes vary by context and regulation.

### Examples:

- Finance: gender, income group, geography
- Healthcare: age, disability status, ethnicity
- Hiring: gender, education background, name bias signals
- Insurance: age, location, occupation

### Engineer Action:
- Identify legally and ethically sensitive attributes
- Check for indirect proxies unique to the domain

## Step 4: Translate Features into Causal Roles

Every feature should be classified based on its role:

- **Protected attributes**
- **Mediators**
- **Proxies**
- **Legitimate predictors**

### Domain Differences Matter:

- Healthcare: symptoms may be mediators
- Hiring: education may be partially structural
- Finance: income may reflect systemic inequality

### Engineer Action:
- Rebuild causal interpretation per domain
- Do NOT reuse assumptions blindly across domains

## Step 5: Reconstruct the Causal Graph (DAG)

The structure stays the same, but variables change.

### Engineer Action:
- Rebuild DAG using domain-specific variables
- Re-evaluate:
  - Direct paths
  - Proxy paths
  - Mediator paths

## Step 6: Re-evaluate Fairness Definitions

Different domains require different fairness priorities.

### Examples:

- **Finance:** Equal opportunity (loan repayment fairness)
- **Hiring:** Demographic parity (representation fairness)
- **Healthcare:** Calibration (risk accuracy across groups)
- **Criminal justice:** False positive minimization (high-stakes error control)

### Engineer Action:
- Choose fairness definition based on harm model
- Document why that definition fits the domain

## Step 7: Adapt Intervention Strategy

Once structure is mapped, reuse the intervention logic:

### Pre-processing:
- Data imbalance correction
- Proxy removal
- Feature transformation

### In-processing:
- Fairness constraints
- Adversarial debiasing
- Regularized learning objectives

### Post-processing:
- Threshold adjustment
- Score calibration
- Ranking adjustments

### Engineer Action:
- Select interventions based on causal diagnosis, not domain habit

## Step 8: Validate in Context

Validation is domain-sensitive.

### Engineer Action:
- Define acceptable performance trade-offs per domain
- Adjust fairness thresholds based on real-world impact
- Run subgroup analysis relevant to the sector

## Common Adaptation Failures

- Copy-pasting fairness assumptions across domains
- Treating proxies as universal (they are not)
- Using wrong fairness metric for the domain
- Ignoring regulatory constraints
- Overgeneralizing causal assumptions

## Example Mapping

| Domain      | Sensitive Attributes | Key Fairness Focus      | Typical Risk |
|-------------|----------------------|--------------------------|--------------|
| Finance     | Gender, income       | Equal opportunity        | Loan denial bias |
| Healthcare  | Age, ethnicity       | Calibration fairness     | Misdiagnosis |
| Hiring      | Gender, education    | Representation fairness  | Hiring imbalance |
| Insurance   | Age, location        | Risk parity              | Pricing discrimination |

## Key Insight

> Fairness is not one-size-fits-all—but the reasoning process is.

You adapt variables, not logic.
You adapt context, not structure.
