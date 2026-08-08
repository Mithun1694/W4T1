# Incident Response Plan — Meridian Health Clinic Network

**Document status:** Training exercise deliverable (fictional organization)
**Framework reference:** NIST SP 800-61 Rev. 2, Computer Security Incident Handling Guide
**Scope:** All clinical and administrative IT systems operated by Meridian Health Clinic Network, including the electronic health record (EHR) system, clinic workstations, and the shared patient scheduling/billing platform.

---

## 1. Purpose and Objectives

This plan defines how Meridian Health Clinic Network detects, responds to, and recovers from cybersecurity incidents affecting its systems or data. The objectives, in priority order, are:

1. Protect patient safety — several systems in scope directly support active clinical care.
2. Protect the confidentiality of patient health information.
3. Contain and eradicate the incident with minimal disruption to clinical operations.
4. Meet legal/regulatory breach-notification obligations.
5. Capture lessons learned to reduce the likelihood and impact of future incidents.

## 2. Incident Response Team & Roles

| Role | Responsibility | Primary Contact |
|---|---|---|
| Incident Commander (IC) | Owns the overall response, makes final containment/escalation calls, coordinates across teams | Director of IT |
| Technical Lead | Runs the hands-on technical investigation and containment/eradication work | Senior Systems Administrator |
| Communications Lead | Manages internal updates, patient/public communication, and regulatory notification drafting | Practice Operations Manager |
| Clinical Liaison | Represents clinical staff, advises on patient-safety impact of any containment action (e.g., taking the EHR offline) | Chief Nursing Officer |
| Legal/Compliance Advisor | Advises on breach notification obligations and regulatory requirements | External Counsel (retained) |
| External IR Support | Deep forensic/technical support beyond internal capacity | Third-party IR firm (retainer) |

A RACI-style rule of thumb used throughout this plan: the **Incident Commander decides**, the **Technical Lead executes**, the **Clinical Liaison and Communications Lead advise and inform**, and **Legal/Compliance is consulted before any external notification goes out**.

## 3. Incident Classification & Escalation Criteria

| Severity | Definition | Example | Escalation |
|---|---|---|---|
| Low | Isolated, contained, no patient data or clinical system impact | Single workstation malware, no spread | Technical Lead handles, IC notified |
| Medium | Multiple systems affected OR any clinical system impact, no confirmed data exposure | Phishing compromise of one staff account with access to scheduling data | Full IR team activated |
| High | Confirmed or likely patient data exposure, OR any disruption to active clinical care systems | Ransomware affecting the EHR, confirmed data exfiltration | Full IR team + Legal/Compliance + External IR Support activated immediately; Executive Director briefed |
| Critical | Immediate patient safety risk | Clinical systems down during active patient care with no safe fallback | All of High, plus activation of downtime/paper-backup clinical procedures |

Any incident touching patient health information is treated as at least Medium severity by default until scope is confirmed, given the regulatory stakes of getting this wrong.

## 4. Incident Response Phases

### 4.1 Preparation

- IR plan reviewed and re-approved annually, or after any major system change.
- Tabletop exercises run at least twice a year (see `exercise/` folder for an example).
- Offline, immutable backups maintained for the EHR and billing systems, tested quarterly.
- Contact list (this document, Section 2) kept current; verified each quarter.
- Clinical staff trained on manual/paper-based downtime procedures in case clinical systems must be taken offline.

### 4.2 Identification

- Sources of detection: EDR alerts, unusual EHR access patterns, staff-reported phishing or suspicious activity, external notification (e.g., a vendor or law enforcement).
- On any suspected incident, the Technical Lead performs an initial triage within 30 minutes of report: what's affected, is it ongoing, does it touch patient data or clinical systems.
- IC is notified immediately for anything that could plausibly be Medium severity or above — default to escalating rather than waiting for full confirmation.

### 4.3 Containment

- **Short-term:** Isolate affected systems from the network immediately. For any clinical system, the Clinical Liaison is consulted before disconnection to confirm safe fallback procedures are ready first (patient safety takes priority over speed of containment).
- **Long-term:** Once immediate spread is stopped, stand up clean, isolated interim systems if clinical operations need to continue during a longer investigation.
- All containment actions are logged with timestamp and rationale — this record feeds directly into both the eradication phase and any later regulatory inquiry.

### 4.4 Eradication

- External IR Support engaged for any Medium-severity-or-above incident to assist with root cause analysis.
- Root cause must be identified and closed (compromised account reset + MFA added, vulnerability patched, malicious tooling removed) before any system is reconnected.
- Affected systems are rebuilt from known-clean sources rather than "cleaned in place" wherever feasible, given the sensitivity of the data involved.

### 4.5 Recovery

- Systems restored in priority order: patient-safety-critical clinical systems first, then billing/administrative systems.
- Each restored system is monitored closely for at least 14 days post-incident for signs of reinfection or renewed unauthorized access.
- Clinical Liaison confirms each clinical system is safe to return to active use before it's reconnected — a technical "all clear" alone is not sufficient sign-off for patient-facing systems.

### 4.6 Lessons Learned

- Formal after-action review within 10 business days of incident closure.
- Report covers: timeline, root cause, what worked, what didn't, and specific follow-up actions with owners and deadlines.
- Any regulatory reporting obligations (e.g., breach notification timelines) are tracked separately by Legal/Compliance and confirmed as complete before the incident is formally closed.

## 5. Communication Plan

- **Internal:** IC provides a status update to the full IR team and Executive Director at least every 4 hours during an active High/Critical incident.
- **Patients/public:** Any communication implying or confirming a data breach is reviewed by Legal/Compliance and approved by the IC before release — no individual should communicate breach status externally without this review.
- **Regulatory:** Legal/Compliance owns tracking and meeting any applicable breach notification deadlines once patient data exposure is confirmed or reasonably suspected.

## 6. Plan Maintenance

This plan is a living document. It should be updated after every real incident (via the Lessons Learned phase) and every tabletop exercise, and reviewed in full at least annually regardless of whether any incident occurred.
