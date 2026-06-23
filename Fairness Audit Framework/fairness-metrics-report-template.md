# Fairness Metrics Report
**Model/System:**  
**Version:**  
**Audit Date:**  
**Prepared By:**  
**Playbook Components Applied:** HCA, FDS, BSI, FMT

---

# 1. Executive Summary

## System Overview
Brief description of the system and decision context.

Example:
> The model predicts the probability that a borrower will successfully repay a loan. The score determines loan approval eligibility and maximum installment limits.

---

## Fairness Definitions Applied

| Priority | Fairness Definition | Reason for Selection |
|---|---|---|
| Primary | Equal Opportunity | Ensures qualified applicants have equal chance of loan approval |
| Secondary | Demographic Parity | Monitors representation differences in approvals |
| Monitoring | Predictive Parity | Ensures score meaning is consistent across groups |

---

## High-Level Findings

| Issue | Severity | Action |
|---|---|---|
| Racial disparity in approval rates | High | Mitigation required |
| Gender disparity in false negatives | Medium | Monitoring and threshold evaluation |
| Intersectional disparity for older minority applicants | Medium | Further investigation |

---

# 2. Dataset Summary

| Attribute | Categories | Sample Size |
|---|---|---|
| Gender | Male, Female | |
| Race | White, Black, Hispanic, Asian | |
| Age Group | <30, 30–50, >50 | |

---

## Representation Table

| Group | Samples | % of Dataset |
|---|---|---|
| Male | | |
| Female | | |
| Black | | |
| Hispanic | | |

---

# 3. Metric Plan

| Metric | Fairness Definition | Purpose |
|---|---|---|
| Statistical Parity Difference | Demographic Parity | Detect differences in approval rates |
| Equal Opportunity Difference | Equal Opportunity | Detect disparity among qualified applicants |
| False Negative Rate Difference | Equal Opportunity | Identify missed qualified applicants |
| Predictive Parity | Predictive Parity | Ensure prediction meaning consistency |

---

# 4. Group Fairness Metrics

| Group | Approval Rate | SPD vs Reference | 95% CI | Sample Size |
|---|---|---|---|---|
| Male | | | | |
| Female | | | | |
| White | | | | |
| Black | | | | |

---

# 5. Error Rate Analysis

| Group | TPR | FNR | FPR | CI |
|---|---|---|---|---|
| Male | | | | |
| Female | | | | |
| White | | | | |
| Black | | | | |

---

# 6. Intersectional Analysis

| Intersection Group | Approval Rate | TPR | FNR | Sample Size |
|---|---|---|---|---|
| Black Women | | | | |
| Hispanic Women | | | | |
| Asian Men | | | | |

---

## Worst Slice Summary

| Intersection | Metric | Disparity |
|---|---|---|
| | | |

---

# 7. Statistical Validation

## Confidence Intervals
Method: Bootstrap (10,000 resamples)

| Metric | Estimate | 95% CI |
|---|---|---|
| SPD (Race) | | |
| EO Difference (Gender) | | |

---

## Significance Testing

| Metric | p-value | Adjusted p-value | Significant |
|---|---|---|---|
| SPD (Race) | | | |
| EO Difference (Gender) | | | |

Multiple comparison correction: Benjamini–Hochberg (FDR)

---

# 8. Small Sample Evaluation

| Group | Sample Size | Reliability | Action |
|---|---|---|---|
| Black women >50 | | Low | Bayesian estimation applied |

---

# 9. Robustness Testing

## Cross Validation Stability

| Metric | Fold Range | Stability |
|---|---|---|
| SPD Race | | |
| EO Gender | | |

---

## Temporal Stability

| Period | Metric Value |
|---|---|
| Q1 | |
| Q2 | |
| Q3 | |

---

# 10. Simpson's Paradox Check

| Attribute | Aggregate Result | Intersection Result | Flag |
|---|---|---|---|
| Gender | | | |
| Race | | | |

---

# 11. Visualizations

Recommended charts:

- Fairness disparity bar chart with confidence intervals
- Intersectional heatmap
- Worst-slice disparity chart
- Simpson’s paradox comparison plot

---

# 12. Fairness Decision Assessment

| Tier | Condition | Decision |
|---|---|---|
| Green | Within thresholds | Deploy |
| Yellow | Uncertain disparity | Monitor |
| Orange | Practical disparity | Mitigate |
| Red | Statistically significant harm | Block deployment |

---

# 13. Recommended Actions

| Issue | Proposed Action |
|---|---|
| Racial disparity | Model retraining with rebalanced dataset |
| Gender FNR disparity | Threshold adjustment |
| Intersectional gaps | Targeted data collection |

---

# 14. Monitoring Plan

| Metric | Frequency | Alert Threshold |
|---|---|---|
| SPD Race | Monthly | >5% |
| EO Gender | Monthly | >3% |

---

# 15. Limitations

Document known constraints:

- Limited representation for some intersectional groups
- Potential proxy variables in financial indicators
- Evaluation dataset reflects historical patterns

---

# 16. Audit Sign-Off

| Role | Name | Approval |
|---|---|---|
| Staff Engineer | | |
| Risk/Compliance | | |
| Product Owner | | |

---
