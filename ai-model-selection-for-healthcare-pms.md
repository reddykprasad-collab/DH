# AI Model Selection for Healthcare Product Managers
## A Decision Framework for Build vs. Buy vs. Fine-Tune in Regulated Environments

Choosing an AI model for a healthcare product is not a technical decision that gets handed to engineering. The intended use statement, the regulatory pathway, the clinical evidence requirement, and the deployment environment all constrain your options before the ML team writes a line of code. This framework gives product managers the structure to lead that decision.

---

## The Decision Nobody Owns

In most healthcare AI projects, model selection falls into a gap. Engineering evaluates what is technically feasible. Regulatory evaluates what is submittable. Clinical evaluates what is safe. Nobody evaluates the intersection of all three against the business case and the deployment reality.

That intersection is a product decision. The PM who does not own it will watch the team default to the most technically interesting option, which is rarely the right one.

---

## Step 1: Define the Use Case Category

Model selection starts with understanding what the AI is actually doing. The category determines your regulatory exposure, your data requirements, and how much explainability you need.

| Category | Examples | Regulatory exposure | Explainability requirement |
| Administrative automation | Prior auth drafting, scheduling optimization, coding assistance | Low | Low |
| Patient education and support | Condition education, medication reminders, adherence coaching | Low to moderate | Moderate |
| Clinical decision support (non-diagnostic) | Risk stratification flags, care gap identification | Moderate | High |
| Diagnostic AI | Image analysis, biomarker detection, disease classification | High (likely SaMD) | High |
| Therapeutic AI | DTx, behavior change, dose recommendation | High (likely SaMD) | High |

If your use case is in the bottom two rows, assume FDA involvement and plan accordingly before selecting a model.

---

## Step 2: Build vs. Buy vs. Fine-Tune Decision Tree

Answer these questions in order. Each answer narrows your options.

### Question 1: Does a cleared or validated solution already exist for this use case?

If yes: evaluate buying before building. A cleared predicate is worth more than a technically superior custom model that needs to go through De Novo or PMA. The regulatory timeline difference is 6 to 36 months.

If no: proceed to Question 2.

### Question 2: How much labeled training data do you have access to?

More than 10,000 labeled cases: fine-tuning a foundation model is viable.

1,000 to 10,000 labeled cases: consider fine-tuning with active learning or transfer learning from a related domain.

Fewer than 1,000 labeled cases: fine-tuning is high risk. Evaluate few-shot prompting on a strong foundation model, retrieval-augmented generation against a curated knowledge base, or a vendor solution trained on a larger dataset.

Most hospital systems have 500 to 2,000 relevant labeled cases per year for any specific clinical use case. If your architecture requires 50,000 cases, you do not have a data problem. You have an architecture problem.

### Question 3: What are the latency requirements?

Under 3 seconds for clinical workflow integration: large frontier models served via API will not reliably meet this. You need a smaller model running locally or on dedicated infrastructure, or a distilled model optimized for inference speed.

3 to 10 seconds acceptable: most API-served models work here.

Batch processing (not real-time): any model size is viable. Focus on accuracy, not latency.

### Question 4: What are the data residency and privacy requirements?

PHI cannot leave the hospital network: cloud API models are not viable. You need a model that runs on-premise or in the hospital's private cloud.

PHI can be de-identified before sending to API: most cloud models become viable, with appropriate BAA and de-identification validation.

No PHI involved: no restriction from a privacy standpoint.

### Question 5: What level of explainability does the clinical use case require?

A clinician must understand why the model produced this output: black-box deep learning is not viable as a standalone. You need attention maps, feature attribution, confidence intervals, or a hybrid architecture that combines the model output with interpretable rule-based logic.

The output is a flag for human review (not a recommendation): explainability requirement is lower. The human makes the decision; the model surfaces the case.

The output drives an automated action: explainability requirement is highest. Regulators and clinicians will both ask for it.

---

## Step 3: Vendor Assessment Criteria

If you are evaluating third-party AI vendors or foundation model providers for a healthcare use case, assess against these criteria before any technical evaluation.

### Regulatory and Compliance

- Does the vendor have a signed Business Associate Agreement process?
- Where is data processed and stored? What are the data residency options?
- Has the model been validated on clinical data? What populations, what settings, what time periods?
- Is there a model card or transparency report covering training data, known limitations, and bias assessments?
- Does the vendor have experience supporting FDA submissions? Can they provide technical documentation in the format required for 510(k) or De Novo?

### Model Performance

- What is the reported performance on the specific task you need (not general benchmarks)?
- What subgroup performance data is available (age, sex, race, ethnicity, disease severity)?
- What is the performance on data from the specific clinical setting you are deploying in (community hospital vs. academic medical center vs. specialty clinic)?
- What is the false positive and false negative rate, and what are the clinical consequences of each error type?

### Integration and Deployment

- What APIs and integration standards does the vendor support (HL7/FHIR, DICOM, EHR-specific)?
- What is the typical time from contract to first deployment? What does the implementation team look like?
- What is the latency profile under production load conditions?
- What monitoring and alerting does the vendor provide post-deployment?

### Model Updates and Change Management

- How often does the vendor update the model?
- What notification process exists before a model update is pushed to production?
- Does a model update require revalidation under your regulatory framework? What is the vendor's process for supporting that?
- If the model is a cleared device, how does the vendor handle FDA submissions for updates?

### Commercial Terms

- How is the model priced (per API call, per seat, per patient, flat license)?
- What are the data use terms? Does the vendor train on your data?
- What are the indemnification terms if the model causes patient harm?
- What are the SLAs for uptime, latency, and support response?

---

## Step 4: Build vs. Fine-Tune vs. Prompt Engineering

If you decide to build or customize rather than buy, the next decision is how much model modification the use case actually requires.

### Prompt engineering only (no model modification)

When it works: the task is within the capability of a strong foundation model, data is not PHI-sensitive or can be de-identified, and you have fewer than 100 labeled examples.

Advantages: fast iteration, no training infrastructure, easy to update, no revalidation required when you change the prompt.

Risks: performance is sensitive to prompt changes, outputs are non-deterministic, behavior can shift when the underlying model is updated by the provider.

### Retrieval-augmented generation (RAG)

When it works: the model needs access to current, specific, or proprietary clinical knowledge (drug labeling, clinical guidelines, protocol documents) that is not in the training data.

Advantages: keeps the knowledge base separate from the model, easier to update and audit, reduces hallucination on factual clinical questions.

Risks: retrieval quality determines output quality. A poorly constructed knowledge base produces plausible-sounding incorrect answers. Requires ongoing knowledge base maintenance.

### Fine-tuning

When it works: the task requires specialized clinical vocabulary, specific output format, or a style of reasoning that foundation models do not produce reliably from prompting alone. You have at least 1,000 labeled examples of the target behavior.

Advantages: better performance on the specific task, more consistent output format, can run smaller models that meet latency constraints.

Risks: training data quality determines model quality. Labeling errors are amplified. Fine-tuned models require revalidation after each retrain. The fine-tuning process itself becomes a regulatory artifact.

### Training from scratch

When it works: almost never for healthcare AI product teams. Foundation models trained on massive general corpora provide a starting point that no team-level training budget can match. The only cases where training from scratch is justified are highly specialized modalities (specific imaging equipment, rare signal types) where no adequate foundation model exists.

---

## Step 5: Regulatory Positioning

Before finalizing model selection, confirm your regulatory position.

### Is this a medical device?

Run through the FDA's three-part test:
1. Is the software a medical device under 21 CFR 201(h)?
2. Does enforcement discretion apply (administrative, general wellness, low-risk clinical decision support)?
3. If it is a device, what class?

Model selection affects this analysis. A model that produces a specific diagnosis recommendation is higher risk than a model that flags a case for human review. How you scope the model's output determines whether you have a Class II or Class III device.

### If it is a device: what does your submission need?

For a 510(k): performance data on a representative test dataset, substantial equivalence to a cleared predicate, human factors validation, cybersecurity documentation.

For De Novo: same as 510(k) plus a risk-benefit analysis establishing a new regulatory classification.

For PMA: clinical study data, typically prospective, showing safety and effectiveness.

The model you select must be capable of meeting the evidence standard for your pathway. A model that peaks at 87% sensitivity on a benchmark dataset may not meet the clinical evidence threshold for your specific indication.

### Algorithm Change Protocol

If you expect to update the model after clearance (and you should plan for this), define your Predetermined Change Control Plan before submission. The PCCP specifies what types of changes can be made without a new submission. Model selection affects what changes are possible under a PCCP. Some architectures support incremental retraining within a PCCP; others require a new submission for any model update.

---

## Common Selection Failures

**Failure 1: Selecting for benchmark performance instead of operational fit**
A model that achieves 99% accuracy on a curated benchmark dataset running on a GPU cluster is irrelevant if the deployment target is a community hospital with standard hardware that requires sub-3-second inference. Operational requirements define the viable solution space. Benchmarks narrow the field within that space.

**Failure 2: Ignoring data residency until after vendor selection**
A contract signed before confirming that PHI cannot leave the hospital network creates a problem that has no good solution. Data residency requirements must be confirmed in Step 1, not after procurement.

**Failure 3: Fine-tuning when prompting would work**
Fine-tuning takes weeks, requires labeled data, creates a revalidation obligation for every model update, and adds complexity to your regulatory submission. If prompt engineering on a strong foundation model produces acceptable performance, that is the right choice for a first deployment. You can always fine-tune later when you have production data showing where the model fails.

**Failure 4: Not accounting for model drift**
A model selected based on performance at deployment will perform differently 12 months later as the distribution of real-world inputs shifts. The vendor contract and your post-market surveillance plan need to address how model drift is detected and handled before you sign.

**Failure 5: Treating model selection as a one-time decision**
The model you deploy first is not the model you will use in 18 months. The field moves fast. Lock-in risk is real. Evaluate vendor switching costs, data portability, and integration abstraction before committing to a model or vendor for multi-year terms.

---

## Model Selection Summary Scorecard

Use this to compare options before committing.

| Criterion | Weight | Option A | Option B | Option C |

| Meets latency requirement | 20% |
| Meets data residency requirement | 20% | 
| Performance on target task and population | 20% | 
| Regulatory pathway compatibility | 15% | 
| Integration with existing EHR/PACS | 10% | 
| Explainability for clinical use case | 10% | 
| Commercial terms and switching cost | 5% | | 

Score each option 1 through 10 per criterion. Apply the weight. The option with the highest weighted total is not automatically the right choice, but it forces the tradeoffs into the open where the full team can see them.

---

## Related Frameworks in This Repo

- [AI Product Development in a Regulated Environment](ai-product-development-regulated-environment.md)
- [AI Guardrails in Healthcare](ai-guardrails-healthcare-framework.md)
- [AI Healthcare Operations Product Strategy](ai-healthcare-operations-product-strategy.md)
- [SaMD Classification Decision Tree](samd-classification-decision-tree.md)

---

*Built from active product strategy and vendor assessment work across pharma AI, AI diagnostics, and digital health platform deployments in regulated clinical settings.*
