# Hotwash / Lessons Learned — "Quiet Weekend" Exercise

Per Section 4.6 of the IR plan, every exercise (not just real incidents) should produce a lessons-learned review. This is that review.

## What the plan got right

- **The default-to-Medium rule for anything touching patient data** (Section 3) did real work in this exercise — it forced early escalation at Inject 1, before there was any confirmed compromise, which meant the team wasn't starting from zero once things got serious at Inject 4.
- **Separating "IC decides" from "Clinical Liaison advises on patient-safety impact"** meant containment decisions near clinical systems weren't purely technical calls — the plan's insistence on consulting the Clinical Liaison before touching anything clinical-adjacent (Injects 5 and 6) is exactly the kind of thing that's easy to skip under time pressure if it isn't written down as a required step.
- **The distinction between "file server" and "EHR terminal"** containment decisions worked well — the plan correctly didn't require the same heavyweight sign-off for isolating a clearly non-clinical system (Inject 4) as it did for anything EHR-adjacent (Inject 6), which kept the response fast where speed mattered and careful where caution mattered.

## What the plan missed or should clarify

1. **No explicit timeline for password reset / session revocation on a reported phishing click.** The plan jumps straight into severity classification but doesn't say "reset credentials within X minutes of a reported click" as a standing, always-do-this-immediately action. In the exercise I did this instinctively, but a newer team member might not, and it should be codified.
2. **The 2 AM / weekend escalation path isn't explicit enough.** Section 3 says High severity gets "immediate" activation, but the plan doesn't specify an actual on-call/paging mechanism for after-hours incidents. In a real Saturday 2 AM scenario, "immediately" only works if there's a real on-call rotation and contact method defined — this needs a dedicated on-call section added to the plan.
3. **No guidance on what "confirmed vs. suspected" data exposure actually requires technically.** The plan references confirmed patient data exposure as a severity/notification trigger, but doesn't define what level of technical evidence counts as "confirmed" versus "suspected" — this ambiguity could cause real disagreement between IC and Legal/Compliance during an actual incident about whether the notification clock has started.
4. **The plan doesn't address a scenario where the attack spans a shift change** (Friday evening into Saturday morning, as in this exercise) — handoff between whoever's on point isn't addressed at all, and that's a realistic gap most timelines will have.

## Recommended follow-up actions

| Action | Owner | Priority |
|---|---|---|
| Add a defined on-call/paging rotation and after-hours escalation procedure to Section 3 | Director of IT | High |
| Add "immediately reset credentials and revoke sessions" as a standing first action on any reported phishing click, independent of severity classification | Technical Lead | High |
| Define technical criteria for "confirmed" vs "suspected" data exposure, in consultation with Legal/Compliance | Legal/Compliance Advisor + Technical Lead | Medium |
| Add explicit shift-handoff procedure for incidents spanning multiple work shifts | Incident Commander | Medium |
| Re-run this same scenario in 6 months after the above changes, to confirm they actually close the gaps | Incident Commander | Low (scheduled) |

## Overall assessment

The plan held up well on the structural decisions that matter most in healthcare specifically — patient safety consultation before clinical-system containment, and defaulting to caution on anything patient-data-adjacent. The gaps found were mostly about operational specificity (exact timelines, on-call mechanics) rather than the underlying decision framework being wrong — which is a reasonable outcome for a plan going through its first real tabletop test.
