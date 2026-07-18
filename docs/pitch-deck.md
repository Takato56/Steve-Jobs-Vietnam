# Four-minute Pitch Outline

## Slide 1 - The repeat-visit problem (0:00-0:35)

Use one citizen story: a parent needs birth registration but does not know which of several closely related procedures applies, completes the form incorrectly and discovers the error only after review. State the target outcome: identify, guide, check, then submit.

## Slide 2 - Live citizen flow (0:35-2:15)

1. Enter `Tôi mới sinh con và muốn đăng ký khai sinh.`
2. Show the sourced document checklist and ordered steps.
3. Open pre-submission checking.
4. Demonstrate an invalid identity format, duplicate permanent/temporary address, reversed dates and missing accommodation proof.
5. Correct the blocking errors and show preliminary readiness plus a separate `needs verification` notice.

Demonstrate birth registration code `1.001193`, then briefly show the temporary-residence validation schema `1.004194`. Use the other 18 PDF procedures only for routing and source-grounded guidance.

## Slide 3 - Why the AI is trustworthy (2:15-2:55)

Show the architecture as three controlled layers: grounded retrieval from versioned public sources, structured generation for guidance, and deterministic rules for blocking validation. Explain that the LLM cannot remove deterministic errors and does not make an official decision.

## Slide 4 - Integration and pilot (2:55-3:35)

Open `/widget`, point to the versioned API and summarize the 30/60/90-day pilot. State the measurable gates: routing accuracy, error precision/recall, blocking false-positive rate, completion rate and reduction in avoidable corrections.

## Slide 5 - Honest boundary and close (3:35-4:00)

The detailed pilot covers two procedures: birth registration and temporary-residence registration. The other 18 catalog records support discovery and guidance. Specialist source sign-off is required before a production pilot. Close with the value proposition: fewer repeat visits for citizens and fewer repetitive corrections for receiving staff.

## Likely judge questions

- **What is genuinely AI-native?** Natural-language routing, grounded retrieval, contextual explanation and optional semantic checking; deterministic validation remains separate for reliability.
- **How do you prevent hallucination?** Versioned public sources, retrieved context, citations, structured outputs, human promotion gates and no official-decision claim.
- **Why only two procedures?** The pilot optimizes depth and measurable accuracy before sector-wide expansion.
- **What happens when OpenAI is unavailable?** Catalog, intake, rule validation and retrieval-only responses remain available.
- **Can it verify a CCCD?** Not in the standalone demo. It distinguishes format checking from verified VNeID/DVCQG claims.

## Demo fallback

Before judging, run `docs/demo-runbook.md`, verify `/api/v1/ready`, and keep a short screen recording of the exact flow. The recording is a continuity fallback, not a substitute for the required live URL.
