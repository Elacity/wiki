# Media Player Troubleshooting

This page collects common troubleshooting paths for browser-based playback integrations.

## Playback Fails Immediately

- Verify DRM/license endpoints are reachable from the browser.
- Confirm the manifest is MPEG-DASH with valid encrypted stream metadata.
- Check browser console/network tab for CORS, certificate, or license errors.

See:
- [DRM Systems](../architecture/drm-systems.md)
- [Player API](../api/player.md)

## Browser Compatibility Issues

- Confirm codec support for your target browsers.
- Verify EME/MSE support in your runtime environment.
- Avoid unsupported WebView/runtime combinations for encrypted playback.

See:
- [Media Formats](../architecture/media-formats.md)
- [MSE Dependency](../architecture/mse-dependency.md)

## Decryption or Key Retrieval Errors

- Validate KID/content-ID mapping between manifest and backend responses.
- Confirm wallet/auth/session state before starting playback.
- Re-check license payload format and signing assumptions.

See:
- [Architecture Overview](../architecture/overview.md)
- [Events](../api/events.md)

## Need Environment-Specific Help

For build-from-source workflows and environment-specific development notes, refer to the
media-player repository README:

- <https://github.com/elacity/media-player/blob/main/README.md#development>
