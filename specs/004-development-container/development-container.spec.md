# Feature Specification: Isolated Development Container

**Feature Branch**: `004-development-container`

**Created**: 2026-10-30

**Status**: Implemented

**Input**: Provide a reproducible Feishin development environment without changing host-installed tools or other workspaces.

## User Scenarios & Testing

### User Story 1 - Open an isolated workspace (Priority: P1)

As a contributor, I can open Feishin in a development container and get the project-supported Node.js and pnpm versions without installing them on the host.

**Why this priority**: A consistent toolchain is the foundation for reliable development and avoids changing tools used by other workspaces.

**Independent Test**: Reopen the repository in the container and verify the reported Node.js and pnpm versions.

**Acceptance Scenarios**:

1. **Given** Docker and the VS Code Dev Containers extension are available, **When** I reopen the repository in the container, **Then** the container provides Node.js `23.11.0` and pnpm `11.5.2`.
2. **Given** the repository is open in the container, **When** dependencies are installed, **Then** pnpm uses the committed lockfile and writes dependencies and package data to writable volumes unique to this workspace.

### User Story 2 - Run web development workflows (Priority: P1)

As a contributor, I can type-check, build, and run Feishin's browser-based development surface from the container.

**Why this priority**: These workflows make the isolated environment useful for day-to-day changes without requiring host-specific desktop integrations.

**Independent Test**: Run the documented typecheck, lint, production web build, and remote development commands from the container.

**Acceptance Scenarios**:

1. **Given** dependencies are installed, **When** I run the project's typecheck and lint commands and build the web client with the documented heap setting, **Then** they execute with the container toolchain.
2. **Given** the remote development server is running, **When** I open its forwarded port, **Then** the browser can reach the development app.

### User Story 3 - Keep the host environment unchanged (Priority: P1)

As a contributor, I can use the container without replacing host Node.js or pnpm installations or writing project dependencies into another workspace.

**Why this priority**: Isolation is the reason for choosing a container.

**Independent Test**: Compare host tool versions and the host repository's `node_modules` before and after opening the container.

**Acceptance Scenarios**:

1. **Given** I open the repository in the container, **When** I install dependencies there, **Then** host-installed Node.js and pnpm remain unchanged.
2. **Given** the container installs project dependencies, **When** I inspect the host repository and another Feishin checkout, **Then** dependencies stay out of both host checkouts and each container uses its own volume.

## Edge Cases

- The container registry or package registry is unavailable during initial setup.
- The lockfile cannot be satisfied by the pinned pnpm version.
- A dependency needs a native build tool not provided by the container.
- Electron GUI, audio, MPV, and operating-system integrations are not available through the initial container setup.

## Requirements

### Functional Requirements

- **FR-001**: The development container MUST provide Node.js `23.11.0` and pnpm `11.5.2`, matching the versions documented for Feishin development.
- **FR-002**: The development container MUST install dependencies using the committed pnpm lockfile without modifying it.
- **FR-003**: The development container MUST keep both `node_modules` and pnpm's package store in writable, workspace-unique container volumes, separate from the host workspace and other Feishin checkouts.
- **FR-004**: Contributors MUST be able to run the project's typecheck, lint, web build, and remote development workflows from the container, using the documented larger Node.js heap for the production web build.
- **FR-005**: The initial development container MUST NOT claim to provide Electron GUI, host audio, MPV, MPRIS, or other desktop host integration.
- **FR-006**: Setup instructions MUST identify the required host container runtime and explain how to open the repository in the container.

## Success Criteria

### Measurable Outcomes

- **SC-001**: The container reports exactly Node.js `23.11.0` and pnpm `11.5.2`.
- **SC-002**: Dependency setup completes from the committed lockfile without modifying `pnpm-lock.yaml` and leaves dependency files writable by the development user.
- **SC-003**: Typecheck and lint pass in the container, and the web production build completes with the documented Node.js heap setting.
- **SC-004**: The remote development server is reachable through a forwarded browser port.
- **SC-005**: Opening and using the container does not install or change Node.js or pnpm on the host, create package caches in the host checkout, or share dependencies with another Feishin checkout.

## Assumptions

- Contributors have Docker and the VS Code Dev Containers extension installed on the host.
- Network access is available for container images and package installation.
- The first version targets JavaScript/TypeScript development and browser-based workflows. Full desktop GUI and audio support can be designed separately if maintainers need it.

## Current Implementation References

- `CONTRIBUTING.md` documents the tested Node.js and pnpm versions and development commands.
- `package.json` pins the package manager and defines typecheck, lint, web build, and remote development commands.
- `.gitignore` excludes `node_modules` from version control.
- `docs/agents/architecture.md` describes the web and remote build surfaces.
- `.devcontainer/devcontainer.json` defines the isolated toolchain, dependency volumes, and forwarded remote development port.

## Approach

- Use the official Dev Container Node feature to pin Node.js `23.11.0`, pnpm `11.5.2`, and native Node build prerequisites.
- Initialize writable `node_modules` and pnpm store volumes unique to each workspace, keep pnpm's store configuration inside the container, install from the frozen lockfile on first creation, and forward the remote Vite port.
- Document container startup, supported workflows, and the desktop GUI/audio exclusions in `CONTRIBUTING.md`.
