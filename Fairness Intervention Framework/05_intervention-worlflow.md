# Intervention Workflows

## Objective
Translate fairness analysis into a structured decision-making process for selecting and applying interventions across the machine learning lifecycle.

This workflow connects causal understanding to practical implementation across pre-processing, in-processing, and post-processing stages.

## Core Idea

Fairness interventions should not be chosen arbitrarily.

They should follow a structured flow:

1. Identify where bias originates  
2. Classify the type of bias  
3. Map it to the correct intervention layer  
4. Apply targeted fixes without breaking model utility  

## End-to-End Workflow

### Step 1: Detect Fairness Issue

Start by identifying a disparity in model behavior.

**Engineer Action:**
- Compare outcomes across protected groups
- Look for:
  - Selection rate differences
  - Performance gaps
  - Error rate disparities

**Output:**
- A documented fairness concern (what is unequal and where)

### Step 2: Diagnose Causal Source (Use Causal Fairness Toolkit)

Before fixing anything, determine *why* the bias exists.

**Engineer Action:**
- Build or review causal graph (DAG)
- Identify:
  - Direct paths
  - Proxy paths
  - Mediator paths
- Run counterfactual checks

**Output:**
- Classified bias source:
  - Direct discrimination
  - Proxy discrimination
  - Mediator-driven disparity
  - Output-level disparity

### Step 3: Map to Intervention Layer

Once the source is known, select the correct intervention stage.

#### Decision Mapping

- **Data-level issue → Pre-processing**
- **Learning-level issue → In-processing**
- **Output-level issue → Post-processing**

### Step 4: Select Intervention Strategy

#### Pre-processing Interventions (Fix the Data)
Use when bias originates in dataset structure.

**Examples:**
- Reweighting samples
- Resampling minority groups
- Removing or transforming proxy variables
- Feature decorrelation

**Engineer Action:**
- Modify dataset before training
- Re-check distribution balance

#### In-processing Interventions (Fix the Model)
Use when bias is embedded in learning process.

**Examples:**
- Fairness constraints in loss function
- Adversarial debiasing
- Regularization against sensitive feature dependence

**Engineer Action:**
- Modify training objective
- Retrain model with fairness constraints

#### Post-processing Interventions (Fix the Output)
Use when model is trained but outputs are unfair.

**Examples:**
- Threshold adjustment per group
- Calibration across subgroups
- Re-ranking predictions

**Engineer Action:**
- Adjust decision boundary or output scores
- Validate fairness-performance tradeoff

### Step 5: Validate Intervention Effect

No intervention is complete without verification.

**Engineer Action:**
- Re-run fairness metrics:
  - Demographic parity
  - Equal opportunity
  - Calibration differences
- Check performance impact
- Run counterfactual tests again

**Output:**
- Before vs after fairness comparison

### Step 6: Iterate if Needed

Fairness is not a one-shot fix.

**Engineer Action:**
- If disparities persist:
  - Revisit causal assumptions
  - Try alternative intervention layer
  - Combine multiple interventions if necessary

## Decision Flow Summary

1. Detect disparity  
2. Diagnose causal source  
3. Classify bias type  
4. Map to intervention layer  
5. Apply fix  
6. Validate results  
7. Iterate  

## Key Principle

> You do not “apply fairness techniques”—you *diagnose and treat causal problems*.

Each intervention is only valid if it matches the underlying cause of bias.

## Common Pitfalls

- Applying post-processing fixes to data problems (ineffective)
- Removing sensitive attributes without checking proxies (false sense of fairness)
- Over-correcting and destroying predictive utility
- Ignoring counterfactual consistency

## Output of This Workflow

By following this process, you should have:

- A diagnosed fairness issue
- A causal classification of bias
- A selected intervention strategy
- Validated fairness improvements
- Documented trade-offs

## Next Step

After completing intervention, move to:

- **Validation Framework** → to rigorously test fairness outcomes
- **Adaptability Guide** → to generalize across domains
