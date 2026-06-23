# Pre-processing Fairness Toolkit (PFT)

## Data Auditing & Bias Diagnostics Framework

## Objective

Understand *where bias exists in the data and how it manifests* before applying pre-processing interventions.

Bias in machine learning systems is often embedded in:

* **data representation**
* **feature relationships**
* **label generation processes**

This toolkit helps engineers:

* identify **data-level bias patterns**
* quantify their **severity and structure**
* map bias → **targeted pre-processing interventions**

## Core Question

> **Where is bias present in the data, and what type of intervention does it require?**

## Why Data Auditing Matters

Model fairness interventions fail when applied blindly.

Data auditing ensures:

* interventions target the **actual source of bias**
* proxy discrimination is not overlooked
* label bias is not mistaken for feature bias

Without auditing, teams risk:

* fixing the wrong problem
* introducing new bias through overcorrection
* masking intersectional disparities

# Analysis Template

## Purpose

This template provides a structured methodology for **fairness-aware data auditing before model training**.

It is designed to help engineers:

* detect representation imbalances
* uncover proxy relationships
* evaluate label integrity
* establish fairness baselines

## How to Use This Template

For each section:

* answer all guiding questions
* document assumptions explicitly
* flag uncertainty where present
* avoid interpreting correlation as causation

# 1. Multidimensional Representation Analysis Framework

## Objective

Identify **who is underrepresented, overrepresented, or unevenly distributed**, including intersectional groups.

## 1.1 Protected Attribute Definition

* What protected attributes exist in the dataset?
* Are intersectional groups relevant?
* Are there missing or unobserved sensitive attributes?

## 1.2 Reference Population Comparison

* What reference population is appropriate?
  *(e.g., census, domain-specific benchmark)*

* How does dataset distribution compare to reference?

## 1.3 Representation Gap Analysis

### Guiding Questions

* Which groups are underrepresented or overrepresented?
* Are there intersectional groups with very low representation?
* Are certain groups missing entirely?

## 1.4 Outcome Distribution Analysis

* How are outcomes distributed across groups?
* Are certain groups disproportionately assigned negative or positive outcomes?

## 1.5 Temporal Representation Analysis

* Does representation change over time?
* Are certain groups becoming more/less represented?
* Could temporal drift introduce bias?

## 1.6 Representation Documentation Template

| Attribute | Group | Dataset % | Reference % | Gap | Flag |
| --------- | ----- | --------- | ----------- | --- | ---- |

### Intersectional Analysis

| Group Combination | Dataset % | Outcome Rate | Flag |
| ----------------- | --------- | ------------ | ---- |

## 1.7 Interpretation Guidelines

* Underrepresentation → missing signal → **reweighting / oversampling**
* Overrepresentation → dominance bias → **undersampling**
* Sparse intersections → hidden bias risk → **synthetic augmentation**
* Temporal drift → instability → **time-aware rebalancing**

# 2. Correlation & Association Analysis Framework

## Objective

Identify **direct, indirect, and proxy relationships** involving protected attributes.

## 2.1 Direct Correlation Analysis

* Are protected attributes directly correlated with outcomes?
* Does this indicate potential discrimination?

## 2.2 Feature Correlation Analysis

* Which features are correlated with protected attributes?
* Could these features act as proxies?

## 2.3 Higher-Order Association Analysis

* Are there interaction effects between features and protected attributes?
* Do conditional dependencies exist?

## 2.4 Stability Analysis

* Do correlations hold across:

  * subgroups?
  * time periods?

## 2.5 Correlation Documentation Template

| Feature | Correlation | MI Score | Proxy Risk | Stability | Action |
| ------- | ----------- | -------- | ---------- | --------- | ------ |


## 2.6 Interpretation Guidelines

* High correlation → proxy risk → **feature transformation**
* Non-linear association → hidden bias → **encoding adjustment**
* Conditional dependence → indirect discrimination → **representation learning**
* Instability → unreliable feature → **feature removal or monitoring**

# 3. Label Quality Assessment Framework

## Objective

Detect **bias embedded in labels**, including historical and annotator bias.

## 3.1 Historical Bias Analysis

* How were labels generated?
* Were past decisions biased or discriminatory?

## 3.2 Annotator Agreement Analysis

* Do annotators agree across demographic groups?
* Are disagreements group-specific?

## 3.3 External Validation

* Can labels be validated against:

  * benchmarks?
  * expert judgment?

## 3.4 Counterfactual Label Testing

* Would labels change if protected attributes were different?
* Does label assignment depend on protected attributes?

## 3.5 Label Assessment Template

| Group | Positive Rate | Annotator Agreement | Counterfactual Sensitivity | Flag |
| ----- | ------------- | ------------------- | -------------------------- | ---- |

## 3.6 Interpretation Guidelines

* Label disparity → historical bias → **relabeling**
* Low agreement → subjectivity → **label smoothing**
* Counterfactual sensitivity → discrimination → **counterfactual labeling**

# 4. Fairness Baseline Metrics Framework

## Objective

Quantify **bias severity and prioritize interventions**.

## 4.1 Metric Selection

* Which fairness definitions apply?

  * demographic parity
  * equal opportunity
  * disparate impact

## 4.2 Metric Calculation

* Compute metrics across:

  * protected groups
  * intersectional groups

## 4.3 Statistical Significance

* Are observed disparities statistically meaningful?
* What are the confidence intervals?

## 4.4 Severity Prioritization

* Which disparities are largest?
* Which affect critical groups?

## 4.5 Metrics Template

| Metric | Group A | Group B | Difference | CI | Severity |
| ------ | ------- | ------- | ---------- | -- | -------- |

## 4.6 Interpretation Guidelines

* Large disparity → high priority intervention
* Intersectional disparity → targeted intervention
* Insignificant disparity → monitor only

# 5. Intersectional Bias Analysis Layer

## Objective

Ensure fairness across **combined demographic identities**, not just individual attributes.

## 5.1 Required Checks

* Representation across intersections
* Correlation patterns per subgroup
* Label quality per subgroup
* Fairness metrics per subgroup

## 5.2 Intersectional Template

| Group Combination | Representation | Outcome Rate | Metric Value | Flag |
| ----------------- | -------------- | ------------ | ------------ | ---- |

## 5.3 Interpretation Guidelines

* Aggregate fairness can hide subgroup harm
* Prioritize **worst-off intersectional group**
* Ensure interventions address subgroup disparities

# 6. Bias Pattern → Intervention Mapping

## Objective

Translate audit findings into **clear engineering actions**

## Mapping Template

| Bias Type           | Detection Signal    | Recommended Intervention |
| ------------------- | ------------------- | ------------------------ |
| Representation bias | Group imbalance     | Reweighting / Resampling |
| Proxy bias          | Feature correlation | Feature transformation   |
| Label bias          | Outcome disparity   | Relabeling / Smoothing   |
| Intersectional bias | Sparse subgroup     | Synthetic generation     |

# 7. Integration with Strategy Selector

## Inputs to Selector

* bias type
* severity level
* affected groups
* confidence level

## Decision Role

This component determines:

> **Which intervention class should be applied and why**

* representation issues → sampling strategies
* correlation issues → feature transformations
* label issues → label correction

# 8. Key Output Artifacts

At the end of this stage, produce:

1. Representation Analysis Report
2. Correlation & Proxy Risk Map
3. Label Quality Assessment Report
4. Fairness Metrics Baseline Dashboard
5. Intersectional Bias Report
6. Bias → Intervention Mapping Table

# Core Principle

This stage does not fix bias.

It establishes:

* where bias exists
* how it manifests
* what type of intervention is appropriate

## Final Insight

If you misdiagnose the source of bias:

* reweighting won’t fix proxy discrimination
* feature transformation won’t fix label bias
* better models won’t fix broken data

> **Fairness interventions only work when they target the correct failure mode.**
