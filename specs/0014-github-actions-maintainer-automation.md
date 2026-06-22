Spec 0014: GitHub Actions Maintainer Automation
===============================================

Title: Use GitHub Actions for repo/maintainer automation (not builds)
Owner: @robwoolley
Status: Draft
Date: 2026-06-22

Goal
- GitHub Actions automates lightweight, event-driven maintainer tasks (PR triage, issue hygiene, upstream-sync detection, release plumbing) while all compute-heavy builds stay on GitLab CI (Spec 0001).
- Acceptance criteria:
  - A PR opened on GitHub gets: a DCO sign-off check, path/layer labels, and a CI status reported from GitLab via a trigger bridge (no build runs on GHA).
  - A scheduled job detects divergence from upstream `ros/rosdistro` and opens a tracking issue.
  - Each automation is a small, independently reviewable workflow file.

Scope
- In scope: PR automation (DCO, labeler, reviewer assignment, EOL-distro comment), issue automation (triage labels, stale, first-timer greeter), upstream-sync detection (rosdistro drift, recipe-update PRs), release plumbing (changelog/Release on tag), and the GitLab CI trigger+status bridge.
- Out of scope: Running Yocto/meta-ros builds on GHA (explicit project preference — builds stay on GitLab, Spec 0001); the error-report server (separate B item); the metrics dashboard (Spec 0010, though it can read GHA/GitLab data).

Dependencies
- Spec 0001 (CI/CD Platform) — the bridge triggers its pipeline and consumes its status.
- Spec 0011 (contributor playbook) — labeler/greeter/first-issue automation operationalize it.
- RFC 0003 (release cadence) — the rosdistro drift detector is the detection half of its sync job; release plumbing supports its cadence.
- `GOVERNANCE.md` (DCO requirement) and `COMMUNITY.md` (channels the greeter links to).

Design/Proposal
- Architectural boundary: GHA handles GitHub-native events cheaply; GitLab handles builds. The two connect through one bridge, not by duplicating pipelines.
- GitLab bridge: a workflow that calls the GitLab pipeline trigger API on PR events and posts the resulting pipeline status back as a GitHub check — gives contributors green/red on GitHub with zero build compute on GHA.
- Reuse proven patterns from `ros/rosdistro`: `labeler.yaml` (+ labeler-config), `reviewer.yaml`, and the `mergify.yml` EOL-comment approach, adapted to meta-ros layer paths.
- Drift detector: scheduled workflow diffs the generated recipe set against upstream rosdistro state; opens/updates a single tracking issue (the rebuild it implies runs on GitLab).
- Recipe-update PRs: scheduled `auto-upgrade-helper` run opens PRs on GitHub; build validation happens on GitLab via the bridge.
- Keep each concern in its own workflow file so they can be added, reviewed, and disabled independently.

Testing
- Acceptance: a test PR exercises DCO + labeler + GitLab status bridge end to end; a forced rosdistro divergence opens the tracking issue.
- Negative: a missing sign-off fails the DCO check; an unreachable GitLab trigger fails closed (visible, not silently green).
- Reference `templates/TEST_PLAN_TEMPLATE.md`.

Rollout
- First tranche (Sep 2026): GitLab bridge + DCO + path labeler. Then: drift detector and reviewer assignment; then issue hygiene (stale, greeter); then release plumbing and recipe-update PRs.
- DCO and labeler can land earlier (Jul) since they have no GitLab dependency.
