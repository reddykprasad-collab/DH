# AI Patient Services Framework
## Product Design and Clinical Integration Guide for AI-Powered Patient Support

This framework covers how to design, build, and deploy AI-powered patient services in regulated healthcare settings. It addresses product architecture, clinical content boundaries, safety guardrails, regulatory positioning, and outcome measurement.

---

## What AI Patient Services Actually Are

AI patient services use conversational AI, predictive models, or recommendation engines to support patients outside clinical visits. They sit between the patient and the care team, handling the high-frequency, low-complexity interactions that consume clinical bandwidth without adding clinical value.

Done well, they extend the reach of the care team without replacing clinical judgment. Done poorly, they create liability, erode patient trust, and generate noise the care team has to clean up.

The difference is in how you define the boundaries.

---

## Service Categories

### Category 1: Navigation and Access
AI helps patients find the right care, understand their benefits, schedule appointments, and complete pre-visit intake.

- Low regulatory risk
- High volume, high ROI
- Examples: appointment scheduling bots, insurance coverage explainers, provider finder tools

### Category 2: Education and Adherence Support
AI delivers condition-specific education, medication reminders, and adherence nudges based on patient profile and behavior.

- Low to moderate regulatory risk depending on how personalized the recommendations become
- Examples: post-discharge education, chronic disease self-management support, GLP-1 or specialty drug adherence programs

### Category 3: Symptom Monitoring and Triage
AI collects patient-reported symptoms, scores severity, and routes patients to the appropriate level of care.

- Moderate to high regulatory risk
- Requires clinical validation of triage logic
- Examples: remote patient monitoring alerts, oncology symptom management, post-surgical follow-up

### Category 4: Clinical Decision Support (Patient-Facing)
AI analyzes patient data and provides recommendations that influence clinical decisions.

- High regulatory risk
- FDA SaMD classification likely applies
- Examples: AI-driven medication adjustment recommendations, diagnostic pre-screening tools

---

## Product Architecture

### Core Components

**Conversation layer**
The interface patients interact with. Can be SMS, in-app chat, voice, or web. Must handle natural language variation, patient health literacy differences, and non-English speakers if the patient population requires it.

**Clinical content layer**
The knowledge base the AI draws from. Must be clinician-reviewed, evidence-based, and version-controlled. Every content update is a clinical change, not a software change.

**Rules and logic layer**
Defines what the AI can and cannot do. Escalation thresholds, out-of-scope topic handling, safety triggers. This layer must be auditable.

**Integration layer**
Connects the AI to EHR, pharmacy systems, patient identity, and care team workflows. Most patient service failures happen here.

**Human escalation layer**
The path from AI to a human. Every AI patient service needs a defined, fast, reliable escalation path. Patients who hit the edge of what the AI can handle must reach a human quickly.

---

## Clinical Boundaries: What the AI Can and Cannot Do

Define this before build. Write it down. Get clinical sign-off. Put it in the product requirements.

| The AI can | The AI cannot |
|---|---|
| Provide general education about a diagnosed condition | Diagnose a new condition |
| Remind patients to take their medication | Change a medication dose or schedule |
| Collect and score patient-reported symptoms | Interpret symptoms and recommend treatment |
| Route a patient to the right level of care based on symptom severity | Make a clinical triage decision without human review for high-acuity scenarios |
| Answer questions about what a medication does | Advise whether a patient should take or stop a medication |
| Summarize what a patient told their doctor | Add to or modify the clinical record |

Any product that crosses from the left column to the right column without clinical oversight is a liability problem.

---

## Safety Guardrails

### Non-negotiable safety requirements for every AI patient service:

**Suicidality and crisis detection**
The AI must detect crisis language and route immediately to a crisis resource. No exceptions. This is not optional.

**Medical emergency detection**
The AI must detect language indicating a medical emergency (chest pain, difficulty breathing, stroke symptoms) and instruct the patient to call 911 or go to the nearest emergency department. No triage logic, no delay.

**Out-of-scope handling**
When a patient asks something outside the AI's defined scope, the AI must say clearly that it cannot help with that and provide the appropriate next step. It must not attempt to answer.

**Escalation confirmation**
When the AI escalates to a human, it must confirm to the patient that a human will follow up, when, and through what channel.

**Audit trail**
Every conversation must be logged with timestamp, patient identifier, content delivered, escalation events, and outcomes. Retention period defined by applicable regulations.

---

## Regulatory Positioning

### Is the AI a medical device?

Use this quick filter:

1. Does the AI analyze patient-specific data to recommend a specific clinical action? If yes, FDA SaMD classification applies.
2. Does the AI provide general education without patient-specific clinical recommendations? If yes, likely not a device.
3. Does the AI score symptoms and route patients to care levels? This sits in a gray zone. Get regulatory counsel.

### HIPAA requirements
Any AI patient service that handles PHI requires:
- Business Associate Agreements with all vendors in the data flow
- Minimum necessary data access
- Breach notification procedures
- Annual security risk assessment

### State-specific considerations
Telehealth and digital health regulations vary by state. If the AI service operates across state lines, review licensure requirements for the clinical content being delivered.

---

## Care Team Integration

AI patient services that operate in isolation from the care team create problems. Patients tell the AI things they don't tell their doctor. That data has clinical value and needs to get to the right place.

**Integration requirements:**

- Patient-reported data surfaces in the EHR or care team workflow, not in a separate portal that requires a separate login
- Escalation alerts reach the right care team member through the channel they already use
- The care team can see the full conversation history when needed
- There is a defined process for the care team to update the AI's knowledge of a patient (new diagnosis, medication change, care plan update)

---

## Outcome Measurement

### Patient outcomes
- Adherence rate (medication fills, appointment completion, program check-in rate)
- Symptom burden trend over time
- Patient-reported quality of life
- Escalation rate to human care team
- Emergency department utilization (if measurable)

### Program outcomes
- Containment rate: percentage of interactions resolved by AI without human escalation
- Escalation response time: how fast a human responds after AI escalation
- Safety event rate: any adverse event linked to AI-delivered content
- Patient satisfaction: NPS or equivalent at 30, 90, 180 days

### Clinical outcomes (where measurable)
- Condition-specific metrics tied to the program's clinical goals (A1c for diabetes, persistence rate for specialty drugs, readmission rate for post-discharge programs)

---

## Common Failure Modes

**Failure 1: Scope creep in the content layer**
Clinical content reviewers add more detail over time. The AI starts giving more specific guidance than the product was designed for. Review content scope quarterly against the original clinical boundary definition.

**Failure 2: Escalation path breaks down**
The AI escalates correctly but the human on the other end doesn't respond in time. Patients learn the escalation doesn't work and stop using it. Define SLAs for escalation response and monitor them weekly.

**Failure 3: EHR integration is read-only**
The AI collects patient data but it never reaches the care team's workflow. Clinicians don't trust the program because they never see evidence it works. Push data into the workflow, don't ask clinicians to go find it.

**Failure 4: Health literacy mismatch**
Content is written at a 10th-grade reading level for a patient population reading at a 6th-grade level. Engagement drops. Validate readability against the actual patient population before launch.

**Failure 5: No human escalation for after-hours**
The AI operates 24/7 but human escalation is only available 9 to 5. Patients in crisis at 2am hit a dead end. Define the after-hours escalation path before go-live.

---

## Vendor Evaluation Criteria

If buying rather than building, evaluate vendors on:

- Clinical content review process: who reviews it, how often, what is the change control process
- Safety guardrail architecture: how are crisis and emergency scenarios handled, can you see the logic
- EHR integration depth: read-only vs. bi-directional, which EHR systems, what data elements
- Regulatory track record: any FDA enforcement actions, any patient safety events
- Data ownership: who owns the conversation data, can you export it, what happens at contract end
- Audit trail completeness: what is logged, retention period, access controls

---

*Built from active AI patient services product design work in primary care, specialty pharma, and digital health settings.*
