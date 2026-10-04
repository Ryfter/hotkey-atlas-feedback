# Hotkey Atlas

> This public repository holds the project README and the issue tracker. The app source lives in a private repository, so source and doc file references below are descriptive only.

A dependency-free web app that draws keyboard-shortcut references as SVG: **Top 10**, **Top 25**, **All**, or **My list**, on full keyboards, compact keyboards, a modifier wheel, category cards, a one-page poster, and printable 9.25 × 8 in mousepad templates. Every view exports as SVG, PNG or PDF.

- **Live site:** https://atomicego.com/proj/hotkeyatlas/
- **Bugs, suggestions, new apps:** https://github.com/Ryfter/hotkey-atlas-feedback/issues (public, issues only; see [Feedback and bug reports](#feedback-and-bug-reports))
- **Status:** proof of concept / work in progress. Expect rough edges.

The in-app title is still "Hotkey Reference" (`config/site.json` → `siteName`). The project name is Hotkey Atlas.


## Why this exists

This started as an experiment. I watched a [video by John Kim](https://www.youtube.com/watch?v=f-Ar8mwm3kQ) ([X](https://x.com/PremiumGoblin), [Substack](https://substack.com/@realjohnkim)) about building animated apps with AI, and I wanted to try the idea myself.

About a year ago I had played with using AI-generated SVG files to make cheat sheets for applications. SVG is portable, scales cleanly and is easy to restyle, so a whole set of cheat sheets seemed worth trying. I wanted a quick MVP and tech demo for my GenAI Projects class at Boise State.

I opened Grok Bot, talked it through with my chief-of-staff bot, and worked out what to build. Then the build ran as a program: I acted as the customer, a Grok Bot agent acted as program manager, and the coding CLIs (Cursor Composer, Grok Build, the Antigravity CLI) wrote the code. The details are in "How it came to be" below.

## What is in it

- **37 command sets** in 8 groups (Office, Google, Code editors, Education, AI, Browsers, Creative & dev, Comms), with **5,496 commands** in total. Omarchy (173 commands) and Unity (80) are the original hand-built references. Minimal Vim (16) is example data. The other 34 sets came out of an automated research pipeline. See `commandsets/index.json`.
- **9 layouts:** `ansi-us-full`, `ansi-75`, `ansi-65`, `modifier-wheel`, `grouped-cards`, `poster-letter`, `mousepad-top10`, `mousepad-top25-map`, `compose-example`.
- **Modes:** Top 10 / Top 25 / All, plus **My list**, a custom list you can share as a link.
- **Platform variants** (Windows/Linux vs macOS) for sets that define them.
- **Combined views** for VS Code forks: Kiro, Antigravity and Cursor can be shown as base, fork-only, or combined with VS Code (`view=base|fork|combined`).
- **Exports:** SVG / PNG / PDF / Print for the current view. **Export all SVGs…** zips many apps × layouts × modes with a `manifest.json`.
- **Optional animation:** staggered reveal on load, spring press, morph and FLIP transitions, driven by shared motion tokens (`docs/animation.md`). Respects reduced motion.
- **Freshness badge**, optional per-command icons, light/dark themes, reduced motion, and a responsive phone/tablet UI.
- **Optional GA4 analytics** with Consent Mode v2. It stays off until a visitor opts in.
- **In-app feedback links** that open prefilled GitHub issue forms.

## Supported apps

Counts are commands per set, from `commandsets/index.json`. Omarchy and Unity were hand-built; the other sets came from the automated research pipeline.

**Office:** Microsoft Excel (671), Power BI (270), Microsoft PowerPoint (597), Microsoft Word (416)

**Google:** Google Docs (196), Google Sheets (154), Google Slides (214), Google Vids (209), NotebookLM (5)

**Code editors:** Google Antigravity (VS Code based IDE) (8), Kiro (6), Visual Studio Code (245), Cursor (47)

**Education:** Canvas LMS (Instructure) (84), Pressbooks (30)

**AI:** ChatGPT (76), Claude (claude.ai web/desktop) (29), Gemini (web app) (12), Grok Bot (37), Perplexity (2)

**Browsers:** Brave browser (97), Google Chrome (232), Perplexity Comet browser (17), Microsoft Edge (176), Mozilla Firefox (290)

**Creative & dev:** Omarchy (173), Unity Editor (80), Minimal Vim (example data) (16), Blender (155), Herdr (145), LM Studio (31), Obsidian (255), Ollama (96)

**Comms:** Discord (85), Panopto (26), Microsoft Teams (188), Zoom (126)

**Info-only entries (no documented shortcuts):** Grok (grok.com web app), Meta Muse (Meta AI app).

## How it came to be (short version)

The whole app was built by AI agents on a cloud computer: Grok Bot agents plus Cursor Agent CLI (Composer), Grok Build, the Antigravity CLI (`agy`) and the GitHub CLI. The maintainer described what he wanted and reviewed results. He did not edit the code himself. The times below are from file timestamps, git history, the GitHub Actions log and the Cloudflare tunnel log, all on **Oct 2, 2026 (MT)** unless noted.

| Time (MT) | What happened |
|---|---|
| 07:45 | ~3-minute voice note to the chief-of-staff bot: use Context7 for the docs, web search for what people actually use, generate SVG layouts with Top 10 / Top 25 / All, add animation (inspired by John Kim's video on animating AI-built apps), and keep it modular so others can add apps. The prototyper bot took the build from there. |
| 08:00–08:20 | Omarchy command/popularity scripts, layouts, animation tokens. First keyboard renders at 08:17. |
| 08:26 | Omarchy mousepad Top 10 screenshot shared in chat |
| 08:39–08:48 | Unity set rendered across layouts; screenshot shared at 08:48 |
| 08:55 → 09:15 | `cloudflared` downloaded; quick tunnel live at 09:15 to test the app from outside the box |
| 12:45–13:22 | Work split into small job specs (J1…); research jobs for ~33 more apps |
| 13:46–13:52 | GitHub repo created and first push |
| ~13:50–13:58 | Grok Build usage balance exhausted (HTTP 402) during popularity research; research moved to `agy` |
| 16:04 | First successful **Deploy to IONOS** run → https://atomicego.com/proj/hotkeyatlas/ (the GitHub Pages workflow failed twice and was later disabled) |
| 19:30 | "Final build" commit: 37 sets, export-all, consent, responsive UI |
| Oct 3, 10:58–11:07 | Public feedback repo with issue forms; in-app "Report a bug / Suggest" links; redeployed |

So a working Omarchy + Unity prototype took **about one hour** from the voice note, and it was on a public tunnel URL in **about 1.5 hours**. The rest of the day went to more apps, tests, and getting deployment working.

## Run it locally

The source repo is private. If you have a copy:

```bash
cd hotkey-atlas            # repo root
python3 -m http.server 8080
# open http://localhost:8080/?set=omarchy&mode=top10
```

There is no build step and no npm install for the app itself. Opening `index.html` from disk does **not** work, because browsers block ES modules on `file://`. Use any static server.

### URL parameters

| param | values |
|---|---|
| `set` | any id in `commandsets/index.json` (e.g. `omarchy`, `unity`, `vscode`) |
| `view` | `base` \| `fork` \| `combined` (sets that inherit, e.g. `kiro`) |
| `platform` | `win` \| `mac` (when the set defines `app.platforms`; defaults from your OS) |
| `layout` | any id in `layouts/index.json` |
| `mode` | `top10` \| `top25` \| `all` \| `mine` |
| `list`, `ltitle` | compact shared custom list (with `mode=mine`) and its title |
| `layer` | e.g. `SUPER%2BSHIFT` |
| `cmd` | command id to select |
| `theme` | `dark` \| `light` |
| `motion` | `off` |
| `icons` | `on` \| `off` |
| `asof` | `YYYY-MM-DD`, for testing freshness thresholds |

Examples: `?set=unity&layout=modifier-wheel&mode=top25`, `?set=unity&platform=mac&layout=poster-letter`, `?set=kiro&view=combined&mode=top25`, `?layout=mousepad-top10`. These work on the [live site](https://atomicego.com/proj/hotkeyatlas/) too.

## Architecture

The app is a static site: `index.html`, ES modules, CSS and JSON. It needs no server runtime and no framework.

```
index.html              page shell
config/site.json        defaults, analytics id, submit + feedback repos, optional example product link
commandsets/            one JSON per app + index.json {sets, groups, info_only}
layouts/                layout templates (JSON) + index.json
data/                   sources.json (where each set's data came from), freshness.json, upstream.*.json snapshots
src/core/               schema.js (validation), model.js, keynames.js, layout-engine.js (geometry),
                        renderer.js (orchestration), compose-engine.js, primitives/ (SVG building blocks),
                        legacy/ (pre-"fit" renderer copies, see below), combine.js (fork views),
                        commandsetIndex.js, listcodec.js / listschema.js (custom lists), icons.js, zip.js
src/anim/               easing.js (tokens, bezier, spring), motion.js (reveal, press, morph, FLIP, particles)
src/ui/                 app.js, export.js, exportScene.js, exportAll.js, lists.js, listpanel.js, settings.js,
                        storage.js, consent.js, analytics.js, sharesubmit.js, feedback.js, freshness.js,
                        mobile.js, stripNativeTitles.js
styles/                 tokens.css (easing/duration/spring custom properties), app.css
scripts/                build_apps.py, build_omarchy.py, build_unity.py, build_deploy.py, make_bundle.py,
                        build_downloads.py, validate*.mjs, test_*.{mjs,py}, e2e.py, qa_layouts.py,
                        snapshot_svgs.mjs, perf_render.mjs, verify_research.py, shots_*.py
tools/                  stamp_metadata.py, check_freshness.py, freshness_lib.py, ingest.py, validate_submission.mjs
research/               _SPEC.md, _POPULARITY_SPEC.md, <app>.json / .md / .popularity.json (cache/ and logs/ are git-ignored)
docs/                   schema, layouts, primitives, adding sets/apps, hosting, feedback, privacy, mobile, roadmap…
downloads/              pre-built Omarchy/Unity mousepad files (svg/png/pdf, with -bleed variants), layout-templates.zip
submit/                 optional PHP endpoint for submissions (submit.php)
inbox/                  drop zone for tools/ingest.py (Hyprland .conf, Lua binds, markdown cheat sheets)
work/                   agent work queue (STAGED.md / DONE.md), see "How it is developed"
.github/workflows/      deploy-ionos.yml, sftp-test.yml, freshness.yml, pages.yml (disabled)
lists*.md               human-readable ranked lists and method notes per set
```

### Rendering pipeline

```
commandsets/<id>.json ──┐
                         ├─► model (filter by mode / layer / platform / view)
layouts/<id>.json  ─────┘        │
                                 ▼
               renderScene(commandSet, layout, mode, options) → { svg, viewBox, … }
                                 │
            ┌────────────────────┼─────────────────────┐
            ▼                    ▼                     ▼
       on screen (+ motion)   SVG/PNG/PDF export   Export-all ZIP (manifest.json)
```

- `renderScene` is a pure function, so it also runs in Node (`scripts/render_all.mjs`). Exports match what is on screen byte for byte. Export-all reuses the same export scene.
- **Fit vs legacy rendering.** Researched sets carry `app.fit: true`. That turns on newer fitting logic: chip shrink, capacity trimming on print layouts, `shortName` titles, badge clamping. Sets without it go through copies of the original renderer in `src/core/legacy/`. This keeps the original Omarchy/Unity/Vim output **byte-identical**: the snapshot test compares 200 SVGs against a baseline.
- Animation is optional, always returns to rest, and turns off with `prefers-reduced-motion` or the Motion button:

| technique | where | purpose |
|---|---|---|
| CSS keyframes + stagger | layout load / switch | the layout "builds" from the top-left; delay = distance, capped at 700 ms |
| Spring (JS integrator) | pointer press on keys/rows | physical feedback, overshoots and settles |
| Path morph | selecting a key | outline eases from keycap to pill and back |
| FLIP | mode switch, filtering | items that stay glide to their new position |
| Particles | selecting | ~10 tiny circles, removed when finished |
| Easing tokens | `styles/tokens.css` | `--ease-*`, `--dur-*`, `--spring-*`, `--stagger` shared by CSS and JS |

## How data, layouts and modes work

**Command sets** (`commandsets/<id>.json`, schema v1):

- `app`: name, `mainModifier`, `platforms` (with `modifierLabels` and `physicalModifierMap`, so that, for example, logical Ctrl prints as Cmd on macOS), `meta` (source version, retrieval date, source URLs, `stale_after_days`), licence/credit text, optional `inherits` (forks).
- `categories`: id, name, colour.
- `modes`: `{ "top10": 10, "top25": 25, "all": null }`.
- `commands[]`: `keys` + `modifiers` (layout key ids such as `W`, `RETURN`, `SUPER`), `keysMode` (`chord` | `any` | `sequence`), `action`, `description` (paraphrased), `category`, `popularity {score, rank, signals, sources}`, `source {url…}`, `verified`, `last_verified`, optional per-platform overrides and `icon`.

**Ranking.** Top 10 / Top 25 are taken from `popularity.rank`. For researched sets, `scripts/build_apps.py` scores each command the same way `build_unity.py` does:
`3 × (appears in a source's "first list") + 2 × (independent sources) + official prominence + log10(1 + observed views) / 2`.
Popularity evidence lives in `research/<id>.popularity.json`, mostly public view counts of tutorials and guides. When no popularity research exists, a set ranks by official prominence only and says so in `ranking.notes`.

**Layouts** (`layouts/<id>.json`) have a `type`: `keyboard`, `wheel`, `cards`, `poster`, `hero`, `map` or `compose`. Keyboard keys are placed in key units. Print layouts carry a `physical` block (inches, units per inch, safe margin, bleed, dpi). The mousepad is 9.25 × 8 in and exports a 2775 × 2400 px PNG at 300 dpi. `compose` layouts are built from SVG primitives.

**Modes.** A mode picks how many ranked commands to draw. Print layouts with limited room cap "All" and mark the SVG with `data-capacity-limited`. "My list" draws the commands you picked. The list is stored locally (cookie/localStorage, only with consent) and can be shared as a compact URL.

## Adding an app

Pipeline route (how the 34 researched sets were made):

1. **Research.** Write `research/<id>.json` per `research/_SPEC.md`. Use Context7 first when a library exists, then the vendor's official shortcut pages. Every command needs a real `source_url`. Third-party rows are `verified: false`. Record what was used in `tools_used`.
2. **Popularity (optional).** Write `research/<id>.popularity.json` per `research/_POPULARITY_SPEC.md`, using only numbers that can actually be seen on the source pages.
3. **Build.**
   ```bash
   python3 scripts/build_apps.py --only <id>     # writes commandsets/<id>.json, lists-<id>.md,
                                                 # updates commandsets/index.json and data/sources.json
   python3 scripts/verify_research.py <id>       # spot-check keys against the cited pages
   node scripts/validate.mjs
   ```
4. **Check it in the browser:** `?set=<id>` on every layout and mode. Run `scripts/qa_layouts.py` to catch text overflow.

Apps with no documented shortcuts go in `info_only` in the index. They show under Settings → "Apps without keyboard shortcuts" and not in the main picker.

**New layout:** add `layouts/<id>.json` and its id in `layouts/index.json`, then run `node scripts/validate.mjs`. That checks overlaps, unknown keys and physical sizes.

**Bulk import:** drop Hyprland `.conf`, Lua binds or markdown cheat sheets in `inbox/` and run `python3 tools/ingest.py`. Proposals go to `inbox/out/` for review. Imported commands start unverified with popularity 0.

If you only want to *request* an app or a correction, you don't need any of this. Open a "Request or correct a command set" issue (below).

## Tests

```bash
node scripts/validate.mjs            # schemas, overlaps, unknown keys, physical sizes
node scripts/validate_sources.mjs
node scripts/test_zip.mjs            # ZIP writer (STORE + CRC32)
node scripts/test_lists.mjs          # custom-list URL codec
node scripts/test_primitives.mjs     # compose layout + primitive registry
node scripts/perf_render.mjs         # worst-case render time (PERF_MS limit)
pip install playwright               # once, for browser checks; set CHROME_PATH to use an installed Chrome
python3 scripts/qa_layouts.py        # text overflow/overlap for every layout × mode (server must be running)
python3 scripts/e2e.py               # interaction, URL params, exports, mobile; E2E_SECTIONS=… / E2E_QUICK=1
node scripts/snapshot_svgs.mjs /tmp/cur && node scripts/snapshot_svgs.mjs --compare /tmp/baseline-svgs /tmp/cur
python3 scripts/test_subpath.py      # dist/ served under a sub-folder (e.g. /proj/hotkeyatlas/)
```

The Oct 2 final report lists the last results: validate 0 errors, 200/200 baseline SVGs identical, qa_layouts 0 issues, e2e sections passing when run in groups, and a worst render of about 1.4 s against a 3 s limit.

## Build and deploy

```bash
python3 scripts/build_deploy.py      # → dist/ + dist/hotkey-atlas-static.zip (relative paths, sub-folder safe)
```

Upload the contents of `dist/` to any static host. Copy `.htaccess` for Apache (MIME types, caching, no directory listing).

The live site uses `.github/workflows/deploy-ionos.yml`. It runs on a published release or by manual dispatch (with an optional tag for rollback and a dry-run flag). It builds `dist/`, attaches the zip to the release, and mirrors it to the web host over SFTP with `lftp`. Credentials come from repository secrets. `sftp-test.yml` is a manual connectivity check. The GitHub Pages workflow is disabled.

## How it is developed

There is no human-written code here. A "program manager" bot breaks work into small job specs and hands them to coding CLIs: mostly Cursor Composer, early on Grok Build, and `agy` for research. The bot then runs the tests itself.

Two lessons shaped the current process:

- **Small jobs beat big prompts.** Long, open-ended requests stalled or caused regressions. One renderer change needed six follow-up jobs (J12–J12f), and the fix in the end was to restore the original renderer as `src/core/legacy/`. The work is now split into many small, testable tasks.
- **Don't interrupt the CLI. Use files.** Instead of the bot interrupting a running coding agent, tasks are appended to a queue file and the coding agent appends its results to a log that the bot tails. In this repo that is `work/STAGED.md` (queued tasks, one block per task with acceptance checks) and `work/DONE.md` (append-only: files changed, what was done, tests, leftovers). Watch it with `tail -f work/DONE.md`. This process is still being refined.

Resource note: web-heavy research through coding-agent CLIs is expensive. Each fetched page goes back into the model's context. Single popularity-research jobs used roughly 1.8–3.3 million tokens each before the Grok Build balance ran out. Plain scripts are a better fit for scraping/fetching. Save the model for extraction and code.

## Feedback and bug reports

All feedback goes through **GitHub Issues** on this public repo:

**https://github.com/Ryfter/hotkey-atlas-feedback/issues/new/choose**

The source repo is private. You don't need access to it to report anything. This repo only holds issues and this README.

The app's **Report a bug**, **Suggest** and **Request or fix a command set** links (footer, header menu, next to the command-set picker) open these forms with the app, layout, mode, browser and build date already filled in. You can also open them directly:

| Form | Use it for |
|---|---|
| [Bug report](https://github.com/Ryfter/hotkey-atlas-feedback/issues/new?template=bug.yml) | something wrong, missing, or broken |
| [Suggestion](https://github.com/Ryfter/hotkey-atlas-feedback/issues/new?template=suggestion.yml) | feature ideas and improvements |
| [Request or correct a command set](https://github.com/Ryfter/hotkey-atlas-feedback/issues/new?template=command-set.yml) | a new app, or a wrong/missing shortcut |

**A good bug report includes:**

1. **App / command set** and its **version**, plus your **OS and version** (e.g. "Unity 6, Windows 11" or "Omarchy 4.0, Arch").
2. **Layout and mode**, e.g. "Mousepad Top 10, macOS platform".
3. **Steps to reproduce.**
4. **Expected vs. actual.** For a wrong shortcut, say which one, what it shows, and what it should be.
5. A **screenshot**. You can paste or drag an image into the GitHub issue.
6. **Browser and device**, e.g. "Firefox 131, Windows 11" or "Safari, iPhone 15".
7. A **link to the exact page**. The URL holds the full state, e.g. `https://atomicego.com/proj/hotkeyatlas/?set=unity&layout=poster-letter&mode=top25&platform=mac`.
8. The **build date** (shown in the bug form when opened from the app).

**Proposing a new app or a correction:** use *Request or correct a command set*. Include the app name and version, and a link to the **vendor's official shortcut documentation**. That link is required, because every shortcut has to be checkable against a source. For corrections, list the shortcut, what it says now, and what it should say. If you have a full set or a ranked list as JSON (from the app's **Share / Submit** dialog), attach it or link a gist. Submissions are reviewed by hand. Nothing is auto-published.

Issues are public. Please don't include personal information.

## Known limits

- Proof of concept. Data was gathered by AI agents with source links. It has been spot-checked by script, not proof-read line by line by a human.
- Some vendors' docs had no usable shortcut tables in Context7, so official pages were used. Grok and Meta Muse had no verified shortcuts and are info-only.
- Unity 6 has no printed default-shortcut table, so the base list is the last full official table (2017.2) plus Unity 6 pages. Omarchy covers Hyprland desktop bindings only (not tmux/Ghostty/Neovim).
- "All" on poster/mousepad layouts is capacity-limited. Export-all ZIPs are uncompressed (STORE) and use a UTC date in the filename.
- The mousepad "Get this mousepad" link in `config/site.json` is an example affiliate slot (`isExample: true`).

## Licence and credits

Code: MIT. Shortcut facts are transcribed from each vendor's documentation, with descriptions paraphrased. Product names are trademarks of their owners and the project is not affiliated with them.
