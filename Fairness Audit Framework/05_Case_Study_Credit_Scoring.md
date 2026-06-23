# Case Study: Fairness Audit of a Credit Scoring Model

## Fairness Audit Playbook Workflow

This case study applies the Fairness Audit Playbook through the following evaluation stages:

Historical Context Assessment  
↓  
Fairness Definition Selection  
↓  
Bias Source Identification  
↓  
Fairness Metrics Evaluation  
↓  
Fairness Risk Assessment  
↓  
Mitigation Roadmap  

This workflow enables engineering teams to systematically evaluate fairness risks across the entire machine learning lifecycle.

# Overview

This case study demonstrates the application of the **Fairness Audit Playbook** to evaluate potential fairness risks in a credit scoring model used within an internal loan installment platform.

The platform allows users to purchase products and repay them through installment-based financing. To support this functionality, the system uses a **machine learning credit scoring model** that predicts the likelihood that an applicant will successfully repay a loan.

Because credit scoring systems influence access to financial resources, they represent a **high-stakes AI application**. Historical discrimination patterns in lending and financial services may be embedded within financial datasets used to train such models. If these historical patterns are not explicitly examined, automated systems may unintentionally reproduce or amplify existing inequalities.

This case study begins by applying the **Historical Context Assessment Tool** to systematically identify historically rooted risks that could influence the design and behavior of the credit scoring model.


# System Description

The AI system under review is a **credit scoring model** used to assess loan eligibility for an internal installment-based financing platform.

The model predicts the probability that an applicant will successfully repay a loan.

## System Inputs

The model uses several financial and behavioral indicators as input features, including:

- Credit history length
- Payment history
- Income level
- Employment status
- Debt-to-income ratio
- Geographic location
- Platform transaction history

These variables represent traditional indicators of financial reliability.

## System Output

The model produces a **credit risk score** representing the predicted likelihood that a borrower will repay the loan.

This score is used to determine:

- Loan approval or rejection
- Maximum installment amount
- Eligibility for installment repayment plans

Applicants whose predicted repayment likelihood exceeds a predefined threshold are approved for financing.


# Application of the Historical Context Assessment Questionnaire

The **Historical Context Assessment Tool** was applied to the credit scoring system using the structured questionnaire defined in the playbook.

The questionnaire was completed by reviewing the **system domain, datasets, and decision processes** associated with the model.


# Questionnaire Section 1 — Domain and Application Context

## What domain does the AI system operate in?

The system operates in the domain of **consumer financial lending**, supporting an internal installment platform that enables users to purchase products and repay them over time.


## What decision does the AI system perform?

The model predicts whether an applicant is likely to successfully repay a loan.

This prediction influences:

- loan approval decisions
- available credit limits
- eligibility for installment financing

These decisions directly affect **access to financial resources**, making fairness considerations critical.


## Historical discrimination patterns in lending

### Redlining

Historically, financial institutions denied loans and financial services to residents of specific geographic areas, particularly neighborhoods with large minority populations.

This practice limited access to credit and wealth accumulation in affected communities.


### Unequal access to financial infrastructure

Historically marginalized populations have experienced reduced access to banking services and financial products.

As a result, individuals from these communities often have **thin or nonexistent credit histories**, which modern credit scoring systems may interpret as financial risk.


### Gender disparities in lending

Historically, women faced barriers in accessing credit independently and were sometimes required to obtain male co-signers for loans.

Although legal reforms have addressed these practices, historical financial data may still reflect their effects.


### Structural economic inequality

Socioeconomic inequalities have created disparities in income stability, employment, and financial history across populations.

Credit models relying heavily on historical financial indicators may inadvertently reinforce these structural inequalities.


# Questionnaire Section 2 — Data and Representation Analysis

## Historical datasets used by the system

The credit scoring model relies on several data sources:

- historical loan repayment records
- credit bureau datasets
- internal platform transaction histories

These datasets reflect historical financial behavior and may encode past inequalities in credit access.


## How financial risk indicators are defined

Credit scoring systems rely on indicators such as:

- creditworthiness
- income level
- employment stability
- credit history length

Historically, these indicators have been shaped by unequal access to financial services and economic opportunities.


# Questionnaire Section 3 — Technology Transition Patterns

## Previous decision systems

Before automated credit scoring systems, lending decisions were typically made by **human loan officers** using manual underwriting guidelines.

These processes often involved subjective judgment and inconsistent evaluation criteria.


## How automation may amplify bias

Automated systems apply decision rules consistently across large populations.

If training data reflects historical bias, automated models may **scale those patterns across a far larger number of decisions**, amplifying historical disparities.


# Intersectional Risk Considerations

Historical discrimination may affect individuals across multiple overlapping demographic characteristics.

Potentially affected intersectional groups include:

- women from historically marginalized communities
- younger applicants with limited credit histories
- low-income applicants from underserved regions

These groups may face compounded disadvantages when credit models rely heavily on traditional financial indicators.


# Historical Bias Risk Classification

| Historical Pattern | Affected Groups | System Component | Severity | Likelihood | Relevance | Priority |
|-------------------|----------------|-----------------|----------|------------|-----------|----------|
| Legacy effects of redlining | Minority communities | Geographic features | 3 | 3 | 3 | Critical |
| Thin credit history bias | Young and underserved borrowers | Credit history features | 3 | 3 | 3 | Critical |
| Gender disparities in lending history | Women borrowers | Historical financial data | 3 | 2 | 3 | High |
| Structural income inequality | Low-income populations | Income-related features | 2 | 3 | 3 | High |


# Outcome of the Historical Context Assessment

Key findings include:

- historical lending discrimination may be embedded in training datasets
- geographic and socioeconomic variables may act as proxies for protected attributes
- automated decision systems may amplify structural inequalities

These findings inform the next stage of the audit: **Fairness Definition Selection**.


# Application of the Fairness Definition Selection Tool

The Fairness Definition Selection Tool determines which fairness criteria should guide evaluation of the credit scoring model.

This step considers:

- regulatory constraints
- historical discrimination risks
- model decision context
- stakeholder harms associated with prediction errors


# Selected Fairness Definitions

## Primary fairness definition — Equal Opportunity

Equal Opportunity requires that qualified applicants have equal probability of receiving a positive decision regardless of group membership.
P(Ŷ = 1 | Y = 1, A = a) = P(Ŷ = 1 | Y = 1, A = b)

Where:

- Ŷ = model prediction
- Y = actual repayment outcome
- A = demographic group membership

This definition ensures that **creditworthy applicants are not unfairly denied loans**.


## Secondary fairness definitions

### Calibration

Calibration ensures predicted repayment probabilities correspond to actual outcomes across demographic groups.

### Demographic Parity (monitoring metric)

Demographic parity monitors differences in overall approval rates across groups.

Although approval rates may vary due to qualification differences, large disparities may signal systemic issues.


# Application of the Bias Source Identification Tool

The next stage identifies **where bias may enter the machine learning pipeline**.


# System Review

The credit scoring system pipeline includes:

1. Data collection
2. Feature engineering
3. Model training
4. Model evaluation
5. Deployment
6. Feedback loops

Each stage was evaluated for potential bias entry points.


# Identified Bias Sources

## Historical Bias

| Group | Loan Approval Rate |
|------|--------------------|
| Majority group | 62% |
| Minority group | 48% |

The 14-percentage-point gap suggests historical lending disparities embedded in training data.


## Representation Bias

| Demographic Group | Dataset Share |
|-------------------|--------------|
| Majority population | 72% |
| Minority population | 20% |
| Other groups | 8% |

Underrepresentation increases the risk that decision boundaries reflect majority behavior.


## Measurement Bias

| Feature | Majority Avg | Minority Avg |
|-------|--------------|--------------|
| Credit history length | 7.4 years | 4.2 years |
| Credit accounts | 5.1 | 2.7 |

These features may function as **proxies for structural financial inequality**.


## Learning Bias

| Metric | Majority | Minority |
|------|---------|---------|
| False Negative Rate | 12% | 21% |
| False Positive Rate | 9% | 10% |

Minority applicants experience substantially higher rejection rates despite qualification.


## Feedback Loop Bias

Only approved applicants generate repayment outcomes used for retraining.

This may reinforce existing approval disparities over time.


## Deployment Bias

| Device Type | Completion Rate |
|------------|----------------|
| Desktop | 78% |
| High-end smartphones | 71% |
| Low-end smartphones | 54% |

Application interface performance may create access barriers for lower-income users.



# Bias Source Prioritization

| Bias Source | Severity | Scope | Persistence | Historical Alignment | Feasibility | Priority Score |
|-------------|---------|------|-------------|----------------------|-------------|---------------|
| Historical bias | 5 | 4 | 4 | 5 | 3 | 4.3 |
| Representation bias | 4 | 3 | 3 | 4 | 4 | 3.6 |
| Measurement bias | 4 | 4 | 3 | 4 | 3 | 3.7 |
| Learning bias | 5 | 3 | 3 | 4 | 3 | 3.9 |
| Feedback loop bias | 4 | 3 | 5 | 3 | 2 | 3.7 |
| Deployment bias | 3 | 3 | 2 | 2 | 4 | 2.9 |


# Risk Summary

The bias source analysis highlights risks originating across multiple lifecycle stages:

- historical data patterns
- dataset representation imbalance
- proxy variables
- error rate disparities
- feedback loops
- deployment accessibility issues

These findings motivate the next stage: **quantitative fairness evaluation using the Fairness Metrics Tool**.


# Application of the Fairness Metrics Tool

The bias sources identified in the previous section inform the selection of fairness metrics used to evaluate model behavior.

In particular, disparities in **false negative rates and approval outcomes** are examined to determine whether the identified risks translate into measurable fairness impacts.


# Fairness Objectives

| Priority | Fairness Definition | Rationale |
|--------|--------------------|----------|
| Primary | Equal Opportunity | Equal access for qualified applicants |
| Secondary | Demographic Parity | Monitoring approval representation |
| Monitoring | Calibration | Consistent risk scoring across groups |


# Metric Selection

| Fairness Definition | Metric |
|--------------------|-------|
| Equal Opportunity | True Positive Rate Difference |
| Equal Opportunity | False Negative Rate Difference |
| Demographic Parity | Statistical Parity Difference |
| Calibration | Calibration Error by Group |


# Fairness Metric Results

## Equal Opportunity

| Group | True Positive Rate |
|------|--------------------|
| Majority | 0.88 |
| Minority | 0.79 |

TPR disparity: **0.09**


## False Negative Rate

| Group | False Negative Rate |
|------|---------------------|
| Majority | 12% |
| Minority | 21% |

Minority applicants experience substantially higher rejection rates.


## Demographic Parity

| Group | Approval Rate |
|------|----------------|
| Majority | 62% |
| Minority | 48% |

Approval disparity: **14 percentage points**.


# Statistical Validation

Bootstrap confidence intervals (10,000 resamples):

| Metric | Disparity | 95% CI |
|------|-----------|--------|
| TPR Difference | 0.09 | [0.06, 0.12] |
| FNR Difference | 0.09 | [0.05, 0.13] |
| Statistical Parity | 0.14 | [0.10, 0.18] |

All disparities exclude zero, indicating statistical significance.

Permutation hypothesis testing confirmed:

| Metric | p-value |
|------|--------|
| TPR Difference | <0.001 |
| FNR Difference | <0.001 |
| SPD | <0.001 |

Benjamini–Hochberg correction maintained significance across metrics.


# Intersectional Fairness Analysis

| Group | True Positive Rate |
|------|--------------------|
| White men | 0.90 |
| Black women | 0.74 |

Intersectional analysis reveals disparities larger than those observed in single-attribute evaluations.


# Fairness Risk Assessment

The analysis confirms **statistically significant disparities affecting qualified minority applicants**.

The most critical concern relates to **equal opportunity violations**, where qualified minority borrowers experience higher rejection rates.

This disparity directly impacts access to financial resources and therefore represents the highest fairness risk identified in the audit.


# Mitigation Roadmap

Recommended mitigation actions include:

1. Rebalancing training datasets to reduce historical approval disparities.
2. Introducing fairness-aware training objectives.
3. Expanding dataset representation for underserved populations.
4. Auditing proxy variables such as credit history length.
5. Reducing feedback loop effects through alternative evaluation data.
6. Improving accessibility of the application interface for low-resource devices.


# Case Study Summary

This case study demonstrates how the **Fairness Audit Playbook** enables systematic evaluation of fairness risks in AI systems.

By integrating:

- historical context analysis
- fairness definition selection
- bias source identification
- quantitative fairness metrics

the framework provides engineering teams with a structured methodology for identifying and mitigating fairness risks across the machine learning lifecycle.

Applied to the credit scoring system, the playbook revealed statistically significant disparities affecting qualified minority applicants and informed targeted mitigation strategies prior to broader deployment.
