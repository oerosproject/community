Spec 0001: CI/CD Platform
=========================

Title: Stand up a public CI/CD platform for meta-ros
Owner: @robwoolley
Status: Draft
Date: 2026-06-22

Goal
- A publicly reachable GitLab CI/CD instance builds the priority matrix from `specs/matrix.md` on every merge to the integration branch and on scheduled nightly runs.
- Acceptance criteria:
  - A maintained `.gitlab-ci.yml` (plus reusable job templates) builds at least one image for the intersection of (Jazzy | Lyrical) x (Scarthgap | Wrynose) x (RPi4 | RPi5).
  - Shared-state (sstate) and downloads (DL_DIR) are persisted between builds in object storage so incremental builds complete in minutes, not hours.
  - Build output (logs, manifests) is retained and browsable for at least 30 days.
  - Runners are reproducible from infrastructure-as-code; no hand-configured hosts.

Scope
- In scope: GitLab instance (or gitlab.com group + self-hosted runners), runner provisioning via OpenTofu, sstate/DL_DIR object storage, build orchestration with kas/bitbake-setup, nightly + per-MR pipelines.
- Out of scope: Binary artifact publishing to end users (see `rfc/0001-binary-artifact-hosting.md`); hash equivalence server (Spec 0005, B); error-report server (B); GitHub Actions mirroring (follow-up).

Dependencies
- `specs/matrix.md` (defines the build matrix).
- `rfc/0001-binary-artifact-hosting.md` — must be decided before any user-facing artifact retention/CDN spend; this spec only retains build artifacts internally.
- Spec 0003 (devcontainer) shares the container image definition used by runners.
- ADR 0001 (build orchestration tool) — pipeline jobs standardize on the tool chosen there.

Design/Proposal
- Orchestration: drive builds with `kas` or `bitbake-setup` so matrix entries are declarative YAML, one file per (distro x release x board).
- Runners: GitLab runners on aarch64-capable cloud instances (native aarch64 build, not qemu-user) provisioned by OpenTofu. Bastion host + private subnet + security groups per the Terraform A item.
- Caching: `SSTATE_DIR` and `DL_DIR` on a mounted object-store-backed cache (S3 + s3fs, or a sstate mirror over HTTP using `SSTATE_MIRRORS`). Seed sstate from nightly to keep per-MR jobs fast.
- Pipeline stages: `lint` (recipe/layer sanity, `yocto-check-layer`) -> `build` (matrix) -> `test` (see Spec 0002) -> `publish-internal` (logs, manifests, buildhistory).
- Concurrency: cap matrix fan-out to control cost; full matrix nightly, reduced matrix per-MR (RPi5 + Scarthgap + Jazzy as the smoke target).

Testing
- See Spec 0002 for the testimage/ptest jobs invoked by the `test` stage.
- CI self-test: a throwaway MR must produce a green pipeline that builds the RPi5/Scarthgap/Jazzy smoke target from cold and from warm sstate; record both wall-clock times.
- Reference `templates/TEST_PLAN_TEMPLATE.md`.

Rollout
- Aug 2026 per `ROADMAP.md`. Milestone order: (1) one runner + one smoke target green; (2) sstate/DL_DIR caching; (3) OpenTofu-provisioned runners; (4) expand to full priority matrix; (5) nightly schedule.
- Decommission any interim hand-built runner once OpenTofu provisioning lands.
