Spec 0008: ROS-in-Containers
============================

Title: Build and run ROS node container images with OpenEmbedded
Owner: @robwoolley
Status: In Progress
Date: 2026-06-22

Goal
- Produce minimal OCI container images for ROS nodes built with bitbake, runnable on the target and on non-OE hosts, with a path for GUI tools.
- Acceptance criteria:
  - `bitbake` produces an OCI/container image (`IMAGE_FSTYPES += "container"` / `oci`) containing a ROS node and only its runtime closure.
  - The image runs under a standard runtime (docker/podman) on the target and on at least one non-OE host (Ubuntu/Debian).
  - A documented pattern exists for running a GUI ROS tool (e.g. rqt) from a container.
  - A documented comparison of the OE-built-image path vs ROS devcontainers/Rocker for development.

Scope
- In scope: container image recipes/classes for ROS nodes, runtime-closure minimization, multi-host run validation, GUI-from-container pattern, dev-workflow comparison (devcontainers/Rocker).
- Out of scope: Full orchestration/compose stacks; the SDK devcontainer (Spec 0003 covers developer environment, this spec covers shipping node images); registry/publishing policy (defer to RFC 0001 scope discussion).

Dependencies
- Spec 0001 (CI) builds and could publish these images.
- Spec 0003 (devcontainer/SDK) — distinct concern (dev env vs shippable node image); cross-reference to avoid overlap.
- Relates to dashboard items: "Create containers with bitbake", "Run GUI tools in container", "Containerize ROS nodes with OE", "ROS devcontainers and/or Rocker", "Run OE containers on other hosts".

Design/Proposal
- Image: use the OE `container` image type (and `oci-image`/`oci` tooling) to emit a rootless-friendly OCI image with only the ROS node's runtime dependencies; avoid pulling the full ROS desktop.
- Minimization: separate build-time (SDK) from runtime; confirm `IMAGE_INSTALL` is the node + ros runtime, not dev/test packages.
- GUI: document the X11/Wayland socket + device passthrough pattern (and its security caveats); ties loosely to the RPi HW-accel work (Spec 0005) for accelerated GUI.
- Portability: validate the same image runs on the target and on Ubuntu/Debian hosts (the "other hosts" dashboard item) under docker and podman; record differences.

Testing
- Acceptance: build a node container; run it on target, Ubuntu host, and Debian host; launch one GUI tool from a container.
- CI: build the container image as a pipeline job; smoke-run the node headless.
- Reference `templates/TEST_PLAN_TEMPLATE.md`.

Rollout
- Sep 2026 (work already in progress). Order: (1) minimal node OCI image building; (2) multi-host run validated; (3) GUI-from-container pattern documented; (4) devcontainer/Rocker comparison written.
