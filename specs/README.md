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
| [0008](0008-ros-in-containers.md) | ROS-in-Containers | B | Sep 2026 | In Progress | @robwoolley |
| [0009](0009-superflore-spdx-licensing.md) | Superflore SPDX and License Detection | B | Sep 2026 | In Progress | @robwoolley |
| [0010](0010-metrics-dashboard.md) | Project Metrics Dashboard | B | Dec 2026 | Draft | @robwoolley |
| [0011](0011-contributor-playbook.md) | Contributor Playbook | B | Oct 2026 | Draft | @robwoolley |
| [0012](0012-onboarding-howtos-quickstart.md) | Onboarding How-Tos and Quickstart | A | Sep 2026 | Draft | @robwoolley |
| [0013](0013-wrynose-bitbake-setup-configs.md) | Wrynose bitbake-setup Configurations | A | Aug 2026 | Draft | @robwoolley |
| [0014](0014-github-actions-maintainer-automation.md) | GitHub Actions Maintainer Automation | B | Sep 2026 | Draft | @robwoolley |
| [0015](0015-error-report-server.md) | Error-Report Server | B | Nov 2026 | Draft | @robwoolley |
| [0016](0016-hash-equivalence-server.md) | Hash Equivalence Server | B | Sep 2026 | Draft | @robwoolley |
| [0017](0017-recipe-uprev-python-upstreaming.md) | Recipe Upreving and Python Upstreaming | B | Sep 2026 | Draft | @robwoolley |
| [0018](0018-ros-package-status-dashboard.md) | ROS-Package Build-Status Dashboard | B | Dec 2026 | Draft | @robwoolley |
| [0019](0019-multi-distro-side-by-side-image.md) | Multi-Distro Side-by-Side Image | C | gated on 0006 | Draft | @robwoolley |

Related
- RFCs: [0001 binary-artifact hosting](../rfc/0001-binary-artifact-hosting.md), [0002 package-name prefix](../rfc/0002-package-name-prefix.md), [0003 release cadence](../rfc/0003-release-cadence.md)
- ADRs: [0001 build orchestration tool](../adrs/0001-build-orchestration-tool.md), [0002 CI runner hosting](../adrs/0002-ci-runner-hosting.md), [0003 sstate caching](../adrs/0003-sstate-caching.md)
- Support matrix: [matrix.md](matrix.md)
- Backlog (not-yet-spec'd ideas): [../BACKLOG.md](../BACKLOG.md)
- Community initiatives: [../COMMUNITY.md](../COMMUNITY.md)

See `templates/SPEC_TEMPLATE.md` for authoring guidance.
