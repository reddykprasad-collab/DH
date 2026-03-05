# GLP-1 Digital Support Program Framework
## Patient Support Program Design for Chronic Disease and Weight Management

This framework covers the design, structure, and clinical integration of digital support programs for patients on GLP-1 receptor agonist therapy. Use it to scope a program, define patient touchpoints, align provider workflows, and set success metrics before build begins.

---

## Why GLP-1 Programs Need Structured Digital Support

GLP-1 therapy produces strong clinical outcomes in trials. Real-world adherence tells a different story. Discontinuation rates at 12 months run between 50% and 70% across published studies. The primary drivers are side effect burden in the first 8 to 12 weeks, lack of expectation-setting at initiation, and absence of ongoing behavioral support between clinical visits.

A well-designed digital support program addresses all three. It does not replace the prescribing clinician. It fills the gap between office visits.

---

## Program Architecture

### Phase 1: Onboarding (Weeks 1 to 4)

Goal: set expectations, reduce early discontinuation from side effects.

**Patient touchpoints:**
- Welcome message within 24 hours of first fill confirmation
- Side effect education: nausea, vomiting, injection site reactions, titration timeline
- Expectation calibration: weight loss trajectory by week (slow first 4 weeks is normal)
- First injection support: step-by-step guide, video if available
- Emergency escalation path: clear criteria for when to call the prescriber

**Provider touchpoints:**
- Automated alert if patient reports severe GI symptoms (score above threshold)
- Weekly summary of patient-reported outcomes to care team

---

### Phase 2: Titration Support (Weeks 5 to 16)

Goal: support dose escalation, maintain adherence through the hardest period.

**Patient touchpoints:**
- Weekly check-in: symptom tracking, injection confidence, missed dose logging
- Titration reminders tied to prescription schedule
- Behavioral nudges: meal timing, hydration, protein intake (evidence-based, not dietary prescriptions)
- Progress visualization: weight trend, symptom burden over time
- Peer connection option if program scale supports it

**Provider touchpoints:**
- Flag to care team if patient misses 2 or more check-ins
- Titration readiness summary: patient-reported symptom burden before scheduled dose increase
- Missed dose pattern alerts

---

### Phase 3: Maintenance (Month 4 onward)

Goal: sustain adherence, prevent plateau-related discontinuation.

**Patient touchpoints:**
- Monthly check-in (reduced frequency from weekly)
- Plateau education: weight loss slows at 6 to 9 months, this is expected
- Refill reminders: 7 days before estimated run-out
- Annual review prompt: preparation for clinical visit

**Provider touchpoints:**
- Quarterly outcome summary: weight, adherence rate, symptom burden trend
- Flag if patient has not refilled within 10 days of estimated run-out

---

## Clinical Content Requirements

All content delivered to patients must meet these standards:

- Reviewed by a licensed clinician before deployment
- Aligned with prescribing label indications (no off-label claims)
- Distinguishes between general wellness information and clinical advice
- Includes escalation language: when to contact the prescriber vs. when to seek emergency care
- Compliant with FDA promotional guidelines if sponsored by a pharma manufacturer

---

## Data Collection and Outcomes

### Patient-Reported Outcomes to Capture

| Metric | Frequency | Instrument |
|---|---|---|
| GI symptom burden | Weekly (phase 1 and 2), monthly (phase 3) | Custom 4-item scale or GSRS subset |
| Injection confidence | Weeks 1, 4, 8 | Single item, 5-point scale |
| Missed doses | Weekly | Binary + reason |
| Weight | Weekly (patient self-report) | Numeric entry |
| Quality of life | Monthly | IWQOL-Lite or EQ-5D |
| Program satisfaction | Month 3, Month 6 | NPS + 2 open items |

### Program Success Metrics

- 90-day persistence rate (primary)
- 180-day persistence rate (secondary)
- Side effect-related discontinuation rate vs. control
- Dose titration completion rate
- Provider alert response rate

---

## Integration Requirements

### EHR Integration (if applicable)
- Patient-reported outcomes surfaced in the care team workflow (not a separate portal login)
- Bi-directional: program pulls medication start date and titration schedule from EHR
- Alert delivery via existing care team communication channel (in-basket, secure message)

### Pharmacy Integration (if applicable)
- First fill confirmation triggers program enrollment
- Refill data feeds adherence tracking
- Specialty pharmacy coordination for prior auth status if relevant

### Identity and Access
- Patient authentication: minimum email plus date of birth, ideally SMS-based MFA
- No PHI stored outside HIPAA-compliant environment
- Data retention policy defined before launch

---

## Provider Engagement Model

Digital support programs fail when providers don't know they exist or don't trust the data coming out of them. Build provider engagement in from day one.

**At program launch:**
- One-page clinical summary: what the program does, what data providers receive, how to refer patients
- EHR workflow integration or a documented manual workflow if integration is not available
- Single point of contact for provider questions

**Ongoing:**
- Monthly program performance report to clinical leads
- Quarterly review with prescribing team: what is working, what is generating noise
- Feedback loop: providers can flag content inaccuracies or escalation threshold issues

---

## Regulatory Considerations

| Question | Answer |
|---|---|
| Is this a medical device? | Depends on intended use. General adherence support with no diagnostic or treatment recommendation function: likely not. Symptom scoring that drives clinical recommendations: possibly yes. Get regulatory counsel before launch. |
| Does 21 CFR Part 11 apply? | Only if the program captures data used in a regulated clinical trial. Commercial patient support programs generally do not fall under Part 11. |
| Does HIPAA apply? | Yes, if the program handles PHI. Business Associate Agreement required with all vendors. |
| Does FDA promotional guidance apply? | Yes, if the program is sponsored by the manufacturer of the GLP-1 product. All patient-facing content is considered labeling. |

---

## Pilot Design Recommendation

Before full-scale launch, run a 90-day pilot with a defined patient cohort.

**Pilot parameters:**
- 50 to 150 patients minimum for meaningful persistence data
- Defined enrollment criteria: new starts only, or include existing patients on therapy
- Control group if feasible (patients not enrolled in program)
- Weekly team review of patient flags and content performance
- Go/no-go criteria defined before pilot starts

**Pilot success criteria:**
- 90-day persistence rate 10 percentage points above historical baseline
- Provider satisfaction score above 7 out of 10
- Zero patient safety events attributable to program content

---

*Built from active GLP-1 patient support program design work in primary care and specialty settings.*
