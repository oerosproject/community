Spec 0005: Hardware Acceleration for Raspberry Pi 4/5
=====================================================

Title: Enable VC4/V3D GPU acceleration for ROS images on Raspberry Pi 4 and 5
Owner: @robwoolley
Status: Draft
Date: 2026-06-22

Goal
- ROS GUI/visualization tooling (RViz, rqt, Gazebo GUI) runs with GPU acceleration on RPi4/RPi5 images rather than software rendering.
- Acceptance criteria:
  - Images for RPi4 and RPi5 enable the VC4/V3D KMS stack (`vc4-kms-v3d`) with Mesa GLES/EGL drivers.
  - `glmark2-es2` (or equivalent) reports hardware rendering (V3D renderer string), not llvmpipe.
  - At least one ROS visualization tool launches and renders accelerated on the target.

Scope
- In scope: kernel/devicetree overlay config, Mesa/VC4 driver enablement, image feature wiring for RPi4 and RPi5, validation method.
- Out of scope: QEMU/VIRGL acceleration (C item, separate spec); Wayland-vs-X11 desktop policy beyond what acceleration requires; non-RPi boards.

Dependencies
- meta-raspberrypi BSP layer.
- Builds on the priority matrix (`specs/matrix.md`) — RPi4/RPi5 are top-priority boards.

Design/Proposal
- Enable the `vc4-kms-v3d` overlay and ensure `MACHINE_FEATURES`/distro config pulls Mesa with `gallium`/`v3d`/`vc4` drivers.
- Confirm GBM/EGL/GLES packages are in the image and that the ROS GUI stack links against them.
- Document any RPi4 vs RPi5 differences (V3D 4.2 vs 7.1, firmware/config.txt).

Testing
- Acceptance: `glmark2-es2` shows the V3D renderer on both boards; an RViz/rqt session renders accelerated.
- Manual: validate on physical RPi4 and RPi5 hardware (ties to Spec 0007 remote-target workflow).
- Reference `templates/TEST_PLAN_TEMPLATE.md`.

Rollout
- Sep 2026. Order: (1) RPi5 accelerated + verified; (2) RPi4 accelerated + verified; (3) document config and known limitations.
