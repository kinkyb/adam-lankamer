# adamlankamer.com — Project Knowledge

## ⛔ "Based on X" = minimal-diff adaptation of X, never rebuild-from-scratch

When explicitly asked to do something **based on / using / like / modelled on** already-completed work (port an app, adapt a page, fork a design, reuse a template or component), START FROM the existing artifact and make the **smallest set of changes** needed to fit the new product. Copy the real source files and edit them in place. Do NOT recreate the work from scratch and then diff it against the base "to ensure uniformity" — that inverts the task: it drifts from the original (subtle layout / behaviour / copy differences), silently DROPS correct details the base had, and INVENTS new ones the base never contained. The base IS the spec; deviate only where the new product genuinely requires it, and be able to name each deviation.

Burned 2026-06-05 (GuildSkills, built "based on" ClaudSkills): (1) the desktop renderer was rebuilt from scratch "to match" ClaudSkills instead of copying ClaudSkills' renderer and minimally adapting it; (2) the homepage Pro copy was written from scratch and shipped a FALSE access claim ("the free catalog never requires an account to browse or install") — ClaudSkills' real copy was the correct honest version ("Free account unlocks search · Pro unlocks one-click install"), which a minimal-diff adaptation would have preserved verbatim. Writing from scratch both lost the base's correct details and invented wrong ones.

## ⛔ Stop on ambiguity (master rule)

If there is even a slight area of uncertainty — anything unclear or ambiguous about the task, scope, file targets, parameter values, or expected outcomes — **do not make assumptions and proceed.** Stop, double-check the relevant state (read files, query systems, inspect state), and/or ask the user before taking action. Applies to every operation with non-trivial blast radius: file writes, deployments, daemon restarts, config edits, deletes, schema interpretation, anything that changes shared state.


## ⛔ One watcher per workflow trigger

When a background watcher (bash poller, log-tail loop, file-system watcher, post-condition handler, or any long-running process whose purpose is to detect a single state transition) already exists for a given workflow event — **do not spawn a second watcher polling the same condition**. Modify the existing watcher to handle the additional action(s) instead. Multiple watchers waiting on the same trigger waste PIDs, multiply log-file reads, and create race conditions between near-simultaneous firings. Examples: if a watcher is already polling for "Batch done" to un-pause file A, and you also need to un-pause file B on the same event, extend the existing watcher rather than spawning a new one.
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
- **⛔ Windows Electron installers: always hide the in-window menu bar before shipping.** Electron defaults to showing a `File / Edit / View / Help` strip across the top of every BrowserWindow on Windows (and Linux), which looks unprofessional for a focused desktop app. Required fix: call `win.setMenuBarVisibility(false)` once per window right after `win.loadFile(...)`, with NO platform guard. macOS treats it as a no-op (menu lives in the system menu bar at the top); Windows hides the strip. The menu can stay registered via `Menu.setApplicationMenu(...)` — visibility and registration are independent, so keyboard accelerators still fire and Alt brings the bar back when needed. Always re-verify on a fresh Windows install before shipping a new desktop-app version. Burned 2026-05-12 on PerfectStudio v1.2.3: `main.js` had `if (process.platform === 'darwin') win.setMenuBarVisibility(false)` — macOS-only guard, so Windows users saw the strip; fixed in v1.2.4 by removing the guard.


## Pages
| File | URL | Purpose |
|------|-----|---------|
| `index.html` | `/` | Homepage — hero (2-col with photo collage), 3 venture cards, dark about strip |
| `translatea.html` | `/translatea` | Translation services page |
| `fotostories.html` | `/fotostories` | Photography portfolio — Behold Instagram feed; Session Themes list over a darkened work-photo collage backdrop |
| `ai.html` | `/ai` | AI projects directory — 12 tool cards |
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

## AI Projects (`ai.html` — 12 cards in 2-col grid)
| # | Name | URL | Status | Pricing |
|---|------|-----|--------|---------|
| 1 | Ucaption | ucaption.online | Live | From €39/mo |
| 2 | TestYourSkills | testyourskills.app | Live | — |
| 3 | Image & Video Tagger | utagger.online | Live | From $49 |
| 4 | UTagger Viewer | utagger.online/viewer | Live | $19 one-time |
| 5 | PerfectStudio (bundles AspectPerfect · GifPerfect · FramePerfect · SlomoPerfect) | perfectstudio.app | Live | From $29 |
| 6 | AutoXPoster | autoxposter.com | Live | Quote-based |
| 7 | Telegram Channel Automation | (bespoke) | Available | — |
| 8 | ClaudSkills | claudskills.com | Live | Free |
| 9 | GuildSkills | guildskills.com | Live | Free · Pro Quality Score |
| 10 | AI-Assisted Website Development | /ai/website-development | Available | Quote-based — showcase: bahianails.com |
| 11 | AutomationFlows | automationflows.io | Live | Free · Pro from $9/mo |
| 12 | Au Naturel | aunaturel.life | Live | Free · Pro coming |

Card anchor IDs: `#ucaption`, `#testyourskills`, `#utagger`, `#utagger-viewer`, `#perfectstudio`, `#autoxposter`, `#telegram`, `#claudskills`, `#guildskills`, `#website-development`, `#automationflows`, `#aunaturel`. HTML card comment numbering uses 01–05, 08–14 (legacy gap from when PerfectStudio absorbed three separate cards). JSON-LD `ItemList` numberOfItems = 12; positions are sequential 1–12. Person schema's `sameAs` list mirrors the registry domain set and includes `https://guildskills.com`.

## Sister-brand footer family

Non-adult product sites share a sister-brand footer link strip. When a new non-adult brand ships, every footer in this list gets the new link added; each site has its own deploy command and must be redeployed individually. AdamLankamer.com itself is *not* part of this strip — it's the personal portfolio that lists everything as cards on `/ai`.

| Site | Footer file | Netlify site ID | Deploy method |
|------|-------------|-----------------|---------------|
| ClaudSkills | `~/Desktop/ClaudSkills/site/index.html` + 8 sub-pages (`install/windows/`, `skills/curated/`, `pack/{ads-mastery,content-growth-factory,outbound-engine,security-pack,official-sources-pack,product-launch-arsenal}/`) | `783c54c4-7f32-4041-bb75-2e76d369d2c9` | git-push auto-deploy (origin `kinkyb/claudskills`) |
| Ucaption | `~/Desktop/Ucaption/index.html` | `103da312-2d90-4fdf-becc-61666b46db60` | CLI `--dir .` |
| PerfectStudio (replaces stale GifPerfect / AspectPerfect / FramePerfect / SlomoPerfect entries) | `~/Desktop/PerfectStudio/site/index.html` | `99fc6122-964c-4253-a35e-e3883c84f73a` | CLI deploys conflict with UI-set build command (`expo export -p web` + publish `/dist`) — must deploy via Netlify dashboard or unset UI build command first |
| SlomoPerfect (deprecated — `_redirects` force-301s slomoperfect.com → perfectstudio.app) | `~/Desktop/SlowMo/site/index.html` | `be156c99-3286-4dfd-8529-bf5dea371ac5` | CLI `--dir site`, but edits to index.html are not user-visible due to the catch-all 301 |
| UTagger | `~/Desktop/ImageTagger/sites/utagger/index.html` | **`0826714b-9e0b-4316-b222-9e199ac86b31`** (NOT the parent `78004ab4-…` linked in `~/Desktop/ImageTagger/.netlify/state.json` — that one is `earnest-mooncake-0e5db3.netlify.app`, a separate stale site) | CLI `--dir .` with explicit `--site 0826714b-…` |
| AutoXPoster | `~/Desktop/AutoXPoster/site/index.html` | `d05e1bda-024e-4a80-af25-ff07db310b8a` | CLI `--dir site` |
| AutomationFlows | `~/Desktop/AutomationFlows/site/index.html` | `3e7588a9-7cd8-49f4-a1b2-b3945d7e6710` | CLI `--dir site` |
| Au Naturel | `~/Desktop/aunaturel-life/site/index.html` | `a63a9e8a-a41b-43a2-8314-ad8856f0ec76` | CLI `--dir site` |

TestYourSkills (web-only, no local repo) is referenced as a link from each footer.

AdamLankamer.com's own footers (`index.html` curated 3-brand strip, plus the sub-pages `translatea.html` / `fotostories.html` / `ai/website-development.html` / `privacy.html` / `terms.html` / `ai.html` — which all have flat AdamLankamer-internal nav) ALSO carry GuildSkills as of 2026-06-04, per the network-wide convention "GuildSkills appears in every footer of the non-adult family."

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

## Photography card on landing page (`/`)

- The middle venture card (`02 Photography`) in the Ventures grid is replaced by a **Foto Stories brand-mark card** instead of the standard number + SVG-icon + title + desc + link layout. CSS rules live as `.photo-card*` selectors after the `.venture-link::after` block in `index.html`; the markup uses `<a class="venture-card photo-card">` so it inherits sibling card sizing in the 3-col grid via the default `align-items: stretch`.
- Card composition: full-bleed Foto Stories logo (`assets/fotostories-card.{webp,png}`) at `object-fit: cover`, a vertical scrim `linear-gradient(rgba(14,14,14, 0.78 → 0.10 → 0.10 → 0.80))` darkening the top 26% and bottom 26%, two `.photo-card-text` paragraphs at `rgba(255,255,255,0.40)` (the original 02 description text, split on its natural sentence boundary), and a centered `.photo-card-cta` "View Fotostories →" at the bottom.
- `min-height: 380px` is required because in the mobile single-column grid (`@media (max-width: 900px)`) the card has no normal-flow children — only absolute-positioned content. Without `min-height` the card would collapse to 0 height. In 3-col desktop the grid stretches it to match the tallest sibling card.
- `.photo-card::before { z-index: 3 }` keeps the inherited `.venture-card:hover::before` red top bar visible above the absolutely-positioned image on hover.
- **Asset pipeline**: source is the GBP logo render at `~/Desktop/foto-stories-logo/foto-stories-square-v2.png` (1080×1080, generated by `~/Desktop/foto-stories-logo/render.py`). Copied to `assets/fotostories-card.png` and converted with `cwebp -q 82 → fotostories-card.webp` (95% smaller — 856 KB PNG → 45 KB WebP). HTML uses `<picture>` with WebP source + PNG fallback per the project image convention.
- If the prices or 02 copy ever changes, edit the two `.photo-card-text` paragraphs in `index.html`; if the brand mark changes, regenerate the PNG via `render.py`, re-run `cp` + `cwebp`, and re-deploy.

## Session Themes (`/fotostories`)
The "Book a Session" block (`.collab-strip`, "Let's create your photo story") lists **11 photoshoot themes**, each a **borderless transparent block** (`.theme-grid` / `.theme-card` — 2-col desktop, 1-col ≤900px) whose title (white Playfair), price (accent red), and 2–3-sentence sensual description sit over the collage backdrop. Followed by the note *"Ask about special offers for couples, families & groups."* → "For more info or booking →" cue → WhatsApp button (`https://wa.me/34641758592`). The old standalone "The work" masonry gallery + lightbox were **removed** — merged into this block as the backdrop.

| Theme | From |
|---|---|
| Fine-art portrait | €200 |
| Motorboat (licensed skipper) | €500 |
| Convertible Mustang | €350 |
| Lifestyle / beach / holiday | €150 |
| Romantic / sunset / sunrise / night | €200 |
| Artistic nudity / boudoir | €250 |
| Instagram spots / viewpoints | €200 |
| Custom theme / cosplay | €200 |
| Property & hospitality | €250 |
| Branding & commercial | €250 |
| Full day / multi-location | €1,000 |

- **Description copy rule**: every distinctive/descriptive word is unique across all 11 descriptions (only photo terms — frame, shot, shoot, session, portraits, images, light, composition — and grammatical connectors may recur). Preserve this if you rewrite a description.
- **Collage backdrop** (`.collab-bg`, dark `#0e0e0e` base): a CSS grid of darkened work photos behind the whole block; overlay gradient (`rgba(14,14,14,0.72→0.5→0.5→0.8)`) + per-tile `filter: … brightness(0.8)` + a text `text-shadow` keep the light text readable. Built by `buildCollabBackdrop()` (bottom `<script>`): shuffles the `PHOTOS` array once, then **tiles to fill COMPLETE rows sized to divide the block height exactly** (`grid-auto-rows` set in JS to `blockHeight / rows`) so the bottom row is flush — never clipped to half-height, never a gap. Re-renders on `load`, `document.fonts.ready`, and window `resize`. Photos repeat only as much as needed to complete the last row.
- **Collage photos**: the `PHOTOS` array = 25 original studio shots in `assets/` + 43 La Herradura shots in `assets/collage/` (web-sized via `sips -Z 900 -s format jpeg 78`). To add more: resize into `assets/collage/`, append `{ src: 'assets/collage/NAME.jpg' }` entries — more photos = fuller wide-screen coverage with less repetition (target ~65-75 for full 1440px coverage at one-use-each).
- **JSON-LD**: `LocalBusiness` `hasOfferCatalog` holds all 11 as `Offer` entries (`price` + `priceCurrency: EUR` + `priceSpecification.minPrice`). When a price/theme changes, edit **both** the `.theme-card` markup AND its `Offer` entry; update the `<meta … description>` trio if the headline `from €150` shifts.

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

## Netlify build diagnosis

- **⛔ Don't act on `Build script returned non-zero exit code: 2 / 4` without reading the actual deploy log + getting explicit user approval.** That surface error wraps multiple unrelated failure modes (secret-scanner false positives, file-count timeouts, plugin install errors, npm install failures, function bundling, build-env limits). The real error lives ONLY in the Netlify web UI at `https://app.netlify.com/projects/$SITE/deploys/$DEPLOY_ID` — the public REST API does NOT expose log content. Before disabling auto-builds, switching deploy mechanisms, adding env vars, overriding plugins, or any other architectural change: pause, ask the user to paste the actual log section, get approval, then act. Burned 2026-05-16 on AutomationFlows — misdiagnosed a 3-day outage as a Netlify plugin issue and shipped 3 architectural commits before the real cause (secret-scanner false positives, fixed with `SECRETS_SCAN_ENABLED=false`) was confirmed. See `~/.claude/CLAUDE.md` "Workflow Rules" for the full version.
