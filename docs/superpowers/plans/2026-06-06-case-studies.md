# Project Case Studies Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:executing-plans or superpowers:subagent-driven-development to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add three first-person case-study pages (one per featured project) under `projects/`, linked from the project cards, with the call-analysis page including pre-rendered static SVG diagrams; fix two factual details on the main page + résumé.

**Architecture:** Plain static HTML pages sharing the existing `style.css`/`main.js` via `../` relative paths (theme toggle persists across pages via `localStorage`). The two Mermaid diagrams are pre-rendered to SVG with `mermaid-cli` (using the system Chrome) and committed as static assets — no runtime/CDN dependency.

**Tech Stack:** HTML5, CSS3 (existing custom properties), vanilla JS (unchanged), `@mermaid-js/mermaid-cli` via `npx` for one-time SVG generation. Pandoc for résumé PDF.

**Project root:** `/home/hiiamhuy/Documents/github/hiiamhuy.github.io`

**Spec:** `docs/superpowers/specs/2026-06-06-case-studies-design.md`

**Note on testing:** Static site. "Verification" = grep gates + opening pages in a browser. Commit after each task.

**Shared page template** (used by Tasks 3–5; `{PLACEHOLDERS}` filled per page):

```html
<!DOCTYPE html>
<html lang="en" data-theme="dark">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>{TITLE} — Huy Nguyen</title>
  <meta name="description" content="{DESC}" />
  <link rel="icon" href="../assets/favicon.svg" type="image/svg+xml" />
  <link rel="stylesheet" href="../style.css" />
</head>
<body>
  <header class="nav subnav">
    <a class="nav__brand" href="../index.html#projects">← Portfolio</a>
    <nav class="nav__links">
      <a class="nav__resume" href="../resume.pdf" download>Resume</a>
      <button id="theme-toggle" class="nav__theme" aria-label="Toggle color theme">◐</button>
    </nav>
  </header>
  <main id="top" class="case">
    <p class="case__eyebrow"><a href="../index.html#projects">Projects</a> / {LABEL}</p>
    <h1>{TITLE}</h1>
    <p class="case__subtitle">{SUBTITLE}</p>
    <ul class="card__tags case__tags">{TAGS}</ul>

    {SECTIONS}

    <footer class="case__foot">
      <a class="btn btn--primary" href="{REPO_URL}">View Repo</a>
      <a class="btn" href="../index.html#projects">← Back to portfolio</a>
    </footer>
  </main>
  <script src="../main.js"></script>
</body>
</html>
```

Note: `main.js` only hijacks links whose href starts with `#`; the `../index.html#projects` links navigate normally. The theme toggle works on every page.

---

### Task 1: Add case-study CSS

**Files:** Modify `style.css` (append at end)

- [ ] **Step 1: Append these styles to the end of `style.css`**

```css

/* CASE STUDY PAGES */
.subnav .nav__brand { font-weight: 600; }
.case { padding-top: 2rem; padding-bottom: 4rem; }
.case__eyebrow { color: var(--muted); font-size: 0.85rem; margin: 0; }
.case__eyebrow a { color: var(--muted); }
.case h1 { font-size: clamp(1.8rem, 5vw, 2.6rem); margin: 0.3rem 0; }
.case__subtitle { color: var(--muted); font-size: 1.1rem; max-width: 65ch; margin: 0.2rem 0 0; }
.case__tags { margin: 1rem 0 2rem; }
.case h2 { font-size: 1.4rem; margin-top: 2.5rem; border-top: 1px solid var(--border); padding-top: 1.5rem; }
.case h3 { font-size: 1.1rem; margin-top: 1.5rem; }
.case p, .case li { max-width: 70ch; }
.case table { width: 100%; border-collapse: collapse; margin: 1.25rem 0; font-size: 0.92rem; }
.case th, .case td { border: 1px solid var(--border); padding: 0.5rem 0.7rem; text-align: left; vertical-align: top; }
.case th { background: var(--surface); }
.diagram { background: #0f1115; border: 1px solid var(--border); border-radius: var(--radius); padding: 1rem; margin: 1.5rem 0; overflow-x: auto; }
.diagram img { max-width: 100%; height: auto; display: block; margin: 0 auto; }
.diagram figcaption { color: var(--muted); font-size: 0.85rem; text-align: center; margin-top: 0.6rem; }
.case__placeholder { border: 1px dashed var(--border); border-radius: var(--radius); padding: 1rem 1.2rem; color: var(--muted); background: var(--surface); margin: 1.5rem 0; }
.case__foot { display: flex; gap: 1rem; flex-wrap: wrap; margin-top: 3rem; border-top: 1px solid var(--border); padding-top: 2rem; }
.case__ack { color: var(--muted); font-size: 0.85rem; font-style: italic; margin-top: 2rem; }
```

- [ ] **Step 2: Verify** — `tail -30 style.css` shows the block; existing CSS above intact (`grep -c "hero__name" style.css` → at least 1).
- [ ] **Step 3: Commit**

```bash
git add style.css
git commit -m "feat: add case-study page styles"
```

---

### Task 2: Pre-render the two Mermaid diagrams to SVG

**Files:** Create `projects/diagrams/call-pipeline.mmd`, `projects/diagrams/call-sequence.mmd`, `puppeteer-config.json` (repo root, gitignored-optional), and generated `projects/diagrams/call-pipeline.svg`, `projects/diagrams/call-sequence.svg`

- [ ] **Step 1: Create `projects/diagrams/call-pipeline.mmd`** with the flowchart (use the owner-supplied flowchart Mermaid verbatim, beginning `graph TD` and including the `subgraph`/`style` lines).

- [ ] **Step 2: Create `projects/diagrams/call-sequence.mmd`** with the sequence diagram (owner-supplied, beginning `sequenceDiagram`).

- [ ] **Step 3: Create `puppeteer-config.json`** at repo root:

```json
{ "executablePath": "/usr/bin/google-chrome", "args": ["--no-sandbox"] }
```

- [ ] **Step 4: Render to SVG** (dark theme, transparent background so the `.diagram` dark wrapper shows through):

```bash
cd /home/hiiamhuy/Documents/github/hiiamhuy.github.io/projects/diagrams
npx -y @mermaid-js/mermaid-cli@11 -i call-pipeline.mmd  -o call-pipeline.svg  -t dark -b transparent -p ../../puppeteer-config.json
npx -y @mermaid-js/mermaid-cli@11 -i call-sequence.mmd -o call-sequence.svg -t dark -b transparent -p ../../puppeteer-config.json
ls -la *.svg
```
Expected: both `.svg` files exist, non-zero size.

If `mmdc` fails (e.g. Chrome sandbox), retry once with `PUPPETEER_EXECUTABLE_PATH=/usr/bin/google-chrome` and `--no-sandbox` already in the config. If it still fails, STOP and report — do not hand-write SVGs.

- [ ] **Step 5: Sanity-check the SVGs** — `head -c 200 call-pipeline.svg` shows `<svg`. Open both in a browser to confirm they render legibly on a dark background.

- [ ] **Step 6: Commit** (keep `.mmd` sources; ignore the puppeteer config)

```bash
cd /home/hiiamhuy/Documents/github/hiiamhuy.github.io
echo "puppeteer-config.json" >> .gitignore
git add projects/diagrams/*.mmd projects/diagrams/*.svg .gitignore
git commit -m "feat: pre-render call-analysis architecture diagrams to SVG"
```

---

### Task 3: Build `projects/call-center-qa.html` (rich case study)

**Files:** Create `projects/call-center-qa.html`

- [ ] **Step 1: Create the page** using the shared template with:
  - `{TITLE}` = "Call Center QA — Transcription & Analysis"
  - `{LABEL}` = "Call Center QA"
  - `{SUBTITLE}` = "Automated transcription, speaker diarization, and AI quality-scoring for every UW IT Service Center support call — running entirely on local LLMs on UW's Hyak supercomputer."
  - `{TAGS}` = `<li>Python</li><li>WhisperX</li><li>Ollama · DeepSeek-R1 32B</li><li>SLURM</li><li>Apptainer</li>`
  - `{REPO_URL}` = `https://github.com/hiiamhuy/uwitsc-call-analysis`
  - `{SECTIONS}` in this order (content from the spec / owner writeup, first-person prose):
    1. `<h2>The problem</h2>` — manual QA bottleneck; a manager could grade only ~5–6 calls per cycle out of hundreds/week.
    2. `<h2>What it does</h2>` — grades every call, auto-triages, privacy-preserving (audio never leaves UW; no cloud API).
    3. `<h2>Architecture</h2>` — prose about decoupled SLURM stages + filesystem hand-off, then both diagrams:
       ```html
       <figure class="diagram">
         <img src="diagrams/call-pipeline.svg" alt="Pipeline: recordings → SLURM orchestrator → WhisperX transcription → Ollama scoring → score-based routing" />
         <figcaption>Per-agent SLURM fan-out; stages hand off through the shared filesystem.</figcaption>
       </figure>
       <figure class="diagram">
         <img src="diagrams/call-sequence.svg" alt="Sequence: manager uploads calls, transcription job writes VTT, analysis job scores and routes" />
         <figcaption>End-to-end sequence from upload to triaged review folders.</figcaption>
       </figure>
       ```
    4. `<h2>Tech stack</h2>` — an HTML `<table>` with the rows from the spec (Languages; Speech-to-text WhisperX large-v2; Diarization pyannote.audio; LLM scoring Ollama / DeepSeek-R1 32B; ML backend PyTorch + CUDA 12.2 / cuDNN 8; Containerization Apptainer/Singularity; Orchestration SLURM; Compute UW Hyak gpu-rtx6k; Storage gscratch filesystem; Data transfer Globus).
    5. `<h2>How it works</h2>` — `<ul>` of the five bullets (fan-out orchestration; transcription+diarization; rubric-based LLM scoring constrained to JSON; automatic triage at threshold 75; human-readable `analysis_report.md`).
    6. `<h3>Scoring rubric</h3>` — an HTML `<table>` with the six criteria and points (NetID acquisition ≤120s 10; Issue resolution 15; Instruction quality 15; Zoom verification 5; Confidentiality 7; Overall technical support quality 48; **Total 100**).
    7. `<h2>What I built</h2>` — the integration story: off-the-shelf WhisperX + Ollama, but mine is the SLURM orchestration, the Apptainer container defs, the six-component rubric + JSON-forcing prompt, and the score-based routing — the hard part was making it all cooperate on HPC.
    8. `<h2>Challenges &amp; learnings</h2>` — systems integration (GPU memory, model pre-staging vs. job timeouts, HF-token-gated diarization); next time: automate input population + a real job queue; rubric calibration is the known gap.
    9. `<h2>Results</h2>` — `<ul>`: works end-to-end; ~300 calls/week in ~8 hours on Hyak; coverage from ~5–6 to every call; demoed to UW ITSC leadership (wanted it ASAP); not yet in production; score-vs-human calibration is next.
    10. Screenshot placeholder:
        ```html
        <div class="case__placeholder">📷 Screenshot to add before publishing: a sample <code>analysis_report.md</code> (score table + rubric breakdown) and the sorted <code>needs_further_attention/</code> vs <code>reviewed/</code> folders — the most convincing artifact for this project.</div>
        ```
    11. `<p class="case__ack">Acknowledgements: Dawn Mai; thanks to Kaichen and Kristen for the Apptainer containers and Hyak guidance.</p>`

- [ ] **Step 2: Verify** —
  - `grep -c "diagrams/call-pipeline.svg\|diagrams/call-sequence.svg" projects/call-center-qa.html` → `2`
  - `grep -c "DeepSeek-R1 32B" projects/call-center-qa.html` → at least `1`
  - Open the page in a browser: nav back-link works, both SVGs render on dark panels, tables styled, theme toggle works.
- [ ] **Step 3: Commit**

```bash
git add projects/call-center-qa.html
git commit -m "feat: call-analysis case study page"
```

---

### Task 4: Build `projects/mrbs-trmnl.html` (medium)

**Files:** Create `projects/mrbs-trmnl.html`

- [ ] **Step 1: Create the page** using the shared template with:
  - `{TITLE}` = "MRBS → TRMNL E-Ink Integration"
  - `{LABEL}` = "MRBS → TRMNL"
  - `{SUBTITLE}` = "Putting live classroom schedules on cheap e-ink displays outside every room."
  - `{TAGS}` = `<li>FastAPI</li><li>SQLAlchemy</li><li>Pydantic</li><li>MySQL</li><li>Docker</li>`
  - `{REPO_URL}` = `https://github.com/hiiamhuy/Yanko-MRBS-FASTAPI`
  - `{SECTIONS}` (first-person prose, ~350–450 words, from the README):
    1. `<h2>The problem</h2>` — a campus/office with ~30 classrooms wants an always-on display outside each room showing that room's daily schedule, plus lobby displays showing all-room availability — without anyone updating them by hand.
    2. `<h2>Why I built it</h2>` — MRBS already holds the bookings; TRMNL e-ink displays are cheap and low-power; the gap was a service to translate one into the other and keep it current.
    3. `<h2>How it works</h2>` — `<ul>`: MRBS webhooks (on create/update/delete) POST to a FastAPI receiver for real-time updates; a background scheduler refreshes on an interval so displays never drift; read-only SQLAlchemy queries against the MRBS MySQL DB build each room's schedule; per-room and central/lobby payloads are pushed to the TRMNL cloud API; preview and manual-refresh endpoints make it testable without hardware.
    4. `<h2>Outcome &amp; what I learned</h2>` — a clean separation between the legacy PHP MRBS app and a small modern FastAPI service that only reads the DB and talks to one external API; learned webhook + scheduler patterns for keeping passive displays in sync, and how to design merge-variable payloads for a third-party display platform.

- [ ] **Step 2: Verify** — `grep -c "Yanko-MRBS-FASTAPI" projects/mrbs-trmnl.html` → `1`; open in browser, nav/theme work.
- [ ] **Step 3: Commit**

```bash
git add projects/mrbs-trmnl.html
git commit -m "feat: MRBS-TRMNL case study page"
```

---

### Task 5: Build `projects/pantheon-storage.html` (medium)

**Files:** Create `projects/pantheon-storage.html`

- [ ] **Step 1: Create the page** using the shared template with:
  - `{TITLE}` = "Pantheon Storage Analysis"
  - `{LABEL}` = "Storage Analysis"
  - `{SUBTITLE}` = "A one-command storage audit for Pantheon-hosted WordPress, Drupal, and generic sites."
  - `{TAGS}` = `<li>Bash</li><li>Terminus</li><li>WP-CLI</li><li>Drush</li>`
  - `{REPO_URL}` = `https://github.com/hiiamhuy/storage-analysis`
  - `{SECTIONS}` (first-person prose, ~350–450 words, from the README + CLAUDE.md):
    1. `<h2>The problem</h2>` — supporting web publishing on Pantheon, I kept needing to answer "what's using all the storage?" across dev/test/live for sites that might be WordPress, Drupal, or neither — and naive shell pipes against remote hosts kept choking on PHP warnings and broken-pipe errors.
    2. `<h2>Why I built it</h2>` — a repeatable, one-command audit that works regardless of platform and produces a shareable report, instead of ad-hoc SSH spelunking.
    3. `<h2>How it works</h2>` — `<ul>`: auto-detects the platform via `terminus site:info --field=framework`; routes to platform-specific analysis using `terminus remote:wp`/`remote:drush` eval (wp-content vs. sites/default/files, largest plugins/modules, DB size); processes command output in arrays rather than shell pipes to dodge broken-pipe errors and suppresses PHP warnings; can analyze one environment or all three with a comparison; exports to JSON, CSV, TXT, HTML, and Markdown.
    4. `<h2>Outcome &amp; what I learned</h2>` — a robust Bash tool that turns a fiddly manual task into one command; the real lesson was defensive shell engineering against messy remote output (error suppression, array-based processing, output filtering) and designing one script to handle multiple platforms cleanly.

- [ ] **Step 2: Verify** — `grep -c "storage-analysis" projects/pantheon-storage.html` → at least `1`; open in browser, nav/theme work.
- [ ] **Step 3: Commit**

```bash
git add projects/pantheon-storage.html
git commit -m "feat: Pantheon storage case study page"
```

---

### Task 6: Link cards + factual corrections (index.html, resume.md)

**Files:** Modify `index.html`, `resume.md`, regenerate `resume.pdf`

- [ ] **Step 1: Add "Case study →" links to each card.** In `index.html`, in each project card's `<div class="card__links">`, add a link after the existing Repo link:
  - Call Center QA card: `<a href="projects/call-center-qa.html">Case study →</a>`
  - MRBS card: `<a href="projects/mrbs-trmnl.html">Case study →</a>`
  - Storage card: `<a href="projects/pantheon-storage.html">Case study →</a>`

- [ ] **Step 2: Fix the call-analysis facts in `index.html`.** In the Call Center QA card description, change "Ollama (Llama 3.2)" to "a local LLM (DeepSeek-R1 32B) via Ollama". In its `<ul class="card__tags">`, change `<li>Docker</li>` to `<li>Apptainer</li>`.

- [ ] **Step 3: Fix the same facts in `resume.md`.** In the Call Center QA project line, change "Ollama (Llama 3.2)" to "DeepSeek-R1 32B via Ollama" and "Python, Docker." to "Python, Apptainer."

- [ ] **Step 4: Regenerate the PDF** — `pandoc resume.md -o resume.pdf && ls -la resume.pdf` (non-zero size).

- [ ] **Step 5: Verify** —
  - `grep -c "Case study →" index.html` → `3`
  - `grep -c "Llama 3.2\|>Docker<" index.html` → `0`
  - `grep -c "Llama 3.2\|Python, Docker" resume.md` → `0`
- [ ] **Step 6: Commit**

```bash
git add index.html resume.md resume.pdf
git commit -m "feat: link case studies; correct model/container facts (DeepSeek-R1 32B, Apptainer)"
```

---

### Task 7: Final verify + deploy

**Files:** none (push only)

- [ ] **Step 1: Whole-site link/content gate**

```bash
cd /home/hiiamhuy/Documents/github/hiiamhuy.github.io
ls projects/*.html | wc -l        # expect 3
ls projects/diagrams/*.svg | wc -l # expect 2
grep -rc "../style.css" projects/*.html   # each page references shared CSS
```
Expected: 3 pages, 2 SVGs, each page links `../style.css`.

- [ ] **Step 2: Browser pass** — open `index.html`, click each "Case study →" link, confirm each page loads with styling, the SVGs render (call page), tables look right, the theme toggle persists across navigation, and "← Back to portfolio" returns to `#projects`.

- [ ] **Step 3: Push (auto-deploys via Pages)**

```bash
git push
```

- [ ] **Step 4: Verify live** — after ~1 min, fetch each URL and confirm HTTP 200:

```bash
base="https://hiiamhuy.github.io/huy-portfolio"
for p in projects/call-center-qa.html projects/mrbs-trmnl.html projects/pantheon-storage.html projects/diagrams/call-pipeline.svg; do
  curl -s -o /dev/null -w "%{http_code}  $p\n" "$base/$p"
done
```
Expected: `200` for all four.

---

## Self-Review Notes

- **Spec coverage:** projects/ structure + shared `../` paths (template, T3–T5) ✓; slim subnav + template sections (T1 CSS, T3–T5) ✓; call page rich content incl. both tables, both diagrams, screenshot placeholder, acknowledgements (T3) ✓; MRBS + Pantheon medium pages from READMEs (T4, T5) ✓; "Case study →" card links (T6) ✓; factual corrections DeepSeek-R1 32B + Apptainer in index + résumé (T6) ✓; diagrams pre-rendered to static SVG via mmdc + system Chrome, sources kept, no CDN (T2) ✓; `.diagram` fixed-dark wrapper for legibility (T1) ✓; deploy (T7) ✓.
- **Placeholders:** the only placeholder is the intentional, spec-listed `analysis_report.md` screenshot callout (T3, non-blocking). No "TBD"/"implement later" steps.
- **Consistency:** class names match across CSS (T1) and pages (template/T3–T5): `subnav`, `case`, `case__eyebrow`, `case__subtitle`, `case__tags`, `diagram`, `case__placeholder`, `case__foot`, `case__ack`; reuses existing `nav`, `card__tags`, `btn`, `theme-toggle`. Repo URLs match index.html. Diagram `<img src>` paths (`diagrams/*.svg`) match the files generated in T2.
