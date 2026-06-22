ADR 0001: Build Orchestration Tool (bitbake-setup vs kas)
=========================================================

Title: Choose the build orchestration / layer-management tool for meta-ros builds and CI
Date: 2026-06-22
Status: Proposed

Context
- Specs 0001 (CI/CD Platform) and 0004 (migration guide) both need a declarative way to fetch layers, configure `local.conf`/`bblayers.conf`, and reproduce a build the same way locally and in CI.
- Two credible options:
  - `kas` — mature, widely adopted, YAML-driven, strong CI ergonomics, large existing ecosystem and examples.
  - `bitbake-setup` — newer, maintained under the bitbake/Yocto project itself, intended to become the upstream-blessed setup flow; aligns with the project's preference to track upstream tooling.
- The roadmap prioritizes Wrynose (a top-priority Yocto release in `specs/matrix.md`), and the backlog already contains an A-priority item "Add Wrynose configurations for bitbake-setup", signalling a lean toward bitbake-setup.
- The choice affects the migration guide's on-ramp commands, the CI job definitions, and what contributors must learn — so it should be made once, early, and recorded.

Decision
- Proposed: adopt `bitbake-setup` as the primary orchestration tool, tracking upstream Yocto tooling, with configurations maintained per matrix cell.
- Rationale: aligns with the project value of contributing to and tracking upstream; avoids divergence from where the bitbake project is heading; matches the existing Wrynose bitbake-setup work item.
- Mitigation for maturity risk: keep a thin `kas` fallback for any matrix cell where bitbake-setup is not yet sufficient, and do not expose tool-specific internals in the migration guide (wrap invocations in project quickstart scripts) so the tool can be swapped without rewriting docs.
- This ADR is `Proposed` pending maintainer ratification after the RFC/feedback window; flip to `Accepted` once Spec 0001's first pipeline is green on the chosen tool.

Consequences
- Specs 0001 and 0004 should reference this ADR and standardize their command blocks on bitbake-setup.
- Contributors learn one tool; CI and local builds share configuration, reducing "works on my machine" drift.
- Trade-off: smaller ecosystem and fewer worked examples than kas today; we may hit rough edges and need to upstream fixes (acceptable, and consistent with the project's collaboration goals).
- If bitbake-setup proves insufficient before Aug, the fallback is to invert this decision to kas; because docs wrap the tool in quickstart scripts, the migration cost is contained.

References
- specs/0001-ci-cd-platform.md
- specs/0004-ubuntu-to-yocto-migration-guide.md
- specs/matrix.md
- Roadmap backlog item: "Add Wrynose configurations for bitbake-setup"
