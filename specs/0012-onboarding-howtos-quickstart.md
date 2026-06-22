Spec 0012: Onboarding How-Tos and Quickstart
============================================

Title: End-to-end developer journey: build, QEMU, SDK, deploy, on-hardware development
Owner: @robwoolley
Status: Draft
Date: 2026-06-22

Goal
- A cohesive set of how-tos plus a quickstart script that walks a developer through the full meta-ros workflow, from a clean machine to developing ROS on real hardware.
- Acceptance criteria — a developer can, following these docs:
  1. Run a quickstart script that builds a Yocto Project / OpenEmbedded image with meta-ros for the priority target.
  2. Boot and develop against that image locally in QEMU via the `runqemu` script.
  3. Install and use the cross-compiled Yocto SDK to build a ROS application.
  4. Deploy a built image/application to supported hardware (RPi5 first).
  5. Develop on the laptop while connected to hardware running ROS (edit/build/deploy/run loop + remote debug).
- Each stage has a copy-pasteable, verified command path for the default target (Jazzy/Scarthgap).

Scope
- In scope: the quickstart build script, and five how-to sections (build, QEMU/runqemu, SDK cross-compile, hardware deploy, on-hardware development) stitched into one journey.
- Out of scope: building the underlying *capabilities* — those live in other specs (this spec is the connective documentation + script that drives them); Ubuntu->Yocto concept mapping (Spec 0004 is the conceptual companion this links to).

Dependencies
- ADR 0001 (bitbake-setup) — the quickstart script wraps bitbake-setup so the tool can change without rewriting docs.
- Spec 0001 (CI) — the script builds the same smoke target CI builds, so instructions stay verified.
- Spec 0002 (runqemu/testimage) — provides the QEMU dev/test path.
- Spec 0003 (devcontainer/SDK) — the SDK the cross-compile section consumes; devcontainer is the alternative low-friction on-ramp.
- Spec 0007 (remote physical target) — supplies the deploy + on-hardware develop/debug loop.
- Spec 0004 (migration guide) — conceptual companion; this spec is the hands-on journey.
- `specs/matrix.md` — default distro/release/board.

Design/Proposal
- Quickstart script: a single entry script that runs bitbake-setup (ADR 0001), selects the priority target, and builds an image; fails fast with actionable messages on missing host deps.
- Stage 1 (build): wrap the script; document expected time cold vs warm (warm via the shared sstate mirror, ADR 0003) and disk requirements.
- Stage 2 (QEMU): `runqemu` invocation for the qemuarm64 image, including networking so ROS nodes are reachable; point at the testimage flow (Spec 0002) for automated runs.
- Stage 3 (SDK): build/install the SDK (`populate_sdk`), source the environment, `colcon build` a sample ROS app; reuse the devcontainer path (Spec 0003) as the no-local-install option.
- Stage 4 (deploy): flash/deploy the image to RPi5; reference HW-accel config (Spec 0005) for GUI tools.
- Stage 5 (on-hardware dev): the cross-build -> deploy -> run -> remote-debug loop from Spec 0007, without reflashing per change.
- Keep every command tied to a specific distro/release so the blocks can be CI-linted later.

Testing
- Acceptance: a developer completes all five stages on the default target reaching ROS running on an RPi5; log and resolve friction.
- Doc CI (stretch): extract and run the command blocks (build + runqemu + SDK build) nightly so the journey cannot silently rot.
- Manual: validate the QEMU and SDK stages on a Linux laptop; note Docker/podman differences for the devcontainer path.
- Reference `templates/TEST_PLAN_TEMPLATE.md`.

Rollout
- Sep 2026 (developer-onboarding milestone). Order: (1) quickstart build script + Stage 1; (2) QEMU/runqemu; (3) SDK cross-compile; (4) hardware deploy; (5) on-hardware development loop; (6) external review pass.
