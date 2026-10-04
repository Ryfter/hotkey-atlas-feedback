# Hotkey Atlas

Keyboard-shortcut cheat sheets drawn as SVG, for the apps people actually use. Pick an app, pick a layout (a full keyboard, a modifier wheel, a poster, a printable mousepad), and see its Top 10, Top 25, or every shortcut. Everything exports as SVG, PNG or PDF.

- **Live site:** https://atomicego.com/proj/hotkeyatlas/
- **Bugs, suggestions, new apps:** [open an issue](https://github.com/Ryfter/hotkey-atlas-feedback/issues/new/choose) (see [Feedback and bug reports](#feedback-and-bug-reports))
- **Status:** proof of concept and a work in progress. Expect rough edges.

## Why this exists

This started as an experiment. I watched a [video by John Kim](https://www.youtube.com/watch?v=f-Ar8mwm3kQ) ([X](https://x.com/PremiumGoblin), [Substack](https://substack.com/@realjohnkim)) about building animated apps with AI, and I wanted to try the idea myself.

About a year ago I had played with AI-generated SVG files to make cheat sheets for applications. SVG is portable, scales cleanly and is easy to restyle, so a whole set of cheat sheets seemed worth trying. I also wanted a quick MVP and tech demo for my GenAI Projects class at Boise State.

I needed example apps. Omarchy was the obvious one because of its enormous number of hotkeys. Unity was the second, because a student had mentioned using AI to help develop in Unity earlier that week.

## What it does

- **37 apps** across Office, Google, code editors, education, AI tools, browsers, creative and dev tools, and communication apps, with about 5,500 shortcuts in total.
- **Top 10, Top 25, or All** shortcuts for each app, plus **My list**: build your own and share it as a link.
- **Nine layouts**, including full and compact keyboards, a modifier wheel, category cards, a one-page poster, and printable 9.25 x 8 in mousepad templates.
- **Exports** to SVG, PNG and PDF, or print the current view. A bulk export zips many apps and layouts at once.
- **Platform variants** (Windows/Linux and macOS) where an app has them.
- **Combined views** for VS Code and its forks (Cursor, Kiro, Antigravity).
- **Light touches of animation**, light and dark themes, and a phone-friendly layout.
- Every shortcut links back to its source, and a freshness badge shows how old the data is.

## Supported apps

**Office:** Microsoft Excel, Power BI, Microsoft PowerPoint, Microsoft Word

**Google:** Google Docs, Google Sheets, Google Slides, Google Vids, NotebookLM

**Code editors:** Google Antigravity, Kiro, Visual Studio Code, Cursor

**Education:** Canvas LMS, Pressbooks

**AI:** ChatGPT, Claude, Gemini, Grok Bot, Perplexity

**Browsers:** Brave, Google Chrome, Perplexity Comet, Microsoft Edge, Mozilla Firefox

**Creative and dev:** Omarchy, Unity Editor, Blender, Herdr, LM Studio, Obsidian, Ollama, plus a small Vim example set

**Comms:** Discord, Panopto, Microsoft Teams, Zoom

Grok (the grok.com web app) and Meta Muse were researched, but neither publishes keyboard shortcuts, so they are listed as info-only.

## How it was built

I built it without touching any code. The tools were Grok Bot plus a set of developer tools running on its cloud computer: Cursor Agent, Grok Build, the Antigravity CLI and the GitHub CLI. I talked to my chief-of-staff bot by voice from my phone and laid out the basic app. It handed development to a prototyper and program-manager bot, which I then worked with by typing. That bot broke the work into jobs and ran the coding tools.

The first working version, covering Omarchy and Unity, took under two hours. Before it moved to my web host it was tested through a Cloudflare tunnel. I connected the project to GitHub for version control, and the GitHub bot walked me through a publishing pipeline from GitHub to the website.

A rough timeline, all on **Oct 2, 2026 (Mountain Time)**:

| Time | What happened |
|---|---|
| 07:45 | A ~3-minute voice note to the chief-of-staff bot describing the app |
| 08:17 | First keyboard renders |
| 08:48 | Omarchy and Unity both working, with screenshots shared |
| 09:15 | Cloudflare tunnel live so the app could be tested from outside |
| midday | Work split into small jobs; research for about 33 more apps |
| 13:46 | GitHub repo created and first push |
| 16:04 | First successful deploy to the web host |
| 19:30 | Final build commit: 37 apps, bulk export, cookie consent, phone-friendly UI. I was an hour from home watching my nephew play football when this build ran and finished. |

## Rendering pipeline

Command sets (one JSON file per app) and layouts (one JSON file per design) are combined by a single render function, so what you see on screen is exactly what gets exported.

```
command set ──┐
              ├─► filter by mode / platform / view ─► render ─┬─► on screen (+ motion)
layout ───────┘                                               ├─► SVG / PNG / PDF export
                                                              └─► bulk export (ZIP)
```

## What went wrong, and what I learned

- **The coding agents stall a lot.** Interrupting a running coding tool from the bot caused many problems. The fix in progress: the bot writes tasks to a queue file for the coding agent and appends updates at the end, and when the agent finishes it appends what it did to a log file, so the bot just reads the end of that file. The bot also learned to break requests into much smaller jobs.
- **Research through coding tools is expensive.** Pulling hotkey information from websites burned through my week of Grok Build usage. Next time: gather and scrape with plain scripts, use the Antigravity CLI for research, use Cursor and Grok models only for small targeted coding prompts, and spend freely only on the big builds.

## Feedback and bug reports

All feedback goes through **GitHub Issues** on this public repo: **https://github.com/Ryfter/hotkey-atlas-feedback/issues/new/choose**

The app's **Report a bug**, **Suggest** and **Request or fix a command set** links open these forms with the app, layout, mode and browser already filled in. You can also open them directly:

| Form | Use it for |
|---|---|
| [Bug report](https://github.com/Ryfter/hotkey-atlas-feedback/issues/new?template=bug.yml) | something wrong, missing, or broken |
| [Suggestion](https://github.com/Ryfter/hotkey-atlas-feedback/issues/new?template=suggestion.yml) | feature ideas and improvements |
| [Request or correct a command set](https://github.com/Ryfter/hotkey-atlas-feedback/issues/new?template=command-set.yml) | a new app, or a wrong or missing shortcut |

**A good bug report includes:**

1. The **app** and its version, plus your **OS and version** (for example "Unity 6, Windows 11").
2. The **layout and mode** (for example "Mousepad Top 10, macOS platform").
3. **Steps to reproduce**, and what you expected versus what happened. For a wrong shortcut, say which one, what it shows, and what it should be.
4. A **screenshot**. You can paste or drag an image into the issue.
5. Your **browser and device** (for example "Firefox, Windows 11" or "Safari, iPhone 15").
6. A **link to the exact page**. The URL holds the full state, for example `https://atomicego.com/proj/hotkeyatlas/?set=unity&layout=poster-letter&mode=top25&platform=mac`.

**To propose a new app or a correction,** use the *Request or correct a command set* form. Include the app name and version and a link to the vendor's official shortcut documentation, because every shortcut has to be checkable against a source. Submissions are reviewed by hand, and nothing is auto-published.

Issues are public, so please don't include personal information.

## Known limits

- Proof of concept. The data was gathered by AI agents with source links and spot-checked by script, not proofread line by line by a person.
- Some vendors publish little or nothing about shortcuts, so a few apps have thin lists.
- Unity 6 has no printed default-shortcut table, so its list combines the last full official table with newer Unity 6 pages. Omarchy covers Hyprland desktop bindings only.

## Licence and credits

Code: MIT. Shortcut facts are transcribed from each vendor's documentation, with descriptions paraphrased. Product names are trademarks of their owners, and this project is not affiliated with them.
