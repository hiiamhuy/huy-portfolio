# Project Case Studies — Design Spec

**Date:** 2026-06-06
**Owner:** Huy Nguyen (hiiamhuy@uw.edu)
**Goal:** Add an individual case-study page for each of the three featured projects, written as a first-person narrative explaining the problem, why it was built, how it works, and the outcome — linked from the project cards on the main portfolio page.

**Parent context:** Builds on `2026-06-06-portfolio-design.md`. The site is plain static HTML/CSS/JS, deployed at https://hiiamhuy.github.io/huy-portfolio/.

## Scope

Three new pages plus links and a few factual corrections. No new framework, no build pipeline at runtime.

## Structure

A new `projects/` folder beside `index.html`:

```
projects/
├── call-center-qa.html
├── mrbs-trmnl.html
├── pantheon-storage.html
└── diagrams/
    ├── call-pipeline.svg      # pre-rendered from Mermaid
    └── call-sequence.svg      # pre-rendered from Mermaid
```

Each page shares the existing stylesheet and script via relative `../` paths
(`../style.css`, `../main.js`, `../resume.pdf`, `../assets/favicon.svg`), so the
dark/light theme toggle keeps working across pages (it persists via
`localStorage`).

## Page Template (all three pages)

1. **Slim top nav** — "← Back to portfolio" (→ `../index.html#projects`), a
   "Resume" link (→ `../resume.pdf`), and the theme-toggle button. Reuses the
   existing `.nav` classes plus a small `.subnav` tweak.
2. **Header** — project title (`<h1>`), one-line subtitle, and tech tags
   (reuse `.card__tags` pill style).
3. **The problem** — the situation that prompted the project.
4. **Why I built it** — goal and motivation.
5. **How it works** — the approach and architecture, in plain prose (plus tables
   / diagrams where noted below).
6. **Outcome & what I learned** — results and takeaways.
7. **Footer** — Repo link + "← Back to portfolio".

**Tone:** first-person, conversational ("I built this because…").
**Length:** call-analysis is the rich one (uses the full supplied content);
MRBS and Pantheon are medium (~350–450 words), written from their READMEs.

## Page Content

### projects/call-center-qa.html (rich)

Source: owner-supplied writeup (authoritative). Includes, in this order:
- Subtitle/blockquote: "Automated transcription, speaker diarization, and AI
  quality-scoring for every UW IT Service Center support call — running entirely
  on local LLMs on UW's Hyak supercomputer."
- **The problem:** QA was manual — a manager could grade only ~5–6 calls per
  cycle out of hundreds/week.
- **What it does / why:** grades every call, auto-triages the ones needing human
  review, keeps audio on UW infrastructure (no cloud API).
- **Architecture:** the two pre-rendered SVG diagrams (pipeline flowchart +
  sequence diagram) with the explanatory prose about decoupled SLURM stages and
  filesystem hand-off.
- **Tech Stack table** (languages, WhisperX large-v2, pyannote, Ollama serving
  DeepSeek-R1 32B, PyTorch/CUDA 12.2, Apptainer/Singularity, SLURM, Hyak
  gpu-rtx6k, gscratch filesystem, Globus).
- **How it works** bullets (fan-out orchestration, transcription+diarization,
  rubric-based LLM scoring constrained to JSON, automatic triage, reporting).
- **Scoring Rubric table** (six criteria, 100 points).
- **Key features** list.
- **What I built** (the orchestration, container defs, rubric+prompt, routing —
  the integration work, not the off-the-shelf models).
- **Challenges & learnings.**
- **Results** (works end-to-end; ~300 calls/week in ~8 hours on Hyak; coverage
  from ~5–6 to every call; demoed to ITSC leadership; not yet in production;
  score-vs-human calibration is the next step).
- **Screenshot placeholder:** a clearly-marked note where an `analysis_report.md`
  screenshot should go (owner to add before publishing — non-blocking).
- Acknowledgements line (Dawn Mai; Kaichen and Kristen).

### projects/mrbs-trmnl.html (medium)

Source: repo README. Cover: the use case (schools/offices with ~30 classrooms,
each with an e-ink display showing its daily schedule, plus lobby displays
showing all-room availability); why (keep cheap always-on displays current
without manual updates); how it works (MRBS webhooks → FastAPI receiver →
TRMNL cloud → e-ink; background scheduler for periodic refresh; preview and
manual-refresh endpoints; read-only MySQL queries); tech (FastAPI, SQLAlchemy,
Pydantic, MySQL, Apache/PHP MRBS, Docker dev env); outcome/learnings.

### projects/pantheon-storage.html (medium)

Source: repo README + CLAUDE.md. Cover: the problem (auditing storage on
Pantheon-hosted sites across WordPress/Drupal/generic, across dev/test/live,
where naive shell pipes hit PHP warnings and broken-pipe errors); why (fast,
repeatable storage reporting for web-publishing support); how it works
(platform auto-detection via `terminus site:info`, platform-specific analysis
through `terminus remote:wp`/`remote:drush` eval, array-based processing to
avoid broken pipes, multi-format export JSON/CSV/TXT/HTML/Markdown); tech
(Bash, Terminus, WP-CLI, Drush); outcome/learnings.

## Links From Main Page

In each project card's `.card__links`, add a second link after "Repo":
`<a href="projects/<slug>.html">Case study →</a>`.

## Factual Corrections (consistency with the authoritative writeup)

In `index.html` and `resume.md`, fix the call-analysis details:
- Scoring model: **DeepSeek-R1 32B** (was "Llama 3.2").
- Containers: **Apptainer/Singularity** (was tag "Docker").
- (Card desc may mention DeepSeek-R1 32B via Ollama; tag list swaps Docker → Apptainer.)

## Diagram Generation (build-time, committed as static assets)

The two Mermaid diagrams are pre-rendered to SVG once and committed; pages embed
them as `<img>`. No runtime JS/CDN dependency.

- Tool: `@mermaid-js/mermaid-cli` (`mmdc`) via `npx`, pointed at the existing
  system Chrome through a puppeteer config (`{"executablePath":
  "/usr/bin/google-chrome","args":["--no-sandbox"]}`) so no browser is
  downloaded.
- Source `.mmd` files are written from the supplied Mermaid, rendered to
  `projects/diagrams/*.svg`. The `.mmd` sources are kept in the repo for future
  edits.
- The diagrams carry their own dark node fills, so each is wrapped in a
  fixed-dark `.diagram` container for legibility in both light and dark themes.
- Fallback if `mmdc` fails for any reason: render the same `.mmd` once via the
  available headless Chromium and the Mermaid library, still committing static
  SVG. (Live mermaid.js in the page is explicitly NOT used.)

## CSS Additions (append to `style.css`)

- `.subnav` — slim back/Resume nav row.
- `.case` — prose container: comfortable max-width (~70ch within the existing
  `--max-width`), heading rhythm, paragraph spacing, list styling.
- `.case table` — bordered, readable tables for tech-stack and rubric.
- `.diagram` — fixed-dark, padded, horizontally scrollable wrapper for the SVGs;
  `img` is `max-width:100%`.
- `.case__placeholder` — dashed-border callout for the screenshot placeholder.
Reuse existing variables and the `.card__tags` pill style for tech tags.

## Out of Scope (YAGNI)

- No comments, RSS, dates, or a real blog engine.
- No required screenshots (a marked placeholder is fine to publish).
- No runtime diagram rendering / no CDN scripts.
- No change to the main page's "Built with plain HTML & CSS" footer (still true;
  diagrams are static SVG).

## Open Items (non-blocking)

- `analysis_report.md` screenshot for the call-analysis page (owner to add).
- The two MRBS/Pantheon pages have no owner-supplied extra content; written from
  READMEs and open to edits.
