# Fairness Intervention Playbook Case Study  
## End-to-End Loan Approval System Remediation (Gender + Structural Bias)

# Executive Summary

This case study presents a full-stack fairness remediation of a production loan approval system exhibiting persistent gender-based disparities despite prior isolated mitigation attempts (pre-processing adjustments, model tuning, and threshold calibration).

The system showed a stable approval gap of approximately **4% between male and female applicants**, consistent across retraining cycles and demographic slices.

To address this, the **Fairness Intervention Playbook** was applied as a unified, layered framework:

1. **Pre-processing Fairness Toolkit (PFT)** → Data auditing & bias diagnosis  
2. **Causal Fairness Toolkit (CFT)** → Structural bias mechanism discovery  
3. **In-processing Fairness Interventions** → Constraint-based learning correction  
4. **Post-processing Fairness Toolkit (PPFT)** → Decision-layer correction  

### Final Outcome

- Gender approval gap reduced: **4% → <1%**
- Model performance impact: **AUC drop < 2%**
- System stability: preserved across multiple retraining cycles
- Regulatory compliance: maintained (fair lending standards satisfied)

# 1. System Context

## 1.1 Decision Environment

The system is a supervised machine learning model used for **loan approval decisions** in a retail banking environment.

- **Decision Type:** Binary classification (Approve / Reject)  
- **Deployment Mode:** Human-in-the-loop underwriting support  
- **Regulatory Context:** Fair lending compliance required  
- **Risk Level:** High-stakes financial decision system  

## 1.2 Model Inputs (High-Level)

The model uses structured financial and behavioral variables:

- Applicant income  
- Credit score  
- Employment history  
- Debt-to-income ratio  
- Loan amount requested  
- Residential postcode  
- Banking history features  
- Derived risk scoring features  

These inputs encode both **financial behavior** and **structural socioeconomic signals**.

## 1.3 Protected Attribute

- **A = Gender (Male / Female)**



# 2. Pre-processing Fairness Toolkit (PFT)
## Data Auditing & Bias Diagnostics


## 2.1 Multidimensional Representation Analysis

### Representation Findings

| Group | Dataset Share | Reference Expectation | Gap |
|------|--------------|----------------------|-----|
| Male | 58% | 50% | +8% |
| Female | 42% | 50% | -8% |

### Key Insight

- Female applicants are underrepresented in historical training data
- Representation gap introduces **sampling imbalance and model prior skew**


## 2.2 Intersectional Representation

| Group | Representation | Outcome Rate |
|------|---------------|--------------|
| Female + low income | 12% | 38% approval |
| Male + low income | 18% | 45% approval |

### Insight

Intersectional groups show **compounded disadvantage**, not visible in aggregate metrics.


## 2.3 Correlation & Proxy Analysis

| Feature | Correlation with Gender | Proxy Risk |
|--------|-------------------------|------------|
| Postcode | High | Strong |
| Employment type | Medium | Moderate |
| Credit history length | High | Strong |
| Industry sector | Medium | Moderate |

### Key Insight

Even without explicit gender input:

> The dataset encodes gender through structural proxies.


## 2.4 Label Quality Assessment

- Historical labels derived from prior underwriting decisions  
- Legacy decision policies embedded into training data  

### Finding:

- Labels reflect **historical human bias + structural inequality**
- Not purely objective financial outcomes


## 2.5 Bias Type Summary (PFT Output)

| Bias Type | Severity | Source |
|----------|--------|--------|
| Representation bias | High | Dataset imbalance |
| Proxy bias | High | Postcode, employment type |
| Label bias | High | Historical underwriting decisions |
| Intersectional bias | Medium-High | Compounded socioeconomic structure |


## PFT Conclusion

> The dataset encodes structural inequality through representation imbalance, proxy variables, and historical labeling effects.

# 3. Causal Fairness Toolkit (CFT)
## Structural Bias Discovery

## 3.1 Causal Structure Overview

Variables:

- A = Gender  
- M = Mediators (credit history, employment stability)  
- P = Proxies (postcode, employment type)  
- X = Features  
- Y = Loan approval  

## 3.2 Causal Graph (Simplified)

A → M → X → Y  
A → P → X → Y  
M → X → Y  
P → X → Y  

No explicit A → Y edge exists.


## 3.3 Mechanism Identification

### A. Direct Influence

- No explicit gender input
- No rule-based discrimination detected

### Finding:
> No direct bias mechanism, but strong indirect propagation


### B. Indirect (Mediated) Influence

- A → credit history length → credit score → Y  
- A → employment stability → income → Y  

### Insight:

Women show systematically shorter credit histories due to structural inequality.


### C. Proxy Influence

- Postcode strongly encodes socioeconomic segregation  
- Employment type reflects labor market segmentation  

### Insight:

> Gender is indirectly encoded through correlated social structure variables.


## 3.4 Causal Interpretation

- Bias is **not explicit**
- Bias is **structural and propagated through valid features**


# 4. Counterfactual Fairness Analysis


## 4.1 Factual Scenario

- Gender: Female  
- Credit score: 710  
- Income: stable mid-range  
- Employment: part-time professional  
- Outcome: Reject (p = 0.52)


## 4.2 Counterfactual Intervention

Change:

- A: Female → Male  

Hold constant:

- income  
- macroeconomic conditions  
- application context  


## 4.3 Counterfactual Result

- Male counterfactual outcome: Approve (p = 0.58)


## 4.4 Counterfactual Effect

ΔY = +0.06 approval increase


## 4.5 Path-Specific Effects

| Pathway | Effect |
|----------|-------|
| A → M → Y | High |
| A → P → Y | Medium |
| Direct A → Y | None |


## Interpretation

> Even identical financial profiles produce different outcomes under counterfactual identity change.

# 5. In-Processing Fairness Interventions

## 5.1 Objectives

- Remove latent gender signal from representation space  
- Reduce dependency on proxy variables  
- Constrain model learning behavior  

## 5.2 Techniques Applied

### 1. Adversarial Debiasing

- Model trained to predict Y while adversary removes gender signal from latent space

### 2. Equalized Odds Constraint

- Enforced equal TPR and FPR across groups

### 3. Reweighted Loss Function

- Increased weight for underrepresented female group samples


## 5.3 Results

| Metric | Before | After |
|------|--------|------|
| Gender leakage in embeddings | High | Low |
| TPR disparity | 0.09 | 0.03 |
| Proxy dependency | High | Reduced |


## 5.4 Key Insight

> In-processing does not remove bias—it reshapes how the model encodes it.


# 6. Post-Processing Fairness Toolkit (PPFT)

## Decision-Layer Correction


## 6.1 Output Bias Diagnosis

| Metric | Male | Female |
|------|------|--------|
| Approval rate | 67% | 63% |
| Calibration error | Low | Medium |
| FNR | 12% | 21% |


## 6.2 Selected Fairness Constraint

- Primary: Equal Opportunity  
- Secondary: Calibration consistency  


## 6.3 Interventions Applied

### 1. Threshold Calibration

- Group-specific thresholds optimized on validation set

### 2. Score Recalibration

- Platt scaling applied per subgroup

### 3. Decision Adjustment

- Boundary smoothing to reduce sharp disparities


## 6.4 Post-Processing Effect

| Group | Before | After |
|------|--------|------|
| Male approval rate | 67% | 65.2% |
| Female approval rate | 63% | 64.5% |
| Gap | 4% | <1% |


# 7. Integrated Results

## 7.1 Fairness Outcomes

- Approval gap: **4% → <1%**
- Equal opportunity gap: significantly reduced
- Proxy leakage: minimized
- Intersectional disparity: reduced but monitored


## 7.2 Model Performance

- AUC drop: <2%  
- Precision: stable  
- Recall: marginal decrease (<1.5%)  

## 7.3 Stability

- 5 retraining cycles tested  
- No fairness regression observed  
- System remained stable under distribution shifts  


# 8. Key System Insight

This system demonstrates a critical principle:

> Fairness failures are rarely caused by a single model. They are caused by causal structure embedded in data, features, and decision policies.

# 9. Final Outcome

### Before Intervention
- 4% gender approval gap  
- persistent proxy leakage  
- structural bias propagation  

### After Intervention
- <1% gap  
- controlled proxy influence  
- stabilized fairness constraints  

# 10. Core Takeaway

> You cannot fix fairness by tuning models alone.  
> You must correct the system that generates the data, the features, and the decisions.

# End of Case Study
