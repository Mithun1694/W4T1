# Tabletop Exercise Walkthrough: "Quiet Weekend"

This is how I actually worked through each inject in `tabletop_exercise_scenario.md`, applying the roles and phases from `docs/incident_response_plan.md` at each step. Written in the order decisions were made, not cleaned up in hindsight.

---

## Inject 1 (Friday, 4:50 PM) — Phishing report

**Phase: Identification**

**Severity call:** Low, provisionally. A reported phishing click with credentials entered is common and often contained by a quick password reset — but per Section 3, anything potentially touching patient data defaults to at least Medium until scope is confirmed, and this touches an account with access to billing systems that connect to patient records. I escalated the classification to **Medium** immediately rather than waiting to see if anything else happened.

**Action taken:**
- Technical Lead resets the employee's password and revokes any active sessions immediately — this is a fast, low-cost action that doesn't need to wait for full investigation.
- IC notified per Medium-severity escalation criteria; full IR team put on alert but not yet formally activated, since there's no confirmed compromise yet, just a credible risk.

**What's still unknown:** Whether the credentials were actually used by an attacker before the reset, and what that account can access.

---

## Inject 2 (Friday, 5:40 PM) — Unauthorized VPN login found

**Phase: Identification → Containment begins**

**Severity call:** This confirms actual unauthorized access occurred before the reset closed it. Still Medium, but now confirmed rather than provisional — the reset in Inject 1 may not have been fast enough if the attacker already established a foothold (e.g., a scheduled task, another new credential).

**Action taken:**
- Full IR team formally activated per plan (IC, Technical Lead, Communications Lead, Clinical Liaison, Legal/Compliance advisor looped in).
- Technical Lead begins checking for persistence — did that VPN session create anything (new accounts, scheduled tasks) beyond just browsing.
- IC decision: no clinical system disconnection yet — nothing so far indicates clinical systems are affected, and the Clinical Liaison confirms it's not yet justified to disrupt Friday-evening clinical operations on this information alone.

**What's still unknown:** What the attacker actually did with that VPN access during the session.

---

## Inject 3 (Friday, 7:15 PM) — File server access outside normal scope

**Phase: Identification, scope expanding**

**Severity call:** Still Medium, but the specific detail that the attacker reached the *nightly EHR export staging folder* is the moment this stops being "a billing account got phished" and starts being "this could become a patient-data incident." I flagged this explicitly as the trigger to start pre-positioning for a possible escalation to **High**, even without confirmed data theft yet.

**Action taken:**
- Legal/Compliance Advisor formally looped in at this point (per plan: Legal is consulted before any external notification, but proactively looping them in early here means they're not starting cold if this does escalate).
- Technical Lead begins reviewing what was actually in that staging folder and whether any of it was accessed/copied, not just whether the folder was browsed.
- IC decision: still holding off on disconnecting anything — no confirmed exfiltration yet, and an overnight/weekend clinical system disruption is a significant cost to impose on a "possible" rather than "confirmed" risk.

**What's still unknown:** Whether data actually left the network, or if the attacker just browsed the folder.

---

## Inject 4 (Saturday, 2:10 AM) — Large outbound transfer detected

**Phase: Identification confirmed → Escalation to High**

**Severity call:** This is the clear escalation point. A large outbound transfer from the folder holding EHR export data is, per Section 3, a **High** severity incident — likely patient data exposure. This should have triggered immediate full escalation the moment the alert fired, not waited for business hours.

**Action taken:**
- IC declares High severity and activates External IR Support per plan, even though it's 2 AM on a Saturday — Section 3 is explicit that High severity gets Legal/Compliance and External IR Support "immediately," not "next business day."
- Containment: the file server is isolated from the network immediately. This one doesn't require Clinical Liaison sign-off first, since the file server itself isn't a direct patient-care system — this is a fast, low-ambiguity containment call.
- Communications Lead is notified to begin preparing (not yet sending) internal and potential patient-notification messaging, since Legal/Compliance will need lead time once scope is confirmed.

**What's still unknown:** Exactly what data was in that transfer, and whether the attacker still has any other foothold.

---

## Inject 5 (Saturday, 6:30 AM) — Ransom note on administrative workstations

**Phase: Containment intensifies**

**Severity call:** Still High — this doesn't change the classification, but it does show the incident has moved from "stealthy data theft" to "the attacker no longer cares about staying hidden," which usually means eradication needs to happen fast before further damage (this is a classic double-extortion pattern: steal data first, then deploy ransomware).

**Action taken:**
- IC authorizes immediate network isolation of all affected administrative workstations.
- **Clinical Liaison is explicitly consulted here** per plan, because Inject 6 (below) hasn't happened yet at this point in real time, but the IC proactively asks: "are any of these administrative workstations used in any clinical workflow, even indirectly (e.g., printing prescriptions, scheduling)?" This is the plan's patient-safety-first principle being applied before being forced to.
- Clinical Liaison confirms: administrative workstations are not used for direct patient charting, so isolation can proceed without activating downtime/paper procedures yet — but recommends alerting clinical staff to be ready, given how close this is to clinical systems.

**What's still unknown:** Whether the EHR database server (separate from EHR terminals) is also compromised.

---

## Inject 6 (Saturday, 8:00 AM) — EHR database server shares the affected subnet

**Phase: Containment escalates to Critical consideration**

**Severity call:** This is the moment I escalated to **Critical** consideration per Section 3 — not because patient safety impact is confirmed yet, but because the plan's Critical definition is about *immediate patient safety risk*, and an at-risk EHR database with active weekend urgent-care hours qualifies as needing that level of caution even before confirmed compromise.

**Action taken:**
- IC, in direct consultation with the Clinical Liaison (not after the fact — this is treated as a joint decision per plan), decides to proactively isolate the EHR database server's network segment as a precaution, while confirming EHR terminals can still function using a local cached mode or fail over to the documented paper-backup procedure if needed.
- Clinical Liaison activates staff awareness of the downtime/paper-backup procedure as a precaution, without necessarily forcing full activation yet, so clinical staff aren't caught flat-footed if it becomes necessary mid-shift.
- This is logged explicitly as a precautionary, not confirmed-compromise, action — important for the later after-action review to distinguish "we lost the EHR" from "we chose to protect the EHR."

---

## Where the exercise stopped

This exercise was scoped to run through Identification and Containment decision-making under the escalating scenario; it doesn't simulate the full multi-day Eradication/Recovery timeline in blow-by-blow detail, since those phases are less about time-pressured decision-making and more about methodical technical work. See `hotwash_lessons_learned.md` for the after-action review of how the plan performed across everything above.
