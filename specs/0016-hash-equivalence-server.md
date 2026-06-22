Spec 0016: Hash Equivalence Server
==================================

Title: Run a bitbake hash-equivalence server to improve sstate reuse
Owner: @robwoolley
Status: Draft
Date: 2026-06-22

Goal
- A shared hash-equivalence server lets CI and contributors reuse sstate across functionally-equivalent task hashes, cutting rebuilds beyond what the sstate mirror alone provides.
- Acceptance criteria:
  - A `bitbake-hashserv` instance is reachable and used by CI (`BB_HASHSERVE` configured, `BB_SIGNATURE_HANDLER = "OEEquivHash"`).
  - A measurable reduction in tasks rebuilt for an unrelated-change build versus mirror-only caching.

Scope
- In scope: deploy/host the hash-equivalence server (self-hosted vs hosted decision), wire CI and quickstart configs to use it, measure the benefit.
- Out of scope: The sstate mirror itself (ADR 0003 — this complements it); orchestration tooling (ADR 0001).

Dependencies
- Spec 0001 (CI) and ADR 0003 (sstate caching) — hash-equivalence amplifies sstate reuse but is not required for it.
- Spec 0013 (bitbake-setup configs) — configs point at the server.

Design/Proposal
- Decide self-hosted `bitbake-hashserv` (in the ADR 0002 footprint) vs a hosted/shared server; record the choice as a short ADR if non-obvious.
- Configure `BB_HASHSERVE` + `OEEquivHash` in the shared bitbake-setup configs so CI and local builds use the same server.
- Persist the server's database; back up periodically.

Testing
- Acceptance: with the server enabled, a no-op-equivalent change rebuilds materially fewer tasks than mirror-only; capture before/after task counts.
- Reference `templates/TEST_PLAN_TEMPLATE.md`.

Rollout
- Sep 2026. Order: (1) hosting decision; (2) deploy + persist DB; (3) wire CI/quickstart configs; (4) measure benefit.
