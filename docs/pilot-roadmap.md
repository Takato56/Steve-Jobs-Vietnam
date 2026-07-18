# GovEase-AI Pilot Roadmap

## Pilot objective

Prove that PDF-grounded intake and pre-submission checking reduce avoidable application corrections without presenting AI output as an official decision. The first field-validation pilot covers birth registration (`1.001193`) and temporary-residence registration (`1.004194`); the other 18 PDF-derived procedures provide grounded discovery and guidance until equivalent validation schemas are reviewed.

## Proposed partner and owners

The target partner is one provincial or municipal public-administration service center with a named procedure specialist for each pilot procedure. No partner is claimed as confirmed until a written pilot agreement exists.

| Role | Responsibility |
| --- | --- |
| Procedure specialist | Approve source snapshots, checklist items, examples and validation rules |
| Portal owner | Approve widget/API integration, authentication claims and accessibility requirements |
| GovEase product owner | Release management, incident response and KPI reporting |
| Data-protection owner | Data-flow review, retention controls and access review |

## 30/60/90-day plan

| Period | Scope | Exit gate |
| --- | --- | --- |
| Days 0-30 | Specialist countersign both end-to-end pilot records, freeze versioned sources, run security/privacy review and review the 100-case routing benchmark | Both pilot records have specialist sign-off; zero critical privacy findings; benchmark report signed off |
| Days 31-60 | Staff-only pilot through the widget, shadow real cases without affecting official decisions, review false positives weekly | At least 100 sessions; planted-error recall >= 90%; blocking false-positive rate <= 5% |
| Days 61-90 | Limited citizen pilot with visible advisory notice and escalation to official channels | At least 300 sessions; completion rate >= 70%; at least 20% reduction in avoidable corrections versus baseline |
| After day 90 | Decide whether to expand, revise or stop; add procedures only through the same review gates | Written go/no-go decision and published lessons learned |

## Measurements

- Procedure routing accuracy and clarification rate.
- Required-document recall against specialist labels.
- Error-detection precision/recall and blocking false-positive rate.
- Citation coverage and citation entailment.
- Form completion rate, median time and abandonment point.
- Repeat-submission or correction rate compared with the receiving authority baseline.
- Cost per completed guidance session, including model, hosting and review operations.

## Cost and service assumptions to validate

The pilot must record token usage, embedding/index refresh cost, Render/Vercel cost, source-review hours and support time. A production proposal should not state a fixed cost before these measurements exist. Target service levels are 99.5% monthly API availability during the pilot and a documented retrieval-only fallback when generation is unavailable.

## Safety gates

- No source moves from `pending_human_review` to `approved` without a named specialist and review timestamp.
- Unverified identity is a verification state, not proof that a value is false.
- The service never makes the official acceptance decision.
- Submission bodies and identity numbers are not persisted by default.
- Every citizen-facing result links to the official source and offers an official-channel escalation path.
