# adamlankamer.com — Project Knowledge

## Live URL
- Production: `https://adamlankamer.com`
- Deploy folder: `/Users/mac/Desktop/AdamLankamer/`
- GitHub repo: `https://github.com/kinkyb/adam-lankamer` (public)
- Netlify site: `adam-lankamer` (site ID: `4ab57f96-b902-47ef-a730-d9e6e4ca89b1`)

## Deploy Command
```bash
export PATH="/opt/homebrew/opt/node/bin:/opt/homebrew/bin:$PATH"
cd /Users/mac/Desktop/AdamLankamer && /opt/homebrew/bin/netlify deploy --prod --dir=. && \
  curl -s "https://api.indexnow.org/indexnow?url=https://adamlankamer.com/&key=5d2e1f3a4b5c6d7e8f9a0b1c3d4e2f6a" >/dev/null
```
The trailing `curl` pings IndexNow so Bing/Yandex pick up changes within minutes.

## Workflow Rules
- **Verify deploy target before deploying**: Before running any deploy command, confirm which Netlify site ID / project it will deploy to. Deploying to the wrong site is a silent failure.
- **Update CLAUDE.md after every push**: After every git push, update this file.
- **Always deploy after push**: Run the deploy command above after every push.
- **All files in sync**: Never leave uncommitted changes — every file must be pushed.
- **English only**: All communication, commit messages, and code comments in English.
- **⛔ Before EVERY new build, assume a bug exists and find it — whether or not one has been reported.** This is a default-on rule, not a conditional. Re-scan the entire pipeline end-to-end — front-end (UI / IPC / renderer) AND back-end (main process, business logic, pipeline, bundling) — actively hunting for the bug you've assumed is there. If you find one, fix it and re-scan. If after a thorough double-check you're 100% sure no bug exists, build a standalone repro of the **full production chain** (every post-processing step — watermarks, composites, encoders, format conversions, IPC envelope) and prove the path produces the expected output. Only at that point do you tag / commit / trigger the build. Burned 2026-05-12 on PerfectStudio v1.2.2: standalone test omitted the trailing watermark composite, missing that sharp's `.composite()` overwrites prior overlays when chained — v1.2.3 was the real fix. Cost of skipping: 2 wasted ~20-min CI builds + user-visible repeat failure + credibility hit.


## Pages
| File | URL | Purpose |
|------|-----|---------|
| `index.html` | `/` | Homepage — hero (2-col with photo collage), 3 venture cards, dark about strip |
| `translatea.html` | `/translatea` | Translation services page |
| `fotostories.html` | `/fotostories` | Photography portfolio — Behold Instagram feed, masonry gallery |
| `ai.html` | `/ai` | AI projects directory — 13 tool cards |
| `ai/website-development.html` | `/ai/website-development` | Case study — bahianails.com (AI-assisted build) |
| `privacy.html` | `/privacy` | Privacy policy (noindex) |
| `terms.html` | `/terms` | Terms of use (noindex) |

## Stack
- **Hosting**: Netlify (static HTML, no build step), pretty URLs via `netlify.toml` redirects
- **Fonts**: Google Fonts — Playfair Display (headings) + DM Mono (body)
- **No frameworks**: pure HTML/CSS/JS

## Design System
- `--bg: #f5f2ee` · `--surface: #eeeae6` · `--border: #d8d2ca`
- `--accent: #c8102e` (wine-red brand colour)
- `--text: #0e0e0e` · `--muted: #6b6259`
- `--nav-bg: rgba(14,14,14,0.94)` (dark nav)
- Typography: **Playfair Display 700** (headings) + **DM Mono 300/400/500** (body)
- Logo treatment: `<span class="logo-a">A</span>dam Lankamer` (red "A") on every page
- Nav: dark `#0e0e0e`, hamburger is a semantic `<button>` with aria-label
- Flat design — no backdrop-filter, no box-shadow, sharp corners
- 1px-gap grids: outer div sets `background: var(--border)`, children use `var(--surface)`

## AI Projects (`ai.html` — 13 cards in 2-col grid)
| # | Name | URL | Status | Pricing |
|---|------|-----|--------|---------|
| 1 | Ucaption | ucaption.online | Live | From €39/mo |
| 2 | TestYourSkills | testyourskills.app | Live | — |
| 3 | Image & Video Tagger | utagger.online | Live | From $49 |
| 4 | UTagger Viewer | utagger.online/viewer | Live | $19 one-time |
| 5 | GifPerfect | gifperfect.com | Live | From $29 |
| 6 | Slo-Mo Perfect | slomoperfect.com | Live | From $29 |
| 7 | Aspect Perfect | aspectperfect.com | Live | From $29 |
| 8 | AutoXPoster | autoxposter.com | Live | Quote-based |
| 9 | Telegram Channel Automation | (bespoke) | Available | — |
| 10 | ClaudSkills | claudskills.com | Live | Free |
| 11 | AI-Assisted Website Development | /ai/website-development | Available | Quote-based — showcase: bahianails.com |
| 12 | AutomationFlows | automationflows.io | Live | Free · Pro from $9/mo |
| 13 | Au Naturel | aunaturel.life | Live | Free · Pro coming |

Each card uses an anchor ID matching its slug (e.g. `#ucaption`, `#automationflows`, `#aunaturel`, `#website-development`) for deep-linking.

## Sister-brand footer family

Non-adult product sites share a sister-brand footer link strip. When a new non-adult brand ships, every footer in this list gets the new link added; each site has its own deploy command and must be redeployed individually. AdamLankamer.com itself is *not* part of this strip — it's the personal portfolio that lists everything as cards on `/ai`.

| Site | Footer file (line ranges shift with edits) |
|------|--------------------------------------------|
| ClaudSkills | `~/Desktop/ClaudSkills/site/index.html` |
| Ucaption | `~/Desktop/Ucaption/index.html` |
| GifPerfect | `~/Desktop/GifPerfect/site/index.html` |
| AspectPerfect | `~/Desktop/AspectPerfect/site/index.html` |
| SlomoPerfect | `~/Desktop/SlowMo/site/index.html` |
| UTagger | `~/Desktop/ImageTagger/sites/utagger/index.html` |
| AutoXPoster | `~/Desktop/AutoXPoster/site/index.html` |
| AutomationFlows | `~/Desktop/AutomationFlows/site/index.html` |
| Au Naturel | `~/Desktop/aunaturel-life/site/index.html` |

TestYourSkills (web-only, no local repo) is referenced as a link from each footer.

## SEO Setup
- Canonical tags on all pages (no `.html` extension — pretty URLs)
- `sitemap.xml` — lastmod bumped on every deploy
- `robots.txt` — allow `*` plus explicit allow rules for AI crawlers (GPTBot, ClaudeBot, anthropic-ai, PerplexityBot, Google-Extended, CCBot, Applebot-Extended, Bytespider)
- `llms.txt` — site summary for AI engines
- IndexNow key: `5d2e1f3a4b5c6d7e8f9a0b1c3d4e2f6a.txt` (pinged automatically by deploy command)
- Favicons: `favicon.ico` + 16/32px PNG + apple-touch-icon (180×180), generated from Adam's portrait
- Per-page security headers + immutable asset cache via `netlify.toml`
- Per-page OG images:
  - `/` → `assets/adam-casual-1.jpg`
  - `/ai` → `assets/og-ai.jpg`
  - `/translatea` → `assets/og-translatea.jpg`
  - `/fotostories` → `assets/DSC00763-25.jpg`
- Structured data per page:
  - `/` Person + WebSite (with SearchAction for sitelinks search box)
  - `/translatea` ProfessionalService + FAQPage + BreadcrumbList
  - `/fotostories` LocalBusiness + ProfessionalService + VideoObject (geo-tagged La Herradura)
  - `/ai` ItemList (numberOfItems 13) + BreadcrumbList + Service (AI Website Development) + FAQPage
  - `/ai/website-development` Article + BreadcrumbList
- Geo meta on `/fotostories`: `geo.region=ES-GR`, La Herradura/Almuñécar/Granada coords

## Internal Linking
- All internal links use clean URLs (no `.html`)
- All external links: `target="_blank" rel="noopener noreferrer"`
- Card anchors enable cross-page deep-linking, e.g. `/ai#ucaption` from `/translatea` body copy

## Brand Notes
- `translatea.com` is email-only (`adam@translatea.com`); all web presence lives at `adamlankamer.com`
- Public contact email per global policy: `acreatorstore@translatea.com` (non-adult side)

## Image pipeline
- WebP siblings live next to JPEG masters at the same path: `adam-casual-3.jpg` ↔ `adam-casual-3.webp`
- HTML uses `<picture>` with WebP `<source>` and JPEG `<img>` fallback
- Convert with: `cwebp -q 80 input.jpg -o input.webp` (cwebp 1.6.0 via Homebrew)

## Related Projects
| Project | Folder |
|---------|--------|
| Ucaption | `~/Desktop/Ucaption` |
| GifPerfect | `~/Desktop/GifPerfect` |
| Image Tagger | `~/Desktop/ImageTagger` |
| Video Tagger | `~/Desktop/VideoTagger` |
| Slo-Mo Perfect | `~/Desktop/SlowMo` (site) · `~/Desktop/electron-apps/packages/slomoperfect` (app) |
| Aspect Perfect | `~/Desktop/AspectPerfect` |
| AutoXPoster | `~/Desktop/AutoXPoster` |
| ClaudSkills | `~/Desktop/ClaudSkills` |
| AutomationFlows | `~/Desktop/AutomationFlows` |
| Au Naturel | `~/Desktop/aunaturel-life` |
| Telegram Channel Bot | `~/Desktop/TelegramApp` |

## File Overwrite Policy

- **⛔ Never silently overwrite a local file.** Before any operation that would replace an existing file on the local machine (image/format conversion, codegen, downloads, copies, moves to an occupied path, save-as targets, batch processing), check whether a same-named file already exists at the destination. If it does, **stop and ask the user**: overwrite, or pick a new name? Do not decide on your own based on file size, content similarity, timestamps, single-frame-vs-animated checks, or any other heuristic. The user has to choose. This applies even when the existing file looks redundant, auto-generated, or "obviously" derived from the same source. **Burned 2026-05-12**: silently overwrote 33 single-frame `.gif` files in `/Volumes/All/Gifs/1-25 MB/` during a batch JPG→GIF conversion after self-deciding it was safe.
