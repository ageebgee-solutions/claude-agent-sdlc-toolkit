# Learnings

Things we learned running a team of AI agents on real software. Newest first. We add to this when we hit something worth sharing; there's no schedule.

## 2026-10-11: Prove the deploy gate's rollback before its first production use

We added a deploy gate that checks the new build after release and automatically redeploys the previous commit if the check fails. Before trusting it, we ran it twice on a dev environment: once with a good build (it passed) and once with a deliberately bad build (it failed the gate and redeployed the previous commit). The rollback needs something to roll back to: the running site has to report which build it is. The first production deploy had no such stamp on the running site, so it could not roll back automatically; we knew that in advance and the gate was set to fail loudly instead. We deployed in a quiet window, and that first deploy passed the gate.

**Do this:** drill the failure path on a non-production environment, not just the happy path. Have the running service report its build, because automatic rollback depends on it. If the very first run has no rollback target, say so before you deploy.

## 2026-10-08: Test customer scripts on Linux, not just on your own machine

We published an update script for customers that polls a deployment status link to confirm an install. It passed every test we ran on Windows, but in a customer-style Linux shell it never confirmed an install and timed out with "not confirmed". The script stripped carriage returns from an HTTP header value, but the stripping did not take effect, so the link kept a trailing carriage return and every status check failed. Our Windows shell tools ignore carriage returns, so the tests could not see it. A real run in the Linux shell found it.

**Do this:** before publishing any customer-facing script that handles HTTP headers, line endings or paths, run it in a clean Ubuntu container against fakes that reproduce real behavior (CRLF headers, "not finished yet" answers, a stopped app). Check the script file for stray carriage-return bytes with a script rather than `grep`, since `grep` on a Windows shell can match every line. Then do one real run on the platform customers use.

## 2026-10-09: A dashboard can be wrong while every panel renders

Building an operations dashboard, three things looked fine and were not: several panels refreshing on a timer starved the database behind them; a panel titled "sends per hour" actually grouped jobs by when they were created, which differs from when messages went out; and a token that lacked alert permissions meant the alert we described did not exist.

**Do this:** ship dashboards with auto-refresh off, title each panel with what it truly measures, compare every number to the source of truth before calling it done, and never report an alert as live until you have seen it in the alert list.

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
