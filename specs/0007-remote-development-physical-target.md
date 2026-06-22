Spec 0007: Remote Development with Physical Target
==================================================

Title: Develop, deploy, and debug ROS on a physical board from the dev host
Owner: @robwoolley
Status: Draft
Date: 2026-06-22

Goal
- A developer cross-builds in the devcontainer (Spec 0003) and iterates on a physically connected board (RPi5, Orin) without reflashing for every change.
- Acceptance criteria:
  - A documented workflow deploys a built ROS workspace/package to a running target over the network and runs it there.
  - Remote debugging (gdbserver or equivalent) attaches from the dev host to a process on the target.
  - The same target can serve as a CI test endpoint (`TEST_TARGET = "simpleremote"`) for Spec 0002.

Scope
- In scope: SDK-based cross-build-to-target deploy loop, remote run + debug workflow, ssh/network setup, hooks for using the target as a runqemu/testimage `simpleremote` endpoint.
- Out of scope: A managed multi-board test farm (future); board bring-up/flashing of the base image (assumed already running); GUI forwarding (B item).

Dependencies
- Spec 0003 (devcontainer/SDK) — provides the cross toolchain.
- Spec 0002 (automated testing) — consumes the `simpleremote` target hook.
- Spec 0005 (HW accel) — validated against the same physical RPi targets.

Design/Proposal
- Deploy loop: `colcon build` in the SDK, then rsync/scp the install space to the target and source it there; document the minimal on-target runtime image requirements.
- Debugging: ship `gdbserver` in the target image; document `gdb` multiarch attach from the SDK environment; note VS Code remote-debug config.
- CI reuse: expose target ssh credentials/IP to the OE test framework so `testimage` can run `simpleremote` against real hardware.

Testing
- Acceptance: edit -> build -> deploy -> run on a physical RPi5 without reflashing; attach gdb and hit a breakpoint.
- Integration: a CI job runs a smoke test on the physical target via `simpleremote`.
- Manual: validate on RPi5 first, then an Orin-class board.
- Reference `templates/TEST_PLAN_TEMPLATE.md`.

Rollout
- Oct 2026. Order: (1) deploy-and-run loop documented and working on RPi5; (2) remote gdb; (3) wire as a CI `simpleremote` endpoint; (4) extend to Orin.
