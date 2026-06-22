Spec 0010: Project Metrics Dashboard
====================================

Title: Dashboard for contributor, PR-lifecycle, and CI-health metrics
Owner: @robwoolley
Status: Draft
Date: 2026-06-22

Goal
- A dashboard that makes the roadmap's success criteria measurable: PR merge time, contributor growth, and CI success rate, tracked over time against a baseline.
- Acceptance criteria:
  - Publishes the three headline metrics from `ROADMAP.md` success criteria: PR merge time (to show the -30% target), contributor count month-over-month, and CI pipeline success rate.
  - A baseline snapshot is captured at first run so improvement is measurable, not just current state.
  - Refreshes automatically (scheduled job), with no manual data entry.

Scope
- In scope: data collection from GitHub (PRs, contributors) and GitLab CI (pipeline outcomes), metric definitions, a published view, scheduled refresh.
- Out of scope: The ROS-package build-status dashboard (separate B item — "does the recipe exist / last successful build / version"); per-developer performance ranking (track project health, not individuals).

Dependencies
- Spec 0001 (CI/CD Platform) — source of CI success-rate data.
- RFC 0003 (release cadence) — consumes cadence/sync metrics; this dashboard is where they surface.
- GitHub meta-ros repo (PR/contributor data per `COMMUNITY.md`).

Design/Proposal
- Collection: scheduled job pulls PR lifecycle and contributor data via the GitHub API and pipeline outcomes via the GitLab API; store time-series so trends (not just snapshots) are visible.
- Metric definitions (write these down to avoid ambiguity):
  - PR merge time = median time from PR open (ready-for-review) to merge, excluding draft time.
  - Contributor count = unique authors with a merged PR per rolling month.
  - CI success rate = passed pipelines / total on the integration branch.
- Presentation: a lightweight published view (static site generated in CI, or a hosted dashboard); keep it cheap and low-maintenance.
- Baseline: snapshot current values on first run and store as the reference line for the -30% goal.

Testing
- Acceptance: dashboard renders all three metrics with a baseline and at least one refresh interval of history; numbers reconcile against a manual spot-check.
- Automation: scheduled refresh runs green in CI and updates the published view.
- Reference `templates/TEST_PLAN_TEMPLATE.md`.

Rollout
- Dec 2026 (review-and-iterate milestone). Order: (1) define metrics + capture baseline; (2) automate collection; (3) publish the view; (4) wire scheduled refresh; (5) review against success criteria when publishing the 2027 roadmap.
