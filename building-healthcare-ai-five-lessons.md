# Building AI for Healthcare: What Five Failed Prototypes Taught Me

We launched our diagnostic AI tool last month. Two hospitals are using it. Two more go live in the next few weeks. Before this success, we killed five prototypes. Here's why each one died and what we learned.

---

## Prototype 1: The "Perfect" Algorithm

Built for 99.2% accuracy on benchmark datasets. Took 14 seconds per scan.

Hospitals need results in under 3 seconds for emergency workflows.

**Lesson: Operational requirements beat academic metrics.**

Benchmark performance is a research objective. Clinical deployment is an engineering objective. The gap between them is where most healthcare AI prototypes die. Before writing a model architecture, write the operational requirements: latency, hardware constraints, concurrent user load, failure behavior. Then build to those constraints.

---

## Prototype 2: The Dashboard

Beautiful UI. 17 different visualizations. Radiologists looked at it twice, then went back to their existing tools.

**Lesson: Adding another screen to a clinician's workflow is adding friction, not value.**

Clinicians are not users who need to be onboarded. They are professionals with existing workflows optimized over years of practice. A new tool has to fit into that workflow or it will be ignored. The right question is not "how do we get clinicians to use our interface?" It is "where in the existing workflow does the AI output belong, and how do we put it there?"

Shadow the clinicians before designing anything. Watch every click, every workflow interruption, every workaround they have built because the existing tools don't do what they need.

---

## Prototype 3: The Black Box

Fed raw DICOM files directly into the neural network. When it flagged a case as high-risk, doctors asked: "Why?"

We had no good answer.

**Lesson: Explainability is not a feature request. It is a regulatory and clinical requirement.**

A clinician who cannot understand why an AI made a recommendation cannot take responsibility for acting on it. They will not use it. Regulators expect evidence that the AI's outputs are interpretable and that clinicians can apply appropriate judgment. Visual heatmaps, confidence scores, and feature attribution are not nice-to-have design choices. They are the minimum viable evidence layer for clinical AI.

Build explainability into the architecture from the start. Retrofitting it is expensive and often requires retraining.

---

## Prototype 4: The Integration Nightmare

A working standalone tool. IT departments needed 3 months to integrate it with existing PACS systems.

**Lesson: Your integration strategy is your deployment strategy.**

A diagnostic AI tool that cannot connect to the radiology workflow within a reasonable timeframe will not be deployed regardless of its clinical performance. Hospital IT environments are complex, heterogeneous, and under-resourced. Every integration dependency you introduce is a deployment risk.

The right architecture question at the start of the project is not "what does the model need?" It is "how does data flow in this hospital, and how do we attach to that flow with the least possible friction?" DICOM standards exist for exactly this reason. Use them.

---

## Prototype 5: The Data Hog

Required 50,000 labeled cases to reach acceptable performance. Most hospitals have 500 to 1,000 relevant cases per year.

**Lesson: Data availability dictates architecture choices, not the other way around.**

Designing a model architecture and then discovering the training data requirement cannot be met by the target deployment environment is a sequencing failure. The first question in model design is not "what architecture achieves the best benchmark performance?" It is "how much labeled data will be available from the hospitals we are targeting, and what approaches are feasible within that constraint?"

Active learning, transfer learning from related domains, and federated learning approaches exist specifically to address this problem. They are not fallback options. For most healthcare AI deployments, they are the primary architecture choice.

---

## What Worked: Version 6

- Inference in 1.2 seconds on standard hospital hardware
- Integrates directly into existing radiology workflows via DICOM tags
- Visual heatmaps showing exactly what the model is detecting
- 94% accuracy with 2,000 training cases using active learning
- Deployed via Docker containers with one-click installation

---

## The Build Process That Emerged

**Weeks 1 to 2: Shadow clinicians**
Watch every click, every workflow interruption, every workaround. Do not design anything yet. Just watch.

**Weeks 3 to 8: Build the minimum viable integration, not the minimum viable product**
The integration is the product. A standalone tool with no deployment path is a demo. Build the DICOM connection, the workflow attachment point, and the output delivery mechanism before building the AI features.

**Weeks 9 to 12: Test with real data on real hardware in real clinical environments**
Benchmark datasets are not clinical environments. Hospital hardware is not a GPU cluster. Latency numbers from your development environment are fiction until validated on the actual deployment target.

**Weeks 13 to 16: Iterate on what breaks, not on what benchmarks say**
The feedback that matters comes from the clinicians using the tool in their actual workflow, not from held-out test set performance. Build the feedback loop first.

Six months from concept to first hospital deployment.

---

## The Core Insight

Healthcare AI is 20% algorithms and 80% infrastructure.

The machine learning is the tractable part. Given enough time and data, you can hit a performance target. The hard part is: latency constraints that eliminate most model architectures, EHR and PACS integration complexity that can block deployment for months, explainability requirements that affect model design from the start, clinical workflow integration that requires deep understanding of how clinicians actually work, and regulatory documentation that needs to be built alongside the product, not after it.

We understood early that we were building infrastructure, not software. That framing changed every architectural decision we made.

---

## Practical Checklist: Before You Build Healthcare AI

Before sprint 1:
- [ ] Operational requirements documented: latency, hardware target, concurrent load
- [ ] Clinical workflow mapped: where does the AI output go, who sees it, in what context
- [ ] Integration pathway confirmed: DICOM tags, HL7/FHIR, EHR API, or PACS plugin
- [ ] Training data availability confirmed: how many labeled cases, from which sites, over what time period
- [ ] Explainability approach defined: heatmaps, confidence scores, feature attribution
- [ ] Regulatory pathway confirmed: SaMD classification, submission type, clinical evidence requirement
- [ ] Deployment architecture defined: container strategy, installation process, update mechanism

If any of these are blank, the project is not ready to build.

---

## Related Frameworks in This Repo

- [SaMD Classification Decision Tree](samd-classification-decision-tree.md)
- [AI Product Development in a Regulated Environment](ai-product-development-regulated-environment.md)
- [AI Guardrails in Healthcare](ai-guardrails-healthcare-framework.md)
- [AI Patient Services Framework](ai-patient-services-framework.md)

---

*Written from direct experience building and deploying FDA-regulated AI diagnostic tools in clinical settings.*
