Spec 0015: Error-Report Server
==============================

Title: Stand up a Yocto error-report server and submit build failures from CI
Owner: @robwoolley
Status: Draft
Date: 2026-06-22

Goal
- A reachable error-report-web instance collects build failures from CI and contributors, making recurring failures visible and triageable.
- Acceptance criteria:
  - An `error-report-web` server is deployed and reachable.
  - CI (Spec 0001) submits failures automatically via `report-error` / `send-error-report`.
  - Contributors can submit local failures, documented in the onboarding/contributor docs.

Scope
- In scope: deploy error-report-web, configure `ERR_REPORT_SERVER`, wire CI submission, document contributor submission.
- Out of scope: Error triage automation/labeling (could later tie to Spec 0014); long-term analytics (Spec 0010/0018 dashboards may read from it).

Dependencies
- Spec 0001 (CI) — the primary submitter.
- ADR 0002 (hosting) — server runs in the same AWS footprint.

Design/Proposal
- Deploy the upstream `error-report-web` app; back it with persistent storage and put it behind the bastion/VPC from ADR 0002.
- Builds inherit `report-error` and set `ERR_REPORT_SERVER` to the instance; CI submits on failure.
- Document the `send-error-report` flow for contributors in the contributor playbook (Spec 0011).

Testing
- Acceptance: a deliberately broken build submits a report that appears in the server UI.
- Reference `templates/TEST_PLAN_TEMPLATE.md`.

Rollout
- Nov 2026. Order: (1) deploy server; (2) CI auto-submit on failure; (3) document contributor submission.
