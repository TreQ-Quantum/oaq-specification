# Governance

## Overview

The OAQ spec. is currently governed under a **BDFL** model. A single project lead has final authority over the direction of the specification and the resolution of disputes. Day-to-day decisions are made by consensus among contributors, with the project lead as the tiebreaker.

## Project Lead

TreQ — responsible for final decisions on spec direction, versioning, and conflict resolution.

## Types of Changes

Changes to the specification are categorized by their impact on existing implementations:

| Type | Examples | Process |
|------|----------|---------|
| **Patch** | Typos, prose clarifications, new examples, documentation improvements | Pull request welcome; light review |
| **Minor** | New objects, new optional fields, new serialization additions | Open an issue for discussion before submitting a PR |
| **Major** | Breaking changes to existing objects, removed fields, incompatible serialization changes | Issue with extended community discussion; project lead approval required |

## Decision Process

1. **Discussion** — proposals are discussed openly in GitHub issues; anyone may participate
2. **Rough consensus** — the project lead assesses whether there is sufficient agreement to proceed
3. **Final decision** — the project lead has the final say, particularly for major changes and cases where consensus cannot be reached

The project lead aims to be responsive and to explain decisions clearly. Disagreement with a decision may be raised by opening a new issue.

## Versioning

The specification follows **semantic versioning** (`MAJOR.MINOR.PATCH`):

- **MAJOR** — breaking changes that require updates to existing implementations (e.g. removed or renamed fields)
- **MINOR** — backwards-compatible additions (e.g. new optional objects or fields)
- **PATCH** — backwards-compatible fixes (e.g. clarifications, corrected descriptions, new examples)

The current version is **0.1.0**. Until a 1.0.0 release, minor version bumps may include breaking changes; this will always be clearly noted in the changelog.

## Becoming a Maintainer

There is currently one maintainer. As the project grows, additional maintainers may be invited based on sustained, high-quality contributions. Currently, the project lead makes these decisions.
