Spec 0017: Recipe Upreving and Python Upstreaming
=================================================

Title: Uprev stale meta-ros common-layer recipes and upstream shared Python deps to meta-python
Owner: @robwoolley
Status: Draft
Date: 2026-06-22

Goal
- Reduce drift and duplication by upreving outdated recipes in the meta-ros common layers and moving general-purpose Python dependencies upstream to meta-python where they belong.
- Acceptance criteria:
  - An audit identifies stale recipes in `meta-ros/meta-*/recipes-*/*.bb` and the Python recipes that are generic enough to live in meta-python.
  - A tracked set of upstreaming PRs to meta-python is opened, with meta-ros consuming them instead of carrying local copies.
  - Priority-matrix builds stay green throughout (no regressions from the moves).

Scope
- In scope: recipe-staleness audit, identify meta-python upstreaming candidates, open/track upstream PRs, switch meta-ros to the upstreamed recipes.
- Out of scope: The auto-upgrade automation itself (Spec 0014 runs auto-upgrade-helper); license/SPDX work on the same recipes (Spec 0009).

Dependencies
- Spec 0014 (auto-upgrade-helper automation) — surfaces upgrade candidates this spec acts on.
- Spec 0009 (Superflore SPDX) — touches the same recipes; coordinate to avoid churn.
- Upstream meta-python review process (collaboration goal).

Design/Proposal
- Audit: script a comparison of common-layer recipe versions against upstream/latest; produce a prioritized list.
- Classify: which recipes are ROS-specific (stay) vs general Python (upstream to meta-python).
- Upstream incrementally: one cohesive PR per dependency group; once accepted, drop the local copy and depend on meta-python.
- Guard with CI: every move validated on the priority matrix before the local copy is removed.

Testing
- Acceptance: upstreamed recipes accepted in meta-python; meta-ros builds green consuming them with local copies removed.
- Reference `templates/TEST_PLAN_TEMPLATE.md`.

Rollout
- Sep 2026 (ongoing collaboration). Order: (1) staleness audit; (2) classify upstreaming candidates; (3) incremental meta-python PRs; (4) switch meta-ros + remove local copies as each lands.
