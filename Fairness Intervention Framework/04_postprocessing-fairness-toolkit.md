# Post-Processing Fairness Toolkit (PPFT)

## Objective

Enforce fairness at the **decision layer** when the model is already trained, deployed, and constrained.

Fairness issues at this stage are no longer about:

* data quality
* model learning
* causal structure

They are about:

> **how model outputs are translated into decisions**

This toolkit helps engineers:

* select appropriate post-processing strategies
* enforce fairness constraints on model outputs
* quantify trade-offs between fairness and performance
* deploy interventions without retraining

## Core Question

**Given fixed model outputs, how can we enforce fair decisions under real-world constraints?**

## Why Post-Processing Fairness Matters

Earlier stages control:

* *what the model learns*
* *how the model learns*

Post-processing controls:

> **what the system actually does**

Without this layer, teams risk:

* shipping unfair decisions despite “fair models”
* being unable to act due to deployment constraints
* failing regulatory or audit requirements

# Analysis Template (Post-Modeling Stage)

## Purpose

This document provides a structured framework for applying fairness interventions **after model training is complete**.

It is designed to help engineers:

* diagnose output-level bias
* select appropriate fairness criteria
* compute and validate intervention strategies
* deploy decision-layer corrections safely

## How to Use This Template

For each section:

* use validation data (not training/test)
* quantify all trade-offs explicitly
* document assumptions and constraints
* prioritize deployability over theoretical optimality

# 1. Output Bias Diagnosis Framework

## Objective

Identify whether fairness violations exist at the **decision or score level**.

## 1.1 System Output Definition

* What does the model output? (probabilities, scores, rankings, logits)
* How are outputs currently converted into decisions? (thresholds, rules, ranking cutoffs)

## 1.2 Observed Disparities

Measure across protected groups:

* selection rate
* true positive rate (TPR)
* false positive rate (FPR)
* calibration error

## 1.3 Bias Localization

| Layer              | Diagnostic Question                              | Outcome |
| ------------------ | ------------------------------------------------ | ------- |
| Score Distribution | Are score distributions different across groups? |         |
| Calibration        | Do scores mean the same thing across groups?     |         |
| Thresholding       | Are decisions skewed given similar scores?       |         |
| Policy             | Are rules amplifying disparities?                |         |

## 1.4 Diagnosis Summary

* What type of bias is present?
* Is it residual or structural?
* Is it small-gap or systemic?

# 2. Fairness Criteria Selection Framework

## Objective

Define the fairness constraint guiding intervention.

## 2.1 Candidate Fairness Definitions

* Demographic Parity → equal selection rates
* Equal Opportunity → equal TPR
* Equalized Odds → equal TPR and FPR

## 2.2 Selection Considerations

* domain requirements
* regulatory constraints
* ethical justification
* business impact

## 2.3 Selection Template

| Criterion | Selected (Y/N) | Justification | Risk |
| --------- | -------------- | ------------- | ---- |

## 2.4 Core Principle

> Fairness definition selection is a **policy decision**, not a technical default.

# 3. Threshold Optimization Framework

## Objective

Adjust decision thresholds to satisfy fairness constraints.

## 3.1 Decision Rule

$$
\hat{Y} = 1 ; \text{if} ; s \geq t
$$

## 3.2 Threshold Computation Strategy

* Demographic Parity → equal selection rates
* Equal Opportunity → equal TPR
* Equalized Odds → equal TPR + FPR (may require randomization)

## 3.3 Procedure

1. Split validation data by group
2. Compute ROC curves
3. Evaluate thresholds
4. Identify feasible regions
5. Select Pareto-optimal solutions

## 3.4 Trade-off Analysis

* accuracy
* precision/recall
* business KPIs

## 3.5 Selection Rule

Select thresholds maximizing utility under fairness constraints.

# 4. Calibration Framework

## Objective

Ensure scores represent true probabilities consistently across groups.

## 4.1 Calibration Model

$$
P(Y=1|s) = \frac{1}{1 + e^{-(As + B)}}
$$

## 4.2 Methods

* Platt Scaling
* Isotonic Regression
* Temperature Scaling

## 4.3 Group-Specific Calibration

* separate calibration per group improves fairness

# 5. Score Transformation Framework

## Objective

Adjust score distributions before decision-making.

## 5.1 Transformation Model

$$
s' = s \cdot \alpha_A
$$

## 5.2 Types

* linear scaling
* group-based scaling
* rank-preserving adjustments

# 6. Decision Policy Adjustment Framework

## Objective

Modify decision logic beyond thresholds.

## 6.1 Techniques

* decision flipping
* rejection option classification
* hybrid policies

# 7. Deployment Strategy Framework

## Objective

Ensure safe production integration.

## 7.1 Scenarios

* Protected attributes available → group-specific thresholds
* Not available → proxy-based or hybrid approaches

## 7.2 Pipeline

1. baseline snapshot
2. shadow deployment
3. A/B testing
4. rollout
5. monitoring

# 8. Integration with Full Fairness Methodology

* uses causal outputs from earlier parts
* applies counterfactual insights
* respects uncertainty constraints

# 9. Outputs

1. Bias diagnosis report
2. fairness criteria selection
3. threshold optimization results
4. calibration + transformation spec
5. deployment plan
6. monitoring framework

## Final Insight

Post-processing fairness does not fix models.

> It fixes system behavior at decision time.
