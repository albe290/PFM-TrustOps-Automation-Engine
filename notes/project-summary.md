# Project Summary

## PFM TrustOps Automation Engine

The **PFM TrustOps Automation Engine** is a production-minded n8n workflow built to automate first-pass firm submission triage in a trust-focused operational environment.

This project was designed to simulate how a platform like Prop Firm Match could scale intake, validation, duplicate detection, and reviewer routing without losing control, consistency, or auditability.

Rather than building a generic automation demo, this workflow focuses on a real operational problem: how to process firm submissions in a way that reduces manual effort while preserving trust and review quality.

---

## Business Problem

Trust-focused platforms eventually run into the same operational issues:

- manual firm vetting becomes slow
- duplicate submissions create wasted effort
- inconsistent intake quality causes rework
- not every submission deserves the same level of review
- reviewers need better context when handling exceptions
- operational decisions become harder to explain and audit as volume grows

Without a structured TrustOps workflow, these problems create friction, slow down review cycles, and weaken consistency.

This project addresses those issues by combining **validation-first workflow design**, **deterministic risk scoring**, and **risk-based review routing**.

---

## What the Workflow Does

The workflow automates the early decisioning stages of TrustOps intake:

- accepts a firm submission payload
- validates whether the submission is usable
- isolates invalid submissions into a separate path
- checks for possible duplicate firms
- routes duplicates into controlled review
- applies deterministic risk scoring to clean submissions
- routes cases by risk level
- prepares audit and reviewer-facing outputs on the current visible review path

This creates a more structured and scalable intake process.

---

## Workflow Architecture

### High-Level Flow

`When clicking 'Execute workflow' → Set Mock Firm Submission → Validate Submission → Is Submission Valid? → Mock Duplicate Check → Is Duplicate Found? → Clean Submission Placeholder → Score Risk → Route by Risk Level → Low / Medium / High Risk branches`

### Detailed Flow

1. **When clicking 'Execute workflow'**  
   Manual trigger used to run the workflow in a controlled demo and testing setup.

2. **Set Mock Firm Submission**  
   Creates the mock intake payload used for downstream validation and routing.

3. **Validate Submission**  
   Applies validation logic to determine whether the firm submission contains the fields and formatting needed to continue safely.

4. **Is Submission Valid?**  
   Decision gate that splits usable submissions from invalid ones.

5. **Invalid Submission Placeholder**  
   Represents the isolated lane for malformed, incomplete, or otherwise invalid submissions.

6. **Mock Duplicate Check**  
   Runs duplicate detection logic to determine whether the firm may already exist in the system.

7. **Is Duplicate Found?**  
   Decision gate that separates duplicate submissions from clean submissions.

8. **Duplicate Review Placeholder**  
   Represents the controlled handling lane for firms requiring duplicate review.

9. **Clean Submission Placeholder**  
   Represents a submission that passed validation and duplicate screening.

10. **Score Risk**  
    Applies deterministic business rules to assign a risk score and risk level.

11. **Route by Risk Level**  
    Routes the submission into one of three downstream handling lanes:
    - Low Risk
    - Medium Risk Review
    - High Risk Escalation

12. **Low Risk Placeholder**  
    Represents a lighter-touch downstream path.

13. **Medium Risk Review Placeholder**  
    Represents the current visible analyst review path.

14. **High Risk Escalation Placeholder**  
    Represents a stronger escalation path for the highest-risk items.

15. **Prepare Audit Log**  
    Builds structured audit information for the visible review path.

16. **Prepare Reviewer Notification**  
    Prepares reviewer-facing context for downstream handling.

> In the current visible build, the audit log and reviewer notification nodes are shown on the medium-risk review branch.

---

## Security and Control Design

The workflow was designed with control points embedded directly into the routing logic.

### 1. Input Control
**Nodes involved:**
- Set Mock Firm Submission
- Validate Submission
- Is Submission Valid?

This is the first trust boundary in the workflow. Incoming submission data should be treated as untrusted until it passes validation. This helps prevent malformed or incomplete records from moving into later decision stages.

### 2. Integrity Control
**Nodes involved:**
- Mock Duplicate Check
- Is Duplicate Found?

Duplicate checks act as record-integrity controls. They help prevent repeated review work, fragmented records, and inconsistent handling of the same firm.

### 3. Risk Control
**Nodes involved:**
- Score Risk
- Route by Risk Level

The risk-scoring layer acts like a policy control. It determines how much scrutiny a submission deserves before it reaches downstream handling.

### 4. Governance Control
**Nodes involved:**
- Low Risk Placeholder
- Medium Risk Review Placeholder
- High Risk Escalation Placeholder

These branches reflect different treatment paths based on risk. This is where human-in-the-loop review and escalation become part of the control design.

### 5. Audit Control
**Nodes involved:**
- Prepare Audit Log
- Prepare Reviewer Notification

These nodes preserve decision context and reviewer handoff information. Good automation should not just make a decision — it should preserve why the decision was made and what happens next.

---

## Why This Project Matters

This project demonstrates a more mature automation mindset.

It is not just about moving records through a workflow. It is about asking the right operational questions at each stage:

- Is the input usable?
- Is this firm already known?
- How risky is this submission?
- Who should handle it next?
- What evidence should be retained?

That is what makes this project valuable as a portfolio piece. It shows workflow design with business logic, control thinking, and reviewer-aware routing.

---

## Business Value

This workflow is designed to help a company:

- reduce time spent on invalid submissions
- reduce duplicate analyst effort
- improve consistency in first-pass triage
- focus human attention where it matters most
- improve auditability of operational decisions
- scale intake operations without scaling manual effort at the same rate

### Pain points solved

- slow manual vetting
- duplicate review work
- inconsistent triage
- limited reviewer context
- weak visibility into routing outcomes
- poor separation between low-risk and high-risk handling

---

## Current Build Status

### Completed
- workflow trigger
- mock submission setup
- validation logic
- valid / invalid branching
- duplicate check branching
- clean submission routing
- deterministic risk scoring
- low / medium / high risk routing
- medium-risk audit log preparation
- medium-risk reviewer notification preparation

### Represented as placeholders
- valid submission placeholder
- invalid submission handling lane
- duplicate review lane
- clean submission output lane
- low-risk downstream lane
- expanded medium-risk analyst process
- high-risk escalation process

This placeholder structure is intentional. It keeps the workflow modular and ready for production expansion.

---

## Example Risk Thinking

The workflow uses deterministic risk scoring to support explainable decisioning.

In a production version, risk factors could include:

- missing support contact
- missing policy URLs
- inconsistent firm metadata
- duplicate indicators
- unusually aggressive commercial terms
- weak transparency signals
- incomplete submission attributes

Deterministic scoring is a strong first-pass design because it is easier to explain, test, and audit than opaque automation.

---

## Production Roadmap

### Phase 2
- replace mock nodes with live integrations
- connect duplicate checks to a real data source
- store audit logs in a durable system
- connect notifications to Slack or email
- add retry logic and failure handling
- improve reviewer handoff structure

### Phase 3
- add dashboards and operational analytics
- track SLA by review lane
- add approval workflows for escalations
- add fuzzy duplicate detection
- add AI-assisted reviewer summaries after deterministic checks
- add workflow health monitoring and alerting

---

## Suggested Metrics

A more production-ready version of this workflow could measure:

- validation failure rate
- duplicate detection rate
- low / medium / high risk distribution
- percentage of cases auto-triaged
- review turnaround time
- high-risk escalation count
- audit log generation success rate
- notification success rate
- workflow retry / failure counts

---

## Interview Summary

A strong way to explain this project is:

> I built a production-minded TrustOps automation workflow in n8n to support first-pass firm submission triage. The workflow validates incoming submission data, isolates invalid records, checks for duplicates, applies deterministic risk scoring, and routes cases into appropriate downstream review lanes. I designed it to reduce manual review effort while preserving trust, consistency, and auditability.

---

## Resume Value

This project demonstrates:

- n8n workflow design
- validation-first automation architecture
- deterministic risk scoring
- risk-based routing
- TrustOps process thinking
- human-in-the-loop workflow design
- audit-ready operational controls
- production-minded automation structure

---

## Author

**Albert Glenn**  
Cybersecurity Engineer | AI Automation Builder | Trust and Control-Focused Workflow Design
