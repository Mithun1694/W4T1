# Tabletop Exercise: "Quiet Weekend" Scenario

## Format

This is a staged tabletop exercise — information is revealed progressively as "injects," the way a real IR tabletop is run, rather than all at once. The point is to test decision-making under incomplete information against the plan in `docs/incident_response_plan.md`, not to solve a puzzle with full visibility from the start.

See `tabletop_exercise_walkthrough.md` for how this was actually worked through, phase by phase.

---

## Inject 1 — Friday, 4:50 PM

A billing department employee reports to the IT helpdesk that she clicked a link in an email that looked like it was from the clinic's insurance-claims clearinghouse, asking her to "re-verify" her portal login. The page asked for her network username and password. She entered them before noticing the URL looked slightly wrong, and immediately reported it.

*(This is all that's known at this point. No confirmed compromise yet — just a reported phishing click with credential entry.)*

## Inject 2 — Friday, 5:40 PM

The Technical Lead checks recent login activity for that employee's account and finds a successful VPN login from an unfamiliar external IP address, about 20 minutes after the phishing report — the employee says she did not initiate that login herself and had already logged off for the day.

## Inject 3 — Friday, 7:15 PM

Overnight monitoring shows the same account being used to access the shared file server, including folders well outside what a billing employee would normally touch — including a folder used to stage nightly EHR database exports for the disaster-recovery backup process.

## Inject 4 — Saturday, 2:10 AM

An automated alert fires: unusually large outbound data transfer from the file server to an external IP address, over roughly 45 minutes, then stops. Total volume transferred is estimated at several gigabytes.

## Inject 5 — Saturday, 6:30 AM

A clinic manager arriving early for weekend urgent-care hours reports that several administrative workstations (not the EHR terminals used directly for patient charting, which are on a separate subnet) are displaying a ransom note and inaccessible files.

## Inject 6 — Saturday, 8:00 AM

Initial check by the Technical Lead confirms the EHR database server itself (separate from the EHR terminals) sits on the same subnet as the affected file server and administrative workstations — meaning it is potentially at risk even though no ransom note has appeared on it yet.

---

## Exercise objectives

Participants (in this solo-run version, just myself working through IC/Technical Lead/Clinical Liaison decision points explicitly) should determine, at each inject:

1. What severity level applies at this point in time (per Section 3 of the IR plan), and does it change as new injects arrive?
2. Who needs to be notified/activated at this point?
3. What containment action (if any) is appropriate right now, and what's the patient-safety consideration?
4. What's still unknown that needs to be identified before further action?
