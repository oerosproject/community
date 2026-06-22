Spec 0011: Contributor Playbook
===============================

Title: How to propose, implement, and land a change in meta-ros / this workspace
Owner: @robwoolley
Status: Draft
Date: 2026-06-22

Goal
- A single playbook a new contributor can follow to take an idea from proposal to merged PR, reducing review friction and supporting the roadmap's PR-merge-time success criterion.
- Acceptance criteria:
  - A `docs/contributor-playbook.md` covering: where work is tracked (spec vs RFC vs ADR), DCO sign-off, the PR lifecycle, review expectations, and how to find a low-effort first task.
  - A new contributor uses it to land a trivial change end-to-end without 1:1 maintainer hand-holding.
  - Cross-linked from `COMMUNITY.md`, `templates/CONTRIBUTING.md`, and `workspace-mapping.md` so there is one canonical path.

Scope
- In scope: the proposal-to-merge workflow, the spec/RFC/ADR decision tree, DCO/sign-off mechanics, review and merge criteria, "good first issue" guidance, escalation/communication channels.
- Out of scope: Governance changes (`GOVERNANCE.md` owns those); CI internals (Spec 0001); the technical onboarding/build journey (Spec 0012).

Dependencies
- `workspace-mapping.md` (when to write a spec vs RFC vs ADR — this playbook operationalizes it).
- `GOVERNANCE.md` (maintainer decision/RFC-window rules) and `COMMUNITY.md` (channels, meetings).
- `templates/CONTRIBUTING.md` (DCO, PR basics — the playbook is the long-form expansion).
- Spec 0010 (metrics dashboard) measures whether this reduces PR merge time.

Design/Proposal
- Decision tree: small change -> spec; cross-cutting/policy -> RFC; architectural choice -> ADR. Mirror `workspace-mapping.md` so guidance does not drift.
- PR lifecycle: branch from `main`, DCO sign-off, link the spec/RFC, required CI green (Spec 0001), review window, maintainer merge (per `GOVERNANCE.md`).
- Reviewer expectations: what a reviewer checks, target response time, how disagreements resolve (RFC if it is a policy question).
- First-task on-ramp: maintain a labelled set of low-effort issues; tie to the mentorship/contribution-drive milestones.

Testing
- Acceptance: a first-time contributor (ideally external) lands a doc-typo-class PR using only the playbook; capture and resolve friction points.
- Keep the decision tree consistent with `workspace-mapping.md` (a divergence is a defect).
- Reference `templates/TEST_PLAN_TEMPLATE.md`.

Rollout
- Oct 2026 (mentorship / contribution-drive milestone). Order: (1) decision tree + PR lifecycle; (2) reviewer expectations; (3) first-task guidance; (4) cross-link canonical docs; (5) dry-run with a real first-time contributor.
