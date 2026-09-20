# Aniraku PoC — Single Provider Integration Package

This archive contains the **original** Aniraku-Backend and Aniraku-App repositories plus audit documentation.

## Contents
- `Aniraku-Backend/` — Go streaming backend (Anikoto primary + 4 others)
- `Aniraku-App/` — Expo/React Native Android client
- `docs/` — RESEARCH_REPORT, ENVIRONMENT_AUDIT, PROVIDER_REPORT, VALIDATION_REPORT, GITHUB_SECRETS_REQUIRED

## Chosen Provider for PoC
**Anikoto** (already primary). No code changes required; domains verified live.

## Quick Start (Backend)
```bash
cd Aniraku-Backend
cp .env.example .env
# fill Supabase + TMDB tokens
go mod download
go run ./cmd/aniraku-server
```

## Quick Start (App)
```bash
cd Aniraku-App
pnpm install --frozen-lockfile
# create .env with EXPO_PUBLIC_* values pointing at your backend
pnpm android   # or download official APK for testing
```

## GitHub Setup
1. Create new empty repo.
2. Upload this ZIP (or `git push` the two folders).
3. Add secrets listed in `docs/GITHUB_SECRETS_REQUIRED.md`.
4. Enable Actions if desired.
5. Point App `EXPO_PUBLIC_API_BASE_URL` at your deployed Backend.

## Next Steps (after PoC succeeds)
1. Promote remaining providers in Manager order if needed.
2. Add live probe tests in CI.
3. Monitor upstream domain changes.
4. Expand CDN allowlist via host learning.
