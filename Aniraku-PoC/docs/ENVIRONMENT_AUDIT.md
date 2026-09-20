# ENVIRONMENT_AUDIT.md

## Backend (Aniraku-Backend)

### Required for basic operation
| Variable | Purpose | Secret? |
|----------|---------|---------|
| ANIRAKU_SUPABASE_URL | Auth + account | Yes (project URL) |
| ANIRAKU_SUPABASE_ANON_KEY | Client JWT validation | Yes |
| ANIRAKU_SUPABASE_SERVICE_KEY | Server-side admin | Yes (critical) |
| ANIRAKU_SUPABASE_JWKS_URL | Optional override | Optional |
| TMDB_READ_ACCESS_TOKEN / ANIRAKU_TMDB_READ_ACCESS_TOKEN | Episode metadata fallback | Yes (v4 token) |
| ANIRAKU_SERVER_HOST / ANIRAKU_SERVER_PORT | Bind address | No |
| ANIRAKU_CORS_ORIGINS | Allowed frontends | No |

### Optional / feature flags
| Variable | Purpose |
|----------|---------|
| ANIRAKU_ANIKOTO_MAPPING_PATH | Local mapping override |
| ANIRAKU_ANIMEX_BASE / ANIRAKU_ANIMEX_PLYR_API | AnimeX endpoints |
| ANIRAKU_FLIXCLOUD_BASE / ANIRAKU_REANIME_BASE | FlixCloud |
| ANIRAKU_ANIZIP_BASE | AniZip |
| ANIRAKU_MAL_CLIENT_ID / SECRET | MAL OAuth |
| ANIRAKU_ANILIST_CLIENT_ID / SECRET | AniList OAuth |
| ANIRAKU_OAUTH_REDIRECT_URL / STATE_SECRET | OAuth |
| ANIRAKU_TRUSTED_PROXY_CIDRS | RealIP |
| ANIRAKU_PROXY_CDN_ALLOWLIST | Extra CDN hosts |

### GitHub Actions secrets (from workflows)
- See GITHUB_SECRETS_REQUIRED.md

## Android App (Aniraku-App)

### Public (EXPO_PUBLIC_*)
- EXPO_PUBLIC_SUPABASE_URL
- EXPO_PUBLIC_SUPABASE_ANON_KEY
- EXPO_PUBLIC_ANILIST_CLIENT_ID
- EXPO_PUBLIC_ANILIST_GRAPHQL_URL
- EXPO_PUBLIC_API_BASE_URL  (points to your Backend)
- EXPO_PUBLIC_MAL_CLIENT_ID

No private keys should be in the app binary.

## Signing / Deployment
- Android: local keystore or EAS credentials (not in repo).
- Backend: Docker / Render / any Go host. No signing secrets in source.
