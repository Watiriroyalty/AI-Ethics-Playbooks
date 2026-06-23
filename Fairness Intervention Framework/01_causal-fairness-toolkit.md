# Causal Fairness Toolkit (CFT)

## Objective

Understand *why* bias exists before attempting to fix it.

Fairness issues in machine learning systems are rarely accidental, they emerge from underlying **causal relationships** in data, historical processes, and model design.

This toolkit helps engineers:

- identify where bias originates  
- determine whether it is acceptable or discriminatory  
- map bias → targeted intervention  


## Core Question

**Where is the bias coming from, and is it acceptable?**


## Why Causal Fairness Matters

Statistical methods tell you **what is happening**.

Causal methods tell you:

> **should this be happening?**

Without causal reasoning, teams risk:

- removing useful signals (over-correction)  
- preserving harmful bias (under-correction)  

# Analysis Template (Pre-Modeling Stage)

## Purpose

This document provides a structured analysis template for early-stage fairness assessment in ML systems.

It is designed to help engineers:

- Identify potential discrimination mechanisms  
- Formulate counterfactual fairness questions  
- Sketch initial causal structures based on domain knowledge  

This is a pre-formal modeling tool. Outputs here will be refined and mathematically formalized in later parts of the methodology.

## How to Use This Template

For each section:

- Answer all guided questions  
- Document assumptions explicitly  
- Flag uncertainty instead of forcing precision  
- Avoid assuming causality unless justified by domain knowledge or evidence  

# 1. Discrimination Mechanism Identification Framework

## Objective

Identify how protected attributes may influence outcomes through different causal pathways.

## 1.1 Protected Attribute Definition

- What protected attributes are relevant in this system?  
  *(e.g., gender, race, age, socioeconomic status)*  

- Are there intersectional groups that must be considered?  

## 1.2 Decision Context

- What decision is being made?  
  *(e.g., loan approval, hiring, ranking, pricing)*  

- What is the model output used for operationally?  

## 1.3 Potential Mechanisms of Influence

### A. Direct Influence

Could protected attributes directly influence the decision outcome?

**Guiding questions:**
- Are there any explicit rules or historical labels tied to protected attributes?  
- Is the attribute encoded or indirectly exposed in the model?  

### B. Indirect Influence (Mediated Paths)

Through which variables could protected attributes influence outcomes indirectly?

**Guiding questions:**
- What variables are plausibly affected by protected attributes?  
- Are these variables socially or structurally shaped (e.g., income, education, employment history)?  

### C. Proxy Influence

Which variables might act as proxies for protected attributes?

**Guiding questions:**
- Which features are highly correlated with protected attributes?  
- Are there structural reasons for this correlation (e.g., geography, job type)?  

## 1.4 Mechanism Documentation Table

| Mechanism Type | Variable(s) | Evidence Type | Confidence | Notes |
|----------------|------------|---------------|------------|------|
| Direct         |            |               | High/Med/Low |      |
| Indirect       |            |               | High/Med/Low |      |
| Proxy          |            |               | High/Med/Low |      |

# 2. Counterfactual Formulation Template

## Objective

Define structured counterfactual queries to evaluate whether outcomes depend on protected attributes.


## 2.1 Base Scenario Definition

Document a real or typical case:

- Input features:  
- Protected attribute value:  
- Model prediction:  
- Decision outcome:  


## 2.2 Counterfactual Scenario Construction

### Counterfactual Change

- What attribute is being changed?  
  *(e.g., Male → Female)*  

### Stability Assumptions

- Which features are held constant?  
- Which features are allowed to change due to causal dependence?  

## 2.3 Counterfactual Question Template

> Would the decision change if the protected attribute were different, holding all else structurally consistent?

## 2.4 Counterfactual Documentation Table

| Scenario       | Protected Attribute | Prediction | Change Observed | Notes |
|----------------|-------------------|------------|-----------------|------|
| Base           |                   |            |                 |      |
| Counterfactual |                   |            | Yes/No          |      |

## 2.5 Fairness Interpretation Notes

- Is the change expected or unexpected given domain knowledge?  
- Does the change indicate potential bias or valid dependence?  
- What assumptions affect this interpretation?  

# 3. Initial Causal Diagram Approach (Preliminary DAG)

## Objective

Sketch a working causal hypothesis representing relationships between:

- Protected attributes (A)  
- Features (X)  
- Outcome (Y)
- 
## 3.1 Construction Guidelines

When drawing the diagram:

- Use arrows only for assumed causal influence  
- Do not confuse correlation with causation  
- Clearly label uncertain edges  

## 3.2 Node Types

- **A** — Protected Attributes  
- **X** — Features / Observables  
- **Y** — Outcome / Decision  

## 3.3 Edge Annotation System

- Solid arrow → high confidence causal assumption  
- Dashed arrow → uncertain or hypothesized relationship  
- Dotted arrow → proxy relationship suspected  

## 3.4 Documentation Template

For each edge:

| From | To | Type | Justification | Confidence |
|------|----|------|--------------|------------|
| A    | X  |      |              | High/Med/Low |

## 3.5 Assumption Logging

- What assumptions were required to build this graph?  
- Which assumptions are weakest?  
- What domain knowledge supports each causal claim?  

# 4. Integration with Later Methodology (Parts 2–4)

This preliminary toolkit serves as the input layer for formal causal modeling.

## 4.1 Interface with Part 2 (Formal Discrimination Modeling)

- Mechanism identification feeds into formal causal structure definition  
- Proxy and indirect paths are formalized into structural equations  

## 4.2 Interface with Part 3 (Counterfactual Analysis Framework)

- Counterfactual questions defined here become formal queries in SCMs  
- Stability assumptions guide intervention modeling  

## 4.3 Interface with Part 4 (Inference & Refinement)

- Preliminary DAG becomes a testable causal hypothesis  
- Uncertain edges are evaluated using formal inference methods  

## 4.4 Consistency Requirements

To ensure integration:

- Use consistent variable naming across all parts  
- Explicitly document assumptions at every stage  
- Avoid redefining mechanisms between parts unless justified  
- Maintain traceability from observation → hypothesis → formal model  

# 5. Key Output Artifacts

At the end of this stage, you should produce:

1. Discrimination Mechanism Map  
2. Counterfactual Scenario Set  
3. Preliminary Causal Diagram (Annotated DAG)  
4. Assumption Register  
5. Confidence-Weighted Mechanism Table  

# 6. Core Principle

This stage does not prove bias or fairness.

It establishes:

- where bias might originate  
- how it might propagate  
- what must be tested formally later  

## Final Insight

If you cannot clearly describe the causal story, you cannot reliably fix fairness.

This toolkit is about turning vague fairness concerns into structured, testable hypotheses before modeling begins.

# Part 3: Counterfactual Fairness Analysis Framework

## Objective

This component translates **structural causal models (Part 2)** into **counterfactual reasoning tools for fairness evaluation**.

It provides structured methods for:

- formulating counterfactual fairness questions  
- classifying causal pathways as fair or unfair  
- measuring counterfactual disparities  

This stage directly informs **which causal mechanisms require intervention** and how they should be addressed in later mitigation stages.

## Core Principle

> If Part 2 defines *how the system works*,  
> Part 3 tests *how the system would behave under alternate realities*.

# 1. Counterfactual Query Framework

## Objective

Formulate structured counterfactual questions that evaluate whether outcomes depend on protected attributes in unjustified ways.

## 1.1 Counterfactual Query Definition

A counterfactual query evaluates:

> What would the outcome be if the protected attribute were different, while preserving the causal structure?

## 1.2 Query Construction Template

Each counterfactual query must define:

### A. Factual Scenario

- Protected attribute: A = a  
- Features: X  
- Outcome: Y = y  

### B. Counterfactual Intervention

- Modify: A → A′  
- Keep fixed: non-descendants of A (as defined in DAG)  
- Allow to change: descendants of A (mediators, proxies)

### C. Counterfactual Question Form

> What would Y be if A = a′ instead of a, given the same structural context?

## 1.3 Variable Stability Rules

### Must remain constant:
- confounders (C)  
- independent features not causally affected by A  

### May change:
- mediators (M)  
- proxy variables (P)  
- downstream variables (Y’s parents in DAG)

## 1.4 Counterfactual Query Template

| Component | Specification |
|-----------|--------------|
| Factual A |  |
| Counterfactual A′ |  |
| Fixed Variables |  |
| Allowed Variation |  |
| Predicted Y (factual) |  |
| Predicted Y (counterfactual) |  |

## 1.5 Interpretation Guidelines

- Small or zero change → potential fairness stability  
- Large change → potential causal dependency on protected attribute  
- Context required before labeling as unfair  

# 2. Pathway Classification Framework

## Objective

Classify causal pathways from **A → Y** as fair or unfair based on structural, ethical, and domain considerations.

## 2.1 Pathway Definition

A pathway is a directed causal chain:

A → X₁ → X₂ → ... → Y

Each path represents a mechanism through which protected attributes influence outcomes.


## 2.2 Pathway Types

### A. Legitimate Pathways (Fair Influence)

- Causal influence is justified by domain relevance  
- Represents meaningful, job-relevant, or decision-relevant information  

Example:
- A → experience → performance → Y  

### B. Unfair Pathways (Discriminatory Influence)

- Influence lacks normative justification  
- Encodes structural or historical bias  

Example:
- A → neighborhood → credit score → Y  

### C. Ambiguous Pathways

- Causal validity exists but ethical status is uncertain  
- Requires stakeholder or legal interpretation  

## 2.3 Pathway Classification Criteria

Each pathway is evaluated using:

- causal validity (from Part 2 DAG)  
- domain relevance  
- ethical acceptability  
- stakeholder constraints  
- regulatory considerations  

## 2.4 Pathway Documentation Template

| Pathway | Type | Justification | Ethical Rationale | Confidence |
|----------|------|---------------|------------------|------------|
| A → M → Y |  |  |  | High/Med/Low |

## 2.5 Decision Rules

- If **normatively justified → keep pathway**  
- If **structurally biased → intervene**  
- If **uncertain → monitor and test sensitivity**  

## 2.6 Annotation Requirements

Each pathway must explicitly document:

- why it exists in the causal graph  
- why it is considered fair/unfair  
- what assumptions support this classification  

# 3. Counterfactual Evaluation Metrics

## Objective

Quantify fairness violations by comparing **factual vs counterfactual outcomes** across pathways.

## 3.1 Core Metric: Counterfactual Difference

ΔY = Y(A = a) − Y(A = a′)

Measures sensitivity of outcome to protected attribute changes.


## 3.2 Path-Specific Effect Measures

Isolate contribution of individual pathways:

- direct effect: A → Y  
- indirect effect: A → M → Y  
- proxy effect: A → P → Y  

## 3.3 Path-Specific Fairness Decomposition

Total effect:

TE = DE + IE + PE

Where:
- DE = direct effect  
- IE = indirect (mediated) effect  
- PE = proxy effect  

## 3.4 Fairness Violation Metrics

### A. Absolute Disparity

|Y_factual − Y_counterfactual|

### B. Path-Specific Sensitivity

Measures contribution of each causal pathway to outcome variation.

### C. Structural Fairness Gap

Difference between:

- observed model behavior  
- behavior under constrained (fair) pathways  

## 3.5 Evaluation Template

| Metric | Value | Interpretation | Threshold |
|--------|------|----------------|----------|
| Counterfactual ΔY |  |  |  |
| Direct Effect |  |  |  |
| Proxy Effect |  |  |  |

## 3.6 Interpretation Guidelines

- High direct effect → likely structural discrimination  
- High proxy effect → encoding bias through correlated features  
- High indirect effect → systemic or historical bias  

# 4. Integration with Causal Analysis Methodology

## 4.1 Interface with Part 2 (Structural Models)

- Uses DAG structure to define valid interventions  
- Uses structural equations for counterfactual simulation  

## 4.2 Interface with Intervention Design

Counterfactual results directly inform:

- pre-processing changes (data transformation)  
- in-processing constraints (model restrictions)  
- post-processing adjustments (decision calibration)  

## 4.3 Intervention Mapping Rule

| Finding | Recommended Action |
|----------|------------------|
| Direct unfair effect | constrain or remove feature influence |
| Proxy effect | de-bias or transform feature |
| Mediated bias | evaluate mediator legitimacy |

## 4.4 Consistency Requirements

To ensure integration:

- keep variable naming consistent across Parts 1–4  
- maintain explicit pathway classification records  
- document all counterfactual assumptions  
- avoid redefining causal structure without justification  

# 5. Outputs of This Component

At completion, produce:

1. Counterfactual Query Set  
2. Pathway Classification Map  
3. Counterfactual Evaluation Report  
4. Path-Specific Effect Decomposition  
5. Intervention Candidate List  

# Final Insight

This stage answers a critical question:

> Not just whether bias exists — but *how it would change under alternative realities*.

## Key Transition Statement

Part 3 identifies *where fairness breaks under counterfactual conditions*.

Part 4 will determine *what interventions actually hold under causal inference and implementation constraints*.

# Part 4: Practical Causal Inference & Uncertainty-Aware Fairness

## Objective

This component translates the **idealized causal structures (Part 2)** and **counterfactual reasoning (Part 3)** into **practical, data-constrained inference procedures**.

It focuses on:

- selecting appropriate causal inference methods under real-world constraints  
- quantifying uncertainty from incomplete or imperfect data  
- communicating causal conclusions responsibly  

This is the **execution layer** of the Causal Fairness Toolkit.

## Core Principle

> If Part 3 asks *“what should happen under alternate realities?”*,  
> Part 4 asks *“what can we reliably conclude from imperfect data?”*

# 1. Method Selection Framework

## Objective

Select appropriate causal inference methods based on:

- data availability  
- causal knowledge strength  
- structural assumptions  
- experimental constraints  

## 1.1 Method Selection Decision Inputs

Each analysis must begin with:

- Data type (cross-sectional / longitudinal / experimental / quasi-experimental)  
- Availability of covariates  
- Presence of policy or threshold rules  
- Existence of valid instruments  
- Strength of causal model from Part 2  

## 1.2 Method Selection Guide

### A. Matching Methods / Propensity Scores

Use when:
- rich covariate data exists  
- treatment assignment is not randomized  

Assumptions:
- conditional ignorability (no unmeasured confounding)  
- overlap / common support  

Limitations:
- sensitive to hidden confounders  
- depends heavily on feature quality  

### B. Instrumental Variables (IV)

Use when:
- valid instrument exists affecting treatment but not outcome directly  

Assumptions:
- exclusion restriction  
- instrument relevance  
- independence from unobserved confounders  

Risk:
- invalid instruments lead to biased causal estimates  

### C. Regression Discontinuity Design (RDD)

Use when:
- treatment assigned via threshold rule  

Assumptions:
- continuity around cutoff  
- no manipulation of assignment variable  

Strength:
- strong local causal identification  

### D. Difference-in-Differences (DiD)

Use when:
- longitudinal data exists  
- policy or intervention changes over time  

Assumptions:
- parallel trends  
- no simultaneous confounding shocks  

### E. Multi-Method Triangulation

Use when:
- no single method is fully reliable  

Approach:
- compare results across methods  
- evaluate consistency of causal estimates  

## 1.3 Method Selection Template

| Data Condition | Available Knowledge | Selected Method | Justification | Assumptions |
|----------------|--------------------|----------------|--------------|-------------|
|                |                    |                |              |             |

## 1.4 Selection Principle

> Method choice is not statistical preference — it is **assumption management under constraint**.

# 2. Sensitivity Analysis Framework

## Objective

Quantify how robust causal conclusions are to **unmeasured confounding and structural uncertainty**.

## 2.1 Core Problem

Even correct models may fail due to:

- missing variables  
- hidden confounders  
- incorrect causal structure  
- measurement error  

## 2.2 Sensitivity Analysis Approaches

### A. E-Value Analysis

Measures:

> How strong an unmeasured confounder must be to invalidate the causal conclusion.

### B. Bounding Approaches

Instead of point estimates:

- compute upper/lower bounds of causal effects  
- reflect uncertainty explicitly  

### C. Alternative Model Testing

- evaluate multiple plausible DAGs  
- compare stability of causal conclusions  

### D. Perturbation Testing

- vary assumptions systematically  
- observe effect on causal estimates  

## 2.3 Sensitivity Analysis Template

| Scenario | Assumption Varied | Effect Change | Robustness Level |
|----------|------------------|--------------|------------------|
| Base | None |  |  |
| Test 1 | Hidden confounder added |  |  |

## 2.4 Interpretation Guidelines

- stable results → high confidence in causal claim  
- unstable results → conclusions are assumption-dependent  
- highly sensitive models → require caution in deployment  

## 2.5 Key Principle

> A causal claim without sensitivity analysis is a point estimate without epistemic grounding.

# 3. Uncertainty Communication Framework

## Objective

Translate causal inference results into **transparent, decision-relevant communication formats**.

## 3.1 Types of Uncertainty

### A. Statistical Uncertainty
- sampling variation  
- estimation error  

### B. Structural Uncertainty
- unknown DAG correctness  
- alternative causal pathways  

### C. Measurement Uncertainty
- noisy or biased data  
- proxy feature distortion  

## 3.2 Output Representation Principles

### Avoid:
- single point causal estimates presented as definitive truth  

### Prefer:
- bounded estimates  
- ranges of effects  
- scenario-based outcomes  

## 3.3 Communication Template

| Effect Type | Estimate Range | Confidence | Key Assumption Risk |
|-------------|---------------|------------|---------------------|
| Total Effect |  | High/Med/Low |  |

## 3.4 Stakeholder Communication Guidelines

### Technical audiences:
- DAGs + sensitivity metrics + bounds  

### Non-technical audiences:
- scenario comparisons  
- directional effects  
- uncertainty bands  

## 3.5 Visualization Guidance

- shaded confidence regions instead of point lines  
- multiple model overlays for comparison  
- scenario-based outcome charts  

## 3.6 Core Principle

> The goal is not to eliminate uncertainty — it is to make uncertainty visible and actionable.

# 4. Integration with Causal Fairness Methodology

## 4.1 Interface with Part 2 (Structural Models)

- Structural equations provide estimation targets  
- DAG structure determines valid adjustment sets  

## 4.2 Interface with Part 3 (Counterfactual Analysis)

- Approximate counterfactuals using observational methods  
- Validate counterfactual stability under uncertainty  

## 4.3 Interface with Intervention Design

- intervention decisions must account for estimation uncertainty  
- weak causal evidence → conservative interventions  
- strong causal evidence → targeted interventions  

## 4.4 Consistency Requirements

Across all parts:

- maintain consistent variable definitions (A, X, Y, M, C, P)  
- document all causal assumptions explicitly  
- distinguish between estimated effects and assumed structures  
- avoid overstating certainty in causal claims  


# 5. Outputs of This Component

At completion, produce:

1. Causal method selection report  
2. Sensitivity analysis results  
3. Uncertainty bounds on causal effects  
4. Communication-ready fairness summaries  
5. Assumption risk register  

# Final Insight

This stage acknowledges a core reality:

> Causal fairness is never fully observable — only inferable under constraints.

## Key Transition Statement

Part 4 closes the loop between **theory and reality**.

Together with Parts 1–3, it completes a full system:

- Part 1 → identify mechanisms  
- Part 2 → formalize structure  
- Part 3 → test counterfactual fairness  
- Part 4 → infer under uncertainty and real-world constraints
