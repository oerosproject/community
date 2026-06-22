Spec 0018: ROS-Package Build-Status Dashboard
=============================================

Title: Dashboard of per-package recipe status (exists / last built / version)
Owner: @robwoolley
Status: Draft
Date: 2026-06-22

Goal
- A public dashboard showing, per ROS package, whether a recipe exists, when it last built successfully, and its version — so contributors and users can see coverage and freshness at a glance.
- Acceptance criteria:
  - For the priority matrix, each package shows: recipe present?, last successful build (and for which cell), current version.
  - Data is generated from CI build output, not hand-maintained.
  - Published and refreshed automatically.

Scope
- In scope: collect per-package status from CI build manifests/buildhistory and the Superflore recipe index; render and publish; scheduled refresh.
- Out of scope: Project health metrics — contributors, PR lifecycle, CI success-rate (that is Spec 0010, the metrics dashboard; this is package-coverage, a distinct artifact).

Dependencies
- Spec 0001 (CI) — source of build manifests/buildhistory per matrix cell.
- Spec 0009 (Superflore) — recipe index / generation metadata.
- Spec 0014 (GHA) — may host the scheduled publish job.

Design/Proposal
- Derive status from CI artifacts: parse image manifests / buildhistory for built packages + versions; cross-reference the Superflore-generated recipe set for "recipe exists".
- Per-cell granularity so "last built" is meaningful across the matrix.
- Publish as a static site generated in CI (cheap, low-maintenance), refreshed on a schedule.

Testing
- Acceptance: dashboard reflects a known package's recipe presence, last build, and version correctly after a CI run; spot-check against the manifest.
- Reference `templates/TEST_PLAN_TEMPLATE.md`.

Rollout
- Dec 2026. Order: (1) extract per-package status from CI artifacts; (2) cross-reference recipe index; (3) render + publish; (4) scheduled refresh.
