# Decision Log

A short, committed record of non-obvious decisions, so a human can review the reasoning without re-reading every diff.

## When to add a line

Add one when you chose between real alternatives, accepted a trade-off, skipped something the task implied, or deviated from the plan. Skip routine choices.

## Format

Tab-separated, one line per decision, appended to a file your project chooses (for example `docs/decision-log.tsv`). Never rewrite old lines.

```
date	task	decision	alternatives	why	who
```

| Field | Content |
|---|---|
| date | `YYYY-MM-DD` |
| task | Task or PR ID (e.g. `BE-014`) |
| decision | What you chose, in one sentence |
| alternatives | What you rejected, in one sentence |
| why | The reason, in one sentence |
| who | The agent role |

Keep each field to a sentence. No tabs or newlines inside a field.

## Rules

- Append at the moment you decide, in the same commit as the change it explains.
- The Engineering Manager skims new lines when verifying the Definition of Done and flags any that look wrong.
- Never put secrets, customer data, or personal data in the log.
