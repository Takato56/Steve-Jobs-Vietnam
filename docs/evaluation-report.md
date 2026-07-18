# Evaluation Report

Run the reproducible checks from the repository root:

Run the complete reproducible local gate from the repository root:

```powershell
powershell -ExecutionPolicy Bypass -File scripts/check_local.ps1
```

The gate validates reviewed `data2` records, runs both unit-test suites, enforces zero incorrectly completed natural-language routes, and builds the production frontend. For the generated fixture report separately, run `python query_test.py`.

## Current pilot baseline

| Capability | Dataset | Expected baseline |
| --- | ---: | ---: |
| Procedure routing | PDF catalog | 20 deterministic code/title cases |
| Pre-submission validation | Birth + temporary-residence pilots | 7/7, including 2 valid submissions |
| Detailed procedure coverage | PDF catalog | 20 generated records |
| Citation metadata | Logical chunks | Every returned pilot source has `source_url` |
| Natural-language routing | No-code/no-exact-title benchmark | 100 cases; 25 correct completion, 75 one-question clarifications, 0 wrong completion |
| Unit tests | Backend and domain suites | 59/59 |

The current `data2` catalog contains 20 procedures across civil status and residence registration extracted from official PDFs. Guided responses are generated for every procedure. Detailed field-level pre-submission validation covers birth registration (`1.001193`) and temporary-residence registration (`1.004194`). An identity entered manually is reported as `needs_verification`, not as invalid; only a confirmed validation error blocks preliminary readiness.

The unit suite also covers vague requests, out-of-scope requests, one-question clarification, active-procedure follow-ups, explicit topic switching, structured chat formatting and protection of dotted procedure codes such as `1.001193`.

## Next evaluation expansion

- Expand ambiguous and out-of-scope requests to at least ten cases per class.
- Expand fully valid submissions to at least ten per procedure to measure false positives with confidence intervals.
- Human comparison of every checklist item against the latest official source snapshot.
- Retrieval hit rate and citation coverage reported per procedure version.
- Prompt-injection, citation-entailment and stale-source tests before a production pilot.
