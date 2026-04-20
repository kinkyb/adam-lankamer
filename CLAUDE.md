# adamlankamer.com — Project Knowledge

## Live URL
- Production: `https://adamlankamer.com`
- Deploy folder: `/Users/mac/Desktop/AdamLankamer/`
- GitHub repo: `https://github.com/kinkyb/adam-lankamer` (public)
- Netlify site: `adam-lankamer` (site ID: `4ab57f96-b902-47ef-a730-d9e6e4ca89b1`)

## Deploy Command
```bash
export PATH="/opt/homebrew/opt/node/bin:/opt/homebrew/bin:$PATH"
cd /Users/mac/Desktop/AdamLankamer && /opt/homebrew/bin/netlify deploy --prod --dir=.
```

## Workflow Rules
- **Verify deploy target before deploying**: Before running any deploy command, confirm which Netlify site ID / project it will deploy to. Check `.netlify/state.json` or use `--site` flag explicitly. Deploying to the wrong site is a silent failure — the correct site gets nothing.
- **Update CLAUDE.md after every push**: After every git push, update the project's CLAUDE.md to reflect any changes made — new features, changed behaviour, updated stack details, new rules. Commit and push the CLAUDE.md update immediately after.
- **Git push on every change**: After any code change, immediately commit and push to GitHub. No exceptions.
- **Always deploy**: After pushing to GitHub, always run the Netlify deploy command above.
- **All files in sync**: Never leave uncommitted changes — every file in the project must be pushed including robots.txt, sitemap.xml, favicons, etc.
- **English only**: All communication, commit messages, and code comments in English.
- **Trust the user — search harder before contradicting**: If the user says something has been solved or exists in another workflow, do not contradict them based on a single failed search. Search again more thoroughly (different terms, subagents, broader scope) before saying it cannot be found. The user is usually right.
- **Verify before confirming anything**: Never confirm that something is true, done, or working without first checking proof. This applies to mid-conversation statements ("yes, X is now the case", "that's already in place", "it's working") as well as task completion. Assume the statement is FALSE until you have verified it with a direct check (read the file, query the state, check the log, hit the endpoint). A successful CLI command is not proof of the outcome. If you haven't checked, don't confirm.

## Site Overview
Personal brand site for Adam Lankamer — professional translator, fine art photographer, and AI entrepreneur. All brands and projects live under one roof at adamlankamer.com.

## Pages
| File | URL | Purpose |
|------|-----|---------|
| `index.html` | `/` | Homepage — hero (2-col with photo collage), 3 venture cards, dark about strip |
| `translatea.html` | `/translatea` | Translation services page |
| `fotostories.html` | `/fotostories` | Photography portfolio — dark hero, Behold Instagram feed, masonry gallery |
| `ai.html` | `/ai` | AI projects — vertical product listing (9 products, NOT a slider) |

## Stack
- **Hosting**: Netlify (static HTML — no build step)
- **Fonts**: Google Fonts — Playfair Display (headings) + DM Mono (body)
- **No frameworks**: Pure HTML/CSS/JS
- **Assets**: `/assets/` folder — photos, images

## Design System (April 2026 redesign — matches acreatorslab.com)
- `--bg: #f5f2ee` — warm off-white background
- `--surface: #eeeae6` — card/panel surface
- `--border: #d8d2ca` — border colour
- `--accent: #c8102e` — wine red (primary brand colour)
- `--text: #0e0e0e` — near-black
- `--muted: #6b6259` — warm grey
- `--nav-bg: rgba(14,14,14,0.94)` — dark nav background
- Fonts: **Playfair Display 700** (headings, serif) + **DM Mono 300/400/500** (body/mono)
- Red "A" in logo: `<span class="logo-a">A</span>dam Lankamer` across ALL pages
- Flat design: NO backdrop-filter/blur, NO box-shadow, sharp corners
- 1px gap grids: outer div sets `background: var(--border)`, children `background: var(--surface)`
- Nav: dark (#0e0e0e) with white/muted links — consistent across all pages
- About strip on homepage: dark (#0e0e0e background), white text

## AI Projects (ai.html — ACreatorStore hero, 2-col flat tool cards, NOT a slider)
| # | Name | URL | Status | Pricing |
|---|------|-----|--------|---------|
| 1 | Ucaption | ucaption.online | Live | From €39/mo |
| 2 | TestYourSkills | testyourskills.app | Live | — |
| 3 | Image & Video Tagger | utagger.online | Live | From $49 |
| 4 | UTagger Viewer | utagger.online/viewer | Live | $19 one-time |
| 5 | GifPerfect | gifperfect.com | Live | From $29 |
| 6 | Slo-Mo Perfect | slomoperfect.com | Live | From $29 |
| 7 | Aspect Perfect | aspectperfect.com | Live | From $29 |
| 8 | AutoXPoster | autoxposter.com | In Development | — |
| 9 | Telegram Channel Automation | contact via email | Available (bespoke) | — |

ai.html layout: centered "ACreatorStore" Playfair hero (100px), eyebrow with flanking lines, 2-col pure-text tool cards (no dark panels), red top-bar hover animation per card. 9 cards in 2-col grid (no full-width span). No Telegram in footer. Ucaption card lists Chrome Extension (Live) + context-menu integration (In Development).

## Instagram Feed
- Embedded on `/fotostories` via **Behold.so** widget (free tier)
- Feed ID: `4hswJ1WJTmaIf1WQpgg6` — account: `@foto__stories`
- Widget placed above the existing Instagram CTA strip
- To change layout/posts: log in to behold.so and edit the widget there

## SEO Setup
- Canonical tags on all pages (no `.html` extension — Netlify pretty URLs)
- `sitemap.xml` — 4 URLs, updated on every deploy
- `robots.txt` — allow all, sitemap pointer
- Favicons: `favicon.ico` (16/32/48px multi-size) + `favicon-32x32.png` + `favicon-16x16.png` + `apple-touch-icon.png` (180×180) — all generated from Adam's portrait photo (`adam-casual-3.jpg`, April 2026). No SVG favicon.
- Favicon source: tight headshot, face centered, works well at 32×32
- Structured data: Person (index), ProfessionalService (translatea, fotostories), ItemList + BreadcrumbList (ai), FAQPage (translatea)
- `og:locale`, OG tags, Twitter Card on all pages
- Google Search Console verified (verification tag on index.html)

## Internal Linking Rules
- All internal links use clean URLs (no `.html` extension) — e.g. `/translatea` not `/translatea.html`
- All external links use `target="_blank" rel="noopener noreferrer"`

## Brand Notes
- `translatea.com` is no longer a website — it is an email domain only (`adam@translatea.com`)
- All projects and brands live under `adamlankamer.com`
- `ucaption.online` footer carries `Copyright © 2026 adamlankamer.com`

## Related Projects
| Project | Folder | Notes |
|---------|--------|-------|
| Ucaption | `~/Desktop/Ucaption` | GitHub: kinkyb/darling-cassata-186fd2 (private) |
| GifPerfect | `~/Desktop/GifPerfect` | |
| Image Tagger | `~/Desktop/ImageTagger` | Internal CLI tool — utagger.online |
| Video Tagger | `~/Desktop/VideoTagger` | Internal CLI tool |
| Telegram Channel Bot | `~/Desktop/TelegramApp` | Bot + mini app — bespoke service |
| Slo-Mo Perfect | `~/Desktop/SlomoPerfect` (check) | slomoperfect.com |
| Ucaption (old landing stub) | `~/ucaptionlanding` | Stale — do not deploy |
