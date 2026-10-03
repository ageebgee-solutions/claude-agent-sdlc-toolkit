# Release Validation Playbook

Every production deployment needs sign-off from **both** QA and product before the Engineering Manager gives the go-ahead. Neither can approve on behalf of the other. Both signals are required.

## Roles

| Role | Responsibility |
|---|---|
| **Engineering Manager** | Prepares the release brief, sequences the work, owns the go/no-go decision, and blocks production until both approve. |
| **QA engineer** | Validates technical correctness: critical paths, regression, edge cases, security controls. Approves from a quality standpoint. |
| **Product sign-off** | Validates product correctness: user-facing flows, acceptance criteria, release-note accuracy. This is you, or a product-owner agent you define. |

## Step 1: Engineering Manager prepares the release brief

- **Version and component** being released
- **What changed**, in plain English
- **Breaking changes**: anything that affects existing customers or integrations
- **Pre-deploy steps completed**: secret rotation, migrations, provisioning
- **Staging URL** where QA and product should validate
- **Acceptance criteria**: the specific outcomes the release must achieve
- **Known risks**: what could go wrong and what to watch for

## Step 2: QA validates technical correctness (against staging)

Tailor this checklist to your system. A starting point:

**Authentication and authorization**
- [ ] Sign-in, sign-out, and session expiry work
- [ ] Invalid or expired credentials are rejected
- [ ] One user or tenant cannot read or act on another's data

**Data and migrations**
- [ ] Migrations applied cleanly on a copy of production-shaped data
- [ ] Existing records still load and behave correctly after the migration

**Integrations and webhooks**
- [ ] Inbound webhooks with a bad signature are rejected, not silently accepted
- [ ] The same event delivered twice is applied once (idempotency)

**Input validation**
- [ ] Malformed or hostile input at API boundaries returns a clean 4xx, not a 500

**Regression**
- [ ] Flows that were not part of this release still work
- [ ] No new errors in the browser console or application logs

## Step 3: Product sign-off validates product correctness (against staging)

- [ ] Every acceptance criterion in the release brief is met
- [ ] All user-facing flows complete without errors
- [ ] Release notes accurately describe the change from a customer's perspective
- [ ] UI copy, labels, and error messages make sense to a non-technical user

## Step 4: Signals required for go-live

```
QA APPROVED       [date] [who]
PRODUCT APPROVED  [date] [who]

EM GO / NO-GO: GO
```

If either is REJECTED, the Engineering Manager works with the engineers to resolve the blockers, then restarts from Step 2 or Step 3 depending on what changed.

## Severity definitions

| Severity | Definition | Release impact |
|---|---|---|
| P0 | Data loss, auth bypass, billing error, wrong entitlements, failure of anything with real-world safety impact | Always blocks release |
| P1 | Feature broken for all users, missing security control | Blocks unless EM and product accept the risk in writing |
| P2 | Feature degraded or an edge case broken | Doesn't block; logged as follow-up |
| P3 | Cosmetic, copy, or minor UX | Doesn't block |

## Post-deploy verification

After the production deploy, the Engineering Manager runs a five-minute sanity check:

- [ ] The production URL loads
- [ ] You can sign in
- [ ] One end-to-end flow works
- [ ] No new errors in logs for the first five minutes

If the check fails, **roll back first and investigate afterward.**
