# AI Product Metrics Framework

## Beyond DAU: how to define success metrics for AI features in healthcare

Standard product metrics break down for AI features. Daily active users tells you nothing about whether a clinical AI tool is helping clinicians make better decisions. Session length tells you nothing about whether a risk prediction model is being trusted or ignored. Conversion rate tells you nothing about whether a recommendation engine is being overridden because it is wrong or because clinicians do not understand it.

AI features require a different measurement architecture. This framework gives product managers a structured approach to defining, collecting, and acting on metrics that reflect actual AI performance in clinical and regulated environments. It covers five layers: model performance, user experience and engagement, clinical outcomes, business and cost, and compliance.

---

## Why standard metrics fail for AI products

Standard product metrics measure user behavior. AI product metrics need to measure five things simultaneously: whether the model performs correctly, whether users trust and act on it, whether it produces better clinical outcomes, whether the economics justify the investment, and whether the system meets regulatory obligations.

A model can have excellent technical performance (high AUC, high sensitivity) and still fail as a product because clinicians route around it. A model can show strong engagement metrics because users interact out of compliance obligation rather than genuine utility. An AI tool can improve clinical outcomes but fail a HIPAA audit because PHI handling was never properly governed.

The measurement framework has to capture all five layers or it will mislead you.

---

## Layer 1: Model and performance metrics

These measure whether the model is doing what it was designed to do, independent of user behavior.

**Accuracy**
The share of all predictions the model gets correct. Useful only when the dataset is balanced between positive and negative cases. In most clinical datasets, class imbalance makes accuracy misleading. A model predicting "no sepsis" for every patient in a 3% prevalence population achieves 97% accuracy while detecting zero cases. Always report alongside sensitivity and specificity.

**Sensitivity (Recall)**
The share of true positive cases the model identifies. In a sepsis alert system, sensitivity measures what percentage of actual sepsis cases generate an alert. A model with 90% sensitivity misses 1 in 10 cases. For screening tools and safety alerts, sensitivity is the primary performance gate: missing a true case is the costly failure mode.

**Precision (Positive Predictive Value)**
When the model predicts positive, how often it is correct. Precision is heavily influenced by prevalence. The same model will have materially different precision in a high-acuity ICU population versus a general outpatient population. This is why validating on one population and deploying on another requires re-evaluation before launch.

**F1 Score**
The harmonic mean of precision and recall: 2 × (precision × recall) / (precision + recall). F1 is the right aggregate metric when both false positives and false negatives carry clinical cost and you need a single number to compare model versions. It penalizes models that trade one heavily against the other. Use it for model comparison, not for setting operating thresholds.

**Specificity**
The share of true negative cases the model correctly excludes. Low specificity generates false alarms. In clinical tools, false alarms drive alert fatigue, which is itself a documented patient safety risk. A high-sensitivity, low-specificity alert system trains clinicians to ignore it.

**NPV (Negative Predictive Value)**
When the model predicts negative, how often it is correct. High NPV is what allows clinicians to safely rule something out based on model output. Critical for triage and risk exclusion tools where the workflow depends on the AI's negative prediction being trustworthy.

**AUC (Area Under the ROC Curve)**
The model's ability to discriminate between positive and negative cases across all possible thresholds. Useful for comparing model versions. Not useful for setting operational thresholds. AUC of 1.0 is perfect separation; 0.5 is a coin flip.

**Calibration**
Whether the model's confidence scores match observed frequencies. A model that assigns 80% probability to a condition should be right approximately 80% of the time on that subset. Poor calibration makes confidence scores misleading even when discrimination (AUC) is good. Calibration matters most when scores are shown to clinicians or used to drive downstream automated decisions.

**Latency**
Time from input to output. For real-time clinical decision support, latency directly affects clinical utility. A sepsis alert that arrives 90 seconds after the triggering lab result is less useful than one that arrives in 5 seconds. Track P95 latency (the 95th percentile response time), not mean latency. Tail latency affects the urgent cases where the tool matters most.

**Throughput**
Number of predictions or requests the system handles per unit time. Relevant for high-volume tools (population risk stratification, prior auth screening, lab result triage) where the system processes thousands of records concurrently. Throughput constraints surface as production failures during high-census periods, which in hospital environments are exactly when clinical AI tools need to perform.

**Model drift indicators**
How model performance changes over time as the underlying patient population or care patterns shift. Track sensitivity, specificity, and PPV monthly for deployed models. Define the degradation threshold that triggers re-validation before launch, not after drift is observed.

---

## Layer 2: User experience and engagement metrics

These measure how users actually behave toward the AI output. This is where most teams have the largest measurement blind spot.

**Intent Resolution Rate (IRR)**
The percentage of user queries or sessions fully resolved by the AI without requiring human escalation, re-routing, or follow-up. In a clinical context: the share of patient inquiries handled end-to-end by an AI patient services tool without escalating to a nurse or care coordinator. IRR is the primary efficiency metric for AI tools designed to reduce human workload. Track it alongside escalation quality: a high IRR achieved by suppressing appropriate escalations is a patient safety risk, not a product success.

**First-Attempt Success Rate**
How often a user achieves their goal without rephrasing the query, correcting the output, or abandoning the session. Low first-attempt success rate signals prompt design problems, interface ambiguity, or a model that requires too much user effort to produce useful output. In clinical workflows where time is the primary constraint, multiple attempts to get a usable output predict low adoption.

**Abandonment Rate**
The percentage of sessions where users exit without completing their intended action due to AI performance issues: incorrect output, low confidence output, or confusing response. Distinguish from abandonment caused by external interruption, which is common in clinical environments. Segment by session type, user role, and time of day. High abandonment during high-acuity periods means the tool fails when clinical need is highest.

**Confidence Score (User Behavior)**
How user behavior changes at different model confidence levels. If the model surfaces confidence scores, track whether clinicians act differently on a 95% confidence recommendation versus a 72% confidence recommendation. If behavior does not change with confidence level, confidence display is not influencing decisions. This has implications for UX design and regulatory positioning.

**CSAT (Customer Satisfaction Score)**
Direct user rating of AI output quality, collected immediately after an interaction. In clinical tools, CSAT captures whether clinicians found the output useful and trustworthy. Segment by output type and clinical context. CSAT below threshold for specific output categories identifies where the model underperforms from the user's perspective, independent of what technical metrics show.

**Override Rate**
The percentage of AI recommendations or alerts that a clinician actively overrides or dismisses. One of the most important metrics for clinical AI and one of the least commonly tracked.

Override rate alone is ambiguous and requires segmentation:

- High override rate + good clinical outcomes: the model generates low-quality signals that experienced clinicians correctly ignore
- High override rate + poor clinical outcomes: clinicians override correct signals, indicating a trust or workflow design problem
- Low override rate + poor clinical outcomes: clinicians follow incorrect recommendations without scrutiny, a patient safety concern
- Low override rate + good clinical outcomes: the model generates useful signals acted on appropriately

Track by user role, department, time of day, and patient acuity. Patterns in who overrides and when are more informative than aggregate override rate.

**Alert Fatigue Index**
For tools that generate alerts or notifications, the ratio of alerts dismissed to alerts generated. An alert fatigue index above 80-85% is a documented patient safety risk pattern. It predicts wholesale alert suppression behavior where clinicians disable or ignore all alerts from the system.

**Time-to-Action**
The interval between an AI recommendation and a clinician action or explicit dismissal. Longer time-to-action may indicate the recommendation is unclear, poorly placed in the workflow, or not trusted enough to act on immediately.

**Recommendation Acceptance Rate**
For advisory AI tools, the share of recommendations that result in the recommended action being taken. Segment by recommendation type, patient population, and clinician experience level. Acceptance rate that varies significantly by clinician seniority identifies a trust calibration problem.

**Escalation Rate**
For tools with a human-in-the-loop review step, the percentage of cases escalated from automated processing to human review. Escalation rate consistently near zero or near 100% indicates the threshold is misconfigured.

---

## Layer 3: Clinical outcome metrics

These measure whether the AI feature actually improves patient care. They are the hardest to collect and the most important for regulatory submissions and business case justification.

**Primary clinical endpoint improvement**
The clinical outcome the product was designed to affect: readmission rate, time-to-diagnosis, medication adherence, A1C reduction, sepsis mortality. Requires a comparison baseline, either pre-implementation performance at the same site or a control group. Without a baseline, outcome data cannot demonstrate the AI's contribution.

**Diagnostic accuracy change**
For diagnostic AI tools, how clinician diagnostic accuracy changes with versus without the AI recommendation. Requires a study design (randomized or quasi-experimental) to isolate the AI contribution from concurrent changes.

**Time-to-treatment**
For tools that flag conditions or risk, the change in interval between condition onset and treatment initiation following deployment.

**Adverse event rate**
Post-market surveillance metric. The rate at which AI-influenced decisions are associated with adverse patient events. Tracking correlation is a regulatory expectation and a patient safety obligation under 21 CFR Part 803.

**Near-miss capture rate**
For safety-oriented AI tools, the percentage of near-miss events the model flagged before they became adverse events. Requires retrospective review of near-miss logs against model output history.

---

## Layer 4: Business and cost metrics

These connect AI performance to financial and operational outcomes. For pharma and health system clients, these metrics justify continued investment and board-level reporting.

**Cost per query**
The fully-loaded cost of a single AI prediction or response: compute, API tokens, infrastructure, and allocated engineering overhead. For tools that process high volumes (population risk scoring, prior auth screening, ambient documentation), cost per query determines whether the unit economics support scale. Model selection, prompt optimization, and caching strategy are PM decisions with direct cost per query implications.

**ROI**
Financial return relative to AI investment. In healthcare, ROI comes from three sources: labor cost reduction (fewer human hours required for tasks the AI handles), revenue impact (faster prior auth, higher care gap closure, improved coding accuracy), and cost avoidance (reduced readmissions, avoided adverse events, reduced redundant testing). Quantify each source separately. Aggregate ROI claims that blend all three are harder to defend in a business case review.

**Call and chat containment rate**
The percentage of service interactions (patient inquiries, prior auth requests, scheduling, triage calls) handled fully by AI without human involvement. The complement of escalation rate. Containment rate rising over time indicates model improvement or expanding scope. Containment rate falling indicates model degradation, scope creep into harder cases, or users learning to escalate immediately rather than trying the AI.

**Customer churn**
For patient-facing or clinician-facing AI tools, the rate at which users stop engaging with the product. Churn driven by poor AI performance is a distinct failure mode from churn driven by clinical workflow changes or competitive alternatives. Segment churn reason to distinguish.

**Robustness**
The model's ability to produce safe, useful output when inputs are outside the expected distribution: incomplete data, unusual formatting, rare clinical presentations, adversarial inputs. Clinical data is consistently messy. Lab results arrive in inconsistent units. EHR fields are missing. A model that performs well on clean validation data but degrades on real-world inputs has a robustness gap. Test explicitly for out-of-distribution inputs before launch and track robustness failures in production through a structured error taxonomy.

---

## Layer 5: Safety and compliance metrics

These measure whether the AI system meets regulatory and operational obligations. For regulated products, these are not optional dashboard items.

**Helpfulness and relevance (hallucination rate)**
For generative AI components, the rate at which outputs are factually accurate, clinically relevant, and free of fabricated content. Track by hallucination type: factual fabrication, source fabrication, extrapolation beyond evidence, omission of safety information, temporal error, and context collapse. See `clinical-ai-hallucination-detection.md` for the full evaluation protocol.

**System availability**
Uptime measured against defined SLAs. For Class II SaMD, availability failures that affect patient care may be reportable events under 21 CFR Part 803.

**PHI handling compliance**
The rate at which PHI is processed, transmitted, and stored in accordance with documented HIPAA controls. Monitored through audit log review and access control reporting.

**Audit trail completeness**
For Part 11 systems, the percentage of model inputs and outputs with complete, retrievable audit records. Any audit trail gap in a Part 11 system requires investigation and remediation. There is no acceptable non-zero threshold.

**Model version consistency**
The percentage of predictions made by the current approved model version. Any production traffic on a deprecated model version is a quality system issue requiring immediate remediation.

**User training completion**
The percentage of active users who have completed required training on intended use, limitations, and appropriate use of AI outputs. Untrained users are a labeled risk in your IFU and a finding in a regulatory audit.

---

## Metric selection by AI product type

| Product type | Tier 1 metrics | Tier 2 metrics | Watch closely |
|---|---|---|---|
| Clinical decision support | Override rate, acceptance rate, clinical outcome improvement | Alert fatigue index, time-to-action, CSAT | Override rate by clinician experience level |
| Diagnostic AI | Sensitivity, specificity, PPV, F1, diagnostic accuracy change | Confidence threshold interaction, time-to-treatment | Subgroup performance disparities |
| Patient-facing AI | IRR, containment rate, clinical endpoint improvement | Abandonment rate, churn, CSAT | Escalation suppression patterns |
| Risk stratification | Calibration, alert fatigue index, clinical outcome improvement | Override rate, escalation rate, drift | PPV drift as population shifts |
| Generative AI / LLM | Hallucination rate, helpfulness score, acceptance rate | Confidence interaction, time-to-action | Source citation verification rate |
| Administrative AI | Containment rate, cost per query, ROI | Abandonment rate, override rate | Robustness on edge case inputs |

---

## Metrics your ML team owns vs. metrics you own

**ML team owns:**
- Model training and validation metrics (AUC, sensitivity, specificity, calibration on held-out test set)
- Model drift detection in production
- Infrastructure metrics (latency, throughput, availability, version consistency)

**You own:**
- Override rate and its clinical interpretation
- Alert fatigue index
- IRR, abandonment rate, CSAT
- Clinical outcome metrics
- Cost per query and ROI
- Regulatory compliance metrics
- The decision of whether aggregate performance is acceptable for the intended use

The division is not clean in practice. Override rate requires data from both the AI system and the clinical workflow system. Clinical outcome metrics require EHR data and a study design. Plan your data collection infrastructure before launch. The metrics you cannot collect at launch are not metrics you can act on post-launch.

---

## Defining thresholds before launch

Metric thresholds must be defined before validation runs, not after. Post-hoc threshold setting creates audit risk for regulated products and undermines the validity of your clinical evidence.

For each metric in your launch dashboard, document:

- The baseline (pre-implementation value or industry reference)
- The minimum acceptable performance threshold
- The target performance threshold
- The threshold that triggers re-evaluation or incident response
- The measurement methodology and data source

This documentation belongs in your Design History File as a design input. FDA reviewers will ask how you determined that your product was performing adequately. "We watched the dashboard" is not a sufficient answer.

---

## Post-market surveillance review cadence

| Frequency | Review scope |
|---|---|
| Weekly | Latency, throughput, model version consistency, alert fatigue index, system availability |
| Monthly | Override rate trends, acceptance rate, IRR, abandonment rate, PHI compliance, audit trail completeness |
| Quarterly | Clinical outcome metrics, subgroup performance, model drift, cost per query, user training completion |
| Annually | Full model re-validation against updated thresholds, regulatory submission update if required, ROI review |

---

## Quick reference: all metrics

| Metric | Formula | Layer | Primary use |
|---|---|---|---|
| Accuracy | (TP + TN) / Total | Model | Balanced datasets only |
| Sensitivity (Recall) | TP / (TP + FN) | Model | Safety-critical screening |
| Specificity | TN / (TN + FP) | Model | Alert fatigue prevention |
| Precision (PPV) | TP / (TP + FP) | Model | Clinician trust calibration |
| F1 Score | 2 × (P × R) / (P + R) | Model | Model comparison with cost on both error types |
| NPV | TN / (TN + FN) | Model | Rule-out reliability |
| AUC | Area under ROC | Model | Discriminative ability comparison |
| Calibration | Predicted vs. observed frequency | Model | Probability trustworthiness |
| Latency (P95) | 95th percentile response time | Model | Real-time clinical tool SLA |
| Throughput | Requests per second | Model | High-volume deployment capacity |
| Model drift | Delta in key metrics over time | Model | Re-validation trigger |
| Intent Resolution Rate | Resolved sessions / total sessions | UX | AI containment efficiency |
| First-attempt success | Goals achieved without correction / total | UX | Workflow friction signal |
| Abandonment rate | Abandoned sessions / total sessions | UX | Failure mode detection |
| Confidence score (behavioral) | Behavior change at threshold levels | UX | Trust calibration signal |
| CSAT | User satisfaction rating | UX | Output quality from user perspective |
| Override rate | Overrides / total recommendations | UX | Trust and workflow signal |
| Alert fatigue index | Dismissed alerts / total alerts | UX | Safety alert design |
| Time-to-action | Mean interval, recommendation to action | UX | Workflow integration quality |
| Acceptance rate | Actions taken / recommendations made | UX | Utility signal |
| Escalation rate | Human-reviewed cases / total cases | UX | Threshold calibration |
| Clinical endpoint delta | Outcome post vs. pre implementation | Clinical | Product value evidence |
| Diagnostic accuracy change | Accuracy with vs. without AI | Clinical | Diagnostic tool validation |
| Time-to-treatment | Interval from onset to treatment | Clinical | Safety and efficacy signal |
| Adverse event rate | AI-associated adverse events / total | Clinical | Post-market safety |
| Cost per query | Total cost / total queries | Business | Unit economics |
| ROI | Financial return / AI investment | Business | Investment justification |
| Containment rate | AI-handled interactions / total | Business | Labor impact |
| Customer churn | Users lost / total users | Business | Adoption health |
| Robustness | Error rate on out-of-distribution inputs | Safety | Production reliability |
| Hallucination rate | Hallucinated outputs / total outputs | Safety | Generative AI output quality |
| Audit trail completeness | Complete records / total records | Compliance | Part 11 compliance |
| Model version consistency | Current version predictions / total | Compliance | Validated state integrity |
| User training completion | Trained users / active users | Compliance | IFU compliance |
