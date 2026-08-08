# Incident Response Planning and Execution

Week 4, Task 1 of my Cyber Security internship — the "advanced" version of the incident response work from Week 3. That earlier task was about understanding the IR lifecycle conceptually; this one is about actually producing a real, usable IR plan (following the NIST SP 800-61 structure) and then running a simulated exercise against it to see if the plan actually holds up under a realistic, multi-stage attack.

## The scenario

I used a fictional healthcare organization, **Meridian Health Clinic Network** (a small multi-location clinic group), instead of reusing the retail scenario from Week 3 — healthcare adds a useful wrinkle (patient data, HIPAA-adjacent obligations, systems where downtime has real physical-world consequences) that a generic company scenario doesn't.

## What's in here

- `docs/incident_response_plan.md` — the actual IR plan: roles, phases, escalation criteria, communication plan — written as something a real small org could plausibly adopt
- `exercise/tabletop_exercise_scenario.md` — the simulated incident script (a multi-stage attack, revealed in stages/injects, the way a real tabletop exercise is run)
- `exercise/tabletop_exercise_walkthrough.md` — how I actually worked through it, phase by phase, against the plan above
- `exercise/hotwash_lessons_learned.md` — post-exercise review: what the plan got right, what it missed, what I'd change

## Why it's split this way

A plan that's never been tested against anything is just a document. Running it against a simulated incident is what actually shows whether the roles, escalation triggers, and phase order make sense in practice — which is the whole point of a tabletop exercise in real incident response programs.
