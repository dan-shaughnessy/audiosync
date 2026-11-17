# AudioSync fork

Upstream: https://github.com/yassersutojoyo/audiosync

This fork starts from v0.9.2, commit
66f99e5d78caa7e356d715c1f633bffd6903ce85.

## macOS AirPlay receiver compatibility

AirTunes/1005.13.5 on macOS 27.2.1 rejects the custom
`User-Agent: AudioSync/0.1.0` header with HTTP/RTSP 403 before pairing.
A controlled receiver test returned 200 for the same request without
that optional header. The AirPlay 2 request builder therefore omits it.
The SETUP body still identifies AudioSync; pairing, encryption and
receiver approval are unchanged. Legacy AirPlay requests are unchanged.

The regression test exercises pairing and stream setup with a fake
receiver that refuses requests containing User-Agent. The existing
AirPlay 2 session and PIN-pairing suites cover the rest of the handshake.

Build using docs/DEVELOPMENT.md. Local builds disable the upstream
updater, and the first launch may require fresh audio-capture approval
because the local signing identity differs from the upstream release.
Keep the original app bundle as a rollback copy before installation.

## Planned channel routing

Per-output Stereo, Left to Both, and Right to Both routing is not yet
implemented. The intended behavior is to split channels only during a
two-Mac streaming session, leaving ordinary playback in stereo.
