# Tasks: Isolated Development Container

## Phase 1: Setup

**Wave 1 - independent files:**

- [x] **T001** [US1] Define the isolated Node.js/pnpm toolchain, lockfile install, dependency and store volumes, and forwarded web port + `.devcontainer/devcontainer.json`
- [x] **T002** [US1] Document container startup, supported workflows, and desktop limits + `CONTRIBUTING.md`
- [x] **T003** [US1] Add the development-container spec to the index + `specs/README.md`

**⟶ Wait for Wave 1 to finish, then:**

## Phase 2: Polish

- [x] **T004** [US2] Validate the Dev Container configuration, pinned tool versions, isolated dependency/store paths, lockfile install, and forwarded development workflow + `.devcontainer/devcontainer.json`

Dependency: T004 follows T001; T002 and T003 can run independently.
