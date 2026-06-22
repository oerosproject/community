Claude workspace mapping
=========================

Purpose
- Provide a clear, machine- and human-readable directory layout so AI agents and contributors can find specs, RFCs, ADPs, and tests.

Top-level layout
- `rfc/` — RFC drafts and templates (discussion-first proposals).
- `adrs/` — Architecture Decision Records.
- `specs/` — Small, implementable feature specs with test plans.
- `templates/` — Authoring templates for RFC, ADR, SPEC, TEST_PLAN, and CONTRIBUTING.
- `CI/` — (future) CI job templates and automation snippets.
- `docs/` — (future) long-form tutorials and onboarding guides.

Agent usage guidelines
- Agents should open `workspace-mapping.md` first, then `specs/` to find work items.
- Assign a single `owner` field in each spec to coordinate human review.
- Link RFCs/ADRs from specs using explicit relative links.

Tracking work
- Small tasks: create a spec under `specs/` with `Status: Draft` and link an issue or PR.
- Larger changes: author an RFC in `rfc/`, gather feedback, then convert to spec(s) for implementation.

Best practices
- Keep specs < 1 page where possible.
- Add `Test Plan` sections with CI matrix rows so CI owners can add jobs efficiently.
