# ADR-0001: Build Blast Radius outside Salesforce

- **Status:** Proposed · 2026-11-26
- **Context:** Impact analysis could run inside an org (Apex/LWC, a managed package) or outside it, on SFDX source files. We want to see impact *before* deploying, and in pull requests, where the change exists only as source.
- **Decision:** Build a standalone Python tool that reads SFDX metadata from disk and runs locally or in CI. No code is installed in the org.
- **Why:** Analyses the change before deploy; metadata never leaves the user's machine or CI; no governor limits or AppExchange security review; Python suits parsing, graphs, and later AI work.
- **Consequences:** Sees only what is in source control, not org-only config or data. Users must retrieve metadata first. A live-org connection (OAuth) is deferred to a later phase.
