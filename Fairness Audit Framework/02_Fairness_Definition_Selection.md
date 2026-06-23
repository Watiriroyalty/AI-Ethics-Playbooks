# Fairness Definition Selection Tool (FDS)

## Playbook Component 2 of the Fairness Audit Playbook
The Fairness Definition Selection Tool is the second component of the Fairness Audit Playbook. It translates historical discrimination analysis into explicit fairness objectives that guide subsequent bias detection and mitigation.

## Purpose

Fairness is not a single metric. It is a family of competing definitions that encode different ethical commitments, legal requirements, and technical constraints. The purpose of the **Fairness Definition Selection Tool (FDS)** is to help engineering teams:

- Translate abstract fairness goals into **explicit, measurable fairness definitions**
- Select definitions that fit the **application context**, **historical discrimination patterns**, and **stakeholder priorities**
- Ensure selections are compatible with **legal and regulatory requirements**
- Make **trade-offs explicit**, since many fairness properties cannot be satisfied simultaneously
- Produce a consistent, auditable **record of fairness definition choices** for governance and iteration

This tool is intended to be used after **Historical Context Assessment** and before:
- Bias Source Identification (BSI)
- Comprehensive Fairness Metrics (CFM)


## Inputs and Outputs

### Required Inputs
- Historical Context Assessment findings (patterns, affected groups, risk register)
- System decision type (binary decision, ranking, pricing, risk scoring)
- Ground truth label definition and reliability assessment (what is Y, how trustworthy?)
- Identified stakeholders and their fairness concerns
- Jurisdictions and applicable domain regulations (e.g., lending/credit rules)

### Required Outputs (Artifacts)
1. **Protected Attributes Registry** (jurisdiction + domain scoped)
2. **Fairness Definition Selection Record** (selected definitions + rationale)
3. **Trade-off Analysis Template** (completed)
4. **Intersectional Coverage Plan** (which intersections, why, feasibility notes)
5. **Feasibility Notes** (data availability, label quality, implementation complexity)


# 1. Conceptual Foundations 

Fairness definitions embody philosophical perspectives and stakeholder goals:

- **Egalitarian (equal outcomes)** → tends to align with *Demographic Parity*
- **Libertarian / procedural fairness (treat similar individuals similarly)** → aligns with *Individual Fairness*
- **Rawlsian justice (prioritize least advantaged)** → aligns with minimizing worst-group harm / subgroup fairness prioritization
- **Utilitarian (maximize overall welfare)** → often emphasizes accuracy/utility but must justify disparities and harms

**Key implication:** choosing a fairness definition is a normative decision. The tool forces that decision to be documented rather than hidden inside metric choices.


# 2. Fairness Definition Catalog 

## 2.1 Group Fairness Definitions

### A) Demographic Parity (Statistical Parity)
**Goal:** equal positive outcome rates across protected groups.

**Formulation:**  
P(Ŷ = 1 | A = a) = P(Ŷ = 1 | A = b)

**Use when:**
- Historical context shows systemic exclusion / underrepresentation
- Outcome parity is a primary equity goal
- You are correcting structural disadvantage where “qualification” is itself historically biased

**Limitations:**
- Can conflict with merit/qualification-based goals
- Can reduce utility when base rates differ
- Does not control error-type disparities


### B) Equal Opportunity (TPR Parity)
**Goal:** qualified individuals have equal chance of positive outcome across groups.

**Formulation:**  
P(Ŷ = 1 | Y = 1, A = a) = P(Ŷ = 1 | Y = 1, A = b)

**Use when:**
- False negatives are more harmful (missing qualified applicants)
- Opportunity allocation domains (lending approvals, hiring, admissions)
- You can defend/validate Y as a meaningful “qualified/repay” label

**Limitations:**
- Does not control false positives
- Depends heavily on label validity (Y may encode historic bias)


### C) Equalized Odds (TPR + FPR Parity)
**Goal:** both true and false positive rates equal across groups.

**Formulation:**  
P(Ŷ = 1 | Y = y, A = a) = P(Ŷ = 1 | Y = y, A = b), for y ∈ {0,1}

**Use when:**
- Both false positives and false negatives cause meaningful harm
- High-stakes decisions with asymmetric or politically sensitive error costs

**Limitations:**
- Typically conflicts with calibration (especially when base rates differ)
- May impose substantial utility/accuracy trade-offs


### D) Predictive Equality (FPR Parity)
**Goal:** equal false positive rates across groups.

**Formulation:**  
P(Ŷ = 1 | Y = 0, A = a) = P(Ŷ = 1 | Y = 0, A = b)

**Use when:**
- False positives are the dominant harm (e.g., wrongly flagging fraud/high-risk)
- You want to avoid disproportionate burden on protected groups from incorrect positives

**Limitations:**
- Does not control false negatives or selection rate parity


### E) Predictive Parity (PPV Parity)
**Goal:** equal precision across groups (positive predictions have same meaning).

**Formulation:**  
P(Y = 1 | Ŷ = 1, A = a) = P(Y = 1 | Ŷ = 1, A = b)

**Use when:**
- Positive predictions trigger costly actions and you want consistent meaning of “approved/flagged”
- Stakeholders interpret positive decisions as comparable signals across groups

**Limitations:**
- Conflicts with equalized odds when base rates differ
- Can mask disparities in false negatives


### F) Calibration / Sufficiency (Risk Score Meaning Consistency)
**Goal:** predicted scores correspond to the same real-world probability across groups.

**Formulation:**  
P(Y = 1 | Ŷ = s, A = a) = P(Y = 1 | Ŷ = s, A = b)

**Use when:**
- The system outputs risk scores (credit scoring, insurance pricing, risk ranking)
- Scores are exposed to users/analysts or used in downstream decisions

**Limitations:**
- Often incompatible with equalized odds when base rates differ
- “Being calibrated” can still allow unequal treatment patterns


## 2.2 Individual and Causal Fairness Definitions

### G) Individual Fairness (Similarity-Based)
**Goal:** similar individuals receive similar predictions.

**Formulation (Lipschitz-style):**  
dᵧ(Ŷ(xᵢ), Ŷ(xⱼ)) ≤ L · dₓ(xᵢ, xⱼ)

**Use when:**
- The domain norm is “treat like cases alike”
- Group parity is not the primary fairness objective
- You can define a defensible similarity metric (what counts as “similar” is not trivial)

**Limitations:**
- Defining similarity is normatively loaded and domain-specific
- Can preserve group disparities if “similarity” embeds structural inequality

### H) Counterfactual Fairness (Causal)
**Goal:** an individual’s prediction would not change if their protected attribute were different, holding other causally-independent factors constant.

**Formulation:**  
P(Ŷ_{A←a}(U) = y | X = x, A = a) = P(Ŷ_{A←a’}(U) = y | X = x, A = a)

**Use when:**
- You need causal reasoning about how protected attributes influence other variables
- You want to separate “legitimate” vs “problematic” causal pathways

**Limitations:**
- Requires causal modeling capacity and assumptions
- Complexity can exceed typical production constraints


# 3. Legal and Regulatory Alignment 

Fairness definitions must be chosen with legal constraints in mind. Technical fairness compliance does **not** guarantee legal compliance, and vice versa.

## 3.1 Protected Attributes Registry (Required Artifact)
Create a registry scoped to:
- domain (e.g., lending/credit decisions)
- jurisdiction(s) where the system operates

For each protected attribute, record:
- Attribute name
- Why it is legally protected (domain/jurisdiction)
- Allowed usage: **training**, **evaluation-only**, or **prohibited**
- Privacy constraints / special category data handling requirements

## 3.2 Disparate Treatment vs Disparate Impact (Practical implications)
- **Disparate treatment:** explicit discriminatory use of protected attributes (high legal risk)
- **Disparate impact:** neutral system causes disproportionate adverse outcomes (common algorithmic risk)

**Operational mapping:**
- Disparate impact screening often aligns with **selection rate and group outcome disparities**, making *Demographic Parity* and related group disparity checks important as *diagnostics*, even if not the final fairness objective.
- Legal compliance often also requires:
  - explanation requirements (e.g., adverse action reasons in lending)
  - documentation of business necessity and less discriminatory alternatives

## 3.3 Cross-Jurisdiction Conflicts
If requirements conflict:
- If it’s a threshold difference → implement the strictest standard as baseline
- If it’s a fundamental conflict → segment by jurisdiction when feasible, and document rationale + legal review


# 4. Incompatibilities and Impossibility Results 

When group base rates differ (common in real data), several fairness properties become mutually incompatible.

## 4.1 Core impossibility constraints 
- Calibration often conflicts with error-rate parity (e.g., equalized odds) when base rates differ
- Equal FPR + equal FNR + equal PPV cannot generally all hold when base rates differ
- Demographic parity can conflict with individual fairness and equal opportunity depending on qualification distributions

## 4.2 Compatibility quick guide 
- If **risk scores must be meaningful across groups** → you will likely prioritize **Calibration**, and treat error-rate parity as bounded/monitored rather than perfectly equal.
- If **error harms must be distributed fairly** → prioritize **Equalized Odds** or a targeted parity (Equal Opportunity / Predictive Equality).
- If **historical exclusion is the core harm** → demographic parity may be a primary objective or a strong secondary diagnostic.
- If **“treat like cases alike”** is core → individual fairness may be primary, but you must still monitor group impacts.


# 5. Decision Tree for Selecting Fairness Definitions (FDS Core)

Follow the steps in order. Document decisions at each step.

## Step 0 — Regulatory Mapping (Mandatory)
1) Identify jurisdictions + domain regulations  
2) Produce the **Protected Attributes Registry**  
3) Identify any mandated explanations / documentation duties (e.g., adverse action notices)

If this step is incomplete, do not proceed.


## Step 1 — Decision Type and System Output
Choose the system output type:

- **Binary decision** (approve/deny)  
- **Ranking** (who gets offered what first)  
- **Risk score** (continuous probability score)  
- **Pricing/limits** (interest rates, credit limits)

Rules:
- If system outputs a **risk score** used downstream → include **Calibration** as a required fairness property candidate.


## Step 2 — Historical Context Trigger
From the Historical Context Assessment, ask:

Did we identify systemic exclusion / structural disadvantage affecting protected groups?

- If **Yes** → include **Demographic Parity as a required diagnostic** and consider it as a candidate objective depending on stakeholder/legal alignment.
- If **No** → proceed.

Note: “required diagnostic” means it must be measured and reported, even if it is not optimized.


## Step 3 — Stakeholder Harm Prioritization (Error Impact)
Determine dominant harm:

- If **False Negatives** are the primary harm → prioritize **Equal Opportunity**
- If **False Positives** are the primary harm → prioritize **Predictive Equality**
- If **both** are high impact → prioritize **Equalized Odds**

Also record:
- Who bears the harm? (borrowers, business, regulators, society)
- Is the harm reversible? (loan denial often not immediately reversible)


## Step 4 — Label Validity Check (Is Y trustworthy?)
Ask:
- Does Y reflect the true concept we claim (e.g., “would repay”)?
- Is Y historically biased (selection bias, measurement bias, label bias)?

Rules:
- If label is questionable → treat group fairness metrics as **diagnostic**, and consider adding:
  - **Individual fairness** constraints for consistency
  - **Counterfactual fairness** exploration (if feasible)
  - stronger documentation of limitations and label risk


## Step 5 — Feasibility Gate (Data + Capability)
Check feasibility:

- Do we have protected attributes (or lawful proxies) for evaluation?
- Do we have enough subgroup sample sizes for stable measurement?
- Do we have capacity for:
  - constrained optimization?
  - post-processing thresholding?
  - causal modeling?

Rules:
- If subgroup sample sizes are small → define an **Intersectional Coverage Plan** with prioritization (see Section 6)
- If causal modeling capacity is absent → do not select counterfactual fairness as primary; consider it as longer-term roadmap

---

## Step 6 — Select Primary + Secondary Definitions
Select:
- 1–2 **Primary** fairness definitions (optimized/targeted)
- 2–4 **Secondary** definitions (monitored/guardrails)
- Any **legal minimum diagnostic checks** required by domain and jurisdiction

Always include:
- a clear statement of what is NOT being satisfied and why (trade-off section)


# 6. Intersectionality Handling (Required)

Single-axis fairness can hide harm. Intersectional analysis is required even if legal frameworks are primarily single-axis.

## Intersectional Coverage Plan (Required Artifact)
Define:
- Protected attributes to intersect (start with 2-way intersections; expand if feasible)
- Priority intersections (based on historical context + risk severity)
- Minimum sample size rules / statistical stability approach
- What will happen if an intersection fails stability checks (e.g., aggregate cautiously, collect more data, defer decision)

Recommended prioritization:
1) historically marginalized intersections identified in Historical Context Assessment  
2) intersections with highest harm severity (loan denial, predatory pricing risk)  
3) intersections with observed large disparities in pilot analysis  


# 7. Trade-Off Analysis Template (Required)

Use this template to document definition choices.

## 7.1 Selected Definitions
**Primary fairness definition(s):**  
-  
-  

**Secondary/monitoring definitions:**  
-  
-  

**Mathematical formulations referenced:**  
(list the definitions from the catalog)


## 7.2 Rationale
### Historical Context Link
- Which historical patterns motivated these definitions?
- Which groups are most impacted?

### Stakeholder Link
- Key stakeholders:
- What each stakeholder considers “fair”:
- How prioritization was decided (including power dynamics / whose voice is at risk of exclusion):

### Legal/Regulatory Link
- Jurisdictions:
- Domain rules relevant to lending/credit decisions:
- Required protected attributes addressed:
- Explanation/documentation obligations noted:


## 7.3 Trade-offs and Incompatibilities
### Incompatibilities acknowledged
(e.g., calibration vs equalized odds; equal opportunity vs demographic parity)

### What we are NOT optimizing (and why)
-  

### Expected impact on utility/performance
- What performance metric will degrade (if any)?
- Why this is acceptable (or bounded)?


## 7.4 Implementation Intent (High-level only)
(This is not the full metrics component; it’s the intention.)
- Constraint-based / regularization / post-processing / hybrid
- Any feasibility constraints or roadmap items


## 7.5 Monitoring Guardrails
- Which secondary metrics will be tracked continuously?
- Escalation triggers (e.g., disparity exceeds threshold, drift appears)
- Review cadence and owners


# 8. User Documentation 

## When to run FDS
Run FDS when:
- the system is high-stakes (credit, employment, housing, healthcare, education)
- you see demographic disparities in outcomes or error rates
- you are preparing for launch approval / governance review

## Who should participate (minimum)
- ML/engineering lead
- Product owner
- Responsible AI / governance representative
- Compliance/legal stakeholder (especially for lending)
- Domain expert
- Optional: customer advocacy / impacted community representation (where feasible)

## Recommended timebox
- Regulatory mapping + registry: 1–2 hours (more if multi-jurisdiction)
- Stakeholder + harm analysis: 1–2 hours
- Definition selection + trade-off documentation: 1–2 hours
- Total typical effort: 3–6 hours (per system iteration)

## Deliverables checklist
- [ ] Protected Attributes Registry
- [ ] Fairness Definition Selection Record
- [ ] Completed Trade-off Analysis Template
- [ ] Intersectional Coverage Plan
- [ ] Feasibility Notes and open questions


# 9. Fairness Definition Selection Record (Output Artifact)

Use this as the final “stamp” record for governance.

**System name:**  
**Decision type:** (binary / ranking / score / pricing)  
**Jurisdictions / domain constraints:**  
**Protected attributes registry link/reference:**  

**Primary fairness definition(s):**  
-  

**Secondary fairness definition(s):**  
-  

**Historical context drivers:**  
-  

**Error harm prioritization (FN vs FP):**  
-  

**Label validity risks:**  
-  

**Incompatibilities acknowledged:**  
-  

**Intersectional coverage plan summary:**  
-  

**Approval / sign-off (roles):**  
- Engineering:
- Product:
- Responsible AI:
- Legal/Compliance:


## End State

Once the FDS component is complete, teams should have:
- a defensible, documented fairness objective set
- explicit acknowledgement of trade-offs and incompatibilities
- a clear plan to implement and measure the selected definitions in subsequent components:
  - Bias Source Identification (BSI)
  - Comprehensive Fairness Metrics (CFM)
