# Living Specifications

These specifications capture Feishin's existing user-visible behavior as a baseline and evolve with the product. They are not a promise that every server or client has identical capabilities.

| Specification | Scope | Status |
| --- | --- | --- |
| [Server connections and music libraries](001-server-library/spec.md) | Supported server types, common library workflows, and server-specific capabilities | Implemented baseline |
| [Music playback](002-music-playback/spec.md) | Playback engines, transport controls, queue, and playback reporting | Implemented baseline |
| [Client platforms and remote control](003-client-surfaces/spec.md) | Desktop, web, and remote app boundaries | Implemented baseline |
| [Isolated development container](004-development-container/development-container.spec.md) | Project-pinned Node.js and pnpm toolchain with isolated dependencies and browser workflows | Implemented |

## Maintenance

- Treat the requirements as user-facing contracts; source code remains authoritative for exact current behavior.
- When changing behavior, update the relevant spec and acceptance scenarios in the same change. Add or split a spec only for a distinct, durable user capability.
- Keep implementation plans, generated task lists, and temporary investigation notes out of these baseline specs. Use `.scratch/` for transient feature work.
- Source references point to current ownership areas and should be updated when code moves.

Baseline captured 2026-10-30 from the repository documentation and source layout.
