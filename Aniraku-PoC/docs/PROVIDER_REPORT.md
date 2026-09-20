# PROVIDER_REPORT.md

Audit date: 2026-09-20. Domains probed via HTTPS HEAD.

| Provider | Domain(s) | Status | Implementation | Obvious breakages | Confidence |
|----------|-----------|--------|----------------|-------------------|------------|
| **Anikoto** | anikototv.to, megaplay.buzz, anivexa-api-*.onrender.com | HTTP 200 | Full (primary): AniList→show→episodes→servers→embed decrypt→m3u8 + subs + intro/outro | None observed in source | **High** |
| **AnimeX** | pp.animex.one, plyr.animex.one, cdnx.aniwatchtv.site, animex.one | HTTP 200 | Full: CDN + plyr API | Possible CDN rotation fragility | Medium-High |
| **Zoko** | zokoanime.video | HTTP 200 | Full: /stream/ani and /stream/mal paths | None observed | Medium-High |
| **OGFLix** | ogflix.tr | HTTP 200 | Full API (search/info/stream) | Hentai-oriented path | Medium |
| **FlixCloud** | flixcloud.cc, reanime.to | HTTP 200 | Embed-focused fallback | Depends on reanime API + embed player | Medium |

## Notes
- All primary domains returned 200 at probe time.
- Anikoto is already wired as the primary provider in Manager + README.
- Hentai titles are gated to OGFLix + FlixCloud only.
- No dead code paths or compile-time stubs detected for the five providers.
