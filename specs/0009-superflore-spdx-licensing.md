Spec 0009: Superflore SPDX and License Detection
================================================

Title: Add SPDX support and license detection to Superflore-generated recipes
Owner: @robwoolley
Status: In Progress
Date: 2026-06-22

Goal
- Superflore-generated meta-ros recipes carry accurate `LICENSE` and SPDX metadata derived from upstream `package.xml`, improving license compliance and enabling SPDX SBOM output.
- Acceptance criteria:
  - Superflore reads the SPDX license attribute from `package.xml` (where present) and emits a valid Yocto `LICENSE` field plus `LIC_FILES_CHKSUM`.
  - A license-detection fallback infers a license for packages whose `package.xml` lacks the SPDX attribute.
  - Generated recipes for the priority matrix parse with no license-related QA warnings (`incorrect-license`, missing `LIC_FILES_CHKSUM`).

Scope
- In scope: Superflore changes to parse SPDX from `package.xml`, map to Yocto `LICENSE` syntax, generate `LIC_FILES_CHKSUM`, and an Apache-compatible license-detection fallback.
- Out of scope: Adding the SPDX attribute to `package.xml` upstream (that is the C-tier upstream item, tracked separately and a blocker for the no-attribute case); full SBOM publishing pipeline (follow-up, ties to Yocto SPDX/`create-spdx`).

Dependencies
- Superflore tooling and the meta-ros generation flow.
- Upstream "add SPDX attribute to packages.xml" (C item) — unblocks the high-fidelity path; until then the detection fallback carries packages that lack the attribute.
- Relates to RFC 0002 (package-name prefix) only insofar as both touch the generator; keep the changes independent.

Design/Proposal
- Parse: extract the SPDX `license` element/attribute from `package.xml`; normalize to SPDX identifiers and map to Yocto `LICENSE` (`&`/`|` syntax) using a maintained alias table.
- Checksums: generate `LIC_FILES_CHKSUM` against the package's license file(s); flag packages where no license file is found.
- Detection fallback: license scan written to be Apache-2.0-compatible (do not pull in a GPL scanner that would constrain redistribution), used only when `package.xml` lacks SPDX.
- Keep SPDX parsing and the detection fallback as separable code paths so the fallback can be retired as upstream `package.xml` coverage improves.

Testing
- Acceptance: regenerate the priority matrix; recipes have valid `LICENSE` + `LIC_FILES_CHKSUM`; no license QA warnings.
- Coverage: report the count of packages resolved via SPDX-from-package.xml vs detection fallback, to track upstream progress.
- Reference `templates/TEST_PLAN_TEMPLATE.md`.

Rollout
- Sep 2026 (work already in progress). Order: (1) SPDX-from-package.xml parsing + LICENSE mapping; (2) LIC_FILES_CHKSUM generation; (3) detection fallback; (4) matrix regeneration clean; (5) coverage report feeding the upstream packages.xml push.
