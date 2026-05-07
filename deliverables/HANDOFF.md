# ENEB352 Final Presentation Deck Handoff

## Current State

The ENEB352 CDN final presentation is live on GitHub Pages and the verified live deck has been synced back into this assignment folder as `deck.html`.

Live desktop deck: `https://kkamali04.github.io/eneb352-deck/`
Live mobile notes deck: `https://kkamali04.github.io/eneb352-deck/mobile.html`
Live speaker notes page: `https://kkamali04.github.io/eneb352-deck/notes.html`
GitHub Pages repo: `https://github.com/kkamali04/eneb352-deck`

## Verified Live Commit

Latest deployed commit: `bd7702d` - `Refine K mark, ships, stat labels, and replay reset`

Recent deck repo commits:

```text
bd7702d Refine K mark, ships, stat labels, and replay reset
116c86c Use project favicon across deck pages
32bac96 Validate CDN claims and align presentation wording
```

## Synced Assignment Files

```text
C:\college-github\college-main\UMD\s2-spring26\Introduction to Networks and Protocols (ENEB352)\Final Research Presentation\deck.html
C:\college-github\college-main\UMD\s2-spring26\Introduction to Networks and Protocols (ENEB352)\Final Research Presentation\favicon.svg
C:\college-github\college-main\UMD\s2-spring26\Introduction to Networks and Protocols (ENEB352)\Final Research Presentation\kkamali_ENEB352_presentation.pdf
C:\college-github\college-main\UMD\s2-spring26\Introduction to Networks and Protocols (ENEB352)\Final Research Presentation\kkamali_ENEB352_presentation.pptx
C:\college-github\college-main\UMD\s2-spring26\Introduction to Networks and Protocols (ENEB352)\Final Research Presentation\technical-report.md
C:\college-github\college-main\UMD\s2-spring26\Introduction to Networks and Protocols (ENEB352)\Final Research Presentation\speaker-notes.md
```

## What Was Fixed

1. Rebuilt the hero K mark as a clean SVG stroke-draw network mark with 3 strokes and 5 nodes.
2. Kept the globe/sphere visual while making the K readable.
3. Rebuilt background ships into small rocket silhouettes with body, fins, cockpit, and intermittent thruster fire.
4. Removed the orange mouse glow blob while keeping mouse-reactive particles and connection lines.
5. Made stat labels reveal with their number counters instead of requiring a separate click.
6. Hardened slide replay reset so returned slides start hidden and replay correctly.
7. Reworked terminal typing so terminals type real characters and preserve formatted HTML.
8. Updated favicon links and included `favicon.svg`.
9. Reworded unsupported CDN claims and aligned key facts with verified sources.

## Verification Evidence

Live verification completed against `bd7702d`:

1. Hero K check: 3 `.globe-arc` strokes, 5 `.globe-pop` nodes, zero console issues.
2. Replay reset check: after returning to Slide 2, only title is visible; body/stat/label content is hidden before replay.
3. Stat timing check: stat number and label opacity both reach `1` in the same reveal step.
4. Synced local assignment deck hash matches deployed source `index.html`.
5. Synced local assignment `deck.html` loads with 3 strokes, 5 nodes, favicon link, and zero console warnings/errors.

Local QA artifacts:

```text
C:\Users\Kian\AppData\Local\Temp\eneb352-deck\qa\eneb352-slide-walkthrough-bd7702d.webm
C:\Users\Kian\AppData\Local\Temp\eneb352-deck\qa\k-globe-live-bd7702d.png
```

## Local Repo Notes

GitHub Pages repo path:

```text
C:\Users\Kian\AppData\Local\Temp\eneb352-deck
```

That repo is clean except local-only untracked artifacts:

```text
convert.py
qa/
```

College repo has unrelated dirty files outside this assignment folder. Do not stage or revert unrelated files.

Relevant current changes in `college-main` for this handoff:

```text
M  UMD/s2-spring26/Introduction to Networks and Protocols (ENEB352)/Final Research Presentation/deck.html
M  UMD/s2-spring26/Introduction to Networks and Protocols (ENEB352)/Final Research Presentation/HANDOFF.md
?? UMD/s2-spring26/Introduction to Networks and Protocols (ENEB352)/Final Research Presentation/favicon.svg
```

## How To Run Locally

```powershell
python -m http.server 8765 --bind 127.0.0.1
```

Run that command from:

```text
C:\college-github\college-main\UMD\s2-spring26\Introduction to Networks and Protocols (ENEB352)\Final Research Presentation
```

Then open:

```text
http://127.0.0.1:8765/deck.html
```

## Obsidian Notes

Current session log:

```text
C:\Users\Kian\Documents\Claude Code Brain\eneb352-cdn-final\session-2026-05-06-deck-stabilization.md
```

Older review notes:

```text
C:\Users\Kian\Documents\Claude Code Brain\01-projects\eneb352-cdn-final\Deck Review 2026-05-05.md
C:\Users\Kian\Documents\Claude Code Brain\01-projects\eneb352-cdn-final\Implementation Plan 2026-05-05.md
C:\Users\Kian\Documents\Claude Code Brain\01-projects\eneb352-cdn-final\GitHub Pages Deployment 2026-05-05.md
```

## Remaining Work

1. Commit the synced `deck.html`, `favicon.svg`, and this updated `HANDOFF.md` in `college-main` if Kian wants the assignment repo updated.
2. Do not commit local QA videos/screenshots unless explicitly requested.
3. Do not stage unrelated dirty files in `college-main`.
