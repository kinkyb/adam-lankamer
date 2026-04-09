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
| `index.html` | `/` | Homepage — hero, ventures overview, about strip |
| `translatea.html` | `/translatea` | Translation services page |
| `fotostories.html` | `/fotostories` | Photography portfolio page |
| `ai.html` | `/ai` | AI projects slider (7 slides) |

## Stack
- **Hosting**: Netlify (static HTML — no build step)
- **Fonts**: Google Fonts — Cormorant Garamond + DM Sans
- **No frameworks**: Pure HTML/CSS/JS
- **Assets**: `/assets/` folder — photos, images

## Design System
- `--bg: #f9f7f4` — warm off-white background
- `--accent: #5b9ba6` — teal (primary brand colour)
- `--text: #1e1a17` — near-black
- `--muted: #8a7f76` — warm grey
- `--warm: #c4a882` — gold/warm accent
- Fonts: Cormorant Garamond (headings, serif) + DM Sans (body)
- Minimal, editorial aesthetic — no Bootstrap, no utility frameworks

## AI Projects (ai.html slider — 7 slides)
| # | Name | URL | Status |
|---|------|-----|--------|
| 1 | Ucaption | ucaption.online | Live |
| 2 | TestYourSkills | testyourskills.app | Live |
| 3 | Image & Video Tagger | utagger.online | In Development |
| 4 | AutoXPoster | autoxposter.com | In Development |
| 5 | GifPerfect | gifperfect.com | Live |
| 6 | Telegram Channel Automation | — (bespoke service, contact via email) | Available |
| 7 | Slo-Mo Perfect | slomoperfect.com | Live |

## SEO Setup
- Canonical tags on all pages (no `.html` extension — Netlify pretty URLs)
- `sitemap.xml` — 4 URLs, updated on every deploy
- `robots.txt` — allow all, sitemap pointer
- `favicon.svg` + `favicon-32x32.png` + `apple-touch-icon.png`
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
