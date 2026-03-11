# Idea 9 — Architect ↔ Engineer AI Skills Continuum

## Clarifying Questions
- Target audience size and regions? Mix of architects vs engineers?
- Which practices to prioritize (architecture-as-code, policy-as-code, runtime conformance, flow literacy, decision framing, influencing)?
- Current learning platforms/tools (LMS, labs, sandboxes)? Budget for hands-on environments?
- Preferred delivery mode: self-paced, cohort-based, pair coaching, hackathons? Expected time commitment per week?
- How will proficiency be measured (capability heatmaps, practical assessments, peer reviews, promotion criteria)?
- Existing communities of practice? Executive sponsors for career-path alignment?
- Tech stacks to align labs with (cloud provider, CI/CD, IaC, K8s, API patterns)?
- Expected business outcomes (cycle time, architecture quality scores, retention)?

## Program Architecture
- Capability Framework: Skills matrix with levels (Foundation, Practitioner, Expert) across architecture-as-code, policy-as-code, data/AI literacy, runtime conformance, influencing/storytelling.
- Learning Paths: Role-based (Engineer, Architect, Hybrid “Spec Driver”) mixing theory, labs, and peer coaching.
- Hands-on Labs: Real repos with IaC, service mesh, OPA policies, API specs; auto-graded via CI (GitHub Actions + OPA + unit tests).
- Coaching & Community: Cohort leads, office hours, brown-bags, pairing rotations architect↔engineer.
- Assessment: Practical exams, code/architecture reviews, capability heatmap published to managers; badges integrated into HR/LMS.
- Metrics: Enrollment, completion, assessment pass rate, cycle time deltas on real teams, incident reduction tied to design defects.

## Implementation Plan (10–14 weeks)
1. Define (Weeks 1–2): Confirm skill matrix, levels, success metrics; pick 2–3 anchor teams; select lab tech stack.
2. Design (Weeks 3–5): Build curricula, design 6–8 labs with automated grading, set up LMS/Lab infra, define assessment rubrics.
3. Pilot (Weeks 6–8): Run first cohort (20–30 people), capture baseline vs post metrics, refine labs and coaching model.
4. Scale (Weeks 9–12): Roll out broader cohorts, add advanced modules (architecture decisions under constraints, influencing stakeholders), publish capability heatmaps.
5. Embed (Weeks 13–14): Integrate into performance and hiring; establish community-of-practice cadence and ongoing lab maintenance.

## Deliverables
- Skills matrix + role-based learning paths and rubrics.
- Automated lab environment with grading and sample repos.
- Coaching model and community calendar.
- Capability heatmaps and reporting dashboard tied to business outcomes.

## Risks & Mitigations
- Low engagement: Limit time per week, manager buy-in, cohort incentives.
- Lab upkeep: Version labs with tech baselines; quarterly refresh; designate lab owners.
- Assessment bias: Use blended automated + human scoring with clear rubrics.
- Tool fragmentation: Standardize on a reference stack for labs; allow limited adapters.
