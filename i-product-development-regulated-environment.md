# AI Product Development in a Regulated Environment
## A Process Guide for Healthcare, Pharma, and Medical Device Product Teams

Building AI products in regulated healthcare is not harder than building in consumer tech. It is different. The constraints are known in advance. The documentation burden is real but manageable if you build it into the process from day one rather than retrofitting it before submission. This guide covers the full development process from problem definition through post-market surveillance.

---

## Why Regulated AI Is Different

Three things separate regulated AI development from standard software development.

**The intended use statement drives everything.** In consumer software, you can pivot the product mid-build without regulatory consequence. In regulated AI, the intended use statement determines device classification, submission pathway, clinical evidence requirements, and labeling. Changing it mid-build is expensive. Changing it after submission restarts the clock.

**The AI model is part of the device.** In most software, the algorithm is an implementation detail. In regulated AI, the model architecture, training data, validation methodology, and performance thresholds are regulatory artifacts. The FDA reviews them. Changes to the model after clearance may require a new submission.

**Failure modes have clinical consequences.** A bug in a consumer app causes a bad user experience. A failure in a regulated AI diagnostic can delay treatment or cause patient harm. Risk management is not a documentation exercise. It is a design activity.

---

## Phase 1: Problem Definition and Regulatory Strategy

Get this phase right before writing a line of code or a single requirement. Every downstream decision traces back to it.

### Define the Intended Use Statement

Write one sentence. Be specific. Be narrow.

Good: "The software is intended to analyze colonoscopy video frames to identify and flag regions of interest for clinician review during colonoscopy procedures."

Bad: "The software is intended to assist clinicians in improving patient outcomes."

The second version sounds better in a pitch deck. It will cause regulatory problems. Broad intended use expands classification risk, widens the clinical evidence requirement, and creates labeling liability everywhere the product is marketed.

### Determine Device Classification

Run through the SaMD classification decision tree before committing to a development timeline.

Questions to answer:
- Does the software meet the FDA definition of a device under 21 CFR 201(h)?
- Does FDA enforcement discretion apply?
- What is the device class (I, II, or III)?
- Does a cleared predicate exist for 510(k) pathway?
- What is the clinical evidence standard for this class and indication?

The answers determine whether you need a 510(k), De Novo, or PMA. They also determine your development timeline. A PMA-pathway product takes 18 to 36 months from submission to clearance. A 510(k) with a strong predicate can clear in 3 to 6 months.

### Establish the Regulatory Strategy Document

Before sprint 1, produce a one-page regulatory strategy document covering:

- Intended use statement (final, signed off by clinical and regulatory)
- Device classification and rationale
- Regulatory pathway and expected timeline
- Predicate device identification (if 510(k))
- Clinical evidence plan: what data you will need to support the submission
- Key regulatory risks and mitigations

This document is a decision-making anchor. When the product team wants to add a feature that expands the intended use, this document is what you hold up.

---

## Phase 2: Design Controls

21 CFR Part 820 requires design controls for Class II and Class III devices. Even if you are building a Class I product or a product outside FDA jurisdiction, design controls are good engineering practice.

### Design and Development Planning

Before build, document:
- Development phases and milestones
- Responsibilities: who owns each phase
- Design review schedule: when reviews happen and who participates
- Verification and validation plan: how you will confirm the product meets requirements

### Design Inputs

Design inputs are the requirements. They must be:
- Specific enough to verify
- Traceable to user needs
- Reviewed and approved before engineering uses them

For AI products, design inputs include:
- Model performance thresholds (sensitivity, specificity, AUC) for the intended clinical use
- Data requirements: training data characteristics, labeling standards, class balance
- Intended user and use environment
- Human factors requirements: how clinicians will interact with model outputs

### Design Outputs

Design outputs are what engineering produces: the trained model, the software, the labeling, the instructions for use. Every design output must be traceable to a design input.

### Design Verification

Verification answers: did we build the product to the specification?

For AI products:
- Model performance on a held-out validation dataset
- Comparison against predefined performance thresholds from design inputs
- Unit testing, integration testing, system testing of the software
- Cybersecurity testing

### Design Validation

Validation answers: does the product meet the needs of the intended user in the intended use environment?

For AI products:
- Clinical study or simulated use study with the intended user population
- Human factors validation: can clinicians use the AI output correctly in their workflow
- Real-world performance testing in the target clinical setting

Verification and validation are not the same. A model that hits its performance thresholds on a test dataset (verification) may still fail when a clinician misinterprets the output in a time-pressured clinical environment (validation failure).

---

## Phase 3: AI/ML-Specific Development Requirements

### Training Data Management

Training data for regulated AI is a regulatory artifact. Document:

- Data sources: where did the data come from, what populations, what clinical settings
- Labeling methodology: who labeled the data, what criteria, what inter-rater agreement
- Data splits: how training, validation, and test sets were constructed, that test set was held out
- Class balance: distribution of positive and negative cases, handling of imbalanced classes
- Data quality controls: how artifacts, errors, and outliers were handled

The FDA will ask all of these questions during review. The answers need to be in your technical file.

### Model Development and Version Control

- Every model version is tracked with a unique identifier
- Training configuration is documented: architecture, hyperparameters, training procedure
- Model performance at each version is logged against the same held-out test set
- The model that ships is the model that was validated. No post-validation tuning.

### Algorithm Change Protocol

If you plan to update the AI model after clearance (and you should), define the Algorithm Change Protocol before submission. The ACP describes:

- What types of changes you will make (retraining on new data, architecture changes, threshold adjustments)
- What level of testing each change type requires
- When a change requires a new submission vs. when it can be implemented under the existing clearance

FDA's guidance on Predetermined Change Control Plans (PCCPs) provides the framework. Build your ACP to align with it.

### Bias and Fairness Assessment

Regulatory submissions increasingly require evidence that the model performs equitably across:
- Demographics: age, sex, race, ethnicity
- Clinical subgroups: disease severity, comorbidities
- Site characteristics: institution type, geography, imaging equipment

Run subgroup performance analysis before submission. If performance gaps exist, document them in your labeling and address them in your clinical evidence plan.

---

## Phase 4: Clinical Evidence

The clinical evidence requirement scales with device class and risk.

### What the FDA Wants to See

For a 510(k) AI submission, the technical documentation typically includes:
- Device description and intended use
- Predicate device comparison (substantial equivalence argument)
- Performance testing: sensitivity, specificity, AUC on a representative test dataset
- Reader study results if the AI assists clinician decision-making
- Subgroup performance analysis
- Cybersecurity documentation

For a De Novo or PMA, add:
- Prospective clinical study results
- Real-world performance data
- Detailed risk-benefit analysis

### Clinical Study Design for AI Products

If a clinical study is required:
- Pre-specify the primary endpoint and statistical analysis plan before data collection
- Power the study for the primary endpoint, not for what you hope to show
- Include the intended user population and intended use environment
- For reader studies: include the actual clinicians who will use the product, not just experts

### Real-World Evidence

Post-market AI performance monitoring is increasingly expected. Design the post-market surveillance plan before launch:
- What performance metrics will you track
- What data will you collect from deployed sites
- What triggers a safety review or a submission update

---

## Phase 5: 21 CFR Part 11 Compliance

If your AI product creates, modifies, maintains, or transmits electronic records required by FDA (clinical trial data, adverse event reports, submission documents), Part 11 applies.

### Core Requirements

**System validation**
Document that the system does what it is intended to do. Validation is not testing. Validation produces a documented record: validation plan, test scripts, test results, summary report.

**Audit trails**
Computer-generated, time-stamped records of all creation, modification, and deletion of electronic records. Audit trails must be retained and must be accessible for FDA inspection.

**Access controls**
Unique user IDs, password controls, role-based permissions. Shared accounts are a Part 11 violation.

**Electronic signatures**
Linked to the record, non-repudiable, includes printed name, date and time, and the meaning of the signature (approved, reviewed, authored).

**Record retention**
Electronic records must be protected and retrievable for the full required retention period.

### Validation for eCOA and ePRO Systems

Clinical trial endpoint capture systems require validation before first patient use. The validation package includes:
- User requirements specification
- Functional requirements specification
- Validation master plan
- Test scripts with expected and actual results
- Validation summary report
- Traceability matrix linking requirements to test cases

Validation does not end at launch. Changes to the validated system require impact assessment and, if the change affects validated functions, re-validation.

---

## Phase 6: Quality Management System

A QMS is required for Class II and Class III devices. Even for Class I and non-device AI products deployed in regulated environments, clients will ask for evidence of a QMS.

### Core QMS Elements for AI Development

**Document control**
Every document that governs your product has a version number, an approval signature, and a change history. No one works from an unapproved document.

**Change control**
Changes to the design, the software, the AI model, or the manufacturing process go through a formal change control process. Impact assessment before implementation, documentation after.

**CAPA (Corrective and Preventive Action)**
When something goes wrong, the CAPA process documents: what happened, root cause analysis, corrective action, verification that the correction worked. CAPA records are a primary focus of FDA inspections.

**Complaint handling**
Every complaint from a customer or user is logged, investigated, and assessed for reportability under MDR (Medical Device Reporting) requirements.

**Supplier controls**
If you use third-party data, models, or infrastructure in your regulated AI product, your QMS must include supplier qualification and monitoring.

---

## Phase 7: Post-Market Surveillance

Clearance is not the end. For AI products, post-market surveillance is where most of the regulatory work happens over the product lifetime.

### AI-Specific Post-Market Monitoring

**Model performance drift**
AI models degrade over time as the distribution of real-world data shifts from the training distribution. Monitor model performance against held-out benchmarks on a defined schedule.

**Data drift detection**
Track the statistical distribution of inputs to the model. If input distributions shift, model performance may shift even if the model itself has not changed.

**Adverse event reporting**
Any serious injury or death where your AI product may have contributed must be reported to FDA under MDR requirements within defined timeframes (30 days for serious injuries, 5 days for immediately threatening events).

**Post-market clinical follow-up**
For higher-risk AI products, plan for ongoing clinical data collection after launch. This supports both regulatory requirements and the evidence base for your PCCP.

---

## Common Failure Modes in Regulated AI Development

**Failure 1: Intended use creep**
The product team adds features that expand the intended use without updating the regulatory strategy. The product ships with a mismatch between what it does and what the submission said it would do.

**Failure 2: Test set contamination**
Training data and test data are not kept separate. The model performs well on internal benchmarks and poorly in clinical validation because the benchmark was not representative.

**Failure 3: Validation study design failure**
The clinical validation study is underpowered for the primary endpoint, uses the wrong user population, or tests in a setting that does not match the intended use environment. The study costs $500K and does not support the submission.

**Failure 4: Algorithm change without submission**
The team retrains the model on new data after clearance without assessing whether the change requires a new submission. FDA classifies this as a major change and requires a new 510(k).

**Failure 5: Part 11 retrofit**
The clinical trial team realizes the eCOA system needs Part 11 validation after first patient enrollment. Retroactive validation is expensive, time-consuming, and may invalidate data already collected.

**Failure 6: QMS built for audit, not for operations**
The QMS is a set of documents that exist to satisfy an auditor, not a system that governs how the team actually works. When FDA inspects, the gap between the documented process and the actual process is immediately visible.

---

## Reference Standards and Guidance Documents

- FDA Guidance: Artificial Intelligence and Machine Learning in Software as a Medical Device (2021)
- FDA Guidance: Predetermined Change Control Plans for AI/ML-Based SaMD (2023)
- FDA Guidance: Clinical Decision Support Software (2022)
- FDA Guidance: Policy for Device Software Functions and Mobile Medical Applications (2022)
- IEC 62304: Medical Device Software Lifecycle Processes
- IEC 62366: Usability Engineering for Medical Devices
- ISO 14971: Application of Risk Management to Medical Devices
- ISO 13485: Quality Management Systems for Medical Devices
- 21 CFR Part 11: Electronic Records and Electronic Signatures
- 21 CFR Part 820: Quality System Regulation
- NIST AI Risk Management Framework (AI RMF 1.0)

---

*Built from active product development and regulatory positioning work on FDA-cleared and FDA-regulated AI products in clinical trial, diagnostic, and patient support settings.*
