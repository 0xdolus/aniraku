# RESEARCH_REPORT.md — Aniraku Repository Audit

**Date:** 2026-09-20  
**Scope:** Aniraku-Backend (Go) + Aniraku-App (Expo / React Native Android)

## 1. Android Modules (Aniraku-App)

- **Stack:** Expo SDK (newArchEnabled: false for compatibility), React Native, TypeScript, pnpm.
- **Structure:**
  - `app/` — file-based routing (tabs, anime, episode, library, search, settings, auth, oauth, legal).
  - `components/` — UI primitives and feature components.
  - `constants/` — theme, config.
  - `drizzle/` — local SQLite schema + migrations (watch progress, library).
  - Player: Media3 / ExoPlayer path via Expo AV / custom native modules for HLS, subtitles, PiP, gestures, AniSkip intro/outro.
- **Build:** EAS + local Gradle (versionCode 67, package `aniraku.anime.app`, Android 9+).
- **Config:** `app.config.ts`, public env via `EXPO_PUBLIC_*`.

## 2. Backend Modules (Aniraku-Backend)

- **Language:** Go (single static binary).
- **Router:** Chi.
- **Key packages under `internal/`:**
  - `api/v1` — handlers (catalog, episodes, stream, proxy, account, sync, metrics).
  - `streaming` — provider implementations + Manager.
  - `metadata` — AniZip + TMDB bidirectional.
  - `netguard` — SSRF protection, uTLS, CDN allowlist.
  - `auth` — Supabase JWT / JWKS.
  - `core` — shared models.
  - `tmdb`, `embed`.
- **Entry:** `cmd/aniraku-server/main.go`.
- **Config:** `config.yaml` + env vars (ANIRAKU_*).

## 3. Streaming Architecture

```
Client (Web / Android)
        │
        ▼
Chi HTTP router (/api/v1/...)
        │
   ┌────┼────┐
   ▼    ▼    ▼
auth  episodes  streaming
Supabase  AniZip↔TMDB  Manager
                 │
        ┌────────┼────────┐
        ▼        ▼        ▼
     Anikoto  AnimeX   Zoko
     OGFLix   FlixCloud
```

- Manager holds ordered list of `Provider` implementations.
- Each provider implements Search / Resolve / Servers / Stream (m3u8 or embed).
- Hentai gate: only OGFLix + FlixCloud for NSFW titles.
- Proxy path: uTLS + allowlist + HLS rewrite for direct streams.
- Host learning feeds CDN allowlist dynamically.

## 4. Provider Manager

- File: `internal/streaming/manager.go`
- Interface: `Provider` with Name(), Search, etc.
- Registration order (primary → fallback) controls preference.
- Anikoto is documented primary; FlixCloud fallback for embeds.

## 5. Player Pipeline (Android)

- Backend returns normalized stream objects (HLS master, subtitles[], intro/outro).
- App selects quality, applies headers/referers, feeds ExoPlayer / Media3.
- Embed fallback for FlixCloud-style sources.
- AniSkip integration for skip buttons.
- Offline download path uses backend download links (allowlisted).

## 6. Network Layer

- Client → Backend API (base URL from EXPO_PUBLIC_API_BASE_URL).
- Backend → upstream providers (custom http.Client, referers, headers).
- Media proxy (`/api/v1/proxy`) for CORS / geo / referer issues.
- SSRF guard + host allowlist.

## 7. API Routes (key)

- `GET /api/v1/anime/{id}/episodes` — episode list + metadata.
- `POST /api/v1/stream` / `GET /api/v1/servers` — stream resolution.
- `GET /api/v1/proxy` — media proxy.
- Catalog: search, trending, seasonal, schedule, etc.
- Account / sync / OAuth for AniList + MAL.

## 8. Environment Configuration

- Backend: `.env.example` already present (Supabase, TMDB, OAuth, CORS, proxy allowlist).
- App: public client keys only (Supabase anon, AniList client id, API base).

## 9. Build System

- Backend: `go build`, Docker, Render.yaml, GitHub Actions (ci, release, keep-awake).
- App: pnpm + Expo / EAS, GitHub Actions for APK releases.

## Conclusion

Architecture is already multi-provider and production-ready. Minimal changes needed for a single-provider PoC: keep Anikoto as primary, document verification, package both repositories intact.
