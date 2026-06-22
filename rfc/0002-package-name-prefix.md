RFC 0002: ROS-Distro Package-Name Prefix
========================================

Title: Prefix generated meta-ros package names with the ROS distro (ros-<distro>-<BPN>)
Authors: Rob Woolley (@robwoolley)
Status: Draft
Date: 2026-06-22

Summary
- Generated meta-ros packages are renamed to carry their ROS distro, e.g. `ros-jazzy-rclcpp` instead of `rclcpp`, so multiple ROS distros can coexist in one image, feed, or sysroot without name collisions.

Motivation
- Today a generated package name maps to exactly one ROS distro at a time. Building or installing two distros (e.g. Noetic + a ROS 2 release, or Jazzy + Rolling) into one image collides on shared `BPN`s.
- This blocks the C-priority goal "Build ROS Noetic and ROS 2 side-by-side in one image" and complicates any shared package feed that spans distros.
- It also brings meta-ros naming closer to the established ROS convention (`ros-<distro>-<pkg>` on Debian/`apt`), which is familiar to the Ubuntu-fluent audience the roadmap targets.
- This is a breaking packaging-policy change, so it follows the RFC-first rule in `rfc/README.md`. Spec 0006 is the implementation and is gated on this RFC.

Proposal
- Canonical scheme: `ros-<distro>-<BPN>` for all Superflore-generated packages, applied systematically to `PN`, `PROVIDES`, `RPROVIDES`, and inter-package `DEPENDS`/`RDEPENDS`.
- Generate the rename in the Superflore tooling, not per-recipe, so it is consistent and maintainable.
- Provide compatibility aliases (`PROVIDES`/`RPROVIDES` of the old unprefixed name) for a defined deprecation window so existing image recipes and downstream layers do not break on the flip.
- Roll out behind a generator flag: produce prefixed names opt-in first, validate the full priority matrix, then make prefixed the default.
- Document a migration guide: how to update `IMAGE_INSTALL`, custom recipes, and rosdep-style references.

Alternatives
- No prefix; rely on separate images/feeds per distro. Rejected: defeats the side-by-side goal and the shared-feed use case.
- Prefix only when a collision is detected. Rejected: non-deterministic names are worse for users and tooling than a uniform scheme.
- Use a different separator or ordering (e.g. `<BPN>-ros-<distro>`). Rejected: `ros-<distro>-<BPN>` matches the upstream ROS apt convention and is least surprising.

Compatibility and Migration
- Breaking for anyone referencing unprefixed generated names. Compatibility aliases soften the transition for one deprecation window (length to be set during feedback).
- `*-default-runtime` and similar meta/alias packages must be renamed/aliased consistently; track the known cases (relates to the maintenance item investigating why `ros-default-runtime` / `rosidl-adapter` need explicit adding).
- Image recipes in this project and the migration guide (Spec 0004) must be updated in lockstep with the default flip.

Implementation
- Owner: @robwoolley. Target: Sep 2026, gated on acceptance of this RFC.
- Steps: (1) accept RFC; (2) generator change behind a flag with compatibility aliases; (3) build full priority matrix with prefixed names; (4) package-name lint to catch unprefixed leftovers; (5) flip default; (6) publish migration guide and announce the deprecation window. See Spec 0006 for the detailed plan and tests.

Unresolved questions
- How long is the compatibility-alias deprecation window?
- Do we prefix only ROS 2 distros, or also ROS 1 (Noetic), given the side-by-side use case explicitly pairs them?
- Should the prefix appear in the recipe filename / `PN` only, or also in install paths and `ROS_DISTRO`-scoped directories?
- Coordination with upstream meta-ros: is this project-local, or proposed upstream so the wider community shares the convention?

Decision Log
- (pending) — record accepted scheme, deprecation-window length, and link Spec 0006's implementation PR.
