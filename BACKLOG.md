Backlog
=======

Lightweight holding area for ideas that are tracked but not yet spec-worthy. These were captured from the original roadmap dashboard so nothing is lost. Promote an item to a `specs/` spec (or an RFC/ADR) when it is picked up; remove it from here once it has a spec.

See `specs/README.md` for active specs and `ROADMAP.md` for the scheduled plan.

UX/DX
- (B) Customize LXQt desktop — theme, icons, menus, wallpaper.
- (B) CLI helper tool for deployment / management.
- (C) Enable hardware acceleration for QEMU (VIRGL). (Spec 0005 covers RPi only.)
- (C) Investigate VSCode extensions useful to ROS. (Spec 0003 recommends a baseline set; this is broader discovery.)
- (C) Automatic hardware discovery (mDNS / Avahi).
- (D) Support for additional simulators — Webots, mvsim, MuJoCo, IsaacSim, Drake, PyBullet, pyrobosim, Stage.
- (D) Go through the ROS Tutorials with the SDK.
- (D) Robot Web Tools support.
- (D) turtlesim in a web browser — e.g. mlauret/web-turtlesim, elector102/Turtlesim-Web.

Infrastructure
- (B) Generate build history and buildstats — may fold into Spec 0001 (CI publishes buildhistory) rather than become its own spec.
- (C) Investigate why `ros-default-runtime` and `rosidl-adapter` need to be added explicitly — prerequisite investigation for Spec 0019 (side-by-side image).

Recently completed (historical record)
- Rebase from meta-scipy to meta-python-ai.
- Research AGL integration with meta-ros.
- Investigate a SpaceROS image with meta-ros; create a SpaceROS package group.
