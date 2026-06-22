Spec 0013: Wrynose bitbake-setup Configurations
===============================================

Title: Provide bitbake-setup configurations for the Wrynose matrix cells
Owner: @robwoolley
Status: Draft
Date: 2026-06-22

Goal
- Declarative bitbake-setup configurations exist for the Wrynose-based priority matrix cells, so contributors and CI reproduce a Wrynose meta-ros build with one command.
- Acceptance criteria:
  - A bitbake-setup config per priority Wrynose cell (Wrynose x {Jazzy, Lyrical} x {RPi4, RPi5, qemuarm64}) fetches the correct layer revisions and writes a working `local.conf`/`bblayers.conf`.
  - `bitbake-setup` + a build from each config produces a booting image (validated via Spec 0002 where applicable).
  - The quickstart script (Spec 0012) and CI (Spec 0001) consume these configs unchanged.

Scope
- In scope: bitbake-setup config files for Wrynose cells, pinned layer revisions, machine/distro selection, sstate-mirror wiring (ADR 0003).
- Out of scope: bitbake-setup vs kas decision (settled in ADR 0001); Scarthgap configs (parallel work, same pattern, separate task); BSP bring-up beyond config.

Dependencies
- ADR 0001 (bitbake-setup chosen as the orchestration tool) — this spec implements that decision for Wrynose.
- ADR 0003 (sstate caching) — configs point `SSTATE_MIRRORS`/`PREMIRRORS` at the S3 mirror.
- `specs/matrix.md` — defines which Wrynose cells are priority (Wrynose is a top-priority Yocto LTS release).
- Spec 0001 (CI) and Spec 0012 (quickstart) are the primary consumers.

Design/Proposal
- One config per matrix cell, sharing a common base for layer set + meta-ros, overriding `MACHINE`/`DISTRO`/ROS distro per cell.
- Pin layer revisions to known-good Wrynose-compatible commits; document the update process (relates to the recipe auto-upgrade item).
- Wire the sstate/DL_DIR mirror (ADR 0003) so first builds are warm.
- Keep configs the single source of truth shared by local quickstart and CI to prevent "works in CI only" drift.

Testing
- Acceptance: each Wrynose config builds a booting image; qemuarm64 cell validated via runqemu/testimage (Spec 0002).
- Regression: a config-only change that breaks a build is caught by the CI matrix (Spec 0001).
- Reference `templates/TEST_PLAN_TEMPLATE.md`.

Rollout
- Aug 2026 (CI matrix milestone, alongside Scarthgap). Order: (1) qemuarm64/Wrynose/Jazzy config building; (2) RPi5 + RPi4 cells; (3) add Lyrical; (4) wire into CI and the quickstart script.
