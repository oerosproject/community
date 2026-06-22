ADR 0003: sstate / DL_DIR Caching Backend
=========================================

Title: Cache shared-state in S3 using mirror_updates.bbclass from OE4T meta-demo-ci
Date: 2026-06-22
Status: Accepted

Context
- Spec 0001 (CI/CD Platform) requires that shared-state (sstate) and downloads persist between builds so incremental jobs complete in minutes rather than hours.
- Runners are ephemeral AWS Spot Instances (ADR 0002): on-instance sstate is lost when a worker is reclaimed or scaled in. Without a *shared, persistent* cache, every job — and every spot retry — rebuilds from scratch, which defeats the cost rationale for spot.
- OE4T's tegra-demo-distro already solves this for a real meta-ros-adjacent CI: `meta-demo-ci` provides a `mirror_updates.bbclass` that publishes sstate (and downloads) to an object-store mirror after a build.

Decision
- Cache sstate in **S3 buckets**, populated by the **`mirror_updates.bbclass` from OE4T tegra-demo-distro (`layers/meta-demo-ci`)**, with `SSTATE_MIRRORS` (and `PREMIRRORS` for `DL_DIR`) pointing builds at the bucket.
- Builds pull warm objects from the S3 mirror on the read path; `mirror_updates.bbclass` pushes newly produced objects back on the write path, keeping the shared cache current.
- This work is already underway; this ADR records the as-built decision.

Consequences
- Ephemeral spot workers (ADR 0002) become viable: a fresh or retried worker pulls a warm cache from S3 instead of building cold. This is the dependency that makes the spot decision safe.
- Two distinct concerns kept separate:
  - The Yocto **sstate/DL_DIR mirror** (this ADR) — large, long-lived, shared across all jobs.
  - GitLab's **distributed job cache** from the CattleOps module (ADR 0002) — per-pipeline artifacts. Different bucket, different lifecycle.
- Retention/cost: the sstate bucket grows continuously and must have an S3 lifecycle policy (e.g. expire objects unused for N days). The ceiling and policy are governed by RFC 0001 — this is internal build cache, not user-facing artifacts, but it still incurs storage/egress cost.
- Seeding: nightly full-matrix builds keep the mirror warm so per-MR smoke builds stay fast.
- Integrity: sstate is hash-keyed, so a populated mirror is safe to share; the separate hash-equivalence server (B-tier) is an optimization on top, not a prerequisite for this caching.
- Trade-off: adopting `meta-demo-ci`'s class couples us to that layer's approach; acceptable since it is proven in OE4T CI and aligns with the project's reuse-upstream value. Track upstream changes to the class.

References
- specs/0001-ci-cd-platform.md
- adrs/0002-ci-runner-hosting.md
- rfc/0001-binary-artifact-hosting.md (retention policy and cost ceilings)
- OE4T/tegra-demo-distro: layers/meta-demo-ci, mirror_updates.bbclass
