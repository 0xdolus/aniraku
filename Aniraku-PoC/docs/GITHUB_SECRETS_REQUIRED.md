# GITHUB_SECRETS_REQUIRED.md

## Required for CI / Release (Backend)

From `.github/workflows/`:

| Secret | Used by | Required? | Notes |
|--------|---------|-----------|-------|
| (none hard-coded in public workflows beyond standard) | ci.yml, release.yml | — | Workflows use standard GitHub token. Add your own for Docker/Render deploy if desired. |

Typical additions for a private fork deploy:

- `DOCKER_USERNAME` / `DOCKER_PASSWORD` or `GHCR_TOKEN` (optional)
- `RENDER_API_KEY` (if using Render)
- Supabase / TMDB tokens should **never** be stored as GitHub secrets that are printed; inject at runtime on the host.

## Required for Android App Releases

| Secret | Used by | Required? | Notes |
|--------|---------|-----------|-------|
| EAS_TOKEN or Expo credentials | EAS build | Optional | Prefer local Gradle for open-source APKs |
| ANDROID_KEYSTORE_BASE64 | Signing | Optional for public APKs | Keep private |
| ANDROID_KEYSTORE_PASSWORD | Signing | Optional | |
| ANDROID_KEY_ALIAS / PASSWORD | Signing | Optional | |

## Optional but recommended
- `TMDB_READ_ACCESS_TOKEN` (if CI runs live metadata tests)
- Any deploy target secrets (Vercel, Cloudflare, etc.) for web frontend if present.

**Rule:** Never invent secrets. Only report what the repository actually references. Real values must be supplied by the repository owner.
