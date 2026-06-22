RFC 0003: Release Cadence and ROS-Sync Policy
=============================================

Title: Define meta-ros release cadence aligned to ROS distro syncs
Authors: Rob Woolley (@robwoolley)
Status: Draft
Date: 2026-06-22

Summary
- Establish a predictable release cadence for this project's meta-ros deliverables, aligned to upstream ROS sync points, and automate the checks that keep us in sync.

Motivation
- The roadmap's November milestone is "stabilize release cadence aligned to monthly ROS syncs" and a success criterion is "predictable release cadence and on-time ROS sync compatibility". Neither is defined yet.
- Release policy is cross-cutting (affects CI, packaging, downstream consumers), so it is RFC-first per `rfc/README.md`.
- Without a stated cadence, consumers cannot plan upgrades and "are we current with ROS?" has no automated answer.

Proposal
- Cadence: define a regular release rhythm (default proposal: a monthly tag tracking the upstream ROS sync, plus immediate out-of-band releases for security fixes).
- Scope of a release: which matrix cells (`specs/matrix.md`) constitute a release, what is published (ties to RFC 0001), and the version/tag scheme.
- ROS-sync automation: a scheduled CI job compares this project's generated recipe set against the upstream `ros/rosdistro` state and opens an issue/MR when drift is detected (the "automate ros-rolling sync checks in CI" item).
- Support window: how long each ROS distro x Yocto release combination is maintained, and the EOL signal (relates to the "comment on EOL distros" CI item).
- Release checklist: build green on the priority matrix (Spec 0001), tests pass (Spec 0002), changelog, tag, publish per RFC 0001.

Alternatives
- Ad-hoc releases when "enough" has changed. Rejected: not predictable; fails the success criterion.
- Continuous/rolling only, no tagged releases. Rejected: downstream consumers need stable reference points; rolling can coexist as a separate channel.
- Align to Yocto release cadence instead of ROS. Considered: the audience is ROS-driven, so ROS syncs are the primary clock; Yocto LTS releases gate which combinations exist (matrix), not the cadence itself.

Compatibility and Migration
- No existing release process to migrate. First tagged release establishes the baseline against which the "PR merge time -30%" and cadence metrics are measured (Dec metrics dashboard).
- Coordinate with RFC 0001 so what a "release" publishes matches the artifact-hosting policy.

Implementation
- Owner: @robwoolley. Target: Nov 2026.
- Steps: (1) accept cadence + version scheme; (2) implement the rosdistro-drift CI job; (3) codify the release checklist; (4) cut the first tagged release; (5) feed cadence metrics into the Dec dashboard.
- Tests: drift job correctly flags a deliberately stale recipe; a dry-run release passes the checklist on the smoke target.

Unresolved questions
- Monthly, or aligned to ROS patch-release timing (which is not strictly monthly)?
- One release stream spanning all matrix cells, or per-ROS-distro streams?
- How many (ROS x Yocto) combinations can realistically be maintained at the stated cadence by a single maintainer? (Same over-commitment risk flagged on the A-tier.)
- Rolling channel: published continuously, or excluded from the tagged cadence?

Decision Log
- (pending) — record accepted cadence, version scheme, support window, and link the rosdistro-drift CI job and first release.
