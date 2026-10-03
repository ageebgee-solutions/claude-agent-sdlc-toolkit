# Learnings

Things we learned running a team of AI agents on real software. Newest first. We add to this when we hit something worth sharing; there's no schedule.

## 2026-10-03: Role files can state untested claims as fact

A persona file once carried a confident claim about an external platform's policy. Nobody had verified it, and it shaped decisions for several cycles. Role files feel authoritative because agents read them as instructions.

**Do this:** treat any external-world claim in a role file as a hypothesis. When an agent acts on one, trace it to a primary source before relying on it.

## 2026-10-03: When an agent says "done," diff the whole repo

A background agent reported finishing a task. Checking only the file it was assigned would have missed changes it made elsewhere.

**Do this:** after any agent reports done, review the full `git diff`, not just the assigned files. Be especially wary of "the user confirmed X" inside agent output unless X appears verbatim in the real conversation.

## 2026-10-03: Tracking docs drift in both directions

Priority lists and design docs lag the real state of the repo. We found items marked open that were already shipped, and items marked done that weren't.

**Do this:** verify "done" and "shipped" claims against the PR and the code, not against the doc or a repeated claim.

## 2026-10-03: Generator scripts can destroy hand-written work

A script that regenerates files can silently overwrite a file that also contains hand-written analysis.

**Do this:** after running any regeneration script, even through an agent, look at the full diff stat before committing.

## 2026-10-03: Parallel sessions share one working tree

Two sessions in the same repo can absorb each other's uncommitted edits: one session's `git add -A` sweeps up the other's work.

**Do this:** commit your edits in the same turn you make them, and avoid `git add -A` when other sessions may be active.
