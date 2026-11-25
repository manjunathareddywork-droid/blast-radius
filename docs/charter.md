# Blast Radius: Project Charter

_Status: draft · Last updated: 2026-11-26_

## Problem

Salesforce orgs build up thousands of interconnected components: Apex classes, Flows, objects, fields, validation rules, permission sets, layouts. Before changing or deleting one, a developer or admin has to work out by hand what depends on it. Salesforce's built-in "Where is this used?" works only in the org, only for custom fields, and can't see dynamic Apex references, so changes ship with unknown impact and break things in production.

## Vision

Before any Salesforce change is deployed, you can see what it affects: from the command line, in every pull request, and eventually by asking in plain English.

## Goals

1. **Build a dependency graph from SFDX metadata.** Parse a Salesforce project from disk into components and the references between them. The graph is deterministic and is the single source of truth.
2. **Answer "where is X used?"** from the command line, e.g. `blast-radius where-used Account.Industry`.
3. **Be honest about uncertainty.** Every dependency carries a confidence level (certain / likely / possible), because dynamic Apex can't be fully resolved statically.
4. **Keep metadata on the user's own machine or CI.** No upload to a third-party service.

## Scope

**Phase 0 (December 2026): Python foundations through the project**

| Epic | Outcome |
|---|---|
| E1 Project setup | Repo, board, charter, sample data, first ADR |
| E2 Metadata scanner | Count metadata files by type in an SFDX project |
| E3 Object & field parser | Objects and fields parsed into a JSON component list |
| E4 Apex reference detection | Field references found in Apex (regex) → first dependency edges |
| E5 `where-used` command | `blast-radius where-used <Object.Field>` returns the components that reference it |

**Later (2027, plan only, not committed):** graph storage and API, CI with GitHub Actions, a GitHub Action that comments impact on PRs, an AI layer that answers plain-English questions using the graph, and cloud deployment.

## Non-goals

- **Not a deployment or release tool.** Blast Radius reports impact; it doesn't deploy, test, or roll back.
- **No live-org connection in Phase 0.** Input is SFDX metadata on disk only.
- **No AI in Phase 0.** When AI is added, it plans queries and explains results; it never decides what depends on what.
- **Not full static analysis of Apex.** Regex-based detection first; dynamic references are flagged as lower confidence, not guessed.
- **No client or employer metadata, ever.** Development and testing use only public sample apps (e.g. `trailheadapps/apex-recipes`) or the owner's own Developer Edition orgs.

## Success metrics

**Phase 0 (by 2026-12-31)**

- `blast-radius where-used Account.Industry` runs against `apex-recipes` and lists every Apex class and object/field component that references the field.
- Spot check: at least 10 fields hand-checked against the source. No missed references in Apex that the regex is designed to catch.
- Code has pytest tests for the parser and reference detection, and they pass.
- Every change merged through a PR linked to a BR ticket.

**Later phases**

- Precision and recall measured against a hand-labelled evaluation set before any PR-comment or AI feature ships.

## Constraints

- **Time:** part-time, roughly 30–45 min on weekdays and 2–3 hrs at weekends.
- **People:** one developer (the owner), who is learning Python through this project.
- **Data:** public sample metadata only (see Non-goals).

## Roles

- **Owner / developer:** product owner and sole developer. Makes final decisions and writes the code.
- **Claude (AI assistant):** project manager, business analyst, and supporting architect. Plans sprints, writes tickets, drafts docs, reviews PRs.

## Risks

| Risk | Mitigation |
|---|---|
| Learning curve slows delivery | Small weekly sprints; scope cut before quality; buffer week in Sprint 5 |
| Scope creep (AI, cloud, extra features) pulled in early | Non-goals above; later phases stay "plan only" until Phase 0 ships |
| Regex misses or misreads Apex references | Confidence levels on every edge; spot-check fields by hand |
| Competing time commitments | Fixed weekly time budget; this project yields when it has to |
