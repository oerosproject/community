Spec 0019: Multi-Distro Side-by-Side Image
==========================================

Title: Build an image with two ROS distros coexisting (e.g. Noetic + ROS 2)
Owner: @robwoolley
Status: Draft
Date: 2026-06-22

Goal
- Demonstrate two ROS distros installed side-by-side in one image without collisions, enabled by the package-name prefix.
- Acceptance criteria:
  - An image builds with two ROS distros (e.g. Noetic + Humble) present, using `ros-<distro>-<BPN>` names.
  - Both distros' core tools run on the booted image without path/package conflicts.

Scope
- In scope: image recipe combining two distros, resolving the runtime/aliasing issues that block coexistence, validation on the target/QEMU.
- Out of scope: The prefix mechanism itself (Spec 0006 / RFC 0002 — this consumes it); production support of every package in both distros (demonstrator first).

Dependencies
- Spec 0006 + RFC 0002 (package-name prefix) — hard prerequisite; this spec is the use case that justifies the prefix.
- Investigate why `ros-default-runtime` / `rosidl-adapter` must be added explicitly (BACKLOG.md) — likely surfaces here as a coexistence blocker.

Design/Proposal
- Build on prefixed packages so the two distros do not collide on shared `BPN`s.
- Identify and resolve meta/alias packages (e.g. `*-default-runtime`) that assume a single distro.
- Validate environment isolation (sourcing one distro's setup does not break the other).

Testing
- Acceptance: image boots; core tools from both distros run; no unresolved file/package conflicts.
- Reference `templates/TEST_PLAN_TEMPLATE.md`.

Rollout
- Gated on Spec 0006 (prefix). Order: (1) prefix landed; (2) two-distro image builds; (3) resolve alias/runtime conflicts; (4) validate both distros run.
