# Feature Specification: Client Platforms and Remote Control

**Feature Branch**: `baseline/client-surfaces`

**Created**: 2026-10-30

**Status**: Implemented baseline

**Input**: Existing Electron, web, and remote app builds and repository architecture guidance.

## User Scenarios & Testing

### User Story 1 - Use Feishin as a desktop app (Priority: P1)

As a listener, I can use Feishin as an installed desktop client and access supported operating-system integrations.

**Why this priority**: The desktop client is the recommended distribution and includes capabilities not available in a browser.

**Independent Test**: Build or launch the desktop app and verify its supported playback and operating-system integrations.

**Acceptance Scenarios**:

1. **Given** a supported desktop installation, **When** I launch Feishin, **Then** I can use its desktop application workflows.
2. **Given** an Electron-only capability, **When** it is unavailable in a non-Electron client, **Then** the client does not assume that capability exists.

### User Story 2 - Use Feishin in a browser (Priority: P1)

As a listener, I can use the web client without requiring Electron-specific APIs.

**Why this priority**: The web client is a separately supported way to access Feishin.

**Independent Test**: Build the web client and verify its library and web-player workflows without desktop APIs.

**Acceptance Scenarios**:

1. **Given** a supported browser and reachable music server, **When** I open the web client, **Then** I can use the browser-supported app workflows.
2. **Given** a workflow that requires a desktop integration, **When** I use the web client, **Then** unrelated web workflows remain available.

### User Story 3 - Control Feishin remotely (Priority: P2)

As a user with the remote service enabled, I can use the separate remote client to interact with the host application's supported remote workflows.

**Why this priority**: Remote access extends the desktop-hosted experience to another device without making the remote app equivalent to the full client.

**Independent Test**: Build the remote app and verify its exposed remote pages and controls against a host with the remote service enabled.

**Acceptance Scenarios**:

1. **Given** a host serving the remote app, **When** I open the remote client, **Then** I can access the remote workflows exposed by that host.
2. **Given** a capability not exposed by the remote client, **When** I use the remote client, **Then** Feishin does not imply full desktop-client parity.

## Edge Cases

- A browser build runs without Electron's preload API.
- A desktop-only integration is unavailable or not configured.
- The remote host is unreachable or a remote operation is unsupported.
- A build-specific feature is accidentally imported across an unsupported process boundary.

## Requirements

### Functional Requirements

- **FR-001**: Feishin MUST provide desktop, web, and remote client surfaces as separate supported builds or apps.
- **FR-002**: The web client MUST support browser workflows without requiring Electron APIs.
- **FR-003**: Desktop-only features MUST be guarded by capability availability and MUST NOT be assumed by web or remote clients.
- **FR-004**: The remote client MUST expose its own supported remote workflows without implying full desktop-client parity.
- **FR-005**: Shared behavior MUST respect the documented import and process boundaries.

### Key Entities

- **Client surface**: The desktop app, browser app, or remote-control app through which a user interacts with Feishin.
- **Desktop capability**: An operating-system or Electron integration available only in the desktop app.
- **Remote host**: A Feishin desktop instance that serves remote app functionality.

## Success Criteria

### Measurable Outcomes

- **SC-001**: The Electron, web, and remote builds complete using their documented build commands.
- **SC-002**: Core browser workflows do not require Electron preload APIs.
- **SC-003**: Desktop-only features do not break clients where the relevant capability is absent.
- **SC-004**: Users can distinguish remote-control workflows from the full desktop application.

## Assumptions

- The desktop app is built with Electron; web and remote clients are browser applications.
- The remote app depends on a reachable host with the remote service available.
- Feature availability varies by client surface; platform parity is not assumed.

## Current Implementation References

- `electron.vite.config.ts`, `web.vite.config.ts`, and `remote.vite.config.ts` define the build surfaces.
- `src/renderer/` is shared by the Electron renderer and web build; `src/remote/` contains the separate remote app.
- `src/preload/` exposes desktop capabilities to the renderer.
- `docs/agents/architecture.md` documents process boundaries, build targets, and import rules.
- `README.md` documents desktop/web distinctions and the web player's limitations.

Baseline reviewed: 2026-10-30.
