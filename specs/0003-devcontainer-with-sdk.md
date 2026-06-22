Spec 0003: Devcontainer with ROS SDK
====================================

Title: Build a devcontainer that ships the meta-ros cross SDK
Owner: @robwoolley
Status: Draft
Date: 2026-06-22

Goal
- An Ubuntu-fluent ROS developer can `git clone`, open in VS Code / `devcontainer`, and get a working cross-build + ROS development environment without learning bitbake first.
- Acceptance criteria:
  - A `.devcontainer/` (Dockerfile + `devcontainer.json`) produces an image with the meta-ros SDK (`populate_sdk` output) pre-installed and the environment auto-sourced.
  - `colcon build` of a sample workspace succeeds inside the container for the priority target (Jazzy/Scarthgap, aarch64).
  - The same image is reusable as a CI build image (shared with Spec 0001) to avoid drift between local and CI environments.
  - A one-paragraph quickstart in `docs/` gets a new user from clone to first cross-compiled package.

Scope
- In scope: SDK generation recipe/target, devcontainer definition, VS Code ROS extension recommendations (`extensions.json`), sample workspace, quickstart doc.
- Out of scope: GUI/RViz forwarding (relates to "Run GUI tools in container", B); remote physical-target flashing (separate A item); multi-distro SDKs in one container (follow-up).

Dependencies
- meta-ros SDK target (`bitbake ros-image -c populate_sdk` or a `meta-toolchain-ros`-style recipe).
- Spec 0001 reuses this image for runners.
- `specs/matrix.md` for the default target.

Design/Proposal
- SDK: define/confirm a `populate_sdk` target that bundles the ROS sysroot + colcon + ament toolchain. Publish the `.sh` installer as a CI artifact.
- Container: base on the SDK installer; install it to `/opt/ros-sdk` and source the environment script from `/etc/profile.d` and `devcontainer.json` `postCreateCommand`.
- devcontainer.json: pin the SDK version, recommend ROS + C++ + Python VS Code extensions, mount the workspace, set `colcon` defaults.
- Versioning: tag the container image with the ROS distro + Yocto release so local and CI stay in lockstep.

Testing
- Acceptance: clean clone -> open in container -> `colcon build` + `colcon test` of the sample workspace passes.
- CI: build the devcontainer image in the pipeline and run the sample-workspace build as a smoke test so SDK regressions are caught.
- Manual: validate on Linux host with Docker and with `devcontainer` CLI; note any podman differences.
- Reference `templates/TEST_PLAN_TEMPLATE.md`.

Rollout
- Jul 2026 (kickoff DX deliverable). Order: (1) reproducible SDK target; (2) devcontainer builds + auto-sources; (3) sample workspace builds; (4) adopt as CI build image; (5) quickstart doc published.
