# Media Player Types

This page documents the key TypeScript-facing value shapes used by the Media Player API.

The player is intentionally API-first, and most integrations can rely on the types inferred
from the setup and player methods directly.

## Common Type Areas

- **Setup configuration types**:
  See [Setup & Configuration](setup.md).
- **Playback/runtime option types**:
  See [Player API](player.md).
- **Event payload types**:
  See [Events](events.md).

## Integration Guidance

- Prefer importing official exported types from the media-player package when available.
- Keep your application-facing wrappers narrow and map only the options/events you actively use.
- For long-lived apps, pin the media-player version and upgrade with type-checking enabled.
