RFC 0001: Binary Artifact Hosting Policy
========================================

Title: Policy and cost model for publishing pre-built meta-ros artifacts
Authors: Rob Woolley (@robwoolley)
Status: Draft
Date: 2026-06-22

Summary
- Decide whether, what, and how the project publishes pre-built binary artifacts (images, SDKs, package feeds, sstate mirrors) to end users, with an explicit cost model and retention policy before any spend is committed.

Motivation
- Pre-built images and SDKs dramatically lower the onboarding barrier for the Ubuntu-fluent ROS audience the roadmap targets — but egress and storage for Yocto artifacts are expensive and grow without bound if unmanaged.
- This decision is currently buried in a Notes field on the Terraform CI item. It is a strategic financial commitment, not an implementation detail, and it gates how Spec 0001 (CI/CD Platform) configures retention and publishing.
- Cautionary precedent: the NixOS Foundation's S3 cost crisis, where unbounded binary cache egress became unsustainable — https://discourse.nixos.org/t/the-nixos-foundations-call-to-action-s3-costs-require-community-support/28672

Proposal
- Define an artifact policy along these axes, each with a default to debate:
  - What is published: images (which boards?), SDK installers, package feeds (`ipk`/`deb`/`rpm`), sstate mirror. Default: SDK installers + smoke-target images only; sstate mirror internal-only at first.
  - Which boards/matrix cells: publish only the top-priority intersection (Jazzy/Lyrical x Scarthgap/Wrynose x RPi4/RPi5) initially.
  - Retention: time-boxed (e.g. nightly kept 14 days, tagged releases kept indefinitely). Default: nightly 14d, release-tagged retained.
  - Access control: anonymous vs login-required downloads (login enables metering and abuse control). Default: anonymous for releases, none for nightly externally.
  - Delivery: object store direct vs CDN fronting. Default: start direct from object store; add CDN only when egress cost justifies it.
- Produce a cost model: estimate artifact sizes x expected downloads x egress price for each option, with a monthly ceiling and an alert threshold.
- Define a funding/ownership path: who pays, and what happens at the cost ceiling (throttle, require login, seek sponsorship).

Alternatives
- Publish nothing; users build everything. Rejected: contradicts the onboarding goal.
- Publish everything for all matrix cells with indefinite retention. Rejected: this is precisely the NixOS failure mode.
- Offload to a third party (GitHub Releases asset size limits, a sponsor's CDN, a mirror network). Considered: viable for releases; keep as a fallback/funding option in the cost model.

Compatibility and Migration
- No existing published artifacts, so no migration burden today. Establishing the policy now prevents a costly walk-back later.
- Whatever is chosen must be expressible as Spec 0001 pipeline config (retention rules, publish targets) and reproducible via OpenTofu.

Implementation
- Owner: @robwoolley. Target: Jul 2026 (decide before CI publishing is wired in Aug).
- Steps: (1) gather artifact sizes from a sample build; (2) model 2-3 scenarios with cost ceilings; (3) pick defaults via the RFC feedback window; (4) record outcome as an ADR; (5) Spec 0001 implements the chosen retention/publish/CDN config.
- Tests: validate retention expiry actually deletes artifacts; validate egress alerting fires below the ceiling.

Unresolved questions
- Is login-required download acceptable to the community, or a barrier that defeats the onboarding goal?
- Do we host package feeds (large, high-churn) or only images + SDKs?
- Is there a sponsor or foundation budget line, or must this be self-funding within a fixed ceiling?

Decision Log
- (pending) — record accepted policy, link the resulting ADR and the Spec 0001 implementation PR.
