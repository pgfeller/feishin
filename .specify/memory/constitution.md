# Feishin Constitution

## Core Principles

### I. User Value and Product Fit

Features must serve Feishin's music-listening workflows and fit the project's product direction. For substantial feature requests, follow the prior-discussion guidance in `CONTRIBUTING.md` before implementation.

### II. Preserve Product and Process Boundaries

Keep server-specific behavior behind the existing API/controller and normalization boundaries. Respect main, preload, renderer, shared, remote, and web import/build boundaries in `docs/agents/architecture.md`; expose Electron capabilities to the UI through the typed preload API.

### III. Treat Client Differences Explicitly

Desktop, web, and remote surfaces do not have identical capabilities. Gate desktop-only behavior and handle unavailable capabilities without breaking supported web or remote workflows.

### IV. Protect User Data

Do not expose credentials, tokens, or other sensitive server data in logs or user-visible diagnostics. Reuse existing sanitization and secure-storage behavior when touching authentication or server settings.

### V. Keep Specifications Living and Evidence-Based

Checked-in specs describe user-visible behavior and constraints, not implementation wishes. Update the relevant spec when behavior changes; cite the owning source/docs and label assumptions or unresolved behavior rather than presenting guesses as fact.

## Engineering Constraints

- Preserve existing public behavior and conventions unless a spec explicitly changes them.
- Prefer existing project utilities, dependencies, and patterns; keep changes scoped to the owning feature.
- Keep backend quirks out of shared user-facing domain behavior where the established normalization layer can handle them.

## Verification and Workflow

- Derive acceptance criteria from user-visible outcomes and make them independently verifiable.
- Validate changed behavior with the narrowest relevant check, then run applicable project typecheck/lint/build checks for the touched surface.
- Follow `CONTRIBUTING.md` for feature approval, manual testing, and pull request expectations.
- Use `specs/` for durable baseline and evolving product behavior. Use `.scratch/` for transient feature proposals, issue notes, and wayfinding work.

## Governance

This constitution guides new and changed specs and implementation work. If it conflicts with an established repository instruction or a deliberate product decision, surface the conflict and resolve it with the maintainer; do not silently override the more specific guidance. Amend this document when project-wide principles change, and keep it concise.

**Version**: 1.0.0 | **Ratified**: 2026-10-30 | **Last Amended**: 2026-10-30
