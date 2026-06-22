6-month Roadmap: July–December 2026
==================================

Overview
- Focus: make Yocto onboarding for Ubuntu-fluent ROS developers easier while maintaining robust multi-distro/meta support.
- Priority ROS distros: Lyrical (LTS), Jazzy (LTS) > Humble, Kilted, Rolling
- Priority Yocto releases: Scarthgap (LTS), Wrynose (LTS) > Whinlatter
- Priority BSPs: Raspberry Pi 4 & 5, Nvidia Orin Nano & AGX, AMD Kria KR260, Qualcomm RB3, Microchip PolarFire
- Architectures: aarch64 (primary); armv7, x86_64, riscv (secondary)

Milestones by month
- July: Project kickoff; publish workspace templates and RFC/ADR templates; start onboarding docs and Ubuntu->Yocto migration guide.
- August: Implement CI test matrix for essential BSPs on Scarthgap and Wrynose; add example builds for Jazzy & Lyrical.
- September: Improve developer onboarding: `rosdep` mapping examples, quickstart build scripts, and cross-compile how-tos.
- October: Launch contributor mentorship program; run first community contribution drive; collect feedback on templates and RFC flow.
- November: Stabilize release cadence aligned to monthly ROS syncs; automate ros-rolling sync checks in CI.
- December: Review metrics, iterate on processes, publish 2027 roadmap.

Deliverables
- Workspace templates and examples (RFC, ADR, SPEC, TEST_PLAN)
- CI job templates and example jobs for GitLab CI and GitHub Actions
- Onboarding guide: Ubuntu→Yocto migration doc, quickstart for building meta-ros
- Contributor playbook: how to propose, implement, and get PRs merged
- Metrics dashboard (contributors, PR lifecycle times, CI success rate)

Success criteria
- PR merge time reduced by 30% vs baseline
- Contributor count growth month-over-month
- Predictable release cadence and on-time ROS sync compatibility
