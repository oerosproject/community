Spec 0004: Ubuntu-to-Yocto Migration Guide
==========================================

Title: Onboarding guide for Ubuntu-fluent ROS developers moving to Yocto/meta-ros
Owner: @robwoolley
Status: Draft
Date: 2026-06-22

Goal
- The roadmap's headline deliverable: a doc that takes a developer who knows `apt`, `rosdep`, and `colcon` on Ubuntu and maps each habit to its meta-ros/Yocto equivalent, ending with a successful first image build.
- Acceptance criteria:
  - A `docs/ubuntu-to-yocto.md` (or docs.ros.org contribution) covering the concept mapping table, environment setup, first build, and adding a package.
  - A reader following it builds the priority target (Jazzy/Scarthgap, RPi5 or qemuarm64) end to end.
  - Reviewed by at least one developer who has not used Yocto before; their blocking questions are folded back in.

Scope
- In scope: concept-mapping (apt->bitbake/IMAGE_INSTALL, rosdep->meta-ros generated recipes, PPA->layer, `colcon build`->SDK workflow), prerequisites, first-build walkthrough, "add your package" walkthrough, common-pitfalls/FAQ.
- Out of scope: Deep bitbake internals; BSP bring-up authoring; SDK reference (Spec 0003 covers the devcontainer path and is linked from here).

Dependencies
- Spec 0003 (devcontainer/SDK) — the recommended low-friction on-ramp the guide points to.
- Spec 0001 quickstart targets — the guide builds the same smoke target CI builds, so instructions stay verified.
- `specs/matrix.md` for the default distro/release/board.
- ADR 0001 (build orchestration tool) — on-ramp command blocks standardize on the tool chosen there.

Design/Proposal
- Lead with the concept-mapping table (Ubuntu habit -> Yocto/meta-ros equivalent -> notes); it is the highest-value asset for the target audience.
- Offer two on-ramps: (A) devcontainer (fastest, Spec 0003); (B) native host build with `bitbake-setup`/`kas`.
- Keep every command block copy-pasteable and tied to a specific distro/release so it can be CI-linted later.
- Cross-link COMMUNITY.md channels for "I'm stuck" and the contributor playbook for "I want to submit a change".

Testing
- Acceptance: a first-time-Yocto reviewer completes the walkthrough and reaches a booting image; log their friction points and resolve blockers before marking Done.
- Doc CI (stretch): extract command blocks and run them in a nightly job so the guide cannot silently rot against meta-ros changes.
- Reference `templates/TEST_PLAN_TEMPLATE.md`.

Rollout
- Jul 2026 (kickoff onboarding deliverable). Order: (1) concept-mapping table; (2) devcontainer on-ramp; (3) native on-ramp; (4) add-a-package walkthrough; (5) external review pass; (6) propose upstream to docs.ros.org.
