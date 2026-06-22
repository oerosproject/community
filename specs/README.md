Spec workspace
===============

This folder holds formal specifications and implementation tasks. Files here should follow the `SPEC_TEMPLATE.md` and be small, reviewable, and linked to RFCs or ADRs when relevant.

Structure
- `specs/` contains feature specs, each as a single Markdown file describing scope, acceptance criteria, owner, and test plan.
- Specs are numbered `NNNN-short-slug.md` in creation order. Numbers are stable identifiers; do not renumber.

Index
-----

| Spec | Title | Priority | Target | Status | Owner |
| --- | --- | --- | --- | --- | --- |
| [0001](0001-ci-cd-platform.md) | CI/CD Platform | A | Aug 2026 | Draft | @robwoolley |
| [0002](0002-automated-testing.md) | Automated Testing (ptest + runqemu) | A | Aug 2026 | Draft | @robwoolley |
| [0003](0003-devcontainer-with-sdk.md) | Devcontainer with ROS SDK | A | Jul 2026 | Draft | @robwoolley |
| [0004](0004-ubuntu-to-yocto-migration-guide.md) | Ubuntu-to-Yocto Migration Guide | A | Jul 2026 | Draft | @robwoolley |
| [0005](0005-hardware-acceleration-rpi.md) | Hardware Acceleration for Raspberry Pi 4/5 | A | Sep 2026 | Draft | @robwoolley |
| [0006](0006-package-name-prefix.md) | Package-Name Prefix (ros-<distro>-<BPN>) | A | Sep 2026 | Draft | @robwoolley |
| [0007](0007-remote-development-physical-target.md) | Remote Development with Physical Target | A | Oct 2026 | Draft | @robwoolley |

Related
- RFCs: [rfc/0001-binary-artifact-hosting.md](../rfc/0001-binary-artifact-hosting.md)
- ADRs: [adrs/0001-build-orchestration-tool.md](../adrs/0001-build-orchestration-tool.md)
- Support matrix: [matrix.md](matrix.md)

See `templates/SPEC_TEMPLATE.md` for authoring guidance.
