ADR 0002: CI Runner Hosting and Provisioning
============================================

Title: Provision GitLab CI runners on AWS Spot Instances via the CattleOps Terraform module
Date: 2026-06-22
Status: Accepted

Context
- Spec 0001 (CI/CD Platform) requires runners that are reproducible from infrastructure-as-code (the A-priority "Terraform/OpenTofu secure runners" item), cost-controlled, and able to absorb a bursty matrix build load.
- Yocto/meta-ros builds are heavy (CPU, RAM, disk) and the priority matrix fans out across distro x release x board, so a fixed always-on runner fleet would be expensive and mostly idle between merges.
- Building runner autoscaling, spot bidding, VPC, security groups, and IAM by hand is exactly the undifferentiated work the roadmap wants to avoid hand-configuring.

Decision
- Provision GitLab runners on AWS using the **CattleOps `terraform-aws-gitlab-runner` module** (cattle-ops/terraform-aws-gitlab-runner), running workers on **EC2 Spot Instances** with autoscaling.
- The module provisions the runner manager, autoscaling spot workers, VPC/subnet, security groups, and IAM, satisfying the IaC + secure-runner requirement in Spec 0001.
- This work is already underway; this ADR records the as-built decision.

Consequences
- Cost: spot pricing cuts runner cost substantially versus on-demand, which is what makes the full nightly matrix affordable.
- Spot interruption is the main risk: a long bitbake build can be reclaimed mid-job. Mitigations:
  - Rely on the persistent S3 sstate mirror (ADR 0003) so a reclaimed-and-retried job resumes from warm shared state instead of rebuilding from scratch — spot ephemerality is the primary reason that mirror is mandatory, not just an optimization.
  - Use GitLab automatic job retry for `runner_system_failure`/spot-interruption.
  - Keep an on-demand fallback runner for the per-MR smoke target so critical-path pipelines do not stall on spot capacity.
- The module's built-in S3 cache is GitLab's distributed *job* cache (artifacts between stages); it is distinct from the Yocto **sstate** mirror in ADR 0003. Do not conflate the two buckets/lifecycles.
- Architecture: prefer native aarch64 spot instances for aarch64 matrix cells to avoid qemu-user build slowdown (consistent with Spec 0001).
- Trade-off: ties runner infrastructure to AWS. Acceptable given the cost model; the bitbake/build logic stays cloud-agnostic, so a future migration is contained to the Terraform layer.

References
- specs/0001-ci-cd-platform.md
- adrs/0003-sstate-caching.md
- rfc/0001-binary-artifact-hosting.md (cost model and ceilings)
- cattle-ops/terraform-aws-gitlab-runner
