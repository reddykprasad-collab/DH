# SaMD Classification Decision Tree
## FDA 21 CFR Part 11 Framework for Digital Health Product Teams

This decision tree walks product teams through the FDA's Software as a Medical Device (SaMD) classification process. Use it at the start of any digital health product build to determine regulatory pathway, documentation requirements, and submission strategy before committing to a development plan.

---

## Step 1: Is This Software a Medical Device?

Start here. Not all health software is a medical device under FDA definition.

**Ask:** Does the software meet the FDA's definition of a device under Section 201(h) of the FD&C Act?

A device is any article intended to diagnose, cure, mitigate, treat, or prevent disease, or intended to affect the structure or function of the body.

| Software type | Device? |
|---|---|
| Administrative functions (scheduling, billing, claims) | No |
| General wellness (fitness tracking, calorie counting) | No |
| Clinical decision support (low-risk, physician-facing) | Maybe |
| Diagnostic algorithm (reads imaging, lab data, vital signs) | Yes |
| Drug dosing calculator with therapeutic recommendations | Yes |
| ePRO/eCOA capturing clinical trial endpoints | Yes |
| Remote monitoring for chronic disease management | Yes |

If No: stop here. Standard software development applies. HIPAA may still apply if PHI is handled.

If Yes or Maybe: continue to Step 2.

---

## Step 2: Does FDA Enforcement Discretion Apply?

FDA has stated it will not enforce device requirements on certain low-risk software categories.

**Enforcement discretion applies if the software:**
- Helps patients with diagnosed conditions self-manage without replacing clinical care
- Automates general administrative tasks for providers
- Enables patients to interact with their EHR
- Provides coaching for behavioral health conditions (general wellness only)

**Enforcement discretion does NOT apply if the software:**
- Analyzes patient-specific data to recommend treatment or diagnose disease
- Drives or influences clinical decision-making in high-risk scenarios
- Is used as a companion diagnostic or in a clinical trial as an endpoint capture tool
- Replaces a regulated medical device function

If enforcement discretion applies: document your rationale and maintain a brief regulatory justification memo. No submission required, but document the decision.

If enforcement discretion does NOT apply: continue to Step 3.

---

## Step 3: Determine Device Class

FDA classifies medical devices into three classes based on risk.

### Class I: Low Risk
- General controls only (labeling, manufacturing, reporting)
- Most are exempt from 510(k)
- Examples: general fitness devices, low-risk administrative clinical software

### Class II: Moderate Risk
- General controls plus special controls
- Requires 510(k) premarket notification (or De Novo if no predicate exists)
- Examples: eCOA/ePRO platforms, clinical decision support with moderate risk, remote patient monitoring for chronic conditions

### Class III: High Risk
- Most stringent controls
- Requires Premarket Approval (PMA)
- Examples: AI diagnostics driving treatment decisions, life-sustaining device software, novel therapeutic digital products

**Classification questions to ask:**

1. Does a predicate device exist (a previously cleared similar device)?
   - Yes: 510(k) pathway likely
   - No: De Novo or PMA pathway

2. What is the intended use? Write it in one sentence. The more specific, the better.

3. What is the indications for use? Who is the patient population, what condition, what clinical setting?

4. What happens if the software fails or produces incorrect output? What is the patient harm scenario?

---

## Step 4: Map the Regulatory Pathway

| Scenario | Pathway | Timeline |
|---|---|---|
| Class I, low risk, predicate exists | 510(k) Exempt | No submission |
| Class II, predicate exists | Traditional 510(k) | 3 to 6 months |
| Class II, no predicate | De Novo | 12 months |
| Class III, novel device | PMA | 18 to 36 months |
| Breakthrough device designation | Expedited review | Varies |

---

## Step 5: Document Before You Build

Before writing a line of code, lock these documents:

- Intended Use Statement (one sentence, specific, defensible)
- Indications for Use (patient population, condition, clinical context)
- Device Description (what it does, what it does not do)
- Risk Classification Rationale (Class I, II, or III with justification)
- Predicate Device Identification (for 510(k) pathway)
- Software Requirements Specification (SRS)
- Software Development Lifecycle Plan (IEC 62304 aligned)
- Quality Management System (QMS) basis (21 CFR Part 820 or ISO 13485)

---

## Step 6: 21 CFR Part 11 Applicability (Electronic Records)

If your software creates, modifies, maintains, archives, retrieves, or transmits electronic records that are required by FDA (clinical trial data, adverse event reports, submissions), 21 CFR Part 11 applies.

**Part 11 requirements your product must meet:**

- Audit trails: time-stamped, computer-generated, captures all record creation and modification
- System validation: documented validation that the system does what it is intended to do
- Access controls: unique user IDs, password controls, role-based permissions
- Electronic signatures: linked to the record, non-repudiable, includes printed name, date/time, meaning of signature
- Record retention: electronic records must be protected and retrievable for the required retention period

**eCOA/ePRO specific:**
Clinical trial endpoint capture systems almost always fall under Part 11. Validate the system before first patient use. Maintain a Validation Master Plan and execution records.

---

## Common Classification Mistakes

**Mistake 1: Writing an intended use statement that is too broad.**
Broad intended use expands regulatory scope and creates enforcement risk. Write the narrowest accurate intended use statement possible.

**Mistake 2: Assuming a wellness claim avoids device status.**
If the software also makes a disease claim anywhere in its labeling or marketing, FDA will treat it as a device.

**Mistake 3: Skipping the predicate search.**
A cleared predicate device is the fastest path to market for Class II. Do the predicate search before deciding on pathway.

**Mistake 4: Treating eCOA as general software.**
Any software capturing regulated clinical trial data endpoints is subject to 21 CFR Part 11 and requires formal validation.

**Mistake 5: Starting development before locking intended use.**
Intended use drives everything: design controls, risk management, testing requirements, and submission content. Changing it mid-build is expensive.

---

## Reference Documents

- FDA Guidance: Policy for Device Software Functions and Mobile Medical Applications (2022)
- FDA Guidance: Software as a Medical Device (SaMD): Clinical Evaluation (2017)
- IEC 62304: Medical Device Software Lifecycle Processes
- ISO 14971: Application of Risk Management to Medical Devices
- 21 CFR Part 11: Electronic Records and Electronic Signatures
- 21 CFR Part 820: Quality System Regulation

---

*Built from active regulatory positioning work on FDA-cleared digital health products and clinical trial platforms.*
