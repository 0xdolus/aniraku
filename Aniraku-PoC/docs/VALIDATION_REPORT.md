# VALIDATION_REPORT.md

## Chosen Provider
**Anikoto** — highest confidence (primary path, full m3u8 + metadata, domains live).

## Validation Checklist

| Item | Status | Notes |
|------|--------|-------|
| Android configuration | Pass | app.config.ts, package, permissions, deep links intact |
| Backend routes | Pass | /api/v1/stream, /servers, /proxy present |
| Provider registration | Pass | AnikotoProvider in Manager; host learner wired |
| Stream resolution path | Pass (source) | AniList ID → mapping → servers → decrypt → HLS |
| Playback expectations | Pass (source) | Returns master.m3u8 + subtitles[] + intro/outro for Media3 |
| Domain reachability | Pass | anikototv.to → 200 |
| Secrets present in repo | Fail (expected) | Only placeholders; real values required at deploy |

## Cannot verify without live keys / device
- End-to-end playback of a specific episode (requires TMDB/Supabase + real upstream response).
- Android APK build success on this sandbox (no Android SDK / Gradle full toolchain).
- Rate-limit / Cloudflare behaviour under sustained load.

## Minimal modifications made
- None to provider code (already correct).
- Documentation only + packaging.
- Existing providers left untouched.
