# Feature Specification: Music Playback

**Feature Branch**: `baseline/music-playback`

**Created**: 2026-10-30

**Status**: Implemented baseline

**Input**: Existing Feishin playback behavior and available player-engine ownership areas.

## User Scenarios & Testing

### User Story 1 - Play music (Priority: P1)

As a listener, I can start music from the library and control the currently playing item.

**Why this priority**: Playback is the central user outcome of a music player.

**Independent Test**: Start an available track and verify that playback state and transport controls respond.

**Acceptance Scenarios**:

1. **Given** a playable item, **When** I start it, **Then** Feishin begins playback using an available player engine.
2. **Given** an item is playing, **When** I pause or resume, **Then** the playback state changes accordingly.
3. **Given** an item is playing, **When** I seek to another position, **Then** playback moves to the requested position when the engine and media support seeking.

### User Story 2 - Manage what plays next (Priority: P1)

As a listener, I can build and adjust a queue so I control the order of upcoming music.

**Why this priority**: Queue management is part of the continuous listening workflow.

**Independent Test**: Add multiple playable items, reorder or remove an upcoming item, and verify the resulting playback order.

**Acceptance Scenarios**:

1. **Given** playable items, **When** I add them to the queue, **Then** they are available for sequential playback.
2. **Given** upcoming items in the queue, **When** I change their order or remove one, **Then** the queue reflects that change.

### User Story 3 - Use an available playback engine (Priority: P2)

As a listener, I can play music through the engine available in my client and setup.

**Why this priority**: Playback options differ by client and configured devices.

**Independent Test**: Verify playback through the relevant configured engine, including the web player in web builds.

**Acceptance Scenarios**:

1. **Given** a web client, **When** I play supported media, **Then** playback uses the web player.
2. **Given** a desktop client with MPV configured, **When** I select MPV playback, **Then** playback uses the MPV engine.
3. **Given** another supported engine is configured, **When** I select it, **Then** Feishin routes playback through that engine where the client supports it.

## Edge Cases

- The selected engine is unavailable or cannot load the current item.
- A server returns a stream or media item that cannot be played by the selected engine.
- The current item ends while the queue contains no further playable items.
- An engine does not support a requested transport operation.

## Requirements

### Functional Requirements

- **FR-001**: Users MUST be able to start and control playback for available playable music.
- **FR-002**: Users MUST be able to add items to a queue and manage upcoming playback order.
- **FR-003**: Feishin MUST use the playback engine selected or available for the current client and setup.
- **FR-004**: Feishin MUST account for engine-specific support when offering playback operations.
- **FR-005**: Where configured, Feishin MUST support reporting playback to the connected music server (scrobbling).
- **FR-006**: Where supported, users MUST be able to view synchronized or unsynchronized lyrics for a playing item.

### Key Entities

- **Playback item**: A playable music item and its associated metadata.
- **Queue**: The ordered set of items available for current and subsequent playback.
- **Playback engine**: A supported mechanism that obtains and plays media in a given client or setup.
- **Playback state**: The current item, transport state, and position exposed to the user.

## Success Criteria

### Measurable Outcomes

- **SC-001**: A user can start, pause, resume, and advance available playback through a supported engine.
- **SC-002**: Queue changes are reflected in the order of subsequent playback.
- **SC-003**: Web playback works without desktop-only capabilities.
- **SC-004**: Scrobbling and lyrics are used only when available and configured for the current item/server.

## Assumptions

- Media availability and format support depend on the connected server, media, and selected engine.
- Some player engines and operating-system integrations are desktop-only.
- Scrobbling and lyrics availability depends on server support and item metadata.

## Current Implementation References

- `src/renderer/features/player/` owns playback UI, state, and engine selection.
- `src/renderer/features/player/audio-player/engine/` contains web, MPV, DLNA, and jukebox engine implementations.
- `src/preload/` exposes desktop playback integrations through the typed preload API.
- `src/renderer/features/lyrics/` owns lyric workflows; `README.md` documents playback backends, scrobbling, and lyrics support.
- `docs/agents/architecture.md` documents desktop-only capability boundaries.

Baseline reviewed: 2026-10-30.
