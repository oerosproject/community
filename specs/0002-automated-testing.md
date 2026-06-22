Spec 0002: Automated Testing (ptest + runqemu testimage)
========================================================

Title: Enable automated ROS test execution in CI via ptest and runqemu
Owner: @robwoolley
Status: Draft
Date: 2026-06-22

Goal
- ROS package tests run automatically in CI against built images, with pass/fail surfaced per matrix cell.
- Acceptance criteria:
  - A `testimage`-based image (via `IMAGE_CLASSES += "testimage"`) boots under `runqemu` in CI for the qemuarm64 build of the smoke target and runs the OE test suite headless.
  - ptest is enabled (`DISTRO_FEATURES += "ptest"`, `IMAGE_FEATURES += "ptest-pkgs"`) and at least one ROS package ships and runs a `run-ptest` script returning TAP/`PASS`/`FAIL`.
  - `colcon test` runs against the ROS SDK (Spec 0003) for at least one workspace package.
  - Test results are collected as JUnit/TAP artifacts and shown in the GitLab pipeline test report.

Scope
- In scope: testimage class wiring, runqemu in CI (KVM where available), a reference ptest recipe for a representative ROS package, colcon-test-with-SDK job, result reporting.
- Out of scope: Physical-target test farm (relates to the "remote development with physical target" A item, separate spec); full per-package ptest coverage (incremental follow-up).

Dependencies
- Spec 0001 (CI/CD Platform) — provides runners and the `test` stage.
- Spec 0003 (devcontainer/SDK) — `colcon test` needs the populated SDK.
- `specs/matrix.md` — defines which qemu machines and distros are exercised.

Design/Proposal
- Image: add a `ros-test-image` recipe inheriting from the core ROS image with `testimage` + `ptest-pkgs`.
- runqemu in CI: run `bitbake ros-test-image -c testimage` with `TEST_TARGET = "qemuboot"` (or `simpleremote` for physical targets later). Require nested virt/KVM on runners; fall back to TCG with a longer timeout for cells without KVM.
- ptest reference: pick one stable ROS package, add a `run-ptest` wrapper around its `ament`/`gtest` tests; register with `ptest` in the recipe and verify `ptest-runner` discovers it.
- colcon test: in a separate job, source the SDK environment and run `colcon test` + `colcon test-result --all` against a sample workspace; emit JUnit XML.
- Reporting: convert ptest TAP and colcon JUnit to GitLab `artifacts:reports:junit`.

Testing
- Acceptance: green testimage run on qemuarm64/Scarthgap/Jazzy; one ptest reporting PASS; one colcon test reporting PASS.
- Negative test: intentionally break a test and confirm the pipeline goes red and the failure is attributed to the right package.
- CI matrix: qemuarm64 + qemux86-64 first; expand to physical boards once the farm exists.
- Reference `templates/TEST_PLAN_TEMPLATE.md`.

Rollout
- Aug 2026. Order: (1) testimage boots under runqemu; (2) one ptest green; (3) colcon-test-with-SDK green; (4) wire JUnit reporting; (5) expand package coverage incrementally.
