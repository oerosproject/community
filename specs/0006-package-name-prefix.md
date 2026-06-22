Spec 0006: Package-Name Prefix (ros-<distro>-<BPN>)
==================================================

Title: Add a ROS-distro prefix to generated package names
Owner: @robwoolley
Status: Draft
Date: 2026-06-22

Goal
- Generated meta-ros packages carry a distro prefix (e.g. `ros-jazzy-<BPN>`) so multiple ROS distros can coexist in one image/feed without name collisions.
- Acceptance criteria:
  - Superflore-generated recipes/packages emit `ros-<distro>-<BPN>` names consistently.
  - The priority-matrix images still build and boot with the renamed packages.
  - A documented migration path exists for users referencing the old names.

Scope
- In scope: naming scheme definition, Superflore generation changes, dependency/provide remapping, image recipe updates, migration notes.
- Out of scope: Actually building two distros side-by-side in one image (C item, Spec TBD — this spec is the enabling prerequisite); non-generated hand-written recipes beyond what the rename requires.

Dependencies
- BREAKING CHANGE — requires an RFC before implementation (packaging policy is explicitly RFC-gated per `rfc/README.md`). Author `rfc/0002-package-name-prefix.md` first.
- Superflore tooling; relates to the Superflore/licensing epic.
- Unblocks the C-priority "Noetic + ROS 2 side-by-side in one image" item.

Design/Proposal
- Define the canonical scheme (`ros-<distro>-<BPN>`) and where it is applied (PN, PROVIDES, RDEPENDS, RPROVIDES, and any `*-default-runtime` aliases).
- Implement in the Superflore generator so the rename is systematic, not per-recipe.
- Provide compatibility `PROVIDES`/`RPROVIDES` aliases for a deprecation window so existing image recipes do not break overnight.

Testing
- Acceptance: full priority-matrix build green with prefixed names; an image installs and boots; dependency resolution succeeds.
- Regression: confirm no unprefixed leftovers via a package-name lint over the generated feed.
- Reference `templates/TEST_PLAN_TEMPLATE.md`.

Rollout
- Sep 2026, gated on RFC acceptance. Order: (1) RFC accepted; (2) generator change behind a flag; (3) build matrix with aliases; (4) flip default; (5) document migration and deprecation window.
