# Bug-Fix Playbook

Agents will happily guess at a fix. This playbook makes them prove the bug exists, prove the fix works, and show both.

## Steps

1. **Reproduce first.** Get the bug to happen on demand: a command, request, or test that fails. If you can't reproduce it, say so and stop. Do not fix what you can't see. Report what you tried and what information is missing.
2. **Write the failing test.** Capture the repro as an automated test that fails for the reported reason. If a test isn't feasible (infrastructure, visual, timing), record the manual repro steps and expected vs. actual result instead, and say why a test wasn't possible.
3. **Find the cause.** List two or three hypotheses, then rule them out one at a time (log the program state, bisect the commit range, narrow the input). Name the confirmed root cause in one sentence before changing code.
4. **Fix the cause, not the symptom.** Keep the change as small as the cause allows. No refactors in the same PR.
5. **Verify with evidence.** The new test passes, the rest of the suite still passes, and the original repro no longer fails. Paste the command output (or before/after result) into the hand-off. "Should work now" is not evidence.
6. **Log the decision** if the fix involved a trade-off or a non-obvious choice (see `decision-log.md`).

## Hand-off format

- **Repro:** how the bug was reproduced
- **Root cause:** one sentence
- **Fix:** what changed and why
- **Evidence:** test names and the pass/fail output before and after

## Rules

- A bug-fix PR with no failing-first test or recorded repro is not done.
- If the cause turns out to be a design problem, stop and escalate to the Engineering Manager and Architect instead of patching around it.
